# Git
https://github.com/TheJacksonLaboratory/jax-omero
https://github.com/TheJacksonLaboratory/jax-gcp-omero

# Docker compose build on jax-omero
```docker-compose -f docker-compose.build.yml build```
Errors on load metadata steps w/ authorization failed

- docker-compose v1 doesn't work on Mac ARM w/o Rosetta (?), docker compose 2 is automatically enabled.
- Docker desktop > Settings > Uncheck Use Docker Compose V2
    - use docker-compose to run v1, docker compose to run v2
- Gets farther in build using docker-compose v1, but now fails on building omero-web-base
    - docker-compose > zeroc_ice-3.6.5-cp36-cp36m-linux_x86_64.whl is not a supported wheel on this platform
    - Tried changing in playbook.yml, but it's the ome.omero_web role line that breaks because ansible-role-omero-web also references the whl: https://github.com/ome/ansible-role-omero-web/blob/d6fa479790aa272f8ebca048dfaf3ba4cca4a07e/defaults/main.yml
- Tried adding platform: "linux/amd64" to docker-compose.build.yml
    - Spent hours building, did get through omero-web-server and ice install, failed at neuroglancer-nginx
    - 
```
#12 4.492 npm ERR! Error while executing:
#12 4.492 npm ERR! /usr/bin/git ls-remote -h -t ssh://git@github.com/mikolajdobrucki/ikonate.git
#12 4.492 npm ERR! 
#12 4.492 npm ERR! Host key verification failed.
#12 4.492 npm ERR! fatal: Could not read from remote repository.
#12 4.492 npm ERR! 
#12 4.492 npm ERR! Please make sure you have the correct access rights
#12 4.492 npm ERR! and the repository exists.
#12 4.492 npm ERR! 
#12 4.492 npm ERR! exited with error code: 128
#12 4.492 npm verb exit [ 1, true ]
```
- Google's neuroglancer package.json uses "mikolajdobrucki/ikonate#a86b4107c6ec717e7877f880a930d1ccf0b59d89" to link ikonate, but idk that that tag exists
    - It's possible the failed verification is because of bad sha now?
- I forked neuroglancer and removed the tag, ran for longer but got errors
    - Could not resolve "ikonate/icons/controls-alt.svg"
    - neuroglancer references it in viewer.ts, but doesn't exist in current ikonate
- Docker system prune between changes, otherwise could try using some already downloaded github
- Add ```--platform linux/amd64``` to docker build command too

**Finished docker compose successfully with govekk/neuroglancer**

# Docker compose up on jax-omero
```
docker-compose -f docker-compose.test.yml up --renew-anon-volumes --force-recreate --abort-on-container-exit --exit-code-from omero-tests
```
- Same errors for both docker-compose and docker compose: omero-tests fail with
    - WARNING:omero.client:None - createSession retry: 1
    - Error (Ice.ConnectionRefusedException: Connection refused) ... will try again.
    - Error ('_BlitzGateway' object has no attribute '_ctx') ... will try again.
    - Exception: OMERO.server still not reachable, giving up

Debugging by trying to omero login, but need cli client
- Install via pip install omero-cli-zarr?
- ```docker exec -it -u 0 jax-omero_omero-server-jax_1 bash```
    - ```-u 0``` gives root user
- ```/opt/omero/server/venv3/bin/pip install zeroc-ice omero-cli-zarr```
- ```docker exec -it jax-omero_omero-server-jax_1 bash```
    - can't run ```omero login``` as root
- ```/opt/omero/server/venv3/bin/omero login --user omero --password omero --port 4064```
    - no server -> hangs on ```Server: [localhost:4064]``` or ```Server: [localhost:6064]``` or really any port I give it
        - If I enter while hanging, Ice.SocketException Cannot assign requested address
    - ```--server host.docker.internal``` givees Ice.ConnectionLostException: recv() returned zero
- Using ip linux commands to get IP (172.21.0.3) gives error Ice.ConnectionRefusedException: 172.21.0.3:6064 isn't running

**Omero server logs in /opt/omero/server/OMERO.server/var/log**

- calling omero admin status once omero-server shows ```Running 99-run.sh, Starting OMERO.server``` gives "/opt/omero/server/venv3/bin/omero admin status     
Missing internal configuration. Run 'omero admin rewrite'" which suggests config files haven't even been created, which is supposed to be first step?

# Docker build on jax-gcp-omero
```docker build --platform linux/amd64 --tag jax-omero-web jax-omero-web```
- Finished fine

```docker build --platform linux/amd64 --tag jax-omero-server jax-omero-server```
- Currently hanging on "head: cannot open '/etc/ssl/certs/java/cacerts' for reading: No such file or directory"
- **Added ca-certificates-java to the Dockerfile installs** and build finished

## Need to steal a docker compose
Inside jax-gcp-omero/containers:
```docker-compose -f docker-compose.test.yml up --renew-anon-volumes --force-recreate```

55-certs.sh
- PermissionError: [Errno 13] Permission denied: '/OMERO'
- from line 73 (```os.makedirs(certdir, exist_ok=True)```) in ome/omero-certificates/certificates.py
- ```certdir = cfgmap["omero.glacier2.IceSSL.DefaultDir"]```
- 
```
set_if_empty(
        "omero.glacier2.IceSSL.DefaultDir",
        os.path.join(cfgdict.get("omero.data.dir", "/OMERO"), "certs"),
    )
```
- Added ```CONFIG_omero_glacier2_IceSSL_DefaultDir: /opt/omero/server/OMERO.server``` to docker-compose yml
- **Nevermind, instead just gave omero-server user write access to all of /opt**

60-database.sh
- Permission error on sql file while initializing database (after successful connectiong to postgres)
- [omego/db.py init()](https://github.com/ome/omego/blob/master/omego/db.py#L102) comes up with a [filename based on timestamp](https://github.com/ome/omego/blob/master/omego/db.py#L106) uses [external.omero_cli](https://github.com/ome/omego/blob/master/omego/db.py#L109) to call ```omero db script```
- [omero db script](https://github.com/ome/omero-py/blob/master/src/omero/plugins/db.py#L304) calls [_create.py](https://github.com/ome/omero-py/blob/master/src/omero/plugins/db.py#L205) in the same file, which acts like it expects to be passed a python file object (i.e. after open()), not a file name string
- permission errors are from omero_ext/argparse.py because omero cli automatically loads [FileType](https://github.com/ome/omero-py/blob/master/src/omero_ext/argparse.py#L1131) arguments with specified mode (in this case, ["w"](https://github.com/ome/omero-py/blob/master/src/omero/plugins/db.py#L64-L74) - which should create a file that doesn't exist)

99-run.sh
- Hangs on stdout after "Starting OMERO.server"
- master.err says ```Ice::NoEndpointException:```
    - If call same command ```omero admin start --foreground``` from command line, get error "No such file or directory: 'icegridnode': 'icegridnode'"
    - [adding Ice.IPv6=0 to config fixed](https://www.openmicroscopy.org/community/viewtopic.php?p=17181)
- ```omero admin diagnostics``` gives "xml.etree.ElementTree.ParseError: no element found: line 1, column 0"
    - empty /opt/omero/server/OMERO.server/etc/grid/templates.xml

```omero admin diagnostics```
```
Commands:   java -version                  11.0.16   (/usr/bin/java)
Commands:   python -V                      3.7.3     (unknown)
Commands:   icegridnode --version          not found
Commands:   icegridadmin --version         not found
Commands:   psql --version                 11.17     (/usr/bin/psql)
Commands:   openssl version                1.1.1     (/usr/bin/openssl)

No icegridadmin available: Cannot check server list

Log dir:    /opt/omero/server/OMERO.server/var/log exists
Log files:  Blitz-0.log                    1.5 KB        errors=0    warnings=2   
Log files:  DropBox.log                    n/a
Log files:  FileServer.log                 n/a
Log files:  Indexer-0.log                  2.9 KB        errors=0    warnings=2   
Log files:  MonitorServer.log              n/a
Log files:  PixelData-0.log                3.5 KB        errors=0    warnings=2   
Log files:  Processor-0.log                1.1 KB       
Log files:  Tables-0.log                   1.3 KB       
Log files:  TestDropBox.log                n/a
Log files:  master.err                     4.3 KB        errors=0    warnings=2   
Log files:  master.out                     empty
Log files:  Total size                     0.01 MB


Environment:OMERO_HOME=(unset)             
Environment:OMERODIR=/opt/omero/server/OMERO.server 
Environment:OMERO_NODE=(unset)             
Environment:OMERO_MASTER=(unset)           
Environment:OMERO_USERDIR=(unset)          
Environment:OMERO_TMPDIR=(unset)           
Environment:PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin 
Environment:PYTHONPATH=(unset)             
Environment:ICE_HOME=(unset)               
Environment:LD_LIBRARY_PATH=(unset)        
Environment:DYLD_LIBRARY_PATH=(unset)  
```
- Tried adding lines from omero-ice36.env to Dockerfile and didn't get as far
- Worked *once* again for no apparent reason. Errors inside container (master.err) were the same
    - Worked a second time and ```./server_venv/bin/omero login --user root --password omero --port 4064 --server 0.0.0.0``` also worked ("Created session for root, Idelte timeout: 10 min") - copied logs to local Documents

Results:
- Sometimes it doesn't even get to creating the log directory after 10 min, and the omero-test network is 0B when I delete it. 
    - 4,5
- Sometimes it gets to creating all the logs within 2 minutes (including Blitz.log) but the web still can't access the server and docker login doesn't work
- Sometimes it creates all the logs and they look the same as when it works, but web server takes a while to try to log in and then says server not responding
    - omero login returns "0.0.0.0:4046 isn't running"
    - 1
- Sometimes it works, but there is still an Ice.NoEndpoint error in the master.err file
    - 2
- Sometimes takes a reaalllly long time on ```omero db script```
    - 3
- Even with ```docker compose up``` instead of ```docker-compose up``` on M1 I get all these different results and it only works some of the time

- Theories:
    - Connect to website right after logs created, but before any timeout errors?
    - Alternating?

Troubleshooting ```docker compose``` authentication errors because looking for _base images on docker.io instead of locally:
- ```docker compose``` automatically builds services in parallel, meaning that *jax* might try to pull *base* before it's created. `--parallel` flag is deprecated, so I'm just creating two docker files to run *base* first then *jax* - works on Linux
- According to [stack overflow](https://stackoverflow.com/questions/20481225/how-can-i-use-a-local-image-as-the-base-image-with-a-dockerfile#comment123595286_20501690), Dockerfile ```FROM``` command doesn't look locally first on M1 Macs
    - Probably gets mad because architecture types are different: **"Now in your Dockerfile change FROM IMAGE_NAME to something like FROM --platform=linux/amd64 IMAGE_NAME and docker would now use the local image."**




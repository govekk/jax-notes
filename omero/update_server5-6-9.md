# Things to update

Necessary
- Add memory cleaning extension jar
    - https://forum.image.sc/t/first-release-of-omero-process-container-steward/85067
        - https://github.com/glencoesoftware/omero-pc-steward#installation
- Bioformats PR fixing NDPI (is it in new bioformats version?)
    - https://github.com/ome/bioformats/pull/4083
    - Bioformats v7.0.1 should include: https://bio-formats.readthedocs.io/en/v7.0.1/about/whats-new.html (under hamamatsu ndpi)
    - Buuuuut OMERO.server 5.6.9 only includes BF 7.0.0
- OMERO.server 5.6.9: https://forum.image.sc/t/release-of-omero-server-5-6-9/86929 (test on dev first)
    - "lets ice choose the default SSL protocol" - might have SSL fun
    - "Bio-Formats memoizer cache will be invalidated"
        - can delete everything under /OMERO/BioFormatsCache to avoid log errors, but new cache files will just be created
    - https://omero.readthedocs.io/en/v5.6.9/sysadmins/server-upgrade.html 
- omero-certificates
    - current version is 0.2.0, suggested upgrade to 0.3.0
    - "If your server has been configured with a version of omero-certificates older than 0.3.0 or manually, the configuration may need to be upgraded in particular to disallow the deprecated TLS 1.0 and 1.1 protocols."
    - This is going to be an issue with the public server, since this version of omero-certificates has the read-only error
- omero-web
    - technically OMERO.server 5.6.9 only requires 5.22.1, which is what we have, but newest version is 5.23.0
    - 5.23.0 requires python 3.8+, so we can't use that one until we update the python on the web servers from 3.6

Nice to have
- Update Python to 3.8+, update ezomero to 2.0.0

Probably for next time
- New import method using transfer prepare
    - Run import via service account with sudo no password for only import, able to run cron tab

# Timeline
- Test necessary changes on Dev before Nov 30
- Submit change request Nov 30 for review Dec 5/6, implementation Dec 7
- Email users about short outage
- Attend CAB meeting Dec 5/6
- Implement change Dec 7

# Steps on dev

10 min before change time, email users with active sessions

## Server
```
ssh ctomerodev.jax.org
sudo su - svc-omero # or svc-omerodev on dev
cd /local/omero_app
wget https://downloads.openmicroscopy.org/omero/5.6.9/artifacts/OMERO.server-5.6.9-ice36.zip
unzip OMERO.server-5.6.9-ice36.zip
source omerovenv/bin/activate
omero admin stop
rm OMERO.server
rm OMERO.server-old
ln -s OMERO.server-5.6.9-ice36/ OMERO.server
ln -s OMERO.server-5.6.8-ice36/ OMERO.server-old

cp OMERO.server-old/etc/grid/config.xml OMERO.server/etc/grid

rsync --dry-run -a --ignore-existing --verbose OMERO.server-old/lib/scripts/ OMERO.server/lib/scripts
rsync -a --ignore-existing OMERO.server-old/lib/scripts/ OMERO.server/lib/scripts

# pip list -> omero-py = 5.15.0
pip install --upgrade 'omero-py>=5.16.0'
# pip list -> omero-py = 5.17.0

# For upgrade from omero-certificates 0.2.0 -> 0.3.0, remove protocol configs and regenerate certs
pip install --upgrade "omero-certificates>=0.3" # omero-certificates 0.3.1
omero config set omero.glacier2.IceSSL.Protocols
omero config set omero.glacier2.IceSSL.ProtocolVersionMax
omero certificates
```

`omero certificates` output:
```
WARNING:omero_certificates.certificates:Your Linux distribution has been detected as RHEL 7 which will reach end of life in June 2024.  TLS 1.3 cannot be enabled and upgrading is recommended.
See https://www.openmicroscopy.org/2023/07/24/linux-distributions.html for more information.
OpenSSL 1.1.1m  14 Dec 2021
certificates created: /local/OMERO/certs/server.pem /local/OMERO/certs/server.p12
```

add memory jar: https://github.com/glencoesoftware/omero-pc-steward#installation 
```
cd OMERO.server/lib/server
wget https://github.com/glencoesoftware/omero-pc-steward/releases/download/v0.1.0/omero-pc-steward-0.1.0.jar
cd ../../..
vi OMERO.server/etc/logback.xml
:/ROOT # search in VI for "ROOT loggers" section
# I decided to put at end of Internal section
#  <!-- omero-pc-steward memory cleanup logger -->
#  <logger name="com.glencoesoftware" level="INFO"/>
ESC :wq
```
Option to change the frequency of checking for import processes to clean - default 1 minute

Remove Bio-Formats memoization cache, because invalidated with Bio-Formats update
```
cd /local/OMERO/BioFormatsCache
rm -rf local
```
Confirmed recreated on dev after server start

```
omero admin start
```

----- server - import env -----
```
sudo su - svc-omerodata
rm -rf ~/.cache/OMERO.py/
pip install --upgrade --upgrade-strategy only-if-needed omero-py ezomero
```
Newest Pillow version doesn't work, so strict upgrade breaks/

### Confirm changes in place
`omero version`
OMERO.py version:
5.17.0
OMERO.server version:
5.6.9-ice36

`tail /local/omero_app/OMERO.server/var/log/Blitz-0.log`
2023-11-29 14:25:00,001 INFO  [       c.g.omero.ProcessContainerSteward] (-thread-14) Number of processes in the container: 0
2023-11-29 14:26:00,001 INFO  [       c.g.omero.ProcessContainerSteward] (-thread-11) Number of processes in the container: 0

## web: can't upgrade to 5.23.0 until upgrade python to 3.8, but not necessary
----- web -----
```
ssh ctomeroweb01.jax.org
sudo su - svc-omero # or omero-web on dev
cd /opt/omero/web
source venv3/bin/activate
omero web stop
rm /tmp/sessionid* # clear sessions store
pip install --upgrade omero-web omero-py omero-marshal
omero web start
```

## current versions
name                current     newest  update?
omero-py            5.15.0      5.17.0  yes
omero-certificates  0.2.0       0.3.0   yes
omero-web           5.22.1      5.23.0  eventually - need python 3.8 first
omero-figure        6.0.1       6.0.1   no
omero-iviewer       0.13.0      0.13.0  no
omero-marshal       0.7.0       0.8.0   yes - updated pypi Oct 26, 2022
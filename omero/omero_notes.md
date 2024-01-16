# JAX omero

## Public OMERO
images.jax.org

Project jax-omero-pub on Google Cloud

## Research OMERO
omeroweb.jax.org

Server: ctomero01lp.jax.org

Log file path: /local/omero_app/OMERO.server/var/log
`cat Blitz-0.log | grep "2023-07-13 9:5" | grep ERROR` Looking in the blitz logs for time and error

on prem

## Omero dashboard
omerodashboard.jax.org

## Omero developer login
Dev server (web client): ctomeroweb01.jax.org

Dev server: ctomerodev.jax.org, port: 4064

user: user-1 ; Trainer-1 ...

password: omero

My personal JAX account (govekk) is an admin

If omero web goes down https://gist.github.com/govekk/14f2895bc7c51c74e2e05f5e985bf9a9

## New servers
ctome01ld thru ctome04ld
govekk has access and sudo, govekk_pa no access

# Check versions
```
ssh ctomero01lp.jax.org # or ctomerodev.jax.org
sudo su svc-omero # or svc-omerodev
cd /local/omero_app/omerovenv/bin
./omero version
```
Need to be `su` to svc-omero or svc-omerodev to see the server version, otherwise just see omero-py version

```
ssh omeroweb.jax.org
cd /opt/omero/web/venv3/bin
./pip list
```

# JAX repos
https://github.com/TheJacksonLaboratory/ezomero

https://github.com/TheJacksonLaboratory/jax-omeroutils 

https://github.com/ome/omero-cli-transfer 

https://github.com/TheJacksonLaboratory/omero_for_developers

https://github.com/TheJacksonLaboratory/zarr_and_dask_demo

## Ezomero docs
sphinx

bash command in ezomero/docs/sphinx

check out documentation -> run sphinx commands (need environment with new ezomero and dependencies) -> push just to documentation branch

For sphinx environment, see conda environment `ezomero_sphinx`, built from `~/Documents/Repos/environment_ezomero.yml` and then `pip install -e ~/Documents/Repos/ezomero`

## Omero cloud (jax-omero-pub, runs at images.jax.org)
Switch cloud projects

Check names of contexts and which one you are in (*), look for koopa
```
kubectl config get-contexts
```

```
kubectl config use-context gke_jax-omero-pub_us-east1_koopa
```

```
gcloud config set project jax-omero-pub
```

## Restore cloud from backup
Restore Postgres database and Filestore (peach) and Files directory from backup
- Database is backed up continuously, Filestore is backed up manually each time there is a new data import to public

Run Kubernetes pods

# Adding new users
Access server (actually don't need to do this)
1. ssh into production server (ctomero01lp.jax.org) with main account
2. `sudo su - svc-omero`
3. omero login??

Create user and add to group
1. conda activate omero_devs
2. `omero ldap create <shortname> --server ctomero01lp.jax.org --port 4064 --user govekk_pa`
3. Add to group on web client, or `omero group adduser ...`
4. Change default group to research group instead of "default"

## Flow chart
Scientific service (histology, microscopy) group -> own group -> delivery -> no
Scientific service group -> own group -> small research or testing -> yes
Scientific service individual -> lab group -> research / helping on lab's data -> yes
JMCRS -> no? (maybe just on dev server for testing)

# Automated imports
Logs at `ctomero01lp.jax.org:/tmp/cron_logs`
Most recent run has full stderr in `cronlog.log`

## Manually import
```
ssh govekk_pa@ctomero01lp.jax.org 
sudo /local/omero_scripts/omerosubvenv/bin/python /local/omero_scripts/jax-omeroutils/import_workflow.py /path/to/folder
```
### Manually import just from import.json
scp import.json into Notes/omero/import_jsons then edit with VScode find and replace, then scp back under diff name
```
ssh govekk_pa@ctomero01lp.jax.org
sudo su - svc-omero
cd /local/omero_scripts
source omerosubvenv/bin/activate
screen
python ./jax-omeroutils/import_annotate_batch.py /hyperfile/omero/autoimport/korstanjelab/tcn_202307..../import_rkorstan.json
```

## KOMP imports
Ignore: fundus, farraa, Awill

# Microscopy delivery folders
1. Go to bh-microscopy in jax.org (mounted to computer)
2. Select folder associated with service
3. If folder not there, create folder ticket is valid
4. Check with Philipp: if ticket comes from him, not an issue
5. Check if requester is service or lab - bill for storage needs to go to correct PI etc
6. Create folder called what they request, but try to keep it standardized
7. Right click, in Security tab need to add AD group for that lab/group to permissions, check box next to Modify permissions, give read-only permission to microscopy group - best way to check for actual name of AD group is to go to different microscopy service folder that they use and check those permissions

## Do sleuthing before even reaching out
- What AD groups is the user in? Double check they haven't been added to MOD
- What AD groups have access to the folders they want?
- Were the removed from MOD in early 2023?

## Windows VM (don't add folders or change permissions from Mac)
- ctsmb01wd.jax.org
- Microsoft Remote Desktop, username govekk_pa
- In File Explorer: this PC -> map network drive -> \\jax.org\jax

## Microscopy docs:
https://github.com/TheJacksonLaboratory/microscopy_data_mgmt

# Will maintain
- Dropbox imports folder check once a week
- Tickets: add Omero users, and microscopy delivery folders

# Trainings
Carpentries tips on online workshops: https://carpentries.github.io/instructor-training-bonus-modules/index.html

# Transfer notes
omero-cli-transfer version 0.4.0 pack, 0.3.3 unpack

error in unpack
```
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/xmlschema/documents.py", line 298, in to_dict
    defuse, timeout, lazy, use_location_hints)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/xmlschema/documents.py", line 84, in get_context
    raise XMLSchemaValueError(msg)
xmlschema.exceptions.XMLSchemaValueError: cannot get a schema for XML data, provide a schema argument
```

Error in ome-types for machines without internet, will likely get fixed on pypi soon but was only pushed to github last week (Feb 15, 2023) - https://github.com/tlambert03/ome-types/issues/168

## Remove annotations
Remove all Rois under Dataset 3860:
```
omero delete Dataset/Roi:3860 --dry-run --report # check what will be deleted first
omero delete Dataset/Roi:3860
```
Remove all Attachments (FileAnnotations) under Dataset 3860:
```
./omero delete Dataset/FileAnnotation:3860 --include FileAnnotation --dry-run --report # check what will be deleted first
./omero delete Dataset/FileAnnotation:3860 --include FileAnnotation
```

## ssh passphrase on ctomero01lt.jax.org
ctomero

# Change certs / certificates
Concatenate certificates in correct order:
```
for f in "ServerCertificate.crt" "Intermediate.crt" "Root.crt"; do (cat "${f}"; echo) >> All.crt; done
```
Get base64:
```
cat images-test.jax.org.key | base64
cat All.crt | base64
```
Put base64 in nginx.sslsecret.yaml (make sure to check name is correct):
```
apiVersion: v1
data:
 nginx.key: <replace with base64 key>
 nginx.crt: <replace with base64 key>
kind: Secret
metadata:
 name: nginx-ssl
```

Note: if copying from other cluster, can use `kubectl get secret nginx-prod-ssl -o yaml > prod_secret.yml` to get the base64 key and crt

# Deleting annotations
```
omero-server@omero-server:/opt/omero/server/server_venv/bin$ ./omero delete Dataset/Roi:1 --dry-run --report --include Annotation
Using session for root@localhost:4064. Idle timeout: 10 min. Current group: system
omero.cmd.Delete2 Dataset/Roi:1 Dry run performed
ok
Steps: 5
Elapsed time: 0.915 secs.
Flags: []
Deleted objects
  Ellipse:13793-13795,13797,13798
  Line:13799
  Polygon:13796
  Roi:13793-13799
```

```
omero-server@omero-server:/opt/omero/server/server_venv/bin$ ./omero delete Dataset/FileAnnotation:1 --dry-run --report --include FileAnnotation
Using session for root@localhost:4064. Idle timeout: 10 min. Current group: system
omero.cmd.Delete2 Dataset/FileAnnotation:1 Dry run performed
ok
Steps: 5
Elapsed time: 0.881 secs.
Flags: []
Deleted objects
  FileAnnotation:6
  ImageAnnotationLink:1
  OriginalFile:97
```

# Upgrade

* Database upgrade and  version checking will be needed for larger version jump to 6

---- server -----

10 min before change time, email users with active sessions

```
ssh ctomerodev.jax.org
sudo su - svc-omero # or svc-omerodev on dev
cd /local/omero_app
# wget https://github.com/ome/openmicroscopy/releases/download/v5.6.7/OMERO.server-5.6.7-ice36.zip
wget https://downloads.openmicroscopy.org/omero/5.6.8/artifacts/OMERO.server-5.6.8-ice36.zip
unzip OMERO.server-5.6.8-ice36.zip
source omerovenv/bin/activate
omero admin stop
rm OMERO.server
rm OMERO.server-old
ln -s OMERO.server-5.6.8-ice36/ OMERO.server
ln -s OMERO.server-5.6.2-ice36-b226 OMERO.server-old

cp OMERO.server-old/etc/grid/config.xml OMERO.server/etc/grid

rsync --dry-run -a --ignore-existing --verbose OMERO.server-old/lib/scripts/ OMERO.server/lib/scripts
rsync -a --ignore-existing OMERO.server-old/lib/scripts/ OMERO.server/lib/scripts

pip install --upgrade omero-py

omero admin start
```

----- server - import env -----
```
sudo su - svc-omerodata
rm -rf ~/.cache/OMERO.py/
pip install --upgrade --upgrade-strategy only-if-needed omero-py ezomero
```
Newest Pillow version doesn't work, so strict upgrade breaks/

----- web -----
```
ssh ctomeroweb01.jax.org
sudo su - svc-omero # or omero-web on dev
cd /opt/omero/web
source venv3/bin/activate
omero web stop
rm /tmp/sessionid* # clear sessions store
pip install --upgrade omero-web omero-figure omero-iviewer omero-py
omero web start
```

## From 5.6.8 upgrade, needed to set new Figure_To_Pdf script
----- omero.figure -----
https://github.com/ome/omero-figure#upgrading-omerofigure
```
# Get the ID of the existing Figure_To_Pdf script:
ssh ctomero01lp.jax.org # or ctomerodev.jax.org
sudo su - svc-omero # or svc-omerodev on dev
cd /local/omero_app
source omerovenv/bin/activate
omero script list # record ID of Figure_To_Pdf script

# Replace the script
wget https://files.pythonhosted.org/packages/c5/75/ef5d6859bed34582d66affdf45842a8b08ca80ec61a7c53c980122bec673/omero-figure-6.0.1.tar.gz
tar -xf omero-figure-6.0.1.tar.gz
cd omero-figure-6.0.1/omero_figure/scripts
omero script replace <SCRIPT_ID> omero/figure_scripts/Figure_To_Pdf.py
```
omero script replace 56787 omero/figure_scripts/Figure_To_Pdf.py
govekk is admin on dev server

----- omero.figure -----
```
wget https://files.pythonhosted.org/packages/c5/75/ef5d6859bed34582d66affdf45842a8b08ca80ec61a7c53c980122bec673/omero-figure-6.0.1.tar.gz
```

# Scripts
For official scripts, don't need to run omero script upload, just put in one of the topic folders in `/local/omero_app/OMERO.server/lib/scripts/omero/`
```
scp script.py govekk_pa@ctomero01lp.jax.org:~
ssh govekk_pa@ctomero01lp.jax.org
cd /local/omero_app/OMERO.server/lib/scripts/omero/export_scripts

```
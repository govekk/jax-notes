---
tags: WIP, NGFF
---

Josh: would be good to know how much of this could be moved to either
 - https://github.com/ome/omero-cli-zarr
 - https://github.com/IDR/SubmissionWorkflow

Comments from Petr for integrating:
 - 1. say whether or not the IMAGE_PATH can point to any old image or does it have to be an image inside the ManagedRepo of OMERO or otherwise “special” image
 - 2. say what is the output of the workflow and give a hint about how to follow-up (…and now open locally in napari) as applicable
 - (basically the need to explain the bigger framework, why are the omero steps there, and why and when can I omit/ignore them)

# NGFF workflow, summarised

A summarised version of
https://github.com/IDR/SubmissionWorkflow/blob/master/zarr.md (private)
that will be used to construct a workflow.

Input parameters:
- `IMAGE_ID`: OMERO Image ID
- `IMAGE_PATH`: Path to raw data on NFS
- `OUTPUT_DIR`: Output parent directory for Zarrs

Temporary files passed between steps:
- `SERIES_CSV`: CSV including image IDs
- `ROIS_CSV`: CSV including ROI IDs

Conda Environment:
- bioformats2raw
- omero-cli-zarr
- omero-py
- zarr

Current workflow:

## 1. Obtain OMERO all image IDs associated with `${IMAGE_ID}`, save to ${SERIES_CSV}:

    omero hql --style=plain -q "select i.id, i.series from Image i where i.fileset.id = (select i.fileset.id from Image i where i.id = ${IMAGE_ID}) order by i.series asc" | cut -f2,3 -d, | tee ${SERIES_CSV}

## 2. Convert raw image data to Zarr

    bioformats2raw --file_type=zarr ${SERIES_CSV} ${IMAGE_PATH} ${OUTPUT_DIR} 

## 3. Add OMERO metadata

    curl -o- http://idr.openmicroscopy.org/webclient/imgData/${IMAGE_ID}/ > ${OUTPUT_DIR}/${IMAGE_ID}.zarr/omero.json

(Presumably need to loop through all image IDs in `${SERIES_CSV}`)

## 4. Merge external OMERO metadata into the Zarr

`merge.py`: https://github.com/IDR/idr-zarr-tools/blob/master/merge.py

    ./merge.py {OUTPUT_DIR}/${IMAGE_ID}.zarr

(Should we delete `${OUTPUT_DIR}/${IMAGE_ID}.zarr/omero.json`?)

## 5. Get mask IDs

    omero hql --style=plain "select distinct s.textValue, s.roi.id from Shape s where s.roi.image.id = ${IMAGE_ID}" --limit=-1 | tee ${ROIS_CSV}

(Presumably need to loop through all image IDs in `${SERIES_CSV}`)

## 6. Add masks

    omero zarr masks Image:${IMAGE_ID} --mask-map=${ROIS_CSV}


----
Looks like steps 3-6 should be in a loop over the output of step 2. One question is whether to keep the loop in the same workflow, or to branch into a different workflow.



# omero-cli-zarr export using Docker

Based on https://github.com/ome/omero-cli-zarr/pull/29#issuecomment-713516616 and https://github.com/ome/omero-cli-zarr/pull/38#pullrequestreview-518083982

```
$ ssh -A ome-zarr-dev1.openmicroscopy.org
```

Or use

```
ssh idr-ngff-1
```
if you have:
```
Host idr-ngff-1
    ProxyCommand ssh idr-pilot.openmicroscopy.org -W zarr1-dev:%p
```


Can run from your home directory...
```
$ mkdir idr0033_zarr_export_docker
$ cd idr0033_zarr_export_docker
$ vi Dockerfile
```
NB: Paste in this docker file, editing the omero-cli-zarr branch you want to use:

```
FROM centos:7

RUN yum install -y git python3 fontconfig

RUN useradd -ms /bin/bash converter
USER converter

RUN python3 -m venv /tmp/venv && /tmp/venv/bin/pip install -U pip wheel && /tmp/venv/bin/pip install https://github.com/ome/zeroc-ice-py-centos7/releases/download/0.2.1/zeroc_ice-3.6.5-cp36-cp36m-linux_x86_64.whl
RUN /tmp/venv/bin/pip install git+https://github.com/ome/omero-cli-zarr.git@refs/pull/38/merge
# or RUN /tmp/venv/bin/pip install git+git://github.com/joshmoore/omero-cli-zarr.git@nested#egg=omero-cli-zarr

COPY export.omero .
ENV OMERO_USERDIR /tmp/omero
ENTRYPOINT ["/tmp/venv/bin/omero", "load", "export.omero"]
```
Create a companion export.omero file

```
vi export.omero
```
And add export commands to connect to idr-testing...
```
login -C public@idr-testing.openmicroscopy.org -w public
zarr --output /data export Plate:5966
```

Then build with

```
$ sudo docker build --tag idr0033_plate5966 .
```


We want files to end up at e.g. (named after current or last-merged PR)
https://minio-dev.openmicroscopy.org/idr/idr0033-rohban-pathways/41744_illum_corrected/pr35/5966.zarr
Location is on /uod/idr/objectstore/minio/idr/ but this is now
accessible from ome-zarr-dev1.openmicroscopy.org.

Make sure target location exists...

```
$ cd /uod/idr/objectstore/minio/idr/idr0033-rohban-pathways/41744_illum_corrected
$ mkdir pr_35
```


Want to run with your own user ID, so you can modify output files
```
$ id wmoore  # shows user ID - e.g. 5098

$ screen -xRR
```
NB: had some permissions problems (fixed) but the session created when the Dockerfile was built above
then expired, so needed to run the container interactively and re-login to idr:

```
[wmoore@idr1-slot2 idr0002]$ sudo docker run -it --rm -u 5098 -e OMERO_USERDIR=/tmp/omero -v /uod/idr/objectstore/minio/idr/idr0033-rohban-pathways/41744_illum_corrected/pr_35:/data idr0033_plate5966
WARNING: Could not load OpenGL library.
WARNING:vispy:Could not load OpenGL library.
Server: [localhost:4064]idr.openmicroscopy.org
Username: [root]public
Password:
Created session for public@idr.openmicroscopy.org:4064. Idle timeout: 10 min. Current group: Public
Exporting to /data/5966.zarr
0.06% done, ETA: 05:20:46
`````
Should now be able to see files appearing on idr0-slot3
e.g. to see how many Wells have been exported:

```
$ cd /uod/idr/objectstore/minio/idr/idr0033-rohban-pathways/41744_illum_corrected/pr_35/5966.zarr/0
$ find . -maxdepth 2 -type d | grep [0-9] | wc
     64      64     430
```

# export with Z-downsampling

Export and then downsample, working with Temp state of
PR: https://github.com/ome/ome-zarr-py/pull/71#issuecomment-759404371

```
ssh -A ome-zarr-dev1.openmicroscopy.org
mkdir idr0077_zarr_z-downsample
cd idr0077_zarr_z-downsample
```


Dockerfile:

```
FROM centos:7

RUN yum install -y git python3 fontconfig

RUN useradd -ms /bin/bash converter
USER converter

RUN python3 -m venv /tmp/venv && /tmp/venv/bin/pip install -U pip wheel && /tmp/venv/bin/pip install https://github.com/ome/zeroc-ice-py-centos7/releases/download/0.2.1/zeroc_ice-3.6.5-cp36-cp36m-linux_x86_64.whl
RUN /tmp/venv/bin/pip install git+https://github.com/will-moore/omero-cli-zarr.git@use_ome_zarr_to_build_pyramid
RUN /tmp/venv/bin/pip install git+https://github.com/will-moore/ome-zarr-py.git@downsample_Z

COPY export_zarr .
ENV OMERO_USERDIR /tmp/omero
ENTRYPOINT ["bash", "export_zarr"]

```

Create an `export_zarr` bash script:

```
/tmp/venv/bin/omero login -C public@idr-testing.openmicroscopy.org -w public
/tmp/venv/bin/omero zarr --output /data export Image:9836831
/tmp/venv/bin/ome_zarr scale /data/9836831.zarr /data/9836831_zscale.zarr --downsample_z
```

Build and run

```
$ sudo docker build --tag idr0077_9836831_zscale_02 .

$ sudo mkdir -p /uod/idr/objectstore/minio/idr/idr0077-valuchova-flowerlightsheet/zscale_02
$ sudo chown wmoore /uod/idr/objectstore/minio/idr/idr0077-valuchova-flowerlightsheet/zscale_02

$ id wmoore     # 5098

$ screen -xRR

$ sudo docker run -it --rm -u 5098 -e OMERO_USERDIR=/tmp/omero -v /uod/idr/objectstore/minio/idr/idr0077-valuchova-flowerlightsheet/zscale_02:/data idr0077_9836831_zscale_02
```


# export via conda environment

Use:
```
ssh -A ome-zarr-dev1.openmicroscopy.org
```
Or with this config:
```
# /.ssh/config

Host pilot-zarr*
    ProxyCommand ssh idr-pilot.openmicroscopy.org -W %h.openmicroscopy.org:%p   ForwardAgent yes
```
Use:
```
ssh pilot-zarr1-dev
```

First install Conda. Using first link in list at https://docs.conda.io/en/latest/miniconda.html#linux-installers

```
$ wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.10.3-Linux-x86_64.sh

$ bash Miniconda3-py39_4.10.3-Linux-x86_64.sh
...
Miniconda3 will now be installed into this location:
/lifesci/groups/jrs/wmoore/miniconda3
...
```

After re-opening a Terminal:

```
$ which conda
~/miniconda3/bin/conda
```

Using `environment.yml` at https://github.com/ome/NGFF-ELMI-2021-Workshop/blob/main/binder/environment.yml ...

```
wget https://raw.githubusercontent.com/ome/NGFF-ELMI-2021-Workshop/main/binder/environment.yml

$ conda create -f environment.yml

CondaValueError: The target prefix is the base prefix. Aborting.

$ conda env create -f environment.yml

CondaValueError: prefix already exists: /lifesci/groups/jrs/wmoore/miniconda3

$ conda env create --prefix ./envs -f environment.yml

NB: could use -n myenv instead of --prefix?

...
ERROR conda.core.link:_execute(699): An error occurred while installing package 'conda-forge::pyopengl-3.1.5-py_0'.
Rolling back transaction: done

LinkError: post-link script failed for package conda-forge::pyopengl-3.1.5-py_0
location of failed script: /lifesci/groups/jrs/wmoore/envs/bin/.pyopengl-post-link.sh
==> script messages <==
<None>
==> script output <==
stdout: Warning: Missing OpenGL driver, install with yum install mesa-libGL-devel or equivalent
...

$ sudo yum install mesa-libGL-devel

# try again...

$ conda env create -f environment.yml -n omero_zarr_export

```

Seemed to work:

```
$ conda env list
# conda environments:
#
                         /lifesci/groups/jrs/wmoore/envs
base                  *  /lifesci/groups/jrs/wmoore/miniconda3
omero_zarr_export        /lifesci/groups/jrs/wmoore/miniconda3/envs/omero_zarr_export
```

Activate and install version of omero-cli-zarr we want to use

```
$ conda activate omero_zarr_export
$ pip uninstall omero-cli-zarr
$ pip install git+https://github.com/will-moore/omero-cli-zarr.git@export_v0.3_ome_ngff

$ cd /uod/idr/objectstore/minio/idr/v0.3/idr0062-blin-nuclearsegmentation
$ omero zarr export Image:6001240
```

Available at:
 - 2D (YX): https://minio-dev.openmicroscopy.org/idr/v0.3/idr0094-ellinger-sarscov2/10503791.zarr
 - 3D (CXY): https://minio-dev.openmicroscopy.org/idr/v0.3/idr0077-valuchova-flowerlightsheet/9836842.zarr
 - 4D (TCYX) https://minio-dev.openmicroscopy.org/idr/v0.3/idr0002-heriche-condensation/179758.zarr
 - 4D (CZYX): https://minio-dev.openmicroscopy.org/idr/v0.3/idr0062-blin-nuclearsegmentation/6001240.zarr
 - 4D (TZYX): idr0099-jain-beetlelightsheet/12557113.zarr (not done)
 - 4D (TCYX): https://minio-dev.openmicroscopy.org/idr/v0.3/idr0077-valuchova-flowerlightsheet/9836849.zarr
 - 5D (TCZYX): https://minio-dev.openmicroscopy.org/idr/v0.3/idr0077-valuchova-flowerlightsheet/9836847.zarr (not done)

Ended with:
```
OSError: [Errno 28] No space left on device: '/uod/idr/objectstore/minio/idr/v0.3/idr0099-jain-beetlelightsheet/12557113.zarr/0/0/54/0/0'
```

Seb: looks like we have used all the Inodes on the objectstore GPFS fileset at UoD
so options are 1- clean up data on GPFS, 2- increase the number of inodes, 3- use EBI S3 rather than our minio

Others:
 - https://minio-dev.openmicroscopy.org/idr/v0.4/2022-01-24/plates/idr0004
 - https://minio-dev.openmicroscopy.org/idr/v0.4/2022-02-03/idr0062/6001240.zarr
 - https://minio-dev.openmicroscopy.org/idr/v0.4/2022-02-03/idr0101/13457227.zarr (13457537.zarr  13457539.zarr)
 - From bioformats2raw, includes OME/OME.xml for MIF https://minio-dev.openmicroscopy.org/idr/bf2raw/mdb/martin/sample_files.zarr/.zattrs is `{"bioformats2raw.layout" : 3}`

# export vizarr & deploy on s3
Checkout https://github.com/hms-dbmi/vizarr/pull/43

$ cd vizarr
$ npm run export   # to /out directory


# export via bioformats2raw

Using pilot-zarr1-dev (ngff data directory: `/data`) or pilot-zarr2-dev (`/data/ngff`).

Install bioformats2raw via conda:
```
conda install -c ome bioformats2raw
```

This is actually just for getting the dependencies installed.
Get the actual bioformats2raw from this PR and just unzip it into your home directory:
https://github.com/IDR/bioformats2raw/pull/1

Create a directory for the idr project and memo files (if it's not already there),
and change into the idr directory.
For example for idr0051:
```
mkdir idr0051 
mkdir memo
cd idr0051
```

Find out where the pattern, screen or companion files are. For example: 
`/nfs/bioimage/drop/idr0051-fulton-tailbudlightsheet/patterns/`

Then run the conversion (using the bioformat2raw from the PR!):
```
for i in `ls /nfs/bioimage/drop/idr0051-fulton-tailbudlightsheet/patterns/`; do echo $i; /home/<USER>/bioformats2raw-0.7.0-SNAPSHOT/bin/bioformats2raw --memo-directory ../memo /nfs/bioimage/drop/idr0051-fulton-tailbudlightsheet/patterns/$i ${i%.*}.ome.zarr; done
```

(`$i` is the pattern file, `${i%.*}.ome.zarr` strips the .pattern file extension and adds .ome.zarr; this should work for pattern, screen and also companion file extensions)


# upload to uk1 s3

Install minio and configure...

```
ssh -A ome-zarr-dev1.openmicroscopy.org

wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc

(base) [wmoore@ome-zarr-dev1 ~]$ ./mc config host add uk1s3 https://uk1s3.embassy.ebi.ac.uk
Enter Access Key: X8GE11ZKP71A8529XFAE
Enter Secret Key: 
Added `uk1s3` successfully.

$ ./mc ls uk1s3/idr/zarr
[2022-10-13 11:07:56 BST]     0B test-data/
[2022-10-13 11:07:56 BST]     0B v0.1/
[2022-10-13 11:07:56 BST]     0B v0.2/
[2022-10-13 11:07:56 BST]     0B v0.3/
[2022-10-13 11:07:56 BST]     0B v0.4/


$ cd /uod/idr/objectstore/minio/idr/v0.4/2022-02-03/
$ /lifesci/groups/jrs/wmoore/mc cp -r idr0101/ uk1s3/idr/zarr/v0.4/idr0101A/
...idr0101/13457539.zarr/labels/0/2/0/0/0/0/0: 13.21 GiB / 13.21 GiB ━━━━━━━
```

Also installed at on idrtesting-pilot to allow direct upload without conversion...

```
ssh -A idr-pilot.openmicroscopy.org
ssh -A idrtesting-omeroreadwrite

# install and config as above...

$ cd /uod/idr/filesets/idr0138-lohoff-seqfish/20210917-Globus/ngff/TimEmbryos-102219/HybCycle_0/
$ /home/wmoore/mc cp -r MMStack_Pos0.ome.zarr/ uk1s3/idr/zarr/v0.4/idr0138A/TimEmbryos-102219/HybCycle_0/MMStack_Pos0.ome.zarr/
.../MMStack_Pos0.ome.zarr/labels/nuclei/4/0/0/0: 186.41 MiB / 186.41 MiB ━━━━━━━
```

# Copy files from IDR


```
rsync -av  -e "ssh -A idr-pilot.openmicroscopy.org ssh" "idrtesting-omeroreadwrite:/uod/idr/filesets/idr0070-kerwin-hdbr/20200414-Batch3-ftp/HDBR_BMP4_ISH/" . -P
```

# Test import to OMERO

```
$ ssh idr2-slot3.openmicroscopy.org

$ cd /uod/idr-scratch/will-test/

$ aws s3 sync --no-sign-request --endpoint-url https://uk1s3.embassy.ebi.ac.uk s3://idr/zarr/v0.4/idr0050A/4995115.zarr 4995115.zarr

$ conda activate omeropy

$ echo $OMERODIR
/lifesci/groups/jrs/wmoore/OMERO.server-5.6.5-ice36-b233

$ omero import --depth=100 4995115.zarr

```


# Update Jan 2023

See https://forum.image.sc/t/converting-other-idr-images-into-public-zarr/44025/12 
Want to convert /uod/idr/filesets/idr0048-abdeladim-chroms/20181217-ftp/Astrop65_BDV/ (astroP65.h5 (73GB) and astroP65.xml)

Seb: "`pilot-zarr1-dev` or `pilot-zarr2-dev`
that has the advantage of being on EBI Embassy (so easier to upload to S3)"

```
ssh -A -o 'ProxyCommand ssh idr-pilot.openmicroscopy.org -W %h:%p' pilot-zarr1-dev
```

Installed conda and bioformats2raw as above in `/home/wmoore/miniconda`, and installed & configured `mc` client as above.

```
rsync -av  -e "ssh -A idr-pilot.openmicroscopy.org ssh" "idrtesting-omeroreadwrite:/uod/idr/filesets/idr0048-abdeladim-chroms/20181217-ftp/Astrop65_BDV/" -P .
```

```
$ bioformats2raw --version
Version = 0.5.0
Bio-Formats version = 6.10.1
NGFF specification version = 0.4

$ ls -lh
total 73G
-rw-rw----. 1 wmoore idrnfs  73G Dec 17  2018 astroP65.h5
-rw-rw----. 1 wmoore idrnfs 2.3K Dec 17  2018 astroP65.xml


$ bioformats2raw astroP65.xml 9846151.zarr
OpenJDK 64-Bit Server VM warning: You have loaded library /tmp/opencv_openpnp3473542386768814439/nu/pattern/opencv/linux/x86_64/libopencv_java342.so which might have disabled stack guard. The VM will try to fix the stack guard now.
It's highly recommended that you fix the library with 'execstack -c <libfile>', or link it with '-z noexecstack'.


$ /home/wmoore/mc cp -r 9846151.zarr/ uk1s3/idr/zarr/v0.4/idr0048A/9846151.zarr/
```

**conda install**

```
conda create -n omero_zarr_export -c ome python=3.9 zeroc-ice36-python omero-py

conda activate omero_zarr_export

# custom versions of ome-zarr-py and omero-cli-zarr
pip install git+https://github.com/ome/ome-zarr-py.git@refs/pull/244/head

pip install git+https://github.com/ome/omero-cli-zarr.git@refs/pull/134/head

```


# March 2023

Rsync local data to minio:

```
rsync -rvP --progress 13425213.zarr ome-zarr-dev1.openmicroscopy.org:/uod/idr/objectstore/minio/idr/idr0113-bottes-opcclones/
```


# IDR NGFF conversion - make buckets - April 2023

```
$ aws --endpoint-url https://uk1s3.embassy.ebi.ac.uk s3 mb s3://idr0010
```

Seb: set the policy on the bucket as described in https://github.com/IDR/deployment/blob/master/docs/object-store.md#policy ? this will be mandatory for accessing it from the pilots (without keys)

Will: also need to configure CORS as described there too.
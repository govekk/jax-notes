# Running this from one of the linux servers (likely prod) so don't have to worry about own computer restarting or anything
```
ssh govekk_pa@ctomero01lp.jax.org
cd /local/omero_scripts/omerosubvenv/bin
./omero login -u govekk_pa -g Korstanjelab -s localhost:4064 --timeout 7200
screen -S chown
```

# Notes about omero chown
```
time ./omero chown 108 Project:455 --include Annotation --dry-run --report
```

```
--include Annotation
```
Transfers any annotations owned by the original user to the new user, but not any owned by another user

# Projects list

Finished?   ID        Project               Owner               Number of images

x            1481/60        1718                  Kelsey Letson       126 - Project owned by Kelsey, single dataset Bear_PAS (702) dataset owned by Kelsey, images owned by Sue
x4s-6s       917       2016-11 LDHB          Susan Sheehan       48
x26s-43s     703       A1CF                  Susan Sheehan       309
x9s-13s      1270      AgedB6                Susan Sheehan       93
x35s-51s     1002      Arsenic               Susan Sheehan       318
x8s-10s      918       Col4a5xFmn1           Susan Sheehan       66
x15s-24s     1380      DGLA                  Susan Sheehan       180
x47s-71s     1413      Diversity 2.0         Susan Sheehan       570
x15hr-21hr   455       DO-Aging              Susan Sheehan       3128 - many ROIs - taking hours, no delete so good to go
x2s-2s       1254      DO-Aging-Seamus       Seamus Mawe         2
x27s-44s     1469      eGPS                  Susan Sheehan       193
x20s-33s     1003      Far2                  Susan Sheehan       258
x52m-78m     867       GlomerularCount       Susan Sheehan       27405
x12s-20s     1397      GRN                   Susan Sheehan       141
x            605       Hibernation           Susan Sheehan       348 - also has Bear_PAS dataset owned by Kelsey
x22s-32s     451       Kidney DNN            Seamus Mawe         60
x4s-8s       1305      LiverB6               Susan Sheehan       45
xxxxx        1383      mouse torpor          Ron Korstanje       120
x10m-16m     405       MRI collodial iron    Susan Sheehan       5897
x14m-20m     1268      NBL1                  Susan Sheehan       7400
x11s-15s     1422      Podosighter           Susan Sheehan       84
x3s-4s       1411      Sennet                Susan Sheehan       27
xxxxx        1354      ShockTumor            Ron Korstanje       451
x2s-2s       1470      thiesa_ShockTumor     Adam Thiesen        12
x2m-3.5m     1271      UCHC_Ming_p21         Susan Sheehan       72 - ROIs

Bear_PAS = Fileset:17826-17867

## Dataset owned by Kelsey
Deleting project fails with graph fail
```
[govekk_pa@ctomero01lp bin]$ time ./omero chown 108 Project:1718 --include Annotation --dry-run --report
Using session for govekk_pa@localhost:4064. Idle timeout: 120 min. Current group: Korstanjelab
omero.cmd.Chown2 Project:1718 failed: 'graph-fail'
failed: cannot read ome.model.containers.Project[1718]
Steps: 5
Elapsed time: 0.022 secs.
Flags: [FAILURE, CANCELLED]

real	0m0.972s
user	0m0.754s
sys	0m0.096s
```


Deleting dataset just deletes dataset image and project dataset links
```
[govekk_pa@ctomero01lp bin]$ time ./omero chown 108 Dataset:702 --include Annotation --dry-run --report
Using session for govekk_pa@localhost:4064. Idle timeout: 120 min. Current group: Korstanjelab
omero.cmd.Chown2 Dataset:702 Dry run performed
ok
Steps: 5
Elapsed time: 0.88 secs.
Flags: []
Included objects
  Dataset:702
Deleted objects
  DatasetImageLink:76000-76125
  ProjectDatasetLink:652,1938

real	0m1.871s
user	0m0.757s
sys	0m0.115s
```

Problem because dataset owned by Kelsey but images owned by Sue?

Same dataset: 702 in two projects: 1718 and Hibernation
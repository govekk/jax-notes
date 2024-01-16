# Project not deleted after chowns
```
deleting contents from user user-30
deleting project 669
omero.cmd.Delete2 Project:669 ok
deleting project 635
omero.cmd.Delete2 Project:635 Image:50041
```
^ no ok after second project delete
```
Created session for user-30@localhost:4064. Idle timeout: 166666667 min. Current group: training
!! 10/06/23 17:07:43.525 error: communicator not destroyed during global destruction.(omero-transfer)
```


```
omero delete Project:635 --include Dataset,Image,Annotation --dry-run --report

omero.cmd.Delete2 Project:635 failed: 'graph-fail'
failed: not permitted to update ome.model.roi.Roi[43420]
Steps: 4
Elapsed time: 5.395 secs.
Flags: [FAILURE, CANCELLED]
```

That Roi is owned by user-28

Re-created issue (without any chown shenanigans) by having another user create an ROI in an image in a brand new Project.
Deletes fine if there are k-v pairs or other things owned by another user
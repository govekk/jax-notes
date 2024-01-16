# Issue
Too-large PNGs > ~3000 px on either side trigger pyramid generation in OMERO, getting "this instance is read-only" errors when trying to load on web, even though omero-server should be generating pyramids?

# Gimmicks of pyramid generation from 2021
https://forum.image.sc/t/pixeldata-threads-and-pyramid-generation-issues/49794/7

# Restart PixelData service to jumpstart pyramid generation?
```
omero admin ice server disable PixelData-0
omero admin ice server stop PixelData-0
omero admin ice server enable PixelData-0
```

Error:
```
Traceback (most recent call last):
  File "./omero", line 8, in <module>
    sys.exit(main())
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero/main.py", line 126, in main
    rv = omero.cli.argv()
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero/cli.py", line 1787, in argv
    cli.invoke(args[1:])
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero/cli.py", line 1225, in invoke
    stop = self.onecmd(line, previous_args)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero/cli.py", line 1302, in onecmd
    self.execute(line, previous_args)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero/cli.py", line 1384, in execute
    args.func(args)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero/plugins/admin.py", line 1028, in ice
    return self.ctx.call(command)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero/cli.py", line 1472, in call
    rv = subprocess.call(args, env=self._env(), cwd=self._cwd(cwd))
  File "/usr/lib/python3.7/subprocess.py", line 323, in call
    with Popen(*popenargs, **kwargs) as p:
  File "/usr/lib/python3.7/subprocess.py", line 775, in __init__
    restore_signals, start_new_session)
  File "/usr/lib/python3.7/subprocess.py", line 1522, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'icegridadmin': 'icegridadmin'
```

Run
```
source /etc/profile
```
To tell bash where ice_home is (/opt/ice...). Based on: https://forum.image.sc/t/omero-installation-in-ubuntu-20-04/66947/4?u=govekk


# Notes
First few job logs of processor after start on day of import (because started right before imports)
```
2023-10-02 19:13:24,509 INFO  [              omero.processor.ProcessorI] (Dummy-4   ) parseJob: Session = 5994d8f1-4d02-40ea-9265-3d63d8254d16, JobId = 264201
2023-10-02 19:13:24,610 INFO  [              omero.processor.ProcessorI] (Dummy-4   ) Using launcher: /opt/omero/server/server_venv/bin/python
2023-10-02 19:13:24,610 INFO  [              omero.processor.ProcessorI] (Dummy-4   ) Using process: omero.processor.ProcessI
2023-10-02 19:13:24,614 INFO  [                omero.processor.ProcessI] (Dummy-4   ) Created 5994d8f1-4d02-40ea-9265-3d63d8254d16 in /home/omero-server/omero/tmp/omero_omero-server/383/processf74gx8d_.dir
2023-10-02 19:13:24,655 INFO  [              omero.processor.ProcessorI] (Dummy-4   ) Downloaded file: 52
2023-10-02 19:13:24,659 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=-,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Activated
2023-10-02 19:13:24,717 INFO  [                            omero.remote] (Dummy-4   )  Meth: ProcessI.wait
2023-10-02 19:13:24,718 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=-,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Waiting
2023-10-02 19:13:25,059 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=1,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Deactivating
2023-10-02 19:13:25,059 INFO  [                            omero.remote] (Dummy-4   )  Meth: ProcessI.shutdown
2023-10-02 19:13:25,059 INFO  [                            omero.remote] (Dummy-4   )  Rslt: None
2023-10-02 19:13:25,250 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=1,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Changed job status from Running to Error
2023-10-02 19:13:25,259 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=1,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : No stdout
2023-10-02 19:13:25,510 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=1,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Uploaded 498 bytes of /home/omero-server/omero/tmp/omero_omero-server/383/processf74gx8d_.dir/err to 313101
2023-10-02 19:13:25,513 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=1,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Lived 0s. Deactivation took 0s.
2023-10-02 19:13:25,514 INFO  [                omero.processor.ProcessI] (Dummy-4   ) <proc:566,rc=1,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Callback processFinished

2023-10-02 19:13:25,912 INFO  [              omero.processor.ProcessorI] (Dummy-6   ) parseJob: Session = dbb4d65f-ccd0-436f-881e-01653d22289b, JobId = 264202
2023-10-02 19:13:25,985 INFO  [              omero.processor.ProcessorI] (Dummy-6   ) Using launcher: /opt/omero/server/server_venv/bin/python
2023-10-02 19:13:25,985 INFO  [              omero.processor.ProcessorI] (Dummy-6   ) Using process: omero.processor.ProcessI
2023-10-02 19:13:25,990 INFO  [                omero.processor.ProcessI] (Dummy-6   ) Created dbb4d65f-ccd0-436f-881e-01653d22289b in /home/omero-server/omero/tmp/omero_omero-server/383/processzckergmf.dir
2023-10-02 19:13:26,016 INFO  [              omero.processor.ProcessorI] (Dummy-6   ) Downloaded file: 307751
2023-10-02 19:13:26,022 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=-,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : Activated
2023-10-02 19:13:26,066 INFO  [                            omero.remote] (Dummy-6   )  Meth: ProcessI.wait
2023-10-02 19:13:26,066 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=-,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : Waiting
2023-10-02 19:13:26,518 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=1,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : Deactivating
2023-10-02 19:13:26,518 INFO  [                            omero.remote] (Dummy-6   )  Meth: ProcessI.shutdown
2023-10-02 19:13:26,518 INFO  [                            omero.remote] (Dummy-6   )  Rslt: None
2023-10-02 19:13:26,629 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=1,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : Changed job status from Running to Error
2023-10-02 19:13:26,637 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=1,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : No stdout
2023-10-02 19:13:26,818 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=1,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : Uploaded 575 bytes of /home/omero-server/omero/tmp/omero_omero-server/383/processzckergmf.dir/err to 313102
2023-10-02 19:13:26,822 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=1,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : Lived 0s. Deactivation took 0s.
2023-10-02 19:13:26,822 INFO  [                omero.processor.ProcessI] (Dummy-6   ) <proc:585,rc=1,uuid=dbb4d65f-ccd0-436f-881e-01653d22289b> : Callback processFinished
```

Then switches to:
```
2023-10-02 19:13:59,727 INFO  [                            omero.remote] (Thread-2  )  Meth: ProcessI.poll
2023-10-02 19:13:59,727 INFO  [                            omero.remote] (Thread-2  )  Rslt: object #0 (::omero::RInt)
2023-10-02 19:13:59,729 INFO  [                omero.processor.ProcessI] (Thread-2  ) <proc:566,rc=1,uuid=5994d8f1-4d02-40ea-9265-3d63d8254d16> : Keep alive failed
```


# Pixeldata

```
2023-10-02 20:23:52,066 INFO  [      ome.services.OmeroFilePathResolver] (2-thread-2) Metadata only file, resulting path: /hyperfile/OMERO/ManagedRepository/Importer_507/2023-10/02/20-23-47.818/07_9574_HE.ndpi [0]_stiched.png
2023-10-02 20:23:52,085 INFO  [      ome.services.OmeroFilePathResolver] (2-thread-5) Metadata only file, resulting path: /hyperfile/OMERO/ManagedRepository/Importer_507/2023-10/02/20-23-47.818/07_9574_HE.ndpi [0]_stiched.png
2023-10-02 20:23:52,116 WARN  [ ome.services.pixeldata.PixelDataHandler] (2-thread-2) Pixels:191449 -- Already locked! /hyperfile/OMERO/Pixels/Dir-191/.191449_pyramid.pyr_lock
2023-10-02 20:23:52,778 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Creating BfPixelBuffer: /hyperfile/OMERO/ManagedRepository/Importer_507/2023-10/02/20-23-47.818/07_9574_HE.ndpi [0]_stiched.png Series: 0
2023-10-02 20:23:52,790 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Destination pyramid tile size: java.awt.Dimension[width=256,height=256]
2023-10-02 20:23:52,791 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 1/504 (0%).
2023-10-02 20:24:21,295 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 51/504 (9%).
2023-10-02 20:24:51,976 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 101/504 (19%).
2023-10-02 20:25:29,540 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 151/504 (29%).
2023-10-02 20:25:54,965 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 201/504 (39%).
2023-10-02 20:26:19,393 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 251/504 (49%).
2023-10-02 20:26:41,712 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 301/504 (59%).
2023-10-02 20:27:05,212 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 351/504 (69%).
2023-10-02 20:27:27,052 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 401/504 (79%).
2023-10-02 20:27:47,618 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 451/504 (89%).
2023-10-02 20:28:08,062 INFO  [                ome.io.nio.PixelsService] (2-thread-5) Pyramid creation for Pixels:191449 501/504 (99%).
2023-10-02 20:28:10,396 INFO  [                ome.io.nio.PixelsService] (2-thread-5) SUCCESS -- Pyramid created for pixels id:191449
2023-10-02 20:28:10,404 INFO  [       loci.formats.in.MinimalTiffReader] (2-thread-5) Reading IFDs
2023-10-02 20:28:10,406 INFO  [       loci.formats.in.MinimalTiffReader] (2-thread-5) Populating metadata
2023-10-02 20:28:10,409 INFO  [  loci.formats.in.JPEG2000MetadataParser] (2-thread-5) Unknown JPEG 2000 box 0x290000 at 3145
2023-10-02 20:28:10,410 INFO  [  loci.formats.in.JPEG2000MetadataParser] (2-thread-5) File is a raw codestream not a JP2.
2023-10-02 20:28:10,414 INFO  [  loci.formats.in.JPEG2000MetadataParser] (2-thread-5) Unknown JPEG 2000 box 0x290000 at 2529311
2023-10-02 20:28:10,414 INFO  [  loci.formats.in.JPEG2000MetadataParser] (2-thread-5) File is a raw codestream not a JP2.
2023-10-02 20:28:10,414 INFO  [  loci.formats.in.JPEG2000MetadataParser] (2-thread-5) Unknown JPEG 2000 box 0x290000 at 5027992
2023-10-02 20:28:10,414 INFO  [  loci.formats.in.JPEG2000MetadataParser] (2-thread-5) File is a raw codestream not a JP2.
2023-10-02 20:28:10,421 INFO  [              loci.formats.in.TiffReader] (2-thread-5) Checking comment style
2023-10-02 20:28:10,440 INFO  [          loci.formats.in.BaseTiffReader] (2-thread-5) Populating OME metadata
2023-10-02 20:28:18,318 INFO  [ ome.services.pixeldata.PixelDataHandler] (2-thread-5) Added StatsInfo:370896 for ome.model.core.Channel:Id_491695 - C:0 Max:238.0 Min:22.0
2023-10-02 20:28:18,322 INFO  [ ome.services.pixeldata.PixelDataHandler] (2-thread-5) Added StatsInfo:370897 for ome.model.core.Channel:Id_491696 - C:1 Max:255.0 Min:37.0
2023-10-02 20:28:18,326 INFO  [ ome.services.pixeldata.PixelDataHandler] (2-thread-5) Added StatsInfo:370898 for ome.model.core.Channel:Id_491697 - C:2 Max:253.0 Min:23.0
2023-10-02 20:28:18,326 INFO  [ ome.services.pixeldata.PixelDataHandler] (2-thread-5) HANDLED EventLog:55077815(entityId=191449) [266258 ms.]
2023-10-02 20:28:18,330 ERROR [        ome.services.util.ServiceHandler] (2-thread-5) Method interface ome.services.util.Executor$Work.doWork invocation took 266288
```

Repeat until:
```
2023-10-02 22:13:05,054 INFO  [                      ome.system.metrics] (r-thread-1) type=TIMER, name=ome.services.pixeldata.PixelDataThread.batch, count=21, min=15.090288999999999, max=742416.103245, mean=344806.4282527619, stddev=238484.30815520012, median=270525.06843499996, p75=552517.8465239999, p95=742211.0627034, p98=742416.103245, p99=742416.103245, p999=742416.103245, mean_rate=0.0019448489111198995, m1=3.98066700804008E-7, m5=0.0013621899390039989, m15=0.0036998255078038223, rate_unit=events/second, duration_unit=milliseconds
```


Error image:
Pixels:191491
Image:191491

Pixels:191449
Image:191449

Fine image:
Pixels:191455
Image:191455

```
2023-10-02 20:28:10,396 INFO  [                ome.io.nio.PixelsService] (2-thread-5) SUCCESS -- Pyramid created for pixels id:191449
2023-10-02 20:59:41,428 INFO  [                ome.io.nio.PixelsService] (2-thread-5) SUCCESS -- Pyramid created for pixels id:191489
2023-10-02 21:10:35,615 INFO  [                ome.io.nio.PixelsService] (2-thread-2) SUCCESS -- Pyramid created for pixels id:191490
2023-10-02 21:12:01,606 INFO  [                ome.io.nio.PixelsService] (2-thread-1) SUCCESS -- Pyramid created for pixels id:191491
2023-10-02 21:19:11,390 INFO  [                ome.io.nio.PixelsService] (2-thread-2) SUCCESS -- Pyramid created for pixels id:191492
2023-10-02 21:21:13,238 INFO  [                ome.io.nio.PixelsService] (2-thread-5) SUCCESS -- Pyramid created for pixels id:191493
2023-10-02 21:29:45,909 INFO  [                ome.io.nio.PixelsService] (2-thread-1) SUCCESS -- Pyramid created for pixels id:191495
2023-10-02 21:30:27,111 INFO  [                ome.io.nio.PixelsService] (2-thread-4) SUCCESS -- Pyramid created for pixels id:191494
2023-10-02 21:37:37,870 INFO  [                ome.io.nio.PixelsService] (2-thread-5) SUCCESS -- Pyramid created for pixels id:191497
2023-10-02 21:42:50,012 INFO  [                ome.io.nio.PixelsService] (2-thread-3) SUCCESS -- Pyramid created for pixels id:191496
2023-10-02 21:48:18,365 INFO  [                ome.io.nio.PixelsService] (2-thread-5) SUCCESS -- Pyramid created for pixels id:191511
2023-10-02 21:51:48,639 INFO  [                ome.io.nio.PixelsService] (2-thread-3) SUCCESS -- Pyramid created for pixels id:191512
2023-10-02 21:52:31,905 INFO  [                ome.io.nio.PixelsService] (2-thread-1) SUCCESS -- Pyramid created for pixels id:191516
2023-10-02 21:56:45,463 INFO  [                ome.io.nio.PixelsService] (2-thread-4) SUCCESS -- Pyramid created for pixels id:191534
2023-10-02 21:57:00,934 INFO  [                ome.io.nio.PixelsService] (2-thread-3) SUCCESS -- Pyramid created for pixels id:191523
2023-10-02 22:00:34,592 INFO  [                ome.io.nio.PixelsService] (2-thread-1) SUCCESS -- Pyramid created for pixels id:191535
2023-10-02 22:01:25,221 INFO  [                ome.io.nio.PixelsService] (2-thread-4) SUCCESS -- Pyramid created for pixels id:191537
```

Pyramids exist:
```
omero-server@omero-server:/hyperfile/OMERO/Pixels/Dir-191$ ls -al
-rw-r--r--  1 omero-server omero-server  7618042 Oct  2 20:28 191449_pyramid
-rw-r--r--  1 omero-server omero-server 16193157 Oct  2 20:59 191489_pyramid
-rw-r--r--  1 omero-server omero-server 17333438 Oct  2 21:10 191490_pyramid
-rw-r--r--  1 omero-server omero-server 18004610 Oct  2 21:12 191491_pyramid
-rw-r--r--  1 omero-server omero-server 15077870 Oct  2 21:19 191492_pyramid
-rw-r--r--  1 omero-server omero-server 18166746 Oct  2 21:21 191493_pyramid
-rw-r--r--  1 omero-server omero-server 17182030 Oct  2 21:30 191494_pyramid
-rw-r--r--  1 omero-server omero-server 15770652 Oct  2 21:29 191495_pyramid
-rw-r--r--  1 omero-server omero-server 18586461 Oct  2 21:42 191496_pyramid
-rw-r--r--  1 omero-server omero-server 16979472 Oct  2 21:37 191497_pyramid
-rw-r--r--  1 omero-server omero-server  5705137 Oct  2 21:48 191511_pyramid
-rw-r--r--  1 omero-server omero-server  3493853 Oct  2 21:51 191512_pyramid
-rw-r--r--  1 omero-server omero-server  4532704 Oct  2 21:52 191516_pyramid
-rw-r--r--  1 omero-server omero-server  4517118 Oct  2 21:57 191523_pyramid
-rw-r--r--  1 omero-server omero-server  5148916 Oct  2 21:56 191534_pyramid
-rw-r--r--  1 omero-server omero-server  5415477 Oct  2 22:00 191535_pyramid
-rw-r--r--  1 omero-server omero-server  4891079 Oct  2 22:01 191537_pyramid
```

Also exist on readonly omero:
```
omero-server@omero-readonly-server-f97f694d8-sqmss:/hyperfile/OMERO/Pixels/Dir-191$ ls -al
-rw-r--r--  1 omero-server omero-server  7618042 Oct  2 20:28 191449_pyramid
-rw-r--r--  1 omero-server omero-server 16193157 Oct  2 20:59 191489_pyramid
-rw-r--r--  1 omero-server omero-server 17333438 Oct  2 21:10 191490_pyramid
-rw-r--r--  1 omero-server omero-server 18004610 Oct  2 21:12 191491_pyramid
-rw-r--r--  1 omero-server omero-server 15077870 Oct  2 21:19 191492_pyramid
-rw-r--r--  1 omero-server omero-server 18166746 Oct  2 21:21 191493_pyramid
-rw-r--r--  1 omero-server omero-server 17182030 Oct  2 21:30 191494_pyramid
-rw-r--r--  1 omero-server omero-server 15770652 Oct  2 21:29 191495_pyramid
-rw-r--r--  1 omero-server omero-server 18586461 Oct  2 21:42 191496_pyramid
-rw-r--r--  1 omero-server omero-server 16979472 Oct  2 21:37 191497_pyramid
-rw-r--r--  1 omero-server omero-server  5705137 Oct  2 21:48 191511_pyramid
-rw-r--r--  1 omero-server omero-server  3493853 Oct  2 21:51 191512_pyramid
-rw-r--r--  1 omero-server omero-server  4532704 Oct  2 21:52 191516_pyramid
-rw-r--r--  1 omero-server omero-server  4517118 Oct  2 21:57 191523_pyramid
-rw-r--r--  1 omero-server omero-server  5148916 Oct  2 21:56 191534_pyramid
-rw-r--r--  1 omero-server omero-server  5415477 Oct  2 22:00 191535_pyramid
-rw-r--r--  1 omero-server omero-server  4891079 Oct  2 22:01 191537_pyramid
```

# Rendering issue??
```
pip install -U omero-cli-render
```
can't pip install because no internet connection
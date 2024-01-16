
# Original errors
```
2023-07-11 05:00:23,023 709        [      main] INFO      ome.formats.importer.ImportCandidates - 1 file(s) parsed into 0 group(s) with 1 call(s) to setId in 146ms. (152ms total) [0 unknowns]
Using OMERO.java-5.6.3-ice36-b228
Target can not be imported by OMERO: /dropbox/dropbox/hek_20230710/Image_H220.vsi. File might be corrupted or invalid. Skipping.
ERROR:intake:Target can not be imported by OMERO: /dropbox/dropbox/hek_20230710/Image_H220.vsi. File might be corrupted or invalid. Skipping.
2023-07-11 05:00:23,641 246        [      main] INFO          ome.formats.importer.ImportConfig - OMERO.blitz Version: 5.5.8
2023-07-11 05:00:23,655 260        [      main] INFO          ome.formats.importer.ImportConfig - Bioformats version: 6.5.1 revision: 6f50e4d52c9d96112635fd8b2dde737f31041cf0 date: 7 July 2020
2023-07-11 05:00:23,693 298        [      main] INFO   formats.importer.cli.CommandLineImporter - Log levels -- Bio-Formats: ERROR OMERO.importer: INFO
2023-07-11 05:00:23,973 578        [      main] INFO      ome.formats.importer.ImportCandidates - Depth: 4 Metadata Level: MINIMUM
2023-07-11 05:00:24,149 754        [      main] ERROR     ome.formats.importer.cli.ErrorHandler - FILE_EXCEPTION: /dropbox/dropbox/hek_20230710/Image_WT1.vsi
java.lang.IndexOutOfBoundsException: Index 3 out of bounds for length 3
	at java.base/jdk.internal.util.Preconditions.outOfBounds(Preconditions.java:64) ~[na:na]
	at java.base/jdk.internal.util.Preconditions.outOfBoundsCheckIndex(Preconditions.java:70) ~[na:na]
	at java.base/jdk.internal.util.Preconditions.checkIndex(Preconditions.java:248) ~[na:na]
	at java.base/java.util.Objects.checkIndex(Objects.java:372) ~[na:na]
	at java.base/java.util.ArrayList.get(ArrayList.java:459) ~[na:na]
	at loci.formats.in.CellSensReader.parseETSFile(CellSensReader.java:1198) ~[formats-gpl.jar:6.5.1]
	at loci.formats.in.CellSensReader.initFile(CellSensReader.java:734) ~[formats-gpl.jar:6.5.1]
	at loci.formats.FormatReader.setId(FormatReader.java:1392) ~[formats-api.jar:6.5.1]
	at loci.formats.ImageReader.setId(ImageReader.java:849) ~[formats-api.jar:6.5.1]
	at ome.formats.importer.OMEROWrapper$4.setId(OMEROWrapper.java:167) ~[omero-blitz.jar:5.5.8]
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650) ~[formats-api.jar:6.5.1]
	at loci.formats.ChannelFiller.setId(ChannelFiller.java:223) ~[formats-bsd.jar:6.5.1]
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650) ~[formats-api.jar:6.5.1]
	at loci.formats.ChannelSeparator.setId(ChannelSeparator.java:293) ~[formats-bsd.jar:6.5.1]
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650) ~[formats-api.jar:6.5.1]
	at loci.formats.Memoizer.setId(Memoizer.java:662) ~[formats-bsd.jar:6.5.1]
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650) ~[formats-api.jar:6.5.1]
	at ome.formats.importer.ImportCandidates.singleFile(ImportCandidates.java:427) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.ImportCandidates.handleFile(ImportCandidates.java:576) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.ImportCandidates.execute(ImportCandidates.java:384) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.ImportCandidates.<init>(ImportCandidates.java:222) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.ImportCandidates.<init>(ImportCandidates.java:174) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.cli.CommandLineImporter.<init>(CommandLineImporter.java:148) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.cli.CommandLineImporter.main(CommandLineImporter.java:997) ~[omero-blitz.jar:5.5.8]
2023-07-11 05:00:24,150 755        [      main] INFO      ome.formats.importer.ImportCandidates - 1 file(s) parsed into 0 group(s) with 1 call(s) to setId in 169ms. (177ms total) [0 unknowns]
Using OMERO.java-5.6.3-ice36-b228
Target can not be imported by OMERO: /dropbox/dropbox/hek_20230710/Image_WT1.vsi. File might be corrupted or invalid. Skipping.
ERROR:intake:Target can not be imported by OMERO: /dropbox/dropbox/hek_20230710/Image_WT1.vsi. File might be corrupted or invalid. Skipping.
Cannot write import.json, no valid import targets. Skipping empty import.
ERROR:intake:Cannot write import.json, no valid import targets. Skipping empty import.

Folder has 8 files left, of which 4 are typical non-images.
```

# Test file in sandbox
```
omero-server@omero-server:/opt/omero/server$ ./server_venv/bin/omero import --transfer ln_s --depth 20 /opt/omero/server/hyperfile/sample_images/20230307_Image_smaller/20230307_Image_smaller.vsi -u root -w omero -s localhost:4064   
Previous session expired for root on localhost:4064
Created session for root@localhost:4064. Idle timeout: 10 min. Current group: system
2023-07-11 15:09:27,717 528        [      main] INFO          ome.formats.importer.ImportConfig - OMERO.blitz Version: 5.6.0
2023-07-11 15:09:27,744 555        [      main] INFO          ome.formats.importer.ImportConfig - Bioformats version: 6.11.1 revision: 383bac974cd52e83908b54e4769ebb1e0d0673ee date: 1 December 2022
2023-07-11 15:09:27,844 655        [      main] INFO   formats.importer.cli.CommandLineImporter - Setting transfer to ln_s
2023-07-11 15:09:27,848 659        [      main] INFO   formats.importer.cli.CommandLineImporter - Log levels -- Bio-Formats: ERROR OMERO.importer: INFO
2023-07-11 15:09:28,318 1129       [      main] INFO      ome.formats.importer.ImportCandidates - Depth: 20 Metadata Level: MINIMUM
2023-07-11 15:09:29,064 1875       [      main] ERROR     ome.formats.importer.cli.ErrorHandler - FILE_EXCEPTION: /opt/omero/server/hyperfile/sample_images/20230307_Image_smaller/20230307_Image_smaller.vsi
java.lang.IndexOutOfBoundsException: Index 3 out of bounds for length 3
	at java.base/jdk.internal.util.Preconditions.outOfBounds(Preconditions.java:64)
	at java.base/jdk.internal.util.Preconditions.outOfBoundsCheckIndex(Preconditions.java:70)
	at java.base/jdk.internal.util.Preconditions.checkIndex(Preconditions.java:248)
	at java.base/java.util.Objects.checkIndex(Objects.java:372)
	at java.base/java.util.ArrayList.get(ArrayList.java:459)
	at loci.formats.in.CellSensReader.parseETSFile(CellSensReader.java:1210)
	at loci.formats.in.CellSensReader.initFile(CellSensReader.java:742)
	at loci.formats.FormatReader.setId(FormatReader.java:1443)
	at loci.formats.ImageReader.setId(ImageReader.java:849)
	at ome.formats.importer.OMEROWrapper$4.setId(OMEROWrapper.java:167)
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650)
	at loci.formats.ChannelFiller.setId(ChannelFiller.java:234)
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650)
	at loci.formats.ChannelSeparator.setId(ChannelSeparator.java:293)
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650)
	at loci.formats.Memoizer.setId(Memoizer.java:662)
	at loci.formats.ReaderWrapper.setId(ReaderWrapper.java:650)
	at ome.formats.importer.ImportCandidates.singleFile(ImportCandidates.java:427)
	at ome.formats.importer.ImportCandidates.handleFile(ImportCandidates.java:576)
	at ome.formats.importer.ImportCandidates.execute(ImportCandidates.java:384)
	at ome.formats.importer.ImportCandidates.<init>(ImportCandidates.java:222)
	at ome.formats.importer.ImportCandidates.<init>(ImportCandidates.java:174)
	at ome.formats.importer.cli.CommandLineImporter.<init>(CommandLineImporter.java:148)
	at ome.formats.importer.cli.CommandLineImporter.main(CommandLineImporter.java:1021)
2023-07-11 15:09:29,067 1878       [      main] INFO      ome.formats.importer.ImportCandidates - 1 file(s) parsed into 0 group(s) with 1 call(s) to setId in 635ms. (749ms total) [0 unknowns]
2023-07-11 15:09:29,141 1952       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Attempting initial SSL connection to localhost:4064
2023-07-11 15:09:30,089 2900       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Insecure connection requested, falling back
2023-07-11 15:09:30,784 3595       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Pinging session every 300s.
2023-07-11 15:09:30,806 3617       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Server: 5.6.6
2023-07-11 15:09:30,806 3617       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Client: 5.6.0
2023-07-11 15:09:30,807 3618       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Java Version: 11.0.18
2023-07-11 15:09:30,807 3618       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Name: Linux
2023-07-11 15:09:30,807 3618       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Arch: amd64
2023-07-11 15:09:30,807 3618       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Version: 5.15.107+
No imports due to errors!
```

End of debug
```
2023-07-11 15:33:01,124 2567       [      main] DEBUG                     loci.formats.Memoizer - start[1689089580043] time[1080] tag[loci.formats.Memoizer.setId]
2023-07-11 15:33:01,124 2567       [      main] DEBUG                      loci.common.Location - Location.mapFile: embedded-stream.raw -> null
2023-07-11 15:33:01,125 2568       [      main] DEBUG                      loci.common.Location - Location.mapFile: embedded-stream.raw -> null
2023-07-11 15:33:01,127 2570       [      main] ERROR     ome.formats.importer.cli.ErrorHandler - FILE_EXCEPTION: /opt/omero/server/hyperfile/sample_images/20230307_Image_smaller/20230307_Image_smaller.vsi
```

# Single vsi file from 
https://github.com/ome/bioformats/issues/4015
See same thing as dgault describes, but successfully finishes import

https://github.com/ome/bioformats/issues/3944

https://forum.image.sc/t/vsi-format-update-unable-to-open-new-files/70366 



# Seems to consistently be an issue with Bioformats and newer versions of VSI files
Fixed in Bioformats 6.12.0 but newest we have is 6.11.1:
"I don’t believe there is a release dat as of yet but it will be included in the next release of each component (likely to be OMERO 5.6.7 and Insight 5.7.3)."
(https://forum.image.sc/t/omero-and-olympus-vsi/78037/5)

Other references:
[.VSI Format update: unable to open new files](https://forum.image.sc/t/vsi-format-update-unable-to-open-new-files/70366/14)
[VSI](https://github.com/ome/bioformats/issues/3944)
[VSI: Exception loading files from VS2000 scanner](https://github.com/ome/bioformats/issues/3859)
[Olympus .vsi: only read pixels from frame_*.ets files](https://github.com/ome/bioformats/pull/3925)

# BTF
```
2023-07-13 10:06:57,328 ERROR [        ome.services.util.ServiceHandler] (erver-8641) Method interface ome.api.ThumbnailStore.getThumbnailByLongestSide invocation took 137352
2023-07-13 10:07:26,586 ERROR [        ome.services.util.ServiceHandler] (erver-8643) Method interface ome.api.ThumbnailStore.getThumbnailByLongestSide invocation took 138273
2023-07-13 10:07:40,723 ERROR [        ome.services.util.ServiceHandler] (erver-8642) Method interface ome.api.ThumbnailStore.getThumbnailByLongestSide invocation took 138014
2023-07-13 10:09:03,421 ERROR [        ome.services.util.ServiceHandler] (erver-8636) Method interface ome.api.ThumbnailStore.getThumbnailByLongestSideSet invocation took 272858
2023-07-13 10:09:11,848 ERROR [        ome.services.util.ServiceHandler] (erver-8641) Method interface ome.api.ThumbnailStore.getThumbnailByLongestSide invocation took 134183
2023-07-13 10:13:40,796 ERROR [        ome.services.util.ServiceHandler] (erver-8640) Method interface ome.api.ThumbnailStore.getThumbnailByLongestSideSet invocation took 542210
2023-07-13 10:13:40,805 ERROR [        ome.services.util.ServiceHandler] (erver-8643) Method interface ome.api.ThumbnailStore.getThumbnailByLongestSide invocation took 372441
```

# Updated OMERO but getting same "index out of bounds exception"
Import still says: Bioformats version: 6.5.1



after deleting svc-omerodata - now jars are OMERO.java-5.6.8-ice36  OMERO.java.txt
svc-omero jars are now OMERO.java-5.6.3-ice36-b228
```
Preparing to import /hyperfile/omero/autoimport/default/govekk_20230802_165701/Image_H120.vsi
 2023-08-02 16:57:04,777 239        [      main] INFO          ome.formats.importer.ImportConfig - OMERO.blitz Version: 5.5.8
2023-08-02 16:57:04,791 253        [      main] INFO          ome.formats.importer.ImportConfig - Bioformats version: 6.5.1 revision: 6f50e4d52c9d96112635fd8b2dde737f31041cf0 date: 7 July 2020
2023-08-02 16:57:04,829 291        [      main] INFO   formats.importer.cli.CommandLineImporter - Setting transfer to ln_s
2023-08-02 16:57:04,831 293        [      main] INFO   formats.importer.cli.CommandLineImporter - Log levels -- Bio-Formats: ERROR OMERO.importer: INFO
2023-08-02 16:57:05,108 570        [      main] INFO      ome.formats.importer.ImportCandidates - Depth: 4 Metadata Level: MINIMUM
2023-08-02 16:57:05,271 733        [      main] INFO      ome.formats.importer.ImportCandidates - 1 file(s) parsed into 1 group(s) with 1 call(s) to setId in 160ms. (163ms total) [0 unknowns]
2023-08-02 16:57:05,301 763        [      main] INFO       ome.formats.OMEROMetadataStoreClient - Attempting initial SSL connection to localhost:4064
2023-08-02 16:57:05,650 1112       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Insecure connection requested, falling back
2023-08-02 16:57:05,907 1369       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Pinging session every 300s.
2023-08-02 16:57:05,917 1379       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Server: 5.6.8
2023-08-02 16:57:05,918 1380       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Client: 5.5.8
2023-08-02 16:57:05,918 1380       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Java Version: 11.0.10
2023-08-02 16:57:05,918 1380       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Name: Linux
2023-08-02 16:57:05,918 1380       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Arch: amd64
2023-08-02 16:57:05,918 1380       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Version: 3.10.0-1160.15.2.el7.x86_64
2023-08-02 16:57:06,861 2323       [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILESET_UPLOAD_PREPARATION
2023-08-02 16:57:07,246 2708       [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILESET_UPLOAD_START
2023-08-02 16:57:07,269 2731       [3-thread-1] INFO   s.importer.transfers.SymlinkFileTransfer - Transferring /hyperfile/omero/autoimport/default/govekk_20230802_165701/Image_H120.vsi...
2023-08-02 16:57:07,471 2933       [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_STARTED: /hyperfile/omero/autoimport/default/govekk_20230802_165701/Image_H120.vsi
2023-08-02 16:57:07,606 3068       [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_COMPLETE: /hyperfile/omero/autoimport/default/govekk_20230802_165701/Image_H120.vsi
2023-08-02 16:57:07,743 3205       [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILESET_UPLOAD_END
2023-08-02 16:57:07,826 3288       [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - IMPORT_STARTED Logfile: 804142
2023-08-02 16:57:08,134 3596       [l.Client-0] INFO   ormats.importer.cli.LoggingImportMonitor - METADATA_IMPORTED Step: 1 of 5  Logfile: 804142
2023-08-02 16:57:08,294 3756       [l.Client-1] ERROR     ome.formats.importer.cli.ErrorHandler - INTERNAL_EXCEPTION: /hyperfile/omero/autoimport/default/govekk_20230802_165701/Image_H120.vsi
java.lang.RuntimeException: Failure response on import!
Category: ::omero::grid::ImportRequest
Name: import-request-failure
Parameters: {stacktrace=java.lang.IndexOutOfBoundsException: Index 4 out of bounds for length 4
	at java.base/jdk.internal.util.Preconditions.outOfBounds(Preconditions.java:64)
	at java.base/jdk.internal.util.Preconditions.outOfBoundsCheckIndex(Preconditions.java:70)
	at java.base/jdk.internal.util.Preconditions.checkIndex(Preconditions.java:248)
	at java.base/java.util.Objects.checkIndex(Objects.java:372)
	at java.base/java.util.ArrayList.get(ArrayList.java:459)
	at loci.formats.in.CellSensReader.openBytes(CellSensReader.java:594)
	at loci.formats.ImageReader.openBytes(ImageReader.java:465)
	at loci.formats.ChannelFiller.openBytes(ChannelFiller.java:167)
	at loci.formats.ChannelSeparator.openBytes(ChannelSeparator.java:229)
	at loci.formats.ReaderWrapper.openBytes(ReaderWrapper.java:348)
	at loci.formats.ReaderWrapper.openBytes(ReaderWrapper.java:348)
	at loci.formats.MinMaxCalculator.openBytes(MinMaxCalculator.java:269)
	at ome.services.blitz.repo.ManagedImportRequestI.parseDataByPlane(ManagedImportRequestI.java:872)
	at ome.services.blitz.repo.ManagedImportRequestI.parseData(ManagedImportRequestI.java:803)
	at ome.services.blitz.repo.ManagedImportRequestI.pixelData(ManagedImportRequestI.java:676)
	at ome.services.blitz.repo.ManagedImportRequestI.step(ManagedImportRequestI.java:525)
	at omero.cmd.HandleI.steps(HandleI.java:448)
	at omero.cmd.HandleI$RunSteps.innerWork(HandleI.java:509)
	at omero.cmd.HandleI$2.doWork(HandleI.java:383)
	at omero.cmd.HandleI$2.doWork(HandleI.java:380)
	at jdk.internal.reflect.GeneratedMethodAccessor310.invoke(Unknown Source)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:566)
	at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:333)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:190)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:157)
	at ome.services.util.Executor$Impl$Interceptor.invoke(Executor.java:568)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at ome.security.basic.EventHandler.invoke(EventHandler.java:154)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.orm.hibernate3.HibernateInterceptor.invoke(HibernateInterceptor.java:119)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:99)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:282)
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:96)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at ome.tools.hibernate.ProxyCleanupFilter$Interceptor.invoke(ProxyCleanupFilter.java:249)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at ome.services.util.ServiceHandler.invoke(ServiceHandler.java:121)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:213)
	at com.sun.proxy.$Proxy73.doWork(Unknown Source)
	at ome.services.util.Executor$Impl.execute(Executor.java:447)
	at omero.cmd.HandleI.run(HandleI.java:379)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at ome.services.util.Executor$Impl$1.call(Executor.java:488)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
, message=Index 4 out of bounds for length 4}

	at ome.formats.importer.ImportLibrary$ImportCallback.onFinished(ImportLibrary.java:807)
	at omero.cmd.CmdCallbackI.finished(CmdCallbackI.java:334)
	at omero.cmd._CmdCallbackDisp.___finished(_CmdCallbackDisp.java:118)
	at omero.cmd._CmdCallbackDisp.__dispatch(_CmdCallbackDisp.java:145)
	at IceInternal.Incoming.invoke(Incoming.java:221)
	at Ice.ConnectionI.invokeAll(ConnectionI.java:2536)
	at Ice.ConnectionI.dispatch(ConnectionI.java:1145)
	at Ice.ConnectionI.message(ConnectionI.java:1056)
	at IceInternal.ThreadPool.run(ThreadPool.java:395)
	at IceInternal.ThreadPool.access$300(ThreadPool.java:12)
	at IceInternal.ThreadPool$EventHandlerThread.run(ThreadPool.java:832)
	at java.base/java.lang.Thread.run(Thread.java:834)

java.lang.RuntimeException: Failure response on import!
Category: ::omero::grid::ImportRequest
Name: import-request-failure
Parameters: {stacktrace=java.lang.IndexOutOfBoundsException: Index 4 out of bounds for length 4
	at java.base/jdk.internal.util.Preconditions.outOfBounds(Preconditions.java:64)
	at java.base/jdk.internal.util.Preconditions.outOfBoundsCheckIndex(Preconditions.java:70)
	at java.base/jdk.internal.util.Preconditions.checkIndex(Preconditions.java:248)
	at java.base/java.util.Objects.checkIndex(Objects.java:372)
	at java.base/java.util.ArrayList.get(ArrayList.java:459)
	at loci.formats.in.CellSensReader.openBytes(CellSensReader.java:594)
	at loci.formats.ImageReader.openBytes(ImageReader.java:465)
	at loci.formats.ChannelFiller.openBytes(ChannelFiller.java:167)
	at loci.formats.ChannelSeparator.openBytes(ChannelSeparator.java:229)
	at loci.formats.ReaderWrapper.openBytes(ReaderWrapper.java:348)
	at loci.formats.ReaderWrapper.openBytes(ReaderWrapper.java:348)
	at loci.formats.MinMaxCalculator.openBytes(MinMaxCalculator.java:269)
	at ome.services.blitz.repo.ManagedImportRequestI.parseDataByPlane(ManagedImportRequestI.java:872)
	at ome.services.blitz.repo.ManagedImportRequestI.parseData(ManagedImportRequestI.java:803)
	at ome.services.blitz.repo.ManagedImportRequestI.pixelData(ManagedImportRequestI.java:676)
	at ome.services.blitz.repo.ManagedImportRequestI.step(ManagedImportRequestI.java:525)
	at omero.cmd.HandleI.steps(HandleI.java:448)
	at omero.cmd.HandleI$RunSteps.innerWork(HandleI.java:509)
	at omero.cmd.HandleI$2.doWork(HandleI.java:383)
	at omero.cmd.HandleI$2.doWork(HandleI.java:380)
	at jdk.internal.reflect.GeneratedMethodAccessor310.invoke(Unknown Source)
	at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.base/java.lang.reflect.Method.invoke(Method.java:566)
	at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:333)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:190)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:157)
	at ome.services.util.Executor$Impl$Interceptor.invoke(Executor.java:568)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at ome.security.basic.EventHandler.invoke(EventHandler.java:154)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.orm.hibernate3.HibernateInterceptor.invoke(HibernateInterceptor.java:119)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.transaction.interceptor.TransactionInterceptor$1.proceedWithInvocation(TransactionInterceptor.java:99)
	at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:282)
	at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:96)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at ome.tools.hibernate.ProxyCleanupFilter$Interceptor.invoke(ProxyCleanupFilter.java:249)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at ome.services.util.ServiceHandler.invoke(ServiceHandler.java:121)
	at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:213)
	at com.sun.proxy.$Proxy73.doWork(Unknown Source)
	at ome.services.util.Executor$Impl.execute(Executor.java:447)
	at omero.cmd.HandleI.run(HandleI.java:379)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at ome.services.util.Executor$Impl$1.call(Executor.java:488)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:834)
, message=Index 4 out of bounds for length 4}

	at ome.formats.importer.ImportLibrary$ImportCallback.onFinished(ImportLibrary.java:807) ~[omero-blitz.jar:5.5.8]
	at omero.cmd.CmdCallbackI.finished(CmdCallbackI.java:334) ~[omero-blitz.jar:5.5.8]
	at omero.cmd._CmdCallbackDisp.___finished(_CmdCallbackDisp.java:118) ~[omero-blitz.jar:5.5.8]
	at omero.cmd._CmdCallbackDisp.__dispatch(_CmdCallbackDisp.java:145) ~[omero-blitz.jar:5.5.8]
	at IceInternal.Incoming.invoke(Incoming.java:221) ~[ice.jar:na]
	at Ice.ConnectionI.invokeAll(ConnectionI.java:2536) ~[ice.jar:na]
	at Ice.ConnectionI.dispatch(ConnectionI.java:1145) ~[ice.jar:na]
	at Ice.ConnectionI.message(ConnectionI.java:1056) ~[ice.jar:na]
	at IceInternal.ThreadPool.run(ThreadPool.java:395) ~[ice.jar:na]
	at IceInternal.ThreadPool.access$300(ThreadPool.java:12) ~[ice.jar:na]
	at IceInternal.ThreadPool$EventHandlerThread.run(ThreadPool.java:832) ~[ice.jar:na]
	at java.base/java.lang.Thread.run(Thread.java:834) ~[na:na]
2023-08-02 16:57:08,298 3760       [2-thread-1] ERROR        ome.formats.importer.ImportLibrary - Error on import
java.lang.Exception: Import failure
	at ome.formats.importer.ImportLibrary.importImage(ImportLibrary.java:701) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.ImportLibrary$1.call(ImportLibrary.java:354) ~[omero-blitz.jar:5.5.8]
	at ome.formats.importer.ImportLibrary$1.call(ImportLibrary.java:328) ~[omero-blitz.jar:5.5.8]
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264) ~[na:na]
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515) ~[na:na]
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264) ~[na:na]
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128) ~[na:na]
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628) ~[na:na]
	at java.base/java.lang.Thread.run(Thread.java:834) ~[na:na]
2023-08-02 16:57:08,298 3760       [2-thread-1] INFO         ome.formats.importer.ImportLibrary - Exiting on error

==> Summary
1 file uploaded, 0 filesets created, 0 images imported, 1 error in 0:00:01.451
Using OMERO.java-5.6.3-ice36-b228
Joined session for govekk@localhost:4064. Expires in : 360 min. Current group: default
ERROR:root:Import of /hyperfile/omero/autoimport/default/govekk_20230802_165701/Image_H120.vsi has failed!
ERROR:root:File /hyperfile/omero/autoimport/default/govekk_20230802_165701/Image_H120.vsi has not been imported
Traceback (most recent call last):
  File "/local/omero_scripts/jax-omeroutils/import_annotate_batch.py", line 59, in <module>
    main(Path(args.import_json))
  File "/local/omero_scripts/jax-omeroutils/import_annotate_batch.py", line 44, in main
    imp_ctl.organize()
  File "/local/omero_scripts/jax-omeroutils/jax_omeroutils/importer.py", line 249, in organize
    if len(self.image_ids) == 0:
TypeError: object of type 'NoneType' has no len()
```

# New Bioformats
Old bioformats can't even read metadata (get array out of bounds exception), new bioformats can
(test Fiji, metadata only; test bftools `showinf -nopix`)
bftools `showinf -crop 0,0,512,512` reads pixels and gets to erroring on lack of X11 (fine)

# How to import?
Import just file or also folder? 
* OMERO import knows to read ets files from _H120_1_ folder when called on corresponding VSI file :D
```
omero-server@omero-server:/opt/omero/server/server_venv/bin$ ./omero import --transfer ln_s --depth 10 /opt/omero/server/hyperfile/sample_images/H120_1.vsi 
Created session for root@localhost:4064. Idle timeout: 10 min. Current group: system
2023-08-07 20:59:23,218 473        [      main] INFO          ome.formats.importer.ImportConfig - OMERO.blitz Version: 5.6.3
2023-08-07 20:59:23,251 506        [      main] INFO          ome.formats.importer.ImportConfig - Bioformats version: 6.14.0 revision: fca72bf84a29aee75ab69d9499f723e64ab424ef date: 5 July 2023
2023-08-07 20:59:23,485 740        [      main] INFO   formats.importer.cli.CommandLineImporter - Setting transfer to ln_s
2023-08-07 20:59:23,492 747        [      main] INFO   formats.importer.cli.CommandLineImporter - Log levels -- Bio-Formats: ERROR OMERO.importer: INFO
2023-08-07 20:59:24,036 1291       [      main] INFO      ome.formats.importer.ImportCandidates - Depth: 10 Metadata Level: MINIMUM
2023-08-07 20:59:24,901 2156       [      main] INFO      ome.formats.importer.ImportCandidates - 1 file(s) parsed into 1 group(s) with 1 call(s) to setId in 854ms. (864ms total) [0 unknowns]
2023-08-07 20:59:24,983 2238       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Attempting initial SSL connection to localhost:4064
2023-08-07 20:59:25,933 3188       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Insecure connection requested, falling back
2023-08-07 20:59:26,499 3754       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Pinging session every 300s.
2023-08-07 20:59:26,522 3777       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Server: 5.6.8
2023-08-07 20:59:26,522 3777       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Client: 5.6.3
2023-08-07 20:59:26,523 3778       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Java Version: 11.0.18
2023-08-07 20:59:26,523 3778       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Name: Linux
2023-08-07 20:59:26,523 3778       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Arch: amd64
2023-08-07 20:59:26,523 3778       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Version: 5.15.107+
2023-08-07 20:59:27,354 4609       [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILESET_UPLOAD_PREPARATION
2023-08-07 20:59:28,038 5293       [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILESET_UPLOAD_START
2023-08-07 20:59:28,066 5321       [3-thread-1] INFO   s.importer.transfers.SymlinkFileTransfer - Transferring /opt/omero/server/hyperfile/sample_images/H120_1.vsi...
2023-08-07 20:59:28,194 5449       [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_STARTED: /opt/omero/server/hyperfile/sample_images/H120_1.vsi
2023-08-07 20:59:29,029 6284       [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_COMPLETE: /opt/omero/server/hyperfile/sample_images/H120_1.vsi
2023-08-07 20:59:29,061 6316       [3-thread-1] INFO   s.importer.transfers.SymlinkFileTransfer - Transferring /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10002/frame_t_0.ets...
2023-08-07 20:59:29,177 6432       [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_STARTED: /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10002/frame_t_0.ets
2023-08-07 21:00:08,926 46181      [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_COMPLETE: /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10002/frame_t_0.ets
2023-08-07 21:00:08,947 46202      [3-thread-1] INFO   s.importer.transfers.SymlinkFileTransfer - Transferring /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f.meta...
2023-08-07 21:00:09,126 46381      [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_STARTED: /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f.meta
2023-08-07 21:00:09,399 46654      [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_COMPLETE: /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f.meta
2023-08-07 21:00:09,426 46681      [3-thread-1] INFO   s.importer.transfers.SymlinkFileTransfer - Transferring /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f_Frame#0.ets...
2023-08-07 21:00:09,563 46818      [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_STARTED: /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f_Frame#0.ets
2023-08-07 21:00:10,816 48071      [3-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILE_UPLOAD_COMPLETE: /opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f_Frame#0.ets
2023-08-07 21:00:10,964 48219      [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - FILESET_UPLOAD_END
2023-08-07 21:00:11,211 48466      [2-thread-1] INFO   ormats.importer.cli.LoggingImportMonitor - IMPORT_STARTED Logfile: 98
2023-08-07 21:00:13,074 50329      [l.Client-0] INFO   ormats.importer.cli.LoggingImportMonitor - METADATA_IMPORTED Step: 1 of 5  Logfile: 98
2023-08-07 21:00:13,618 50873      [l.Client-1] INFO   ormats.importer.cli.LoggingImportMonitor - PIXELDATA_PROCESSED Step: 2 of 5  Logfile: 98
2023-08-07 21:00:15,248 52503      [l.Client-1] INFO   ormats.importer.cli.LoggingImportMonitor - THUMBNAILS_GENERATED Step: 3 of 5  Logfile: 98
2023-08-07 21:00:15,298 52553      [l.Client-0] INFO   ormats.importer.cli.LoggingImportMonitor - METADATA_PROCESSED Step: 4 of 5  Logfile: 98
2023-08-07 21:00:15,332 52587      [l.Client-2] INFO   ormats.importer.cli.LoggingImportMonitor - OBJECTS_RETURNED Step: 5 of 5  Logfile: 98
2023-08-07 21:00:15,597 52852      [l.Client-1] INFO   ormats.importer.cli.LoggingImportMonitor - IMPORT_DONE Imported file: /opt/omero/server/hyperfile/sample_images/H120_1.vsi
Image:7,8
Other imported objects:
Fileset:5

==> Summary
4 files uploaded, 1 fileset created, 2 images imported, 0 errors in 0:00:48.270
```

It just worked?? Maybe I ran too early last time and was still uploading?

But then getting pixel buffer errors when try to load
```
2023-08-07 21:02:23,825 ERROR [                ome.io.nio.PixelsService] (l.Server-9) Error instantiating pixel buffer: /OMERO/ManagedRepository/root_0/2023-08/07/20-59-27.516/H120_1.vsi
2023-08-07 21:05:53,522 ERROR [         ome.io.bioformats.BfPixelBuffer] (l.Server-9) Failed to instantiate BfPixelsWrapper with /OMERO/ManagedRepository/root_0/2023-08/07/20-59-27.516/H120_1.vsi
```

# omero import -f 
/opt/omero/server/hyperfile/sample_images/H120_1.vsi
/opt/omero/server/hyperfile/sample_images/_H120_1_/stack10002/frame_t_0.ets
/opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f.meta
/opt/omero/server/hyperfile/sample_images/_H120_1_/stack10000/blob_21_f_Frame#0.ets

## omero import -f from old sms import
```
#======================================
# Group: /dropbox/dropbox/sms_20230706/Image_NegativeControl.vsi SPW: false Reader: loci.formats.in.CellSensReader
/dropbox/dropbox/sms_20230706/Image_NegativeControl.vsi
/dropbox/dropbox/sms_20230706/_Image_NegativeControl_/stack1/frame_t.ets
/dropbox/dropbox/sms_20230706/_Image_NegativeControl_/stack10000/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_NegativeControl_/stack10002/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_NegativeControl_/stack10000/blob_21_f.meta
/dropbox/dropbox/sms_20230706/_Image_NegativeControl_/stack10000/blob_21_f_Frame#0.ets
#======================================
# Group: /dropbox/dropbox/sms_20230706/Image_H120.vsi SPW: false Reader: loci.formats.in.CellSensReader
/dropbox/dropbox/sms_20230706/Image_H120.vsi
/dropbox/dropbox/sms_20230706/_Image_H120_/stack1/frame_t.ets
/dropbox/dropbox/sms_20230706/_Image_H120_/stack10000/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_H120_/stack10002/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_H120_/stack10000/blob_21_f.meta
/dropbox/dropbox/sms_20230706/_Image_H120_/stack10000/blob_21_f_Frame#0.ets
#======================================
# Group: /dropbox/dropbox/sms_20230706/Image_H220.vsi SPW: false Reader: loci.formats.in.CellSensReader
/dropbox/dropbox/sms_20230706/Image_H220.vsi
/dropbox/dropbox/sms_20230706/_Image_H220_/stack1/frame_t.ets
/dropbox/dropbox/sms_20230706/_Image_H220_/stack10000/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_H220_/stack10002/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_H220_/stack10000/blob_21_f.meta
/dropbox/dropbox/sms_20230706/_Image_H220_/stack10000/blob_21_f_Frame#0.ets
#======================================
# Group: /dropbox/dropbox/sms_20230706/Image_WT1.vsi SPW: false Reader: loci.formats.in.CellSensReader
/dropbox/dropbox/sms_20230706/Image_WT1.vsi
/dropbox/dropbox/sms_20230706/_Image_WT1_/stack1/frame_t.ets
/dropbox/dropbox/sms_20230706/_Image_WT1_/stack10000/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_WT1_/stack10002/frame_t_0.ets
/dropbox/dropbox/sms_20230706/_Image_WT1_/stack10000/blob_21_f.meta
/dropbox/dropbox/sms_20230706/_Image_WT1_/stack10000/blob_21_f_Frame#0.ets
File moved to /hyperfile/omero/autoimport/korstanjelab/sms_20230807_170004/Image_NegativeControl.vsi
File moved to /hyperfile/omero/autoimport/korstanjelab/sms_20230807_170004/Image_H120.vsi
File moved to /hyperfile/omero/autoimport/korstanjelab/sms_20230807_170004/Image_H220.vsi
File moved to /hyperfile/omero/autoimport/korstanjelab/sms_20230807_170004/Image_WT1.vsi
Ready for import at:/hyperfile/omero/autoimport/korstanjelab/sms_20230807_170004/import.json
Preparing to import /hyperfile/omero/autoimport/korstanjelab/sms_20230807_170004/Image_NegativeControl.vsi
```

So `omero import -f` does see ets but the nonly vsi is moved?

# tried to import folder
```
2023-08-08 05:00:17,456 198        [      main] INFO          ome.formats.importer.ImportConfig - OMERO.blitz Version: 5.6.3
2023-08-08 05:00:17,470 212        [      main] INFO          ome.formats.importer.ImportConfig - Bioformats version: 6.14.0 revision: fca72bf84a29aee75ab69d9499f723e64ab424ef date: 5 July 2023
2023-08-08 05:00:17,517 259        [      main] INFO   formats.importer.cli.CommandLineImporter - Log levels -- Bio-Formats: ERROR OMERO.importer: INFO
2023-08-08 05:00:17,881 623        [      main] INFO      ome.formats.importer.ImportCandidates - Depth: 4 Metadata Level: MINIMUM
2023-08-08 05:00:18,067 809        [      main] INFO      ome.formats.importer.ImportCandidates - 6 file(s) parsed into 1 group(s) with 1 call(s) to setId in 179ms. (186ms total) [0 unknowns]
Using OMERO.java-5.6.8-ice36
Traceback (most recent call last):
  File "/local/omero_scripts/jax-omeroutils/prepare_batch.py", line 58, in <module>
    main(Path(args.import_batch_directory),Path(args.log_directory))
  File "/local/omero_scripts/jax-omeroutils/prepare_batch.py", line 41, in main
    message = mover.move_data()
  File "/local/omero_scripts/jax-omeroutils/jax_omeroutils/datamover.py", line 101, in move_data
    result = file_mover(src_fp, self.server_path)
  File "/local/omero_scripts/jax-omeroutils/jax_omeroutils/datamover.py", line 41, in file_mover
    shutil.copy(file_path, destination_dir)
  File "/usr/lib64/python3.6/shutil.py", line 245, in copy
    copyfile(src, dst, follow_symlinks=follow_symlinks)
  File "/usr/lib64/python3.6/shutil.py", line 120, in copyfile
    with open(src, 'rb') as fsrc:
IsADirectoryError: [Errno 21] Is a directory: '/dropbox/dropbox/govekk_20230807/H120_1_folder'
Traceback (most recent call last):
  File "/local/omero_scripts/jax-omeroutils/import_annotate_batch.py", line 59, in <module>
    main(Path(args.import_json))
  File "/local/omero_scripts/jax-omeroutils/import_annotate_batch.py", line 19, in main
    batch_md = json.load(fp)
  File "/usr/lib64/python3.6/json/__init__.py", line 296, in load
    return loads(fp.read(),
  File "/usr/lib64/python3.6/codecs.py", line 321, in decode
    (result, consumed) = self._buffer_decode(data, self.errors, final)
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe4 in position 24: invalid continuation byte
```

# Importing into Fiji

java.lang.NegativeArraySizeException


# Unpacking error due to server crash
Errored on first image
```
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
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero_cli_transfer.py", line 125, in _wrapper
    return func(self, *args, **kwargs)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero_cli_transfer.py", line 195, in unpack
    self.__unpack(args)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero_cli_transfer.py", line 347, in __unpack
    hash, folder, self.metadata)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/generate_omero_objects.py", line 475, in populate_omero
    create_rois(ome.rois, ome.images, img_map, conn)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/generate_omero_objects.py", line 337, in create_rois
    img_id_dest = img_map[img.id]
KeyError: 'Image:80380'
```
Can't scroll up on screen, but also get "Image corresponding to {img.id} not found. Skipping." type messages repeatedly

Errored twice

./omero transfer unpack --ln_s_import --folder /hyperfile/omero/public_data/p1401/ --metadata img_id timestamp software version


Failed: 4075, p1401, p1401_fail

## Real error was due to lingering lock file after server crash
errors:
"Cannot exclusively use the managed repository"
```
2023-05-15 18:29:29,025 2153       [      main] INFO      ome.formats.importer.ImportCandidates - 1 file(s) parsed into 1 group(s) with 1 call(s) to setId in 1329ms. (1336ms total) [0 unknowns]
2023-05-15 18:29:29,077 2205       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Attempting initial SSL connection to localhost:4064
2023-05-15 18:29:29,613 2741       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Insecure connection requested, falling back
2023-05-15 18:29:29,998 3126       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Pinging session every 300s.
2023-05-15 18:29:30,009 3137       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Server: 5.6.6
2023-05-15 18:29:30,010 3138       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Client: 5.6.0
2023-05-15 18:29:30,010 3138       [      main] INFO       ome.formats.OMEROMetadataStoreClient - Java Version: 11.0.18
2023-05-15 18:29:30,010 3138       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Name: Linux
2023-05-15 18:29:30,010 3138       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Arch: amd64
2023-05-15 18:29:30,010 3138       [      main] INFO       ome.formats.OMEROMetadataStoreClient - OS Version: 5.10.162+
2023-05-15 18:29:40,414 13542      [      main] ERROR                   ome.system.UpgradeCheck - Error reading from url: http://upgrade.openmicroscopy.org.uk?version=5.6.0;os.name=Linux;os.arch=amd64;os.version=5.10.162%2B;java.runtime.version=11.0.18%2B10-post-Debian-1deb10u1;java.vm.vendor=Debian "connect timed out"
2023-05-15 18:29:40,422 13550      [2-thread-1] ERROR        ome.formats.importer.ImportLibrary - Error on import
java.lang.RuntimeException: Cannot exclusively use the managed repository.

Likely no ManagedRepositoryPrx is being returned from the server.
This could point to a recent server crash. Ask your server administrator
to check for stale .lock files under the OMERO data directory. This
is particularly likely on a server using NFS.

	at ome.formats.importer.ImportLibrary.checkManagedRepo(ImportLibrary.java:902)
	at ome.formats.importer.ImportLibrary.createImport(ImportLibrary.java:432)
	at ome.formats.importer.ImportLibrary.importImage(ImportLibrary.java:613)
	at ome.formats.importer.ImportLibrary$1.call(ImportLibrary.java:354)
	at ome.formats.importer.ImportLibrary$1.call(ImportLibrary.java:328)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:829)
2023-05-15 18:29:40,422 13550      [2-thread-1] INFO         ome.formats.importer.ImportLibrary - Exiting on error

==> Summary
0 files uploaded, 0 filesets created, 0 images imported, 0 errors in 0:00:00.008
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
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero_cli_transfer.py", line 125, in _wrapper
    return func(self, *args, **kwargs)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero_cli_transfer.py", line 195, in unpack
    self.__unpack(args)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/omero_cli_transfer.py", line 347, in __unpack
    hash, folder, self.metadata)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/generate_omero_objects.py", line 475, in populate_omero
    create_rois(ome.rois, ome.images, img_map, conn)
  File "/opt/omero/server/server_venv/lib/python3.7/site-packages/generate_omero_objects.py", line 337, in create_rois
    img_id_dest = img_map[img.id]
KeyError: 'Image:80380'
```

```
omero-server@omero-readonly-server-7dc576b-fpmgk:/hyperfile$ find . -name *.lock*
./OMERO/.omero/repository/c77783ea-a8c4-4913-b9a4-31b354774b1c/.lock
./omero/OMERO/ManagedRepository/.omero/repository/c77783ea-a8c4-4913-b9a4-31b354774b1c/.lock
```

# Want to only publish some images inside an original .lif file
Cannot split images from one Fileset between groups:

Testing results on an ndpi with multiple images:
```
omero.cmd.Chgrp2 Image:5 failed: 'graph-fail'
failed: may not move Image[5] while Image[4] remains as they share Instrument[1]
```

This is more-so the error we would get with Ewelina's lif files:
```
omero.cmd.Chgrp2 Image:8 failed: 'graph-fail'
failed: within Fileset[5] may not move Image[8] while Image[7] remains
```

```
omero-server@omero-server:/opt/omero/server/server_venv/bin$ ./omero chgrp 53 Image:190076 --dry-run --report
Using session for Importer@localhost:4064. Idle timeout: 10 min. Current group: staging
omero.cmd.Chgrp2 Image:190076 failed: 'graph-fail'
failed: within Fileset[47142] may not move Image[190076] while Image[190077] remains
Steps: 4
Elapsed time: 3.734 secs.
Flags: [FAILURE, CANCELLED]
```

## Deleting Fileset notes

Get all Filesets in current group
```
./omero hql -q "select 'Fileset:'||f.id from Fileset f"
```

```
 #  | Col1          
----+---------------
 0  | Fileset:47101 
 1  | Fileset:47102 
 2  | Fileset:47103 
 3  | Fileset:47104 
 4  | Fileset:47105 
 5  | Fileset:47106 
 6  | Fileset:47107 
 7  | Fileset:47108 
 8  | Fileset:47109 
 9  | Fileset:47110 
 10 | Fileset:47111 
 11 | Fileset:47112 
 12 | Fileset:47113 
 13 | Fileset:47114 
 14 | Fileset:47115 
 15 | Fileset:47116 
 16 | Fileset:47117 
 17 | Fileset:47118 
 18 | Fileset:47119 
 19 | Fileset:47120 
 20 | Fileset:47121 
 21 | Fileset:47122 
 22 | Fileset:47123 
 23 | Fileset:47124 
 24 | Fileset:47125 
 25 | Fileset:47126 
 26 | Fileset:47127 
 27 | Fileset:47128 
 28 | Fileset:47129 
 29 | Fileset:47130 
 30 | Fileset:47131 
 31 | Fileset:47132 
 32 | Fileset:47133 
 33 | Fileset:47134 
 34 | Fileset:47135 
 35 | Fileset:47136 
 36 | Fileset:47137 
 37 | Fileset:47138 
 38 | Fileset:47139 
 39 | Fileset:47140 
 40 | Fileset:47141 
 41 | Fileset:47142 
 42 | Fileset:47143 
 43 | Fileset:47144 
 44 | Fileset:47145 
 45 | Fileset:47146 
 46 | Fileset:47147 
 47 | Fileset:47148 
 48 | Fileset:47149 
 49 | Fileset:47150 
 50 | Fileset:47151 
 51 | Fileset:47152 
 52 | Fileset:47153 
 53 | Fileset:47154 
 54 | Fileset:47155 
 55 | Fileset:47156 
 56 | Fileset:47157 
 57 | Fileset:47158 
 58 | Fileset:47159 
 59 | Fileset:47160 
 60 | Fileset:47161 
 61 | Fileset:47162 
 62 | Fileset:47163 
 63 | Fileset:47164 
 64 | Fileset:47165 
 65 | Fileset:47166 
 66 | Fileset:47167 
 67 | Fileset:47168 
 68 | Fileset:47169 
 69 | Fileset:47170 
 70 | Fileset:47171 
 71 | Fileset:47172 
 72 | Fileset:47173 
 73 | Fileset:47174 
 74 | Fileset:47175 
 75 | Fileset:47176 
```


NOTES:
omero hql -q "select 'Image:'||i.id from Image i left outer join i.wellSamples ws left outer join i.datasetLinks dil where i.details.owner.id = 2 and ws is null and dil is null" --style=plain | cut -f2 -d,

./omero hql -q "select 'Fileset:'||f.id from Fileset f"
./omero hql -q "select 'Image:'||i.id from Image i" --style=plain | cut -f2 -d "," | sort > /hyperfile/tmp_kiya/img_ids.txt

Fileset: 47101-47114,47116,47118-47141,47143-47176

189351-189385,189690,190006-190075, 

failed: within Fileset[47142] may not move Image[190076] while Image[190077] remains
Images: 190076-190382


Deleting Fileset: 47101-47176
Deletes images: 189351-190416

Same images as when hql: 189351-190416

```
omero-server@omero-server:/opt/omero/server/server_venv/bin$ ./omero delete Fileset:47101-47176 --include Annotation --dry-run --report
Using session for Importer@localhost:4064. Idle timeout: 10 min. Current group: staging
omero.cmd.Delete2 Fileset:47101-47176 Dry run performed
ok
Steps: 4
Elapsed time: 88.146 secs.
Flags: []
Deleted objects
  Filter:170201-173188
  Instrument:97651-98646
  LightPath:169651-172638
  LightPathEmissionFilterLink:169651-172638
  Microscope:97651-98646
  Objective:97751-98746
  ObjectiveSettings:176901-177896
  CommentAnnotation:163301-163376
  FilesetAnnotationLink:47001-47076
  ImageAnnotationLink:275501-277935
  MapAnnotation:163377-164518,164520-164821
  TagAnnotation:164519
  DatasetImageLink:200601-200902
  Channel:485901-489168
  Image:189351-190416
  LogicalChannel:478601-481868
  OriginalFile:311221,311222,311224,311225,311227,311228,311230,311231,311233,311234,311236,311237,311239,311240,311242,311243,311245,311246,311248,311249,311251,311252,311254,311255,311257,311258,311260,311261,311263,311264,311266,311267,311269,311270,311272,311273,311275,311276,311278,311279,311281,311282,311284,311285,311287,311288,311290,311291,311293,311294,311296,311297,311299,311300,311302,311303,311305,311306,311308,311309,311311,311312,311314,311315,311317,311318,311320,311321,311323,311324,311326,311327,311329,311330,311332,311333,311335,311336,311338,311339,311341,311342,311344,311345,311347,311348,311350,311351,311353,311354,311356,311357,311359,311360,311362,311363,311365,311366,311368,311369,311371,311372,311374,311375,311377,311378,311380,311381,311383,311384,311386,311387,311389,311390,311392,311393,311395,311396,311398,311399,311401,311402,311404,311405,311407,311408,311410,311411,311413,311414,311416,311417,311419,311420,311422,311423,311425,311426,311428,311429,311431,311432,311434,311435,311437,311438,311440,311441,311443,311444,311446,311447
  Pixels:189351-190416
  PlaneInfo:574101-577158
  ChannelBinding:654101-657368
  QuantumDef:258001-259066
  RenderingDef:258001-259066
  Thumbnail:264201-265266
  Fileset:47101-47176
  ... + ROIs and stuff
```
Should be the same:
```
omero-server@omero-server:/opt/omero/server/server_venv/bin$ ./omero delete Image:189351-190416 --include Annotation --dry-run --report
Using session for Importer@localhost:4064. Idle timeout: 10 min. Current group: staging
omero.cmd.Delete2 Image:189351-190416 Dry run performed
ok
Steps: 4
Elapsed time: 91.886 secs.
Flags: []
Deleted objects
  Filter:170201-173188
  Instrument:97651-98646
  LightPath:169651-172638
  LightPathEmissionFilterLink:169651-172638
  Microscope:97651-98646
  Objective:97751-98746
  ObjectiveSettings:176901-177896
  CommentAnnotation:163301-163376
  FilesetAnnotationLink:47001-47076
  ImageAnnotationLink:275501-277935
  MapAnnotation:163377-164518,164520-164821
  TagAnnotation:164519
  DatasetImageLink:200601-200902
  Channel:485901-489168
  Image:189351-190416
  LogicalChannel:478601-481868
  OriginalFile:311221,311222,311224,311225,311227,311228,311230,311231,311233,311234,311236,311237,311239,311240,311242,311243,311245,311246,311248,311249,311251,311252,311254,311255,311257,311258,311260,311261,311263,311264,311266,311267,311269,311270,311272,311273,311275,311276,311278,311279,311281,311282,311284,311285,311287,311288,311290,311291,311293,311294,311296,311297,311299,311300,311302,311303,311305,311306,311308,311309,311311,311312,311314,311315,311317,311318,311320,311321,311323,311324,311326,311327,311329,311330,311332,311333,311335,311336,311338,311339,311341,311342,311344,311345,311347,311348,311350,311351,311353,311354,311356,311357,311359,311360,311362,311363,311365,311366,311368,311369,311371,311372,311374,311375,311377,311378,311380,311381,311383,311384,311386,311387,311389,311390,311392,311393,311395,311396,311398,311399,311401,311402,311404,311405,311407,311408,311410,311411,311413,311414,311416,311417,311419,311420,311422,311423,311425,311426,311428,311429,311431,311432,311434,311435,311437,311438,311440,311441,311443,311444,311446,311447
  Pixels:189351-190416
  PlaneInfo:574101-577158
  ChannelBinding:654101-657368
  QuantumDef:258001-259066
  RenderingDef:258001-259066
  Thumbnail:264201-265266
  Fileset:47101-47176
```
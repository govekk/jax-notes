/hyperfile/omero/autoimport/gates_foundation/kjp_20230531_170003/Atp13a3_A_12584_OV_03.ndpi

```
2023-08-08 11:09:21,028 INFO  [        ome.services.util.ServiceHandler] (Server-353)  Executor.doWork -- ome.services.RenderingBean.getPixelBuffer[]
2023-08-08 11:09:21,028 INFO  [        ome.services.util.ServiceHandler] (Server-353)  Args:	[null, InternalSF@254917009]
2023-08-08 11:09:21,029 INFO  [    ome.security.basic.BasicEventContext] (Server-353)  cctx:	group=703
2023-08-08 11:09:21,030 INFO  [         ome.security.basic.EventHandler] (Server-353)  Auth:	user=513,group=703,event=null(User),sess=55050001-edaf-4996-8b36-84f65043d7f2
2023-08-08 11:09:21,031 INFO  [      ome.services.OmeroFilePathResolver] (Server-353) Metadata only file, resulting path: /local/OMERO/ManagedRepository/kjp_513/2023-05/31/17-43-31.649/Atp13a3_A_12587_UT_04.ndpi
2023-08-08 11:09:21,040 INFO  [                   loci.formats.Memoizer] (Server-353) Old version of memo file: 3 not 4
2023-08-08 11:09:21,040 DEBUG [                   loci.formats.Memoizer] (Server-353) start[1691507361039] time[0] tag[loci.formats.Memoizer.loadMemo]
2023-08-08 11:09:21,050 INFO  [ ome.services.blitz.fire.SessionManagerI] (Server-347) Found session locally: 30d4bced-643a-479e-bb46-a97160734ec1
2023-08-08 11:09:21,051 INFO  [ ome.services.blitz.fire.SessionManagerI] (Server-347) Rejoining session ServiceFactoryI(session-9358ffe8-7a00-4d0b-84f1-0849dd050265/30d4bced-643a-479e-bb46-a97160734ec1) (agent=OMERO.web)
2023-08-08 11:09:21,055 INFO  [o.services.sessions.SessionContext$Count] (Server-354) -Reference count: 30d4bced-643a-479e-bb46-a97160734ec1=0
2023-08-08 11:09:21,055 INFO  [                      omero.cmd.SessionI] (Server-354) cleanupSelf(ServiceFactoryI(session-9358ffe8-7a00-4d0b-84f1-0849dd050265/30d4bced-643a-479e-bb46-a97160734ec1)).
2023-08-08 11:09:21,334 ERROR [            loci.formats.tiff.TiffParser] (Server-353) Error reading IFD type at: 1768099013
2023-08-08 11:09:21,337 ERROR [            loci.formats.tiff.TiffParser] (Server-353) Error reading IFD type at: 1768099013
2023-08-08 11:09:21,341 ERROR [            loci.formats.tiff.TiffParser] (Server-353) Error reading IFD type at: 1768099013
2023-08-08 11:09:21,343 ERROR [            loci.formats.tiff.TiffParser] (Server-353) Error reading IFD type at: 1768099013
2023-08-08 11:09:21,346 ERROR [            loci.formats.tiff.TiffParser] (Server-353) Error reading IFD type at: 1768099013
2023-08-08 11:09:21,382 INFO  [                loci.formats.ImageReader] (Server-353) NDPIReader initializing /local/OMERO/ManagedRepository/kjp_513/2023-05/31/17-43-31.649/Atp13a3_A_12587_UT_04.ndpi
2023-08-08 11:09:21,385 INFO  [       loci.formats.in.MinimalTiffReader] (Server-353) Reading IFDs
2023-08-08 11:09:21,446 INFO  [       loci.formats.in.MinimalTiffReader] (Server-353) Populating metadata
```

# bfconvert to ome.btf
```
Error reading IFD type at: 518585088
Error reading IFD type at: 518585088
Error reading IFD type at: 518585088
Error reading IFD type at: 518585088
Error reading IFD type at: 518585088
NDPIReader initializing ./Atp13a3_A_12584_OV_03.ndpi
Reading IFDs
Populating metadata
Populating OME metadata
[Hamamatsu NDPI] -> ./Atp13a3_A_12584_OV_03.ome.btf [OME-TIFF]
Switching to BigTIFF (by file extension)
Tile size = 1024 x 1024
        Series 0: converted 1/1 planes (100%)
Tile size = 1024 x 1024
        Series 0: converted 1/1 planes (100%)
Tile size = 1024 x 1024
        Series 0: converted 1/1 planes (100%)
        Series 1: converted 1/1 planes (100%)
        Series 2: converted 1/1 planes (100%)
[done]
2937.293s elapsed (95.8+587104.2ms per plane, 1085ms overhead)
```

showinf of btf
```
Checking file format [OME-TIFF]
Initializing reader
OMETiffReader initializing ./Atp13a3_A_12584_OV_03.ome.btf
Reading IFDs
Populating metadata
Initialization took 0.161s

Reading core metadata
filename = /dropbox/dropbox/tmp_govekk/./Atp13a3_A_12584_OV_03.ome.btf
Used files = [/dropbox/dropbox/tmp_govekk/./Atp13a3_A_12584_OV_03.ome.btf]
Series count = 5
Series #0 :
	Image count = 1
	RGB = true (3) 
	Interleaved = false
	Indexed = false (false color)
	Width = 234240
	Height = 92928
	SizeZ = 1
	SizeT = 1
	SizeC = 3 (effectively 1)
	Tile size = 1024 x 1024
	Thumbnail size = 128 x 50
	Endianness = intel (little)
	Dimension order = XYCZT (certain)
	Pixel type = uint8
	Valid bits per pixel = 8
	Metadata complete = true
	Thumbnail series = false
	-----
	Plane #0 <=> Z 0, C 0, T 0

Series #1 :
	Image count = 1
	RGB = true (3) 
	Interleaved = false
	Indexed = false (false color)
	Width = 58560
	Height = 23232
	SizeZ = 1
	SizeT = 1
	SizeC = 3 (effectively 1)
	Tile size = 1024 x 1024
	Thumbnail size = 128 x 50
	Endianness = intel (little)
	Dimension order = XYCZT (certain)
	Pixel type = uint8
	Valid bits per pixel = 8
	Metadata complete = true
	Thumbnail series = false
	-----
	Plane #0 <=> Z 0, C 0, T 0

Series #2 :
	Image count = 1
	RGB = true (3) 
	Interleaved = false
	Indexed = false (false color)
	Width = 14640
	Height = 5808
	SizeZ = 1
	SizeT = 1
	SizeC = 3 (effectively 1)
	Tile size = 1024 x 1024
	Thumbnail size = 128 x 50
	Endianness = intel (little)
	Dimension order = XYCZT (certain)
	Pixel type = uint8
	Valid bits per pixel = 8
	Metadata complete = true
	Thumbnail series = false
	-----
	Plane #0 <=> Z 0, C 0, T 0

Series #3 :
	Image count = 1
	RGB = true (3) 
	Interleaved = false
	Indexed = false (false color)
	Width = 1188
	Height = 402
	SizeZ = 1
	SizeT = 1
	SizeC = 3 (effectively 1)
	Tile size = 1188 x 1
	Thumbnail size = 128 x 43
	Endianness = intel (little)
	Dimension order = XYCZT (certain)
	Pixel type = uint8
	Valid bits per pixel = 8
	Metadata complete = true
	Thumbnail series = false
	-----
	Plane #0 <=> Z 0, C 0, T 0

Series #4 :
	Image count = 1
	RGB = true (1) 
	************ RGB mismatch ************
	Interleaved = false
	Indexed = false (false color)
	Width = 594
	Height = 201
	SizeZ = 1
	SizeT = 1
	SizeC = 1
	Tile size = 594 x 1
	Thumbnail size = 128 x 43
	Endianness = intel (little)
	Dimension order = XYCZT (certain)
	Pixel type = uint8
	Valid bits per pixel = 8
	Metadata complete = true
	Thumbnail series = false
	-----
	Plane #0 <=> Z 0, C 0, T 0


Reading global metadata
AHEX[0]: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[0].fluorescence: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[0].ploidy: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[1]: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[1].fluorescence: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[1].ploidy: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[2]: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[2].fluorescence: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
AHEX[2].ploidy: 0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
BitsPerSample: 1
Capture mode: Brightfield
Compression: JPEG
Created: 2022/02/24
File write time (seconds): 12
Focus offset (nm): 0
Focus time (seconds): 906
Fully automatic focus: 0
ImageLength: 92928
ImageWidth: 234240
JPEG quality: 90
Magnification: 40.0
MetaDataPhotometricInterpretation: RGB
NDP.S/N: 000231
NDP.image version: 1
NumberOfChannels: 3
Objective.Lens.Magnificant: 35.16
PSHV: 106
PSHV.10x: 106
PSHV.40x: 106
PSHV.ploidy: 106
PSHV.ploidy.10x: 106
PhotometricInterpretation: YCbCr
Product: C13239-01
Reference: Atp13a3_A_12584_OV_03
Refocus interval (minutes): 0
ResolutionUnit: Centimeter
SamplesPerPixel: 3
Scan time (seconds): 576
Scanner serial number: 000231
Slide center X (nm): 9117700
Slide center Y (nm): -388059
Slide center Z (nm): 0
Tissue index: 0
Updated: 2023/05/30
YCbCrSubSampling: chroma image dimensions = luma image dimensions
YRNP[0]: 0,0,0,0
YRNP[1]: 0,0,0,0
YRNP[2]: 0,0,0,0
calibration.version: 361
ccd.height: 0
ccd.width: 8486
ccd.width.ploidy: 8486
coarse.focus.pitch: 10000
colorfilterID: 0
cube.kind: 0
exposure.barcode.macro: 100
exposure.slide.darkfield.macro: 10
exposure.slide.macro: 3
fine.focus.pitch: 250
focalplane.leftbottom: 91275,692133,88440
focalplane.lefttop: 91275,492133,88063
focalplane.rightbottom: 491275,692133,88258
focalplane.righttop: 491275,492133,87830
lane.shift.amount: 0
roi.barcode.macro: 929,295,1240,700
roi.slide.macro: 53,298,1241,700
slant.leftbottom: 0,0,0
slant.lefttop: 0,0,0
slant.rightbottom: 0,0,0
slant.righttop: 0,0,0
slide.tickness: 0
stage.center: 181275,592133
system.version: 1.0
target.white.intensity: 235
valid.DDKP: 0
valid.DLTP: 0
valid.DSHP: 0
variable.exposuretime: 0
zCoarse[0]: 0,0,0,0
zCoarse[1]: 0,0,0,0
zCoarse[2]: 0,0,0,0
zFine[0]: 0,0,0,0
zFine[1]: 0,0,0,0
zFine[2]: 0,0,0,0

Reading series #0 metadata
```

# Patterns
In dataset 2973 (21_781_DKO):
Artifact: 1, 35, 56 (lowest 4.4 Gb)
Not: 55, 50, 33 (highest 4389M)

In dataset 3674 (Atp13a3):
Artifact: Atp13a3_A_12584_OV_01.ndpi (4342M, just a little)
Not: Atp13a3_A_12589_OV_03.ndpi (4175M)


# Find files
```
find . -type f -size +4000M -name "*.ndpi" | sort
```

# Other files

## Working files
LDS5667D1_80_MTC_11L_1micron.ndpi - 6.4 Gb, img ID 408321, imported 2023-04-14, 20x z-stack
- pixels 94208 x 33792, 11 x 1 Z x T - maybe it's fine to load since each Z slice is small

Topors_A-2634_OV_01.ndpi - 4.20 Gb, img ID 380519, imported 2022-08-31, 40x
- pixels 218880 x 90112, 1 x 1
19723714560

Topors_A-2634_UT_05.ndpi - 4.29 Gb, img ID 380546, imported 2022-08-31, 40x
- pixels 203520 x 95744, 1 x 1
19485818880

## Compare to tiniest amount, but working
Dguok_A_6275_Con_OV_05.ndpi - 4.303Gb, img ID 393095, imported 2023-02-02, 40x
- pixels 234240 x 78848, 1 x 1
18469355520

## Artifact files
Topors_A-2634_OV_03.ndpi - 4.31 Gb, img ID 380525, imported 2022-08-31, 40x
- pixels 211200 x 95744, 1 x 1
20221132800

Topors_A-2634_OV_04.ndpi - 4.38 Gb, img ID 	380528, imported 2022-08-31, 40x
- pixels 207360 x 95744, 1 x 1
19853475840

Topors_A-2634_UT_03.ndpi - 4.40 Gb, img ID 380540, imported 2022-08-31, 40x
- pixels 203520 x 92928, 1 x 1
18912706560

## Tiniest amount of artifact
Dguok_A_6275_Con_OV_01.ndpi - 4.30 Gb, img ID 393083, imported 2023-02-02, 40x
- pixels 234240 x 81664, 1 x 1
19128975360

# Zooms??
Ok while drafting an image.sc post I realized I could probably link some images.jax.org links instead of zenodo, but ran into something else weird. 
https://omeroweb.jax.org/iviewer/?images=380540 (UT-03) does zoom into 40x+ resolution, while https://omeroweb.jax.org/iviewer/?images=380546 (UT-05) does not
and
https://omeroweb.jax.org/iviewer/?images=380525 (OV-03) does zoom into 40x+ resolution, while https://omeroweb.jax.org/iviewer/?images=380519 (OV-01) does not
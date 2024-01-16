# Omero extensions
Neither compatible with 0.5.0rc

## qupath-omero-extension
Downloads on jpeg thumbnails to machine, so usable for annotations but not for pixel measurements

## qupath-extension-biop-omero
Downloads actual pixels, so much slower but usable for analyses needing pixels

# QuPath analysis
- Pixels are not touched, just read - so copying an image in qupath doesn't duplicate pixels on disk
- Annotations (editable, large, few) vs detections (uneditable, faster for many single cells, non-overlapping)
- Best to train classifieres with line rather than region - fewer pixels with more variability

## Stardist extension
Is just a script, requires user to go into script to copy in path, which took a while for people not comfortable with scripting

# apple silicone
New beta version of QuPath runs directly on arm64 instead of emulation - rumor that StarDist works on that version even though it doesn't work on the 0.5.0 emulation version (because of opencv issues?)


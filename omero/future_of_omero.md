# Meeting 6/13/2023 "Future of OMERO"

- BioFormats
    - Does it read from object-storage natively? Can use JAVA IO layers like Eric doing for conversions
    - Maybe OMERO shouldn't use bioformats for files it doesn't have to i.e. ome-tiffs and zarrs
        - they aren't trying to develop bioformats anymore?

- We want something that can read directly from object storage or at least is performant with all file types - then we could convert all public data to zarrs

- Redis for caching? - thumbnails, so never have to go to disk 
    - Glencoe is doing this
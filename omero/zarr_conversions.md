# bf2raw

- Right set of flags to use?
    - Size of chunks - if want visualization, 1Mb; analysis, 4Mb?
    - Blosk? larger chunks
    - Method of downsampling?
        - cubic, gaussian, nn


# GCS
- compute instance - bowser
- docker container?
- does google have equivalent of AWS Batch? - run containerized program on list of data

# Script
- talk to omero, get path, update omero with new path
- uses NIO library
- gcsfuse lets gcs paths get handled?
- can take s3 options? difficulty is google doesn't take string options, libraries needs to be correct type
    - kludge patch pr?
- priority
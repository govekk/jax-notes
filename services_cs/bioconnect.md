# Links
https://bioconnect.jax.org/portal-home

https://docs.bioconnect.jax.org/platform/background/

# Notes

## Technical notes
- Angular app
- "Backends for frontends"
- all in Azure

## Data notes
- ISA data model
- Data files currently stored in JAX Box (not ideal)
    - Currently require permissions in box to access data (likewise might link to Omero data limited to group)
- Lots of data entry so far has been pasted from excel file
- 60% of Cube studies in Bioconnect, full time curators adding data
- Can record data transformations between input and output data files
- Can export RO-Crates from OMERO (cli-transfer), "data package" exports RO-Crates from Bioconnect

# Modules
Expression - violin plots of gene expression per tissue for a population

Search - text browse/search or feature-based radio buttons

Identity Service - unique identity search by metadata (many duplicate / uncurated search terms e.g. sex/tissue/health)
- IDs for mice, diff IDs for diff services

Admin modules: Curation, Annotation, Files, Metadata

# Data pipeline
- L7 completes study -> something? -> input into Bioconnect
    - Connection between iLab and L7??

- There are multipe L7 instances (essentially one per scientific service)

# With Omero
- Lots of Cube data is in Omero, so as a test today we could do an example curation between Bioconnect and Omero for a Cube dataset
- Could just have a query that looks for Climb ID and gets back jpeg, image links, etc
    - Climb is mouse colony management - not unique per image

# Future goals
- Omero goals for future 
    - JupyterHub running in cloud to link to public omero data
    - Run tiny Vizarr in cloud that can point to our zarrs
    - IDR proof of concept to sell to higher ups to get budget for graphic designer
        - Bioconnect hired an awesome UX designer a while ago
    - Omero doesn't hold sample metadata, just points to L7 - don't need to replicate metadata

- Bioconnect goal for future is to connect to glue/jupyter (glupyter) on front end
    - Also provide external access

- L7 import into OMERO - button in L7 by images that imports to OMERO with all metadata + L7 sample id

Mutable data?

Glue starting work with Zarr/Dask: https://glue-genes.readthedocs.io/en/stable/

# Other people
- Sara Davis UX designer
- Matthew Gerring on cloud images project?
    - very large images access quickly through cloud
    - has private and public data
    - algorithms for cropping, fibrenuclei count
    - Google buckets for data management
    - Matthew's group is "scaled data engineering" - big data pipelines


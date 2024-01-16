Notes also in Purple work notebook

# Other events

image.sc live Jan 2024 18-19th

GloBIAS wants to start running yearly exchange of experience in maybe 2025

# Communities

Comulis: correlated multimodal imaging in life sciences
https://www.comulis.eu/

# Training resources

https://bioimagebook.github.io/ - Training on Fiji by Pete Bankhead's group

https://github.com/BioImageTools/ome-zarr-image-analysis-nextflow

https://github.com/NEUBIAS/training-resources

https://www.bioimagingguide.org/welcome.html 

# Misc notes about software

Airflow industry pipeline workflows on kubernetes clusters
scivision.substack.com - Alan Turing institute - datasets and models

K3s instead of k8s and kubeadm??
Calico over weave for networking
Webknossos is using kubeadm

# Talks

## Natasa Sladoje (Uppsala University): "Automated alignment of multimodal images: To (deep) learn - or not?
- elastix
    - Medical image registration library based on similarity measure optimization
    - Allows for multiple diff distance metrics
- CLEM-reg / ec-CLEM
    - Landmark-based registration for CLEM (correlative light and electron microscopy)
- VoxelMorph
    - deep learning based image registration

- Natasa's group tried using generative AI (Cycle Gan) to create "monomodal" problem (e.g. convert brightfield image to H&E-looking to register with H&E)
    - Cycle Gan does style transfer between domains (e.g. make horse video look like a zebra)
        - "Double U-net Cycle Gan for 3D MRI" - paper by diff group
    - sped up computation of Mutual Information using Fourier domain

## Lucas von Chamier (MDC): "Style transfer and artifact-free stitching for generative AI"
- Cycle GAN can only be run on small tiles, so requires stitching
- artifact-free stitching on u-net
    - normally u-net uses no padding, so loses pixels in convolution
    - both no (valid) padding and (same) padding lead to stitching artifacts
    - other group published artifact free stitching: use no (valid) padding, output of model must be cropped specifically, and must use same normalization for all patches

## Matouš Elphick (Francis Crick Institute): "CLEM-Reg: An automated point cloud based registration algorithm"
- vCLEM: correlative light and (volume) electron microscopy
    - light: fluorescence - any, but usually confocal?
        - can have: live imaging, functional labelling
    - electron: EM
        - can have: high resolution and context
- CLEM-reg steps
    1. mitochondria segmentation in both modalities
        - MitoNet in EM
    2. point cloud sampling and registration
        - sample points around membrane of mitochondria
        - coherent point cloud drift algorithm
- point cloud alignment (arxiv 2009)
    - coherent point drift
    - doesn't require same number of points per cloud
    - global similarity
    - "probability density estimation problem"
        - one point set represents GMM centroids and the other represents data points sampled from those GMM
    - linear complexity fast version

## Emil Rozbicki (Glencoe): "OMERO Plus for Data Management and Image Analysis at Scale"
(one of the company tech talks)
- OMERO segmentation connector
    - serverside utility that can run cellprofiler, cellpose, and stardist or custom models to be deployed on HPC or cloud batch
        - intensity and morphometric measurements
- Pageant
    - object-level "data mining" for spatial biology
- omero2pandas - omero.tables to pandas
- how store segmentations in OMERO? (I asked)
    - file annotation mask and omero.tables with polygon points - not ROI objects
        - not limiting number of points in polygon
- Did they say something about "Glencoe's bioformats"??

## Analle Abuammar (University of Cambridge): "segmentation and organelle contact analysis from Volume Electron Microscopy data"
- vEM: 3D EM (via 2D stack registration)
- segmentation techniques
    - Panoptic Deep-Lab architecture (unsupervised learning)
    - MitoNet (supervised learning)

## Priya Lakshmi Narayanan (Institute of Cancer Research): "BVSegNet: Enhancing Blood Vessel Segmentation with Attention-Based Distance Regularization"
- in training data, watch out for "anything but marked" negative classes - might overfit negative if too much negative data compared to positive
- normalized distance mask helps w/ variability of shapes in seg

## Pete Bankhead (University of Edinburgh): "Why I use QuPath for lots of things (but not for everything)"
QuPath
- Pros: 
    - streamlined for common tasks
    - brush tool and auto segmentation are zoom dependent
    - scripts and extensions - pytorch models
- Cons:
    - less general purpose than Fiji/python
    - 2D only
    - inability to modify pixels
        - e.g. on style transfer of guinea pig, had to open ImageJ window to view because can't change pixels in QuPath
- QuPath analyses are **object based**
    - pixels mostly used first to segment
    - segmentations can be hierarchical and are saved with measurements, so pixels used less for analysis
- bioimagebook.github.io
- Future features
    - 0.5.0: context help window, new log viewer
    - future: new script editor, better cell segmentation (Instanseg), better automated threshold methods
- Python + QuPath: QuPath Py4J extension and QuBAlab
    - Py4J help link up Python and QuPath from QuPath side (maybe coming soon?)
    - Paquo (python library to work with QuPath)

## Tong Li (Wellcome Sanger Institute): "WebAtlas pipeline for integrated single cell and spatial transcriptomic data"
- spatial transcriptomics types:
    - imaging-based: true single-cell resolution, 100-1000 genes in panel
    - sequencing-based: not usually true single-cell resolution, whole transcriptome-wide sequencing
- easy-to-use platform
    - should be browser based
    - most single-cell and spatial data formats are not "web-friendly"
- Spatial Data format
    - scalable zarr formats
    - labels (ngff), image (ngff), tables (anndata), points (anndata), polygons (geojson)
- Vitessce web portal example tools:
    - visium spots overlay on h&e
    - visium spots colored by celltype
    - umap colored by gene expression
- currently WebAtlas has CLI nextflow pipeline instructions for upload, but reviewers said "not user friendly enough" so building web portal

## Christian Tischer (EMBL Heidelberg): "Reproducible and Scalable Bioimage Analysis"
- use mattermost for communication (like slack) and gitlab for code sharing
- run everything on cluster (slurm, nextflow, conda, easybuild module load)
- **EMBL data management app - internal app that manages groupshare data better than Linux groups**
- analysis with gui uses kubernetes and jupyter - one dockerfile, users can edit to add things inside
- struggles with conda directly on hpc if user has any python in home folder - next steps to put this in container so more isolated
- project templates on gitlab
    - image.sc zulip bioimage analysis service project template
    - data access and naming schemes suggested in readme
    - workflows and automated batch commands provided
    - https://github.com/BioImageTools/ome-zarr-image-analysis-nextflow
- MoBIE - Java ImageJ plugin supported/inspired? by BigDataViewer
- https://github.com/NEUBIAS/training-resources
    - inspired by Carpentries but more modular - individual tools
    - ~37 modules, ~12 courses
        - same activity with dropdown option for steps for different tools
- BAND (Bioimage Analysis Desktop)
    - in-browser desktop
    - 20 people can use at once
    - can reserve for a week to run courses

## Kristina Ulicna (The Alan Turing Institute): "Quantifying cell cycle annotation uncertainty along single-cell trajectories with human-like intuition"
- VAE (variational auto-encoder) is self-supervised - it checks its work by comparing to reconstructed image
- dynamic programming: structural similarity between sequences

## Jordao Bragantini (Chan Zuckerberg Biohub): "ultrack: large-scale versatile cell tracking in Python under segmentation uncertainty"
- with time series images, might be useful to use tracking info across time in segmentation step, because some frames diff cells may be easier to segment
- using hierarchical segmentation
- combined separate segmetations on separate channels into multicolor contour map

## Martin Weigert (EPFL): "(Self)supervised learning for live-cell microscopy"
- e.g. detecting mitochondria division/fusion and turning on microscopy for data acquisition during that event time
    - so not wasting as much time/data for whole time series if events are infrequent
- permutation equivalent classifier: if flip input of classifier, flips output in same way (not guaranteed)
- spot detection
    - starfish, RS-FISH (radial symmetry) - classical LoG (laplacian of gaussian) blob detection - require manual thresholding
    - Spotipy - deep learning spot detection - Napari plugin - 

## Norman Rzepka (Scaleable Minds): "WEBKNOSSOS: AI-assisted, collaborative annotation and sharing of large 3D images"
- visualize 3D images - axes plots that scale/zoom together - often neuron volumetrics examples
- quick select based on segment anything
- new features this year:
    - time series, multimodal registration (affine transformations), segment statistics, automated segmentation of neurons in EM w/ proofreading tools
- next year:
    - automated registration tools
- python library API
- based on Zarr

## Ana Stojiljkovic (University of Bern): "napari-morphodynamics: quantification of cellular dynamics made easy"
- napari-morphodynamics
    - cellular dynamics quantification
    - any segmentation: stardist, cellprofiler, ilastik (random forest) - they really like ilastik
- convpaint (convolutional paint) - trainable classification based on drawn labels
- napari-roidynamics generalizes to more shapes

## Daniel Krentzel (Institut Pasteur): "Deep learning identifies antibiotic mode of action from label-free high-throughput images"
- brightfield images sufficient to determin antibiotic mode of action phenotype

## Sreenivas Bhattiprolu (Zeiss Microscopy USA): From Machine Learning to Deep Learning: A Case for Reproducibility in Microscopy Image Analysis
- Cellpose is trained on divers datasets so that it works with various modalities and cell types - this means it might not work as precisely as something trained specifically for a problem, but remotes time needed to train own model
- ML is compute features and classify on features, DL is learn features from pixels via downsampling and convolution

## Angelica Aviles Rivero (University of Cambridge): "From variational modelling to deep learning for biomedical imaging"
- we are in an era of multimodal data and minimal learning (labelled data is hard to obtain)
- semi-supervised learning performs better than supervised w/ less labelled data
- use hypergraph to represent imaging data, then can map non-imaging data onto same hypergraph to train together
- contrastive learning on a graph is perturbation on a graph (vs. on an image it's various transformations)

## Beth Cimini (Broad Institute): Making the most of your microscopy images
- Broad Imaging Platform tools: CellProfiler, Cellprofiler Analyst, PyCytominer, Piximi
- desktop vs web software
    - desktop involves challenging installation, local storage and compute
    - web is no installation, storage and compute can pull from other resources
- https://www.bioimagingguide.org/welcome.html
    - inspired by Pete's jupyterbook

## Panel
- part of project design should be what is minimum data/resolution you need? don't just go w/ max on the shiny tool - data bloat slows down research
- BioImageArchive - "AI ready" datasets with zarr and metadata
- Galaxy, BAND - web-based tools on public data
- image analysis infrastructure/platform/software as a service - need to move deeper into platform/infrastructure to make more accessible
- need to convince companies to want to support standardized formats
    - conversely standardized formats need to include all data/features/metadata coming off the microscopy, so need to engage companies in design and adoption
    - Quarep-Limi has been working with companies on metadata
- make tools more purpose and question drivin than atomic task and computation driven to facilitate usage and understanding
- filenames are not metadata! anyone can change them and you can't keep track
- software engineer for one of the microscope companies said they are watching ome-zarr, some current frustrations around hdf5 as a bottleneck?

# Posters

## OMERO-screen for comprehensive single-cell data analysis in fluorescence microscopy
- Pipeline: cellpose for cell segmentatio, integrating OMERO and Napari for data management and analysis
- Saving segmentations back to OMERO as file annotation mask, I think? 
- Simple, solid pipeline connecting OMERO with cellpose and napari, not really coded tools on top of that

## Advancing FAIR data sharing for multiplexed tissue imaging data from the Human Tumor Atlas Network
- met Adam Taylor - has Brian White's old job
- Minerva.im - pre-rendered jpeg tiles
    - developed by LSP: https://www.minerva.im/overview/#what-is-minerva
- image data is hosted on object storage, but not viewed off of object storage 
    - this is just for data exploration before researchers can download and analyse, so all that is viewed online is thumbnails

## An AI-ready BioImageArchive
- converting to zarr from cold object storage?
    - asked how doing that, because bioformats usually used to convert and it can't read off object storage yet
    - Aybuke Kupcu Yoldas offered to ask her colleague what they are doing for this

## Fractal
https://fractal-analytics-platform.github.io/

# Other companies
PhaseFocus - LiveCyte: single cell tracking on a 96 well plate
Evident Scientific - FV4000 confocal with fancy detector for low noise and linear relationship between signal and photons
FocalPlane
    - blog-based community site for microscopists
    - in collab with MicroscopyDB and microlist
Nikon - AI analysis software tools
Thermo Fisher - Amira analysis software - uses bioformats and stardist, 3D visualizations
Luxendo Bruker
    - image processor for light sheet microscope images (company provides light shee microscopes)
    - open file formats: Luxendo-Image, Fiji, BigDataViewer, Imaris, BigTiff
    - headless mode (python, cli)
Zeiss
    - Zen / Intellesis / Arivis AI toolkit
        - supports non-zeiss formats
        - cellpose
        - plugin to port segmentations from arivis to napari
    - CZI shrink (bioformats >= 6.8 works with compressed CZI)
Bitplane - Imaris - this is what microscopy core has on their machines
Sartorius - incucyte live cell imaging - experiment scheduling in protected incubator - analysis "real time"?
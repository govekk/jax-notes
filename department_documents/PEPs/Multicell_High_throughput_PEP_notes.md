# Overview

Multicell High Throughput System

Cellular physiology and screening - contractility and membrane potential analyses

# CytoCypher?

# Multicell High Throughput System
https://www.ionoptix.com/products/systems/multicell-high-throughput-system/

- Microscope that measures hundreds of myocytes per hour
    - tabular info position, size, focus, orientation
    - automated cell finding image analysis
    - uses IonWizard equipment
    - process: put plate in microscope, open program, manually or automatically find cells & bring into focus, run automated measurements (which can be customized), save file
        - is file just measurements? 
    - video trace measurements?
    - motion measurements: reference frame captured, changes in pixle intensity for other frames compared
    - Thumbnails used for cell list in program, but not saved?
    - Some amount of tmp data saved because can open previous cell list (w/o thumbnails)

I think it requires a seaparate computer? Where are they running IonWizard? - Interface and acquisition core?
    - IAC100DM -- Interface and acquisition core - includes system interface with FPGA chip and acquisition-configured workstation with I/O card

# IonWizard
https://ionoptix.wpenginepowered.com/wp-content/uploads/2019/09/IonWizard-7_4_2_159.pdf

System requirements
- Requires 32-bit Windows for acquisition functions, 32 or 64bit for core and analysis

Data
- .zpt (Zone Plot file)
- Each data set is accessed through a single root file which
may or may not have associated auxiliary files (images, etc.).
    - "it is a best (though not necessary) to close the current experiment in order to free up memory. Effects of this are most noticeable if the traces are very long or have lots of associated images. "
- Types of data in dataset:
    - Hierarchical Data: Data that is organized and accessed in a hierarchical structure. This includes
trace arrays, image arrays, constant values and image points.
    - Marks: Transient and Event marks that indicate points of interest within time resolved
data.
     -Notes: Free form notes and other descriptive text associated with the data set.
    - Zones: Information regarding regions of interest and sub-regions of interest as related to
image data.
- May be dependent on whether they are setting up an imaging system? "Users who have imaging systems have access to additional image-specific functions described in the next chapter"
    - trace data is EM/Ca
- Export image arrays as figures to jpg/pdf/tiff, export img array binary pixels as proprietary format (.ixp??)

# Software
```
1 IONWIZ -- IonWizard Core + Analysis
1 CYTMOT -- CytoMotion Pixel Intensity and Correlation Acquisition add-on
1 SARACQ -- SarcLenTM Sarcomere Length Measurement add-on
1 EDGACQ -- SoftEdgeTM Edge-Detection add-on
1 PMTACQ -- PMT Acquisition add-on
1 CYCHTA -- CytoCypher CytoSolver Batch Analysis (includes 1 year subscription)
1 CYCCFI -- CytoCypher Mark and Find Automated Cell Finder 1 MULACQ -- MultiCell Acquisition add-on for IonWizard
```

# Take aways
IonWizard is technically capable of saving images in a proprietary data format, but unsure if it will do this with multicell

No suggestion of external network information needed to run software packages

A lot of the software module pages on their website are missing / redirect to main page - new system??
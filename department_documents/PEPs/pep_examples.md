# Online file link
https://jacksonlaboratory-my.sharepoint.com/:w:/g/personal/dave_mellert_jax_org/EUstZ1dO0a1IjwqzObKVxRABJvjbJX8pKMo78S3S9Of_RA?e=4%3ACrWRwo&at=9&CID=a240eca3-170d-e112-c444-3efbd93c3234

# Smaller PEPs

This is a standalone, Android tablet-based microscope with no specified network interface. The volume of image data is expected to be low (sub 100GB/year), and the specified mechanism for data  export  is via  USB.  Images  are  exported  in  common  formats  (TIFF,  JPEG), although  it  is  not clear as to how instrument metadata is exported with the images, if such a thing is possible. This device will have minimal IT impact.

## Visium CytAssist
Visium CytAssist is standalone instrument for automated spatial transcriptomics. Internet connection is optional for firmware updates, but not required. Each run produces 10s of mb of Run Data consisting of two brightfield images in the well-supported TIFF format along with a CSV file of metadata. Additionally, each run produces 100s of mb of Support Data including instrument sensor information. The instrument stores up to 100 runs of Run Data, which is not deleted automatically, and 10 runs of Support Data, which is deleted automatically. Data can be exported from the machine via USB. This device will have minimal IT impact.

# Larger PEPs

## Chromium Connect
"The Chromium Connect system is expected to increase service throughput, which will have a modest impact on data generation by the service that can largely be handled by rigorous data management. The Xenium system, on the other hand, has the potential to create large amounts of data, which we anticipate could require 50-100T of storage over the current service allocation to provide a buffer for data processing and delivery. Data processing for the Xenium will take place on the instrument workstation and the Elion cluster. The Single Cell service should work with Research IT to scope and acquire the additional storage in anticipation for the new equipment and to determine if compute needs will be adequately covered by current resources. Any associated workstations for running these instruments and/or processing data will need to be configured to JAX IT security standards to access the JAX network."

## NeuroNexus SmartBox
"This device is used to generate electrophysiological data from biological tissue. It can capture a large amount of data if being used at full capacity (multiple TB of data per day) and it does not provide local storage. While this might not represent the actual usage of the instrument, storage solutions will need to accommodate bursts of data generation in this order of magnitude. Data transfer uses USB 2.0 to a laptop or workstation and data will need to be moved from that computer regularly.  (we eventually estimated total data usage on this equipment to be 2-5TB/year)
 
The instrument generates files that store data in raw binary format with a header specified in documentation. The vendor only provides a reader using MATLAB, so readers for e.g. Python or R would need to be developed internally.  (the MATLAB reader is about 100 lines of fairly straightforward code, so this is not a big software project)
 
The laptop provided by the vendor comes with Windows 7 pre-installed and would need to be configured to JAX standards for network access. "

## Hamilton Microlab Vantage
"The  Hamilton Microlab  Vantage  instrument may  require  standard  instrumentation  networking. The Harmony software license will require the purchase of a workstation, which, along with any computer associated with the Hamilton, will be configured to the JAX standards for accessing the JAX instrumentation network. While these impacts on IT resources will themselves be minor, they do  represent  a  scale-up  of  usage  of  an  existing  instrument,  the  Opera  Phenix  high-content screening  platform.  Thus  we  expect  that,  as  the  Phenix  moves  from  testing  to  production concordant  with  the  use  of  the  Hamilton  Liquid  Handling  System  and  the  additional  Harmony license, the data storage and analysis needswill increase significantly.Single Cell Biology has engaged Research IT to plan for the scale-up of the Phenix platform and has  created  a  preliminary  data  management  plan  that  includes  additional  capacity  for  data storage  and  compute.  The  hardware  and  software  required  to  support  this  plan,  summarized below, should be bundled with this PEP.

Columbus software, hardware, and associated storage to manage high-volume image data:
Columbus server + data transfer node --$40,000
Columbus software --$60,000
Isilon H500 storage – $275,000.00 (After further assessment, updated 122118)
Additional workstations and license to accommodate multiple concurrent users performing local analysis:
2 workstations for Harmony software --$10,000 ($5K each)
Additional Harmony software license --$15,000
Total capital  IT  resources  needed  to  support this  combined  PEP: $$400,000.00 "
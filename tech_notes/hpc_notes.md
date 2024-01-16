# Overview
https://jacksonlaboratory.sharepoint.com/sites/ResearchIT/SitePages/HPC-Overview.aspx

Sumner = general purpose

Winter = GPU

Elion = Scientific Services (GT, Compsci, Protein Sciences, Singlecell, GRS, etc)

# Login

`ssh login.sumner.jax.org`

`ssh -X login.sumner.jax.org` to connect X11


# Request interactive session

`srun -q batch --time=1:00:00 --mem=32GB --cpus-per-task=8 --x11 --pty bash -i`
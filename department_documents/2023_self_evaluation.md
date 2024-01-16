# Goals

## conference

Attended MIA QBI 2023 virtually, Northeast Bioimage Analysis meeting (hosted by JAX), and CBIAS 2023.

Completed 11/21/2023
Meets expectations

## codebase

Set up local testing environments for both ezomero and omero-cli-transfer. Submitted PRs to ezomero for ROI and testing bug fixes and documentation updates. Submitted PR to omero-cli-transfer for docker compose syntax update.

Completed 11/16/2023
Meets expectations

I have not yet reviewed larger core function-related pull requests, but I have familiarized myself enough with the codebases to be comfortable with testing and submitting small bug fixes and documentation updates, which is a good foundation for more serious development or reviewing should the need arise.

## harmonize

These probably should have been separate goals, so I'll address individually:
1. Build a containerized option for on-prem OMERO instance: Proof-of-concept kubernetes templates for OMERO both on cloud and on-prem/local were tested extensively are published at omero-k8s-tem-plates (Sept 25, 2023). I've been running the local version in my sandbox linux v as a test server.
Building kubernetes infrastructure on the new on-prem servers is underway - I have set up a test minikube installation on one of the servers to troubleshoot networking and firewalls, but that is not a stable enough infrastructure for production.
2. Improve security and data backup of cloud-based OMERO: Transitioned to a private kubernetes cluster (not accessible outside VPC) and control plane authorized networks (cluster administration only from toad VM) (Jan 20, 2023). Attended a 4 day workshop on networking and security in Google Cloud, along with additional kubernetes GCP training (Nov 2023). I have performed a few backups of the filestore instances, and the database backs itself up, but a new method of backup needs to be decided for the object storage (see below).
3. Test proof-of-concept OMERO deployments backed by object storage: Tested and deployed into production a public cloud OMERO running with data on object storage via gesfuse. Performed extensive performance testing between two mount options (gcsfuse and goofys) with both normal image file formats and zarr. Updated kubernetes templates on jax-gcp-omero and omero-k8s-templates with option to mount object storage via gesfuse. This has been running as production on images.jax.org since July 31, 2023.

Completed 9/25/2023
exceeds expectations

This is hard to rate given how each task is at a different stage of completion, but I'll average it out to exceeds because I'm proud of the work I put into the object storage OMERO and that it's been running in production for a few months now. My work on building kubernetes templates for cloud and on-prem hopefully means that troubleshooting setting up the containerized on-prem will go more smoothly, but there is still lingering work to set up actual kubernetes infrastructure and harmonize with the current on-prem deployment. I hope to set up logging and monitoring alerts on the cloud-based OMERO next year, after completing the related GCP training this year.

## images portal

This goal relied on external contractors first developing a working model of an images portal, which hasn't progressed very far this year.

Not applicable

## bioconnect

Met with BioConnect (Jake Emerson) and L7 (Janet Bakeman) in April 2023 to discuss connections between the different services. As was discussed then and since, at most from our side this will be adding an extra map annotation to link to L7 IDs, and/or helping set up embedded image viewers to explore OMERO image data in BioConnect. Any further work on this requires BioConnect to be a more established link, which we may be waiting on the Data Science Initiative to push.

Completed 4/27/2023
Meets expectations

## workshops

Joined Erick in running the OMERO workshop in March, then ran again by myself in November. Attended Carpentries instructor training to gain teaching skills and ideas in data and programming-related lesson topics.

Completed 10/31/2023
Meets expectations

## meet with researchers

Met with 7 PIs in Bar Harbor in April, would have liked to meet with more in Farmington this October but the RIT retreat and NE-BIA meeting very much limited that time. I did manage to sneak in a meeting with Bill Flynn from single cell though, and I feel we were able to help him troubleshoot a few of his image processing hangups as a team. OMERO continues to not be the *greatest* solution for single-cell image data. I did code a pipeline to run CellProfiler in Python on OMERO images and save the ROIs back to OMERO (github.com/TheJacksonLaboratory/CP-to-OMERO), but I have since had discussions with people at CBIAS that suggest the way to store single-cell data in OMERO may be more along the lines of a mask image and tables. I'll also add learning a bit about connecting QuPath with OMERO to this goal, since most of our work related to advising researchers is knowing different tools that might fit their needs and work in our ecosystem.

Completed Oct 19, 2023
Meets expectations

I think this goal ended up being more about learning than advising - I'm continually learning more about the imaging landscape at JAX and more generally popular open-source image tools. But that's definitely a good start. 

## add a goal? Maintain and troubleshoot OMERO production instances and assist users with access, import, and data transfer.

Based on tickets in ServiceNow, I added 50 new users to OMERO, created 19 new microscopy delivery folders, and transferred about 20 datasets or projects to the public OMERO instance. Each week I checked the OMERO import folders for unsuccessful logs and emailed users to troubleshoot. We updated the OMERO instance versions for dev, prod, and public once in August. Various other small customization projects or debugging has come up along the way: troubleshooting large NDPI issues and pyramid generation, fixing a figure label export bug, changing ownership on all Korstanjelab projects in OMERO, and creating an OMERO ROI overlay export script for pathology. 

Completed 12/31/2023
Excels? exceeds?

This ended up being maybe a third of my daily work this year. It's rewarding work for me, and I think I do well interfacing with users. I like troubleshooting the OMERO quirks/bugs that users bring to us, and I think that more users are starting to recognize me as a point of contact for OMERO questions or issues.

## Strengths + achievements
This year, my biggest successes have been implementing the public OMERO instance to read from object storage and general user support on all OMERO instances. The public OMERO project built on my existing skills in developing containers to become comfortable with Kubernetes and use it to deploy application servers on cloud. I am now pretty proficient with Kubernetes commands, GCP infrastructure, and OMERO server administration. I have gained a lot of familiarity with the commands, files, and system requirements of running an OMERO server and the data structures and design of OMERO and related clients/packages (ezomero, OMERO web). This familiarity is useful both in development of the OMERO environment and also administration on the user side. For example, troubleshooting pyramid generation issues when transferring data to the public instance required understanding pyramid file formats, some drawbacks of OMERO itself and our read-only implementation, and the server file structure of OMERO including log files and pyramid data, none of which I knew prior to this year. In terms of user support, I would say I am conscientious about maintaining the systems, clear in my communications, and happy to work through issues with users.

## Opportunities
I would like to put more direct effort into some of the learning and networking I've started this year to have a broader knowledge of tools and the people in the bioimage analysis community. One way to do so would be to spend more time reading posts on image.sc and being more willing to post either my own questions or advice. I am inspired by Peter's curiosity to play with and break new tools, so I'd like to foster more of that in myself. This would also apply to breaking and testing the tools or environments that I'm building. One of the bigger things I would like to work on next year would be better time management to balance between small tasks (that are usually motivated by users), larger projects (that are usually motivated by team / goals), and nice-to-have projects (that are usually motivated by myself) - this might involve breaking apart larger projects more into achievable tasks and reserving time to work on specific projects.


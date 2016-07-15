# Workflow example

In this last section, we want to put everything together and look at how Docker containers, common interfaces for distributed computing infrastructure, automated provenance data for files and jobs, notifications, and even Jupyter Notebooks can work together to make a collaborative and reproducible environment for science.  This is about synthesizing the information, working together, and solving problems, we will only lay out the objectives and the measures of success.  How you get there step by step is up to you.

## Objectives

Perform a two-step analysis using a Docker container version of *Muscle 3.8.31* and *RaxML 8.2.3*

Step 1: Muscle

* Muscle executable - http://www.drive5.com/muscle/downloads3.8.31/muscle3.8.31_i86linux64.tar.gz
* Dockerize the container.  It must accept a fasta file and output an interleaved alignment file
* The "sequence type" parameter should be DNA
* Sample Data - agave://data.iplantcollaborative.org/shared/iplant_training/de_walkthrough/DE_sample_plants.fas
* Really only one person/group needs to dockerize the app, one person/group needs to make the Agave App, and one person/group needs to run the test job.  In fact it's probably best to make sure that not everyone is running the first job at the same time.
* Whoever creates the Agave app should share it with others, and whoever runs the job can share that with others too.

Step 2: RaxML

* An Agave app named RAxML-small-8.2.3u1 already exists and runs on a supercomputer.  We are free to use it if we can create a job submission script for it.
* Sample Data - /iplant/home/shared/iplant_training/de_walkthrough/phylip_interleaved.aln (this can be used if there is an issue with Step 1 for some reason)
* the command ```jobs-template``` will let you create a job submission script for an app that you specify.  Notice the -a parameter causes the job template to contain all the optional fields.
* The output of this job should be a Newick tree.
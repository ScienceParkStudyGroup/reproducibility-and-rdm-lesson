---
title: "Analysing Data"
teaching: 30
exercises: 30
questions:
- "What happens to my data when I start analysing them?"
- "What is the difference between raw and processed data?"
- "How can I ensure the traceability of my results in regards to raw and processed data?"
- "What is a scientific workflow manager?"
objectives:
- "Find out where to process large datasets and store research-data."
- "Find out how to store data to be archived."
- "How can I analyse a dataset using an HPC environment?"
keypoints:
- "A workflow (e.g. an R script) link raw to processed data."
- "A workflow is essential to keeping trace of the steps that a dataset underwent."

---

## Table of Contents
1. [Overview](#overview)
2. [What you will learn](#what-you-will-learn)
3. [Tools To Connect](#tools-to-connect)
4. [Connecting](#connecting)
5. [Transferring Data](#transferring-data)
6. [Analysing Data/Running a Job](#analysing-data/running-a-job)


## What you will learn

1. **Where to process large datasets and store research-data** 
    - How does the current high performance compute environment (HPCe) job system work?
    
2. **Where to store data to be archived**
    - What is a tape archiving system?
    - How can I store my data on tape?

## Tools To Connect

The tools used for uploading/accessing/transferring are common for both data processing and managing archive data. 



Some differ depending on operating systems.

#### _Windows_
  - _SSH client_ [Mobaxterm](https://mobaxterm.mobatek.net/download.html) is recommended if you don't already have a preferred method.
  - To use a windows 10 machine as if it were linux: _Windows subsystem for linux (WSL)_ - details on setting this up can be found [here](https://www.windowscentral.com/install-windows-subsystem-linux-windows-10). _THIS IS NOT A NECESSARY STEP_. The above can be used instead.




#### _MacOS/Linux_
  - _Terminal_ is built in for both Mac and Linux machines. Can be used as an ssh client.



Files can also be transferred across the data processing/archiving systems using GUI filemanagers like [Cyberduck](https://cyberduck.io/).



## Connecting 
### Using command line
Open up preferred method for ssh connection and type the following:

_For Genseq_
~~~
ssh -X USER@genseq-cn02.science.uva.nl
~~~
{: .language-shell}

_For Crunchomics_
~~~
ssh -X USER@omics-h0.science.uva.nl
~~~
{: .language-shell}

_For SURF Data Archive_
~~~
ssh -X USER@archive.surfsara.nl
~~~
{: .language-shell}



### Using a GUI
Cyberduck is the recommended GUI for file transfer with the Data Archive. See the [Cyberduck website](https://cyberduck.io/) for more information. Or for some [tips from SURF](https://userinfo.surfsara.nl/systems/shared/archiving-high-performance).



## Transferring data
### Using command line



### Using a GUI



When using the GUI for file transfer between the servers be sure to open two separate windows with separate connections.


## Analysing Data/Running a Job
The transition from Genseq to Crunchomics means a more regulated compute environment. This is beneficial to everyone as jobs are processed with more constraints on time and computing power. Therefore, you need to estimate how long and how many nodes need to be used for your job. 
Crunchomics uses Slurm - "Simple Linux Utility for Resource Management".

### Slurm
Slurm is a resource manager and job scheduler. Slurm is used to allocate resources within the cluster. It has built in knowledge about nodes/sockets/cores/hyperthreads (network topology).

Slurm is used to launch and manage jobs.When there is more work than resources Slurm will consider the following:
  - Network topology
  - Fair share scheduling
  - Advanced reservations
  
More in depth information can be found [here](https://slurm.schedmd.com/documentation.html).




### _Key points to run a job:_


You typically need a job file which essentially is a list of the (shell) commands you want to run. This file is put in the queue, if there is already sufficient resources to run it then it will start right away. 

#### Example Job File:


~~~
#!/bin/bash
#
#SBATCH --ntasks=1
#SBATCH --time=10:00
#SBATCH --mem-per-cpu=100
#

command -options -file -arguments -etc.
~~~
{: .language-shell}




This file should have the file extension .sh

To submit the job use the command:

~~~
sbatch jobfile.sh
~~~
{: .language-shell}

Importantly you should calculate an estimation of how long your job will take on a certain number of nodes. This needs to be included in your jobfile. The above example specifies one CPU for 10 minutes and 100MB of RAM. 

This is of course a very small amount of processing power and time. For the moment there are no limits set by Crunchomics on power and time, apart from the limits of the system. These are: 64 CPUS (or cores) and 512GB of memory. However it is recommended to use a maximum of 8 cores for your jobs. This is because not all parallel processes scale linearly. For example: alignment using hisat2 on 32 cores is not 4 times faster than on 8 cores. The less cores you specify for your job means that it is likely to start quicker - more likely that 8 cores will be freed up than 32!



For step by step tutorials on job scheduling see [here](https://hpc-carpentry.github.io/hpc-intro/13-scheduler/index.html) and  [here](https://support.ceci-hpc.be/doc/_contents/QuickStart/SubmittingJobs/SlurmTutorial.html).


### Slurm and Snakemake
Snakemake is a commonly used language for data analysis pipelines. If you have pre-existing pipelines made for a non-slurm system (i.e. Genseq) these can still be run using the snakemake command and no adaptations to the original files are necessary. What you need to do is create an additional json file specifying the requirements for each rule.

To run the pipeline it is still necessary to activate the required python environment.





## References

https://slurm.schedmd.com/faq.html#steps



### Teaching materials
This lesson was originally formatted according to the [Carpentries Foundation](https://carpentries.org/) lesson template and following their recommendations on how to teach researchers good practices in programming and data analysis.   

This material has been collected from the various links as well as communications with the Crunchomics team.

{% include links.md %}

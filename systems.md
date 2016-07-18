
# Managing systems
---

The Agave API provides a way to access and manage the data storage and compute resources you already use (or maybe the systems you want to use), but first you have to tell Agave where they are, how to login, and how to communicate with that system.  That is done by giving Agave a short JSON description for each system.  

## Storage systems

Storage systems tell Agave where data resides.  You can store files for running compute jobs, archive results, share files with collaborators, and maintain copies of your Agave apps on storage systems.  Agave supports lots of communication protocols and the permissions models that go along with them, so you can work privately, collaborate with individuals, or provide an open community resource.  It's up to you.  Here is an example of a simple data storage system accessed via SFTP:

```json
{
    "id": "stampede-storage-XSEDE16-${USERNAME}",
    "name": "TACC Stampede Storage (${USERNAME})",
    "description": "The Stampede supercomputer at TACC",
    "site": "tacc.xsede.org",
    "type": "STORAGE",
    "storage": {
        "host": "stampede.tacc.utexas.edu",
        "port": 22,
        "protocol": "SFTP",
        "rootDir": "/",
        "homeDir": "/home1/0002/${USERNAME}",
        "auth": {
            "username": "${USERNAME}",
            "password": "${PASSWORD}",
            "type": "PASSWORD"
        }
    }
}
```

---

**Note: When possible you should not use username/password to login!  Agave supports SSH Keys and several other better authentication mechanisms listed below.  For this training we use username and password since it requires no setup and these systems are temporary.**

---

Let's go through the key items one-by-one:
* **id** - this is the unique name that you will use with Agave to use your system.  It must be unique across the entire Agave tenant, so unless you are an admin creating public system, you should probably put your username somewhere in there. You shouldn't use spaces in the system ID.
* **name** - A human-readable name
* **type** - either STORAGE or EXECUTION.
* **site** - This field is just for logically grouping systems by the site where they are located.
* **rootDir** - If you specify an absolute path on this Agave system, it will always be relative to the rootDir that is set.  For private systems, it is commonly left as "/".  For public systems, it is commonly set to a subdirectory to hide the root level system directories and restrict what other users can see.
* **homeDir** - If you specify a relative path on this Agave system, it will always be relative to the homeDir that is set/
* **auth** - This block tells Agave how to login.  An Agave system always includes information on how to authenticate.
  * **type** - There are a number of options: APIKEYS, LOCAL, PAM, PASSWORD, SSHKEYS, or X509.  For this example, we are using PASSWORD, since it is the simplest and requires no setup, but setting up SSHKEYS (or one of the other methods if they fit) is better for a production context.  More info is here: http://agaveapi.co/documentation/tutorials/system-management-tutorial/#supported-data-and-authentication-protocols

### Hands-on

As a hands on exercise, register a data storage system that you use.  This could be a remote VM (like the Jetstream VM available for the workshop), a storage space on a cluster at a university, or something in the commercial cloud like an S3 bucket.  Other examples of storage systems are here: http://agaveapi.co/documentation/tutorials/system-management-tutorial/#storage-systems

---
## Execution systems

Execution systems in Agave are very similar to storage systems.  They just have additional information for how to launch jobs.  In this example, we are using a HCP system, so we have to give scheduler and queue information.

```json
{
    "id": "stampede-XSEDE16-${USERNAME}",
    "name": "TACC Stampede (${USERNAME})",
    "description": "The Stampede supercomputer at TACC",
    "site": "tacc.xsede.org",
    "public": false,
    "status": "UP",
    "type": "EXECUTION",
    "executionType": "HPC",
    "startupScript": "",
    "environment": null,
    "login": {
        "auth": {
            "username": "${USERNAME}",
            "password": "${PASSWORD}",
            "type": "PASSWORD"
        },
        "host": "stampede.tacc.utexas.edu",
        "port": 22,
        "protocol": "SSH"
    },
    "maxSystemJobs": 50,
    "maxSystemJobsPerUser": 50,
    "queues": [
        {
            "name": "development",
            "default": true,
            "maxJobs": 1,
            "maxUserJobs": 1,
            "maxNodes": 16,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "02:00:00"
        },
        {
            "name": "gpu-dev",
            "default": false,
            "maxJobs": 6,
            "maxUserJobs": 1,
            "maxNodes": 4,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "04:00:00"
        },
        {
            "name": "gpu",
            "default": false,
            "maxJobs": 12,
            "maxUserJobs": 4,
            "maxNodes": 32,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "24:00:00"
        },
        {
            "name": "normal",
            "default": false,
            "maxJobs": 12,
            "maxUserJobs": 4,
            "maxNodes": 256,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "48:00:00"
        },
        {
            "name": "serial",
            "default": false,
            "maxJobs": 16,
            "maxUserJobs": 4,
            "maxNodes": 1,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "12:00:00"
        },
        {
            "name": "normal-mic",
            "default": false,
            "maxJobs": 12,
            "maxUserJobs": 4,
            "maxNodes": 256,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN --reservation XSEDE_2016_4",
            "maxRequestedTime": "48:00:00"
        },
        {
            "name": "serial",
            "default": false,
            "maxJobs": 16,
            "maxUserJobs": 4,
            "maxNodes": 1,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "12:00:00"
        },
        {
            "name": "normal-2mic",
            "default": false,
            "maxJobs": 12,
            "maxUserJobs": 4,
            "maxNodes": 256,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "48:00:00"
        },
        {
            "name": "serial",
            "default": false,
            "maxJobs": 16,
            "maxUserJobs": 4,
            "maxNodes": 1,
            "maxProcessorsPerNode": 16,
            "maxMemoryPerNode": "32GB",
            "customDirectives": "-A TRAINING-OPEN",
            "maxRequestedTime": "12:00:00"
        },
        {
            "name": "largemem",
            "maxRequestedTime": "48:00:00",
            "default": false,
            "maxJobs": 4,
            "maxUserJobs": 2,
            "maxNodes": 2,
            "maxProcessorsPerNode": 32,
            "maxMemoryPerNode": "1000GB",
            "customDirectives": "-A TRAINING-OPEN"
        }
    ],
    "storage": {
        "host": "stampede.tacc.utexas.edu",
        "port": 22,
        "protocol": "SFTP",
        "rootDir": "/",
        "homeDir": "/home1/0002/${USERNAME}",
        "auth": {
            "username": "${USERNAME}",
            "password": "${PASSWORD}",
            "type": "PASSWORD"
        }
    },
    "workDir": "/work/0002/${USERNAME}"
}
```

We covered what some of these keywords are in the storage systems section.  Below is some commentary on the new fields:

* **executionType** - Either HPC, Condor, or CLI.  Specifies how jobs should go into the system. HPC and Condor will leverage a batch scheduler. CLI will fork processes.
* **scheduler** - For HPC or CONDOR systems, Agave is "scheduler aware" and can use most popular schedulers to launch jobs on the system.  This field can be LSF, LOADLEVELER, PBS, SGE, CONDOR, FORK, COBALT, TORQUE, MOAB, SLURM, UNKNOWN. The type of batch scheduler available on the system.
* **environment** - List of key-value pairs that will be added to the Linux shell environment prior to execution of any command.
* **scratchDir** - Whenever Agave runs a job, it uses a temporary directory to cache any app assets or job data it needs to run the job.  This job directory will exist under the "scratchDir" that you set.  The path in this field will be resolved relative to the rootDir value in the storage config if it begins with a "/", and relative to the system homeDir otherwise.

Complete reference information is located here: http://agaveapi.co/documentation/tutorials/system-management-tutorial/#execution-systems

### Hands-on

As a hands on exercise, register an execution system that you use using the ```systems-addupdate``` command.  This could be a remote VM, a cluster at a university, or something in the commercial cloud.  Other examples of execution systems are here: http://agaveapi.co/documentation/tutorials/system-management-tutorial/#supported-data-and-authentication-protocols

When you have a system description ready, register it with Agave like this:

```
systems-addupdate -F system-description.json
```

In this case, the ```-F``` argument is used to specify the system description file.  You don't have to memorize that.  For every CLI command, the ```-h``` argument will give you quick help information.  That's the main thing to memorize.  Try this:

```
systems-addupdate -h
```

### Exploring systems

Before we move on to data, take some time to look at the other systems-* commands.  In the bash shell of Jupyter, you can type ```systems-``` an then hit the Tab key twice to see what autocomplete options are available.  What arguments does ```systems-list``` accept?  Oh, and be careful with ```systems-delete```.  System IDs must be universally unique, so if you gave your system a special name that you love and then you delete it, that name is gone for good.

# Managing data

Data movement between systems can become tricky when you start dealing with different protocols (iRODS, ssh, etc.) and different login credentials for each system.  So let's jump in and look at just that use case. We have:

* one Docker container with our Jupyter Notebook (if you were developing locally, this would represent your own laptop) communicating via HTTPS that is using our Agave credentials through OAuth2
* Stampede at TACC communicating via SSH using a demo account
* and a public storage system using a community account on Corral, which is a multi-petabyte GPFS filesystem at TACC.

## Data movement

We haven't told Agave anything about our Jupyter Notebook instance, but we do have CLI tools installed (We actually have a Python library as well that can interact directly with the APIs, but we won't play with that today).  In this case, the "files-upload" command will take files stored locally and upload them to a system that Agave knows about.  Let's move our system description from the previous Hand-on example to the home directory of our shiny new Agave system.  Your file and system names will be different, but the command should look something like this:

```
files-upload -F path/to/system-description.json -S stampede-storage-XSEDE16-${USERNAME} ./
```

Can you see it on the system now?

```
files-list -S stampede-storage-XSEDE16-${USERNAME} ./
```

Let's make a copy under a different name?

```
files-copy -D ./system-copy.json -S stampede-storage-XSEDE16-${USERNAME} ./system-description.json
```

Or we can just move it.

```
files-move -D ./system-moved.json -S stampede-storage-XSEDE16-${USERNAME} ./system-description.json
```

We can bring in any publically accessible data from the web.  Just to be meta, let's import this very tutorial from GitHub:

```
files-import -U https://raw.githubusercontent.com/johnfonner/XSEDE16/master/systems.md -S stampede-storage-XSEDE16-${USERNAME} ./
```

And finally, we can move our own files between Agave systems by specifying the source with the "agave://" notation.  Try moving system.md from our Stampede system to the shared Corral system

```
files-import -U agave://storage.xsede16/demo.txt -S stampede-storage-XSEDE16-${USERNAME} /work/0002/${USERNAME}
```

## Hands-on

While this whole section has been hands-on, take a few minutes to move some data around and also explore the other files-* commands.

* Can you make a directory with one of the files- commands and move one of your files into it?
* what happens if you use the "-v" argument on any of these commands?  What about the "-V" command?

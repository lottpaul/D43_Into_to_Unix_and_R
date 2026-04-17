# Introduction to HIVE

The UCD HIVE is a high performance cluster (HPC) on the UC Davis Campus.  It consists of 77 servers and additional storage linked together to give members of campus access to high performance computing resources.  HIVE utilizes SLURM resource management to ensure everything runs smoothly and limit users to assigned resources and limits.

Things to remember when you're using HIVE

1.  It's a shared computing environment, so play nice with others
    1.  Don't ask for excessive amounts of resources or reservation times
2.  Keep your projects file structure organized and well documented
3.  You only have 20GB in your Home Directory
    1.  If you get any warning about not being able to write to your home directory, it's most likely that you've used up all of your Home Directory
    2.  Utilize /scratch drives for short-term local storage.

* * *

## Getting Logged into HIVE via OnDemand

The HIVE utilizes Open OnDemand to provide easy access to cluster resources through your browser.

**OnDemand GUI:** https://ondemand.hive.hpc.ucdavis.edu/

You'll use your UCDavis Username/Email and Password to login. Once, logged in you'll see the option to select one of the Pinned Apps.

![fd1bc393e9216a5c7e61d32cec0fe727.png](../_resources/fd1bc393e9216a5c7e61d32cec0fe727.png)

### OnDemand Apps:

1.  RStudio Server - An interactive enviroment that's ideal for data analysis using R
2.  Hive Desktop (Virtual Desktop) - a basic Linux desktop using XFCE - ideal for when you need a graphical interface
3.  Juptyer Lab - An interactive environment that's ideal for data analysis using Python or other languages.
4.  VSCode Server - A lightweight code-editor

By clicking on one of the Apps icons, you'll be taken to a form page for that app, where you'll be asked to select:  
   1. Account: *publicgrp*  
   2. Partition: *Low or High* - Use Low for faster access but you may get kicked off. Use High, if you don't want to get kicked off but are willing to wait  
   3. Number of Cores: *(1-8)* - Use 1-2 CPUs for this course  
   4. Amount of Memory: *(1-128GB)* - Use 1-8 GB for this course  
   5. Number of GPUs: 0  
   6. Type of GPUs: *empty*  
   7. Additional Modules: *empty*  
   8. Number of Hours: 2  
   9. Working Directory: /home/username  
   10. Would you like to recieve an email when session starts: Leave empty/unchecked  
   11. Save Settings: You can save the setting so you can quickly choose the settings the next time you login.  
   12. Press **Launch**

![a332112ab5892af2fe00d21ad59dff07.png](../_resources/a332112ab5892af2fe00d21ad59dff07.png)

Once you click Launch, you'll be take to the "My Interactive Sessions" Page where you will see a block for each OnDemand app session you are currently running.

At the bottom of the block is **Status text** "Please by patient as your job currently sits in queue...".

![9aa8f638d66d96d24dd8b12f154d8821.png](../_resources/9aa8f638d66d96d24dd8b12f154d8821.png)

Typically in less than a minute it'll change to a **Connect to ....** button. Press the button and it'll open a separate tabe for your App.

![c3c9de288a14d90367163069d9014b65.png](../_resources/c3c9de288a14d90367163069d9014b65.png)

If you need to cancel a currently running app, you can go back to the OnDemand **My Interactive Sessions** webpage where you'll see all your currently running apps and old completed apps. You can click on the red **Cancel** Button - to cancel a currently running job. Or **Delete** to delete an old job.

![338e860e78a0801fb7c5fa6c0e754c0f.png](../_resources/338e860e78a0801fb7c5fa6c0e754c0f.png)

### OnDemand Apps - Behind the Scenes

Behind the scenes, when you click **Launch** the OnDemand server is creating a Slurm Job request for you. Once your requested resources have been allocated, your application is started on the node where resources were available.

* * *

## Getting Logged Into Hive via SSH

**Hive SSH URL:** hive.hpc.ucdavis.edu

You can use your UCDavis Account Password or SSH Public Key to login to HIVE Cluster Head node.  
***Note:*** (Be certain to change `username` to your actual username and ignore the `$`, which just represents the command prompt)

```bash
$ ssh username@hive.hpc.ucdavis.edu

Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 6.5.0-45-generic x86_64)
 _   _   _   _
/ \_/ \_/ \_/ \             Welcome to Hive.
\_/ \_/ \_/ \_/
/ \_/  _   _ _              Documentation available here:
\_/ \ | | | (_)_   ____     https://docs.hpc.ucdavis.edu/
/ \_/ | |_| | \ \ / /_ \
\_/ \ |  _  | |\ V / __/    For more information:
/ \_/ |_| |_|_| \_/\___|    https://hpc.ucdavis.edu
\_/ \_  .hpc.ucdavis.edu
/ \_/ \_                    Need help? hpc-help@ucdavis.edu
\_/ \_/ \_/
/ \_/ \_/                   Maintenance changes:
\_/ \_/                     https://docs.hpc.ucdavis.edu/maintenance/december-2025/


Last login: Thu Feb 12 10:21:34 2026 from 128.120.136.214

***** Slurm resources available to you.
Default account: genome-center-grp
                             |   Account Limits   |  Limits Per User   |   Limits Per Job   |
Account            Partition | CPUs  MEM(G) GPUs  | CPUs  MEM(G) GPUs  | CPUs  MEM(G) GPUs  |
---------------------------------------------------------------------------------------------
genome-center-grp⁺ gpu-a100  | 128   2,000  8     | *     *      *     | *     *      *     | 
genome-center-grp⁺ high      | 616   9,856  0     | 64    1,024  0     | *     *      *     | 
publicgrp          high      | 128   2,000  5     | *     *      *     | 8     128    1     | 
publicgrp          low       | *     *      *     | *     *      *     | *     *      *     | 
⁺ default account. Use --account=... to use resources from other accounts.
* in a column means no limits of this type.

username@login2 $
```

**Notation:**  Be certain to ignore the `username@login2 $`, which just represents the command prompt.  In the examples, focus on what follows the \*\*\$\*\*

On the Login Node, you can submit SLURM Batch Jobs or allocate resources for an interactive session. Please note that if you become disconnected from the Slurm session - your currently interactive sessions attached to your session will be cancelled unless you have session manger running. I recommend running \*\*TMUX\*\* or \*\*GNU Screen\*\* to manage different sessions and keep you connected.

```bash
#Start Screen Session
username@login2 $ screen -URRd
```

**!! Don't Run anything of significance on the Head Node!!** The head node is only meant to allow you to manage your other Slurm SSH sessions

* * *

### Requesting a Slurm Interactive Session

### In order to create an interactive session, you need to request resources from the Slurm Head node.

You request a session using `salloc` command and specifying what you need:

**Notation:**  In commands, I use `<....>` to denote a parameter or option that you need to enter.  Please do not include the brackets `<>`

```bash
salloc --mem=<Amount of Memory>  --time=<Amount of Time> --nodes=<How many sessions>  --cpus-per-task=<Amount of CPUs> --partition=high

```

1.  Memory (GB): `--mem=32g`
2.  CPUs: `-c 8` or `--cpus-per-task=8`
3.  Number of session: `--nodes=1` or `-N=1`
4.  Time (days-hours:minutes:seconds): `--time=2-00:00:00` or `-t=2-00:00:00`
5.  Partition: `--partition=high` or `-p=high`

The following will reserve and allocate for you 1 node with 16 CPUs and 32GB RAM for 2 days.

(Be certain to ignore the `username@login2 $`, which just represents the command prompt)

```bash
username@login2 $ salloc --mem=32g --time=2-00:00:00 --nodes=1 -c 16 --partition=high

salloc: Pending job allocation 7604311
salloc: job 7604311 queued and waiting for resources
salloc: job 7604311 has been allocated resources
salloc: Granted job allocation 7604311
salloc: Waiting for resource configuration
salloc: Nodes hive-dc-7-6-50 are ready for job
username@hive-dc-7-6-50 $
```

You are now logged into your working interactive session.

If you `exit` a session or time runs out you'll receive a similar message and be logged out of the interactive session

```bash
salloc: Relinquishing job allocation 7603513
salloc: Job allocation 7603513 has been revoked.
```

* * *

## <span style="color: rgb(0, 0, 0);">Partition: High vs Low</span>

<span style="color: rgb(0, 0, 0);">For **publicgrp** there are two different partitions that can make a lot of difference regarding how long it takes for your job to start.</span> 

- <span style="color: rgb(0, 0, 0);">**High:**  The HIVE has 128 CPUs and 2000GB RAM available to anyone one the cluster to reserve for a given amount of time.  Each job is limited to 8 CPUs, 128 GB RAM.  If you select `--partition=high` your job request will be put in a queue for these resources and you will be guaranteed those resources for your required amount of time.</span>
- **<span style="color: rgb(0, 0, 0);">Low:</span>**  <span style="color: rgb(0, 0, 0);">During a typical day, many of the HIVE resources may not be used. Those resources are made available to the Low Partition on a first call basis and with the caveat that if someone with higher access requests those resources - your job will be cancelled and those resources will be made available to them.</span>

So, when it comes to requesting resources from the publicgrp, if your job can wait 48-72hrs to start and requires a fixed amount of time - use High. 

If you want access quickly, choose Low.

<span style="color: rgb(53, 152, 219);">**For this course, I'd suggest using:**</span>

```bash
$ salloc --mem=4g --partition=low --cpus-per-task=1 --nodes=1 --time=0-02:00:00
```

&nbsp;This minimal request for resources is going to be allocated a lot faster than something larger.

* * *

### GNU Screen or Tmux (Session Managers)

If you ran `screen -URRd` or `tmux attach` on the Login Node and you get disconnected, you'll be able to get back to this session by re-logging in to the Hive and then retyping the corresponding `screen -UURd` or `tmux attach` you used prior.

If you forgot to run `screen` or `tmux` on the login server before allocating resources, there isn't a way to get back to it. They were relinquished as soon as you got disconnected.

**Basic GNU Screen Shortcuts**

| Keyboard Shortcut | Description |
| --- | --- |
| CTRL + a and c | Create a new Session Window |
| CTRL + a and k | Kill the current Session Window |
| CTRL + a and space | Switch to the next Session Window |
| CTRL + a and backspace | Switch to the previous Session Window |

### Basic Tmux Shortcuts

| Keyboard Shortcut | Description |
| --- | --- |
| CTRL + b and c | Create a new Session Window |
| CTRL + b and x | Kill the current Session Window |
| CTRL + b and n | Switch to the next Session Window |
| CTRL + b and p | Switch to the previous Session Window |

* * *

## Software on HIVE

There are multiple ways to use bioinformatics software on HIVE.  Some software is preinstalled because it's used by a lot of HIVE users, and some is must be installed by yourself - because it's more specific to your research.

### Using Environment Modules on HIVE  (Common Software/Tools)

The HIVE utilizes **Linux Environment Modules** to provide users access to pre-installed software. It is a simple way to manage software on a large shared system without needing to install or configure everything manually. Think of them like “profiles” you can switch on and off. When you load a module, it sets up your environment so a specific program (and its correct version) is ready to use—adjusting paths and settings behind the scenes. This is especially helpful on shared systems, like research servers, where multiple versions of the same software exist. Instead of dealing with complicated setup steps, you can just run a command like module load python and start working.

```bash
$ module avail # List all available modules
$ module load conda   # Loads the conda module
$ module list # Prints all the loaded modules
$ module unload conda   # Unloads the conda module
$ module rm conda   #Also, unloads the conda module

```

* * *

### Using Conda on HIVE to Get Software not on Environment Modules

Conda is a package and environment manager that allows you install pre-configured software packages and utilities on you computer. It provides the ability to create different environments and access different repositories of software. The HPC recommends that if you need a program first to look at the Environment Modules available, and then if it's not there to use Conda to install it.

🚫**Warning:** You shouldn't install Conda or Anaconda on the HIVE, as they have already installed it for you to use with Linux Modules.

You can activate the `base` conda with modules uisng

```bash
$ module load conda
Loading conda/base/latest

(base) $
```

⚠**Note:** the Base environment may not be writeable to install any packages, so you'll need to create your own environment.

By default, the conda environments will be installed in the your home directory `~/.conda/envs`

### Creating an Conda Environment

You can create a conda environment, you'll need to provide a name of the environment and optionally any packages you want installed when the environment is created.

```
(base)$ conda create --name <Name_of_Environment>  <Any installation Packages>
(base)$ conda activate <Name_of_Environment>
```

**Example**  
To create a environment called `samtools` and install the `samtools` packages

```
(base)$ conda create --name samtools samtools`

#After creating the environment, you can activate the environment with:
(base)$ conda activate samtools
```

#### Installing Packages into your Environment (Post-Create)

To install other packages or programs into an environment, first activate the environment and then install using 'conda install'

```
(base)$ conda activate samtools
(samtools)$ conda install bcftools    # Will install bcftools into the samtools environment
```

#### Creating an Anaconda Environment

**Note:** Anaconda is a large environment that includes many different Python Datascience tools and modules.

In order to create an Anaconda environment with all the Anaconda Data Science Tools, you need to first add `defaults` channel to your conda configuration.

```
(base)$ conda config --add channels defaults
(base)$ conda create --name anaconda anaconda
(base)$ conda activate anaconda
(anaconda)$
```

### Deactivating an Environment

You can go back to the previous conda environment by deactivating your current environment

```
(anaconda)$ conda deactivate   #Deactivate anaconda environment and go back to base

(base)$
```

* * *

# Additional Slurm Commands

## squeue - View your job allocations

`squeue` allows you to see all the current session on the HIVE SLURM Cluster.   To view your current allocations and information about them:

1.  JOBID:  is the ID assigned by the head node to your session
2.  USER: Username that the job is assigned to
3.  PARTITION: Which partition the session is registered under
4.  NAME: Current name of the session
5.  ST:  Status of your session (R): Running   (ST): Stopped  (F): Failed (PD): Pending
6.  TIME_LEFT:  How many days-hours-minutes-seconds your job has left
7.  CPUS: How many CPU your session was allocated
8.  MIN_MEMORY:  Amount of Memory that you were allocated
9.  NODES: How many sessions this entry represents
10. NODELIST(REASON):  Which node your session was assigned to

```bash
$  squeue --user=<Your username>
JOBID        JOBID              USER            PARTITION      NAME                 ST  TIME_LEFT    CPUS   MIN_MEMORY NODES NODELIST(REASON)
10063828     10063828           plott           high           interactive          R   5-01:59:18   4      32G        1     hive-dc-7-7-26
```

* * *

## scancel - Stopping your Job

`scancel` allows you to cancel a session or range of sessions.

```bash
scancel <JOBID> or <JOBIDs separated by commas>

```

* * *

## sacctmgr - View, modify your account defaults

`sacctmgr` allows the user to view their association and change their default account

```bash
sacctmgr show user <username> withassoc  # Show the accounts the user is associated with
sacctmgr update user <username> set defaultaccount=<account>  # Change the default account for the user
```

* * *

## sbatch - Slurm Script Job Submission (Non-interactive Sessions)

`sbatch` submit a sbatch script for a non-interactive job to SLURM.  The script provides the job requirements and parameters.

Creating a script to run your analysis and name it `my_analysis.sh`

```bash
#!/bin/bash
#SBATCH --job-name=myjob
#SBATCH --output=result.txt
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task 2
#SBATCH --time=01:00:00
#SBATCH --mem=4G
#SBATCH --account=publicgrp
#SBATCH --partition=high

# Commands to run
module load conda
conda activate anaconda
python my_analysis.py
```

To submit the job using sbatch:

```bash
$ sbatch my_analysis.sh
```

You can then use `squeue` to monitor scheduled tasks

* * *

## Additional Help

1.  https://hpc.ucdavis.edu/faq
2.  https://docs.hpc.ucdavis.edu/scheduler/resources/
3.  https://docs.ianwashere.com/hpc
4.  https://www.redhat.com/en/blog/introduction-tmux-linux
5.  https://wiki.archlinux.org/title/GNU_Screen
6.  https://hpcc.sdsu.edu/wp-content/uploads/2019/05/SlurmCheatsheet.pdf
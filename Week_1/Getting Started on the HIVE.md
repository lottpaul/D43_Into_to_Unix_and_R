Hive SSH URL:   hive.hpc.ucdavis.edu
OnDemand GUI:  https://ondemand.hive.hpc.ucdavis.edu/

## OnDemand Apps:
1. RStudio Server
2. Hive Desktop (Virtual Desktop)
3. Juptyer Lab
4. VSCode Server

## Getting Logged Into Hive Via SSH
You can use your UCDavis Account Password or SSH Public Key to login to HIVE Cluster Head node.
Note: (Be certain to change `username` to your actual username and ignore the `$`, which just represents the command prompt)
```
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

On the Login Node, you can submit SLURM Batch Jobs or allocate resources to for an interactive session.  Please note that if you become disconnected from the Slurm session - it will be cancelled.   I recommend running TMUX or GNU Screen to manage different sessions and keep you connected.

(Be certain to ignore the `username@login2 $`, which just represents the command prompt)
```
#Start Screen Session
username@login2 $ screen -UURd
```

**!! Don't Run anything of significance on the Head Node!!**  The head node is only meant to allow you to manage your other Slurm SSH sessions
 
### Set you default account to genome-center-grp
By default your account default is the public-grp, however, you'll want to ensure you utilize the **genome-center-grp**.

To change your default group:     (Be certain to change `username` to your actual username and ignore the `username@login2 $`, which just represents the command prompt)
```
username@login2 $ sacctmgr update user username set defaultaccount=genome-center-grp
```

To show your default account settings:  (Be certain to change `username` to your actual username and ignore the `username@login2 $`, which just represents the command prompt)
```
username@login2 $ sacctmgr show user username withassoc
```

## Allocating Interactive Session to do some work
In order to create an interacte session, you need to request resources from the Slurm Head node.  

You request a session using `salloc` command and specifying what you need:  
1. Memory (GB): `--mem=32g`
2: CPUs  `-c 8`
3. Number of session:  `--nodes=1`
4. Time (days-hours:minutes:seconds):  `--time=2-00:00:00`

The following will reserve and allocate for you 1 node with 16 CPUs and 32GB RAM for 2 days. 

(Be certain to ignore the `username@login2 $`, which just represents the command prompt)
```
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
```
salloc: Relinquishing job allocation 7603513
salloc: Job allocation 7603513 has been revoked.
```

If you ran `screen -URRd` or `tmux attach` on the Login Node and you get disconnected, you'll be able to get back to this session by re-logging in to the Hive and then retyping the corresponding  `screen -UURd` or `tmux attach`  you used prior.

If you forgot to run `screen` or `tmux` on the login server before allocating resources, there isn't a way to get back to it.  They were relinquished as soon as you got disconnected.



## Using Conda on HIVE
You shouldn't install Conda or Anaconda on the HIVE, as they have already installed it for use with Linux Modules.

You can activate the `base` conda with modules uisng

```
$ module load conda
Loading conda/base/latest

(base) $
```

Note the Base environment may not be writeable to install any packages, so you'll need to create your own environment. 

By default, the conda environments will be installed in the your home directory `~/.conda/envs`, which can cause your home directory over the 20GB limit.    To get around this potential issue, you can create a .conda directory in your `/quobyte/luisccgrp/USERS/username/` folder.  (Be certain to replace `username` with your actual username)

```
#Create a .conda folder in your luisccgrp/USERS directory
$ mkdir -p /quobyte/luisccgrp/USERS/username/.conda

#Go to Home Directory
$ cd

# Create a symbolic link in home directory to /quobyte folder
$ ln -sf /quobyte/luisccgrp/USERS/username/.conda .

```


## Creating an Conda Environment
You can create a conda environment, you'll need to provide a name of the environment and optionally any packages you want installed when the environment is created.
```
(base)$ conda create --name Name_of_Environment  <Any installation Packages>
(base)$ conda activate Name_of_Environmeent
```

**Example**
To create a environment called `samtools` and install the `samtools` packages 
```
(base)$ conda create --name samtools samtools`

#After creating the environment, you can activate the environment with:
(base)$ conda activate samtools
```

### Installing Packages into your Environment (Post-Create)
To install other packages or programs into an environment, first activate the environment and then install using 'conda install'

```
(base)$ conda activate samtools
(samtools)$ conda install bcftools    # Will install bcftools into the samtools environment
```

### Creating an Anaconda Environment
In order to create an Anaconda environment with all the Anaconda Data Science Tools, you need to first add `defaults` channel to your conda configuration.

```
(base)$ conda config --add channels defaults
(base)$ conda create --name anaconda anaconda
(base)$ conda activate anaconda
(anaconda)$
```


## Deactivating an Environment
You can go back to the previous conda environment by deactivating your current environment

```
(anaconda)$ conda deactivate   #Deactivate anaconda environment and go back to base

(base)$
```



# Old Packages or Applications
Old packages installed on Carcino are still available on the HIVE, but they may not be in your PATH.  Try looking in the old Packages directories for the package you want.  

**Old Packages Directories**:  `/quobyte/luisccgrp/PACKAGES/src`

 If you find it, you can run it directly from the folder, or create a link to the application in `/quobyte/luisccgrp/PACKAGES/bin` folder and then add the `/quobyte/luisccgrp/PACKAGES/bin` folder to your PATH

 ```
export PATH=/quobyte/luisccgrp/PACKAGES/bin:$PATH
```



# REFERENCE Files
Reference files are located at `/quobyte/luisccgrp/REFERENCE_DATA`

Cut and Run References:   According to config files for NextFlow Core Cut & Run Pipeline many of the references can be downloaded from Illumina iGenomes (https://support.illumina.com/sequencing/sequencing_software/igenome.html)

I've copied the hg38, GRCh38, GRCH8_decoy, E.coli K12-DH10B, and E.coli K12-MG1655 genome files to the following folder:  `/quobyte/luisccgrp/REFERENCE_DATA/Illumina/iGenomes`




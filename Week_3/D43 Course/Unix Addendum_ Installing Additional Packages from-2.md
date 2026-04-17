Over the 56 year history of Unix, many tools have been developed for specific tasks. However, it would be impossible to have them all installed by default. So, typically your base Unix contains tools such as `less`, `wc`, `grep`, and other tools. If you need to install other programs, they are often available from a repository or from Source.

Some applications aren't available via Environment Modules on the HIVE. There are a few different options available to get an application up and running on the cluster

1.  **Install a compiled binary into your CONDA environment via `conda install`**
2.  **Download a x86_64 compiled binary file and put it in your `PATH`**
3.  **Download a container (Docker) using Singularity/Apptainer**
4.  **Compile a program from it's source code**

* * *

# HIVE: 1. via Conda Install  (Beginner)

We covered installing applications via Conda in the Introduction to Hive lesson.  There are 3 main conda channels or repositories:

1.  defaults  (Anaconda Data science tools)
    
2.  [conda-forge](https://conda-forge.org/packages/) (Community collection of a wide-range of software)
    
    ```
    conda config --add channels conda-forge
    ```
    
3.  [bioconda](https://bioconda.github.io/conda-package_index.html) (Communtiy collection of bioinformatics software and packages)
    
    ```
    conda config --add channels conda-forge
    ```
    

You can search package online via web browser or via Unix command-line and install specific versions.

# HIVE: 2. Via Compiled Binary  (Beginner to Intermediate)

Some applications provide statically linked pre-compiled binary applications that can be downloaded and run. Oftentime, these are found in the applications Github Repository Releases section: They'll often be described as pre-compiled binaries - the required version for HIVE would be **linux-x86_64**

An example of this is **[salmon](https://github.com/COMBINE-lab/salmon/releases)**, a transcript level quantification application used for RNA-seq. In order to run this, you'll need to download the file and extract the binary file.

Then ensure that the directory where the app exists is added to your PATH varianle

```
$ export PATH=/home/plott/APPS/<directory>:$PATH
```

After exporting the PATH, you should be able to call it directly from the command-prompt.

# HIVE: 3. Via Container  (Intermediate to Expert)

In HPC and Bioinformatics, the use of containers has become a very common way of sharing and using utilities.  Most commonly, it's applied in Pipelines, such as NextFlow, SnakeMake, or Common Workflow Language.  The benefit is that instead of relying on you to compile a utility, someone can create a container, which is like a small virtual machine, and install the application in there.  Then now matter what system you run the container on you'll get the expected behavior - ensuring forward, backwards, and cross compatibility. 

On your own computer, you can install Docker to create and run containers.  However, on the HIVE HPC users aren't allowed to use Docker - because of issues with permissions.  The alternative is a program called Singularity or Apptainer (Singularity was renamed to Apptainer).   This allows users to download, mount, and run containers on the HPC.

Here, I'm going to use `samtools` as an example.

You can look at Docker Hub and find a samtools container.  I found **dnalinux** has a recent samtools version. 

I'll first show how to pull the container from Docker and then run an interactive shell, or run a specific command in the container after mounting my directory to the container.

```bash
$ apptainer pull docker://dnalinux/samtools:latest
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
INFO:    Fetching OCI image...
3.4MiB / 3.4MiB [=======================================================================================================================================================================] 100 % 1022.5 KiB/s 0s
8.9MiB / 8.9MiB [=======================================================================================================================================================================] 100 % 1022.5 KiB/s 0s
10.3MiB / 10.3MiB [=====================================================================================================================================================================] 100 % 1022.5 KiB/s 0s
111.5MiB / 111.5MiB [===================================================================================================================================================================] 100 % 1022.5 KiB/s 0s
27.3MiB / 27.3MiB [=====================================================================================================================================================================] 100 % 1022.5 KiB/s 0s
27.8MiB / 27.8MiB [=====================================================================================================================================================================] 100 % 1022.5 KiB/s 0s
INFO:    Extracting OCI image...
INFO:    Inserting Apptainer configuration...
INFO:    Creating SIF file...
[====================================================================================================================================================================================================] 100 % 0s

$ apptainer shell docker://dnalinux/samtools:latest
Apptainer> samtools
...
Apptainer> exit


#Bind a directory and run a command
$ apptainer run --bind /path/to/data:/data docker://dnalinux/samtools:latest samtools view -c /data/my_bam_file.bam

```

# HIVE: 4: From Source Code (Intermediate to Expert Level)

There are multiple build system on Linux. The most common is the `make` and `autogen` system. These utilize compilers to compile the source code specificially for your system. However, it can get very complicated very quickly. Some of the standards tools, such as Samtools utilizes the `autogen` system to assess the current system and if all the dependencies are already installed. If not, you'll need to pre-install these and then tell the compiler where these are.   You'll typically, download **tar.gz** archive files of source code from Github or the programmers site.  Once, you've decompressed and expanded the archive, you'll typically follow the author's installation instructions.

A simple example may look something like this.

```
$ wget https://github.com/samtools/samtools/releases/download/1.23.1/samtools-1.23.1.tar.bz2
$ tar xvf samtools-1.23.1.tar.bz2
$ cd samtools-1.23.1

$ ./configure --prefix=<Install Directory>  #Checks the configuration and then configures the 
#compiler and installer scripts. The install directory structure will follow Linux canonical 
#app structure. There will be a bin folder holding the binaries, a lib folder for the library 
#files, an include folder for the source include files for the library files, and a share 
#folder for man pages or other shared info.

$ make   #Compiles the application in the current source code directory using the generated Makefile
$ make install  # Moves application and libraries to installation directory
```

* * *

# As Root or Sudo-er on your own system

&nbsp;

* * *

# Windows WSL2 - Installing Tools from Repository

Ubuntu and other Debian variants use a tool called `apt` to interact with a repository. It allows you to search, update, upgrade, and install programs from the repository.

For different linux distributions, you'll see package managers such as `yum`, `dnf`, or `zypper`

* * *

# MacOS - Installing Tools from Repository

MacOS provided a very limited set of tools to be downloaded by default. However, developers have created **Homebrew** to act as a repository tool. **Homebrew** acts similar to `apt` for linux

To install **Homebrew**:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

To access Homebrew, we use the `brew` command.

* * *

## Finding a Package

Let's see if we can find the package called `tree`. The option `search` works for the `apt` or `brew`

```
$ apt search tree

$ brew search tree

```

## Installing a Package

To install `tree`. If you get warned that you don't have permissions to install, you may need to type `sudo` prior to the command. `sudo` stands for `super user do` - it means execute this command with administrative permissions. This give the command the permissions it needs to write the installed files to the system directories.

```
# For Windows WSL2 Ubuntu
$ sudo apt install tree


#For MacOS
$ brew install tree

```

# Finding the Packages you want

You'll notice that when you search for a package, you may get back a few different options. Google is a good place to determine which one is what you want.

**Homebrew** also has some additional pages: https://formulae.brew.sh/formula/
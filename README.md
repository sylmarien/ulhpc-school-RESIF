`README.md`

Copyright (c) 2015 Maxime Schmitt <maxime.schmitt@telecom-bretagne.eu>

-------------------

# UL HPC Tutorial: Use and manage software with module and RESIF on the UL HPC platform

The objective of this tutorial is to present how to interact with the software installed on the UL HPC platform, from using provided software to extending the stack by adding new software on the platform, and also replicating the architecture on a local machine.

This course is divided in three chapters that are going through each of the previously stated use cases.

The [first chapter](#understanding-and-using-the-software-available-on-the-ul-hpc-platform) goes through the architecture of the software stack and the basic tools to start using them.  
The [second chapter](#adding-a-software-to-the-existing-stack) goes through the process of adding new software to the existing stack as a user using RESIF, an internally developped tool to manage the software stack on the platform.  
The [third chapter](#replicating-the-architecture-of-the-platform-on-a-local-environment) goes through the process of replicating the software stack present on the platform, or part of it, on a local environment.

## Understanding and using the software available on the UL HPC platform

Before starting this tutorial, please make sure you are on a compute node and not on the access node. To get ressources on a compute, use the following command:  
`(access)$> oarsub -I -l nodes=1,walltime=1:00:00`  
(for more details about this command and the node reservation process on the clusters, please referer to the [ULHPC documentation](https://hpc.uni.lu/users/docs/oar.html).)

The software architecture on the platform revolves around the `module` tool. This command is at the core of the workflow to use a software on the platform, so we're going to cover its most basic command before going any further.

### `module` command basics

`module` can list all the software modules installed in the software stack:  

    (node)$> module avail
    ------------------- /opt/apps/devel/v0.0-20150212/core/modules/bio ----------------
    bio/ABySS/1.3.4-goolf-1.4.10-Python-2.7.3        bio/Bowtie2/2.2.2-goolf-1.4.10   (D)
    bio/ABySS/1.3.4-ictce-5.3.0-Python-2.7.3  (D)    bio/Cufflinks/2.0.2-goolf-1.4.10
    [...]
Note that this would output a lot of text on the clusters since there are a lot of installed software, to limit the output we can limit it to what we are interested in, for example the GROMACS software :
    
    (node)$> module avail gromacs
    ----------- /opt/apps/devel/v0.0-20150212/core/modules/bio --------------
    bio/GROMACS/4.6.1-ictce-5.3.0-hybrid    bio/GROMACS/4.6.1-ictce-5.3.0-mt
    bio/GROMACS/4.6.5-goolf-1.4.10-mt (D)
    [...]
This will only output the software modules from the software stack that contain "gromacs" in their name.

To start using the version you want, for example `bio/GROMACS/4.6.5-goolf-1.4.10-mt`, we are going to `load` the software module:  
`(node)$> module load bio/GROMACS/4.6.5-goolf-1.4.10-mt`

You can now use the software and work with it. To check that it is actually loaded, list the loaded software modules:

    (node)$> module list
    Currently Loaded Modules:
        1) compiler/GCC/4.7.2             4) toolchain/gompi/1.4.10                            7) numlib/ScaLAPACK/2.0.2-gompi-1.4.10-OpenBLAS-0.2.6-LAPACK-3.4.2
        2) system/hwloc/1.6.2-GCC-4.7.2   5) numlib/OpenBLAS/0.2.6-gompi-1.4.10-LAPACK-3.4.2   8) toolchain/goolf/1.4.10
        3) mpi/OpenMPI/1.6.4-GCC-4.7.2    6) numlib/FFTW/3.3.3-gompi-1.4.10                    9) bio/GROMACS/4.6.5-goolf-1.4.10-mt

When you're finished working with it, unload the software module:  
`(node)$> module unload bio/GROMACS/4.6.5-goolf-1.4.10-mt`

However, this will only unload the `bio/GROMACS/4.6.5-goolf-1.4.10-mt` software module itself, not its dependencies, as you can see it:

    (node)$> module list
    Currently Loaded Modules:
        1) compiler/GCC/4.7.2             4) toolchain/gompi/1.4.10                            7) numlib/ScaLAPACK/2.0.2-gompi-1.4.10-OpenBLAS-0.2.6-LAPACK-3.4.2
        2) system/hwloc/1.6.2-GCC-4.7.2   5) numlib/OpenBLAS/0.2.6-gompi-1.4.10-LAPACK-3.4.2   8) toolchain/goolf/1.4.10
        3) mpi/OpenMPI/1.6.4-GCC-4.7.2    6) numlib/FFTW/3.3.3-gompi-1.4.10
You could unload all these dependencies by hand, but it would be too long and painful, the efficient way is to `purge` the loaded software modules to restore the initial state of the session:  
`(node)$> module purge`  
This unloads *all* the software modules that you see with `module list`.

Now that we have the basics to manipulate the software modules, we're going to deeper look at them and the different concepts surrounding them.

### Software stack architecture

The upper layer of the architecture is what we call the *sofwtare set*. It is a collection of software, common example are a _core_ software set that would contain only tested software and an _experimental_ one that would contain untested software.  
The goal of these groupings is to provide information on the degree of support for the various software.

Inside of these software sets, software are named with regard to a *naming scheme* which provides information on the software and allows for a better presentation of results of the `module avail` command.  
Software named following following this naming scheme have the following skeleton: **software_class/software_name/software_complete_version** where  
- software_class describes the category in which the software is classified and can be found among the following classe: [base, bio, cae, chem, compiler, data, debugger, devel, lang, lib, math, mpi, numlib, phys, system, toolchain, tools, vis]
- software_name is the name of the software, e.g. GROMACS or ABySS
- software_complete_version is the complete version of the software: it contains the version of the software itself followed by the type and version of the main dependencies it relies on (e.g. compiler) with the following format: software_version-dependencies_versions

What all this architecture allows when considering the usage of the software stack is that it provides more information in a more structured way.  
Firstly, all the software of a given software set will be grouped together when listing the software with the `module avail` command.

## Add a software to the existing stack

## Replicating the architecture of the platform on a local environment
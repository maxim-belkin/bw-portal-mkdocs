# Getting Started Guide

This brief guide is intended to give fairly experienced users the basic
information they need to get on the system, set up their environment,
compile applications, and run batch jobs.

A [New User
Webinar](https://bluewaters.ncsa.illinois.edu/new-user-webinar) is also
available for playback of video recordings.

## Contents

[Obtaining an
Allocation](https://bluewaters.ncsa.illinois.edu/getting-started/#Allocation)

[Logging
In](https://bluewaters.ncsa.illinois.edu/getting-started/#Logging)

[Level of Expertise Expected of Blue Waters
Users](https://bluewaters.ncsa.illinois.edu/getting-started/#Expertise)

[Transfering Files and
Data](https://bluewaters.ncsa.illinois.edu/getting-started/#Transfer)

[Setting Up Your
Environment](https://bluewaters.ncsa.illinois.edu/getting-started/#Environment)

[Building
Executables](https://bluewaters.ncsa.illinois.edu/getting-started/#Building)

[Running Batch
Jobs](https://bluewaters.ncsa.illinois.edu/getting-started/#Running)

[Quick Hardware
Summary](https://bluewaters.ncsa.illinois.edu/getting-started/#QuickHardwareSummary)

### []{#Allocation}Obtaining an Allocation

There are several pathways available for researchers and educators to
apply to use Blue Waters for their work. Visit the [Allocations
page](https://bluewaters.ncsa.illinois.edu/aboutallocations).

### []{#Logging-duo}[]{#logging-in-duo}Logging In

You may connect to Blue Waters via the external login hosts at
**bw.ncsa.illinois.edu** using ssh with your NCSA DUO passcode or push
response from your smartphone. This multi-factor authenication scheme
provides significant security benefits. The bw.ncsa.illinois.edu address
is a DNS roundrobin alias for h2ologin\[1-4\]. If you find that
connecting to bw.ncsa.illinois.edu is not successful please try
specifying a particular login host.

For help activating your NCSA Duo account, reference [this
page](https://wiki.ncsa.illinois.edu/display/cybersec/Duo+at+NCSA).

To check if your NCSA Duo is working proplerly,
visit [here](https://duo.security.ncsa.illinois.edu/portal). Depending
on the choice you make there, you should receive a pass code or a push
from Duo.

Myproxy login and GSI enabled ssh are not supported with NCSA Duo
authentication.

### []{#Expertise}Level of Expertise Expected for Blue Waters Users

Most users of systems like Blue Waters have experience with other large
high-performance computer systems.  The instructions on this portal
generally assume that the reader knows how to use a Unix-style command
line, edit files, run (and modify) Makefiles to build code, write
scripts, and submit jobs to a batch queue system.  There *are* some
things that work slightly differently on the Cray XE system than other
systems; the portal documentation covers those in detail, but we assume
that you know the basics already.

If you\'re not at that level yet (if you\'re unfamiliar with things like
ssh, emacs, vi, jpico, qsub, make, top) then you\'ll need to gain some
knowledge before you can use Blue Waters effectively.  Here are a few
links to resources that will teach you some of the basics about Unix
command line tools and working on a high-performance computing system:

-   <https://www.xsede.org/web/xup/online-training>
-   <https://newton.utk.edu/bin/view/Main/LinuxCommandLineBasics>
-   <http://websistent.com/linux-acl-tutorial/>  \# explains linux
    Access Control Lists (ACL) compared with chmod

### []{#Transfer}Transfering Files and Data

It is recommended that Globus Online (GO) is used for file transfers to
and from Blue Waters. Blue Waters has dedicated import/export resources
to provide superior I/O access to the filesystems. Most HPC centers
provide GO endpoints and GO provides clients for desktop and laptop
transfers.

Please see the [Data
Transfer](https://bluewaters.ncsa.illinois.edu/data-transfer-doc)
section of the [User Guide](/user-guide).

### []{#Environment}Setting Up Your Environment

The default shell is /bin/bash.  You can change it by sending a request
via email to help+bw\@ncsa.illinois.edu.

The user environment is controlled using the modules environment
management system. Modules may be loaded, unloaded, or swapped either on
a command line or in your \$HOME/.bashrc (.cshrc for csh ) shell startup
file.

The command \"module avail\" will display the avail modules on the
system.

Please see the [Programming
Environment](https://bluewaters.ncsa.illinois.edu/programming) section
of the User Guide.

### []{#Home_Directory}Home Directory Permissions

By default, user home directories and /scratch directories are
closed (permissions 700) with a parent directory setting that prevents
users from opening up the permissions. See the File and Directory Access
Control List  page (<https://bluewaters.ncsa.illinois.edu/facl>) for
Blue Waters file system policies.  The /projects file system is designed
as common space for your group; if you want a space that all your group
members can access, that\'s a good place for it.  As always, your space
on the /scratch file system is the best place for job inputs and
outputs.

### []{#Building}Building Executables

The compilers are defined in the PrgEnv module for each family of
compilers.  Invoke the ftn, cc, or CC (Fortran, C, or C++, respectively)
commands after loading the appropriate programming environment, and the
underlying compilers will be employed.  To see the options specific to a
compiler (pgi vs. cray) consult the man pages for the vendor\'s compiler
(man pgf90 or man crayftn).  The correct xt-libsci providing the lapack
and other libs for your programming environment is automatically
included in the PrgEnv module you choose.

The default programming environment (module PrgEnv-cray ) invokes the
Cray compilers. PGI compilers are available via the PrgEnv-pgi
module.Intel compilers are available via the PrgEnv-intel module. Gnu
compilers are available via the PrgEnv-gnu module.

OpenACC support is provided by both the Cray and PGI compilers.

Please see the
[Compiling](https://bluewaters.ncsa.illinois.edu/compiling) section of
the User Guide for more information.

### []{#Running}Running Batch Jobs

The batch environment is Torque/MOAB from Adaptive Computing which talk
to the Cray\'s Application Level Placement Scheduler (ALPS) to obtain
resource information.

The aprun utility is used to start jobs on compute nodes. Its closest
analogs are mpirun or mpiexec as found on many commodity clusters.
Unlike clusters, the use of aprun is mandatory on Blue Waters, which is
not a Linux cluster but a massively parallel system (MPP), in order to
start any jobs including non-MPI ones that run on a single node. If the
PBS script does not use aprun to start the application the latter will
start on a service node, which is a shared resource, and that will be a
violation of the usage policy. If by any reason the single-node job does
not properly run when invoked with help of aprun, the alternative is to
run the single-node application in
[CCM-mode](https://bluewaters.ncsa.illinois.edu/cluster-compatibility-mode).

aprun supports options that are unique to the Cray.  There are flags to
set process affinity by NUMA node, control thread placement, and set
memory policy between NUMA nodes in a job.  See the aprun man page for
more details.  Some common flags are highlighted and documented in the
sample batch scripts.

Example scripts are located in /sw/userdoc/samplescripts/

Following is a minimalistic PBS script to start a job app.exe running
under GNU environment on two XE nodes

    #!/bin/bash
    #PBS -l nodes=2:ppn=32
    #PBS -l walltime=00:30:00
    #PBS -N testjob

    . /opt/modules/default/init/bash
    module swap PrgEnv-cray PrgEnv-gnu

    cd $PBS_O_WORKDIR

    aprun -n 64 ./app.exe < input.dat > output.out

Once you have prepared a job script (called jobscript.pbs for example),
you then use the qsub command to submit the job to the resource handling
and job scheduling system.

`$ qsub jobscript.pbs`

The job is now in the queue (normal queue in this case as it was not
specified).

Please see the [Running Your
Jobs](https://bluewaters.ncsa.illinois.edu/running-your-jobs) section of
the User Guide for more information.

### []{#QuickHardwareSummary}Quick Hardware Summary

The Blue Waters system is a Cray XE/XK hybrid machine composed of AMD
6276 \"Interlagos\" processors and NVIDIA GK110 \"Kepler\" accelerators
all connected by the Cray Gemini torus interconnect. 

#### System Totals

  ---------------------------------- --------------
  **Cabinets**                       **288**
  **Peak Performance** **(XE+XK)**   **13.24 PF**
  **System Memory** **(XE+XK)**      **1.476 PB**
  **Compute Nodes (XE+XK)**          **26,864**
  **Bulldozer Cores (XE+XK)**        **405,248**
  **Kepler Accelerators**            **4,228**
  **Torus Dimensions**               **24x24x24**
  **Usable Disk Storage**            **26.4 PB**
  ---------------------------------- --------------

####  

Please see the detailed [Hardware
Summary](https://bluewaters.ncsa.illinois.edu/hardware-summary) page for
more information.

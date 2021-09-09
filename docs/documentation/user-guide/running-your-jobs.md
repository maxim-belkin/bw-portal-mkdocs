# ![](https://bluewaters.ncsa.illinois.edu/liferay-content/image-gallery/content/Running-jobs-overviewc-MOM.png){style="height:450px; width:600px"}

The Blue Waters system uses TORQUE Resource Manager integrated with
the Moab Workload Manager to schedule and manage user jobs. Since Torque
is based on OpenPBS, most of the commands for managing your jobs on Blue
Waters will be the same as PBS commands. 

The batch system works with the following components to start a job

-   Application launcher (aprun) utility launches applications on
    compute nodes (in case you\'re used to using mpirun on other systems
    to launch jobs, the aprun command takes its place on the Cray
    system). 
-   Application Level Placement Scheduler (ALPS) has applications
    submitted to it by aprun for placement and execution. 

The basic process to execute a job on Blue Waters is as follows:

-   Determine the resources needed for your job
-   Pick a queue that will provide the required resources
-   Create a job script that includes the aprun command and describes
    the resources needed for your job
-   Verify that your job is setup for checkpointing (at the application
    level) if running at scale or for long wall time
-   Submit the job script to the batch system using qsub
    -   You can set the enviroment variable  \'NOAPRUNWARN=1\' in the
        job submission environment to suppress the message: \"WARNING: A
        scan of your job script did not find an invocation of
        \'aprun\'/\'ccmrun\'. This may be due to using another command
        to invoke \'aprun\'/\'ccmrun\', or the use of a shell variable.
        Job will be submitted as usual, but please ensure your job
        script eventually invokes \'aprun\'/\'ccmrun\' command to
        execute tasks on allocated compute nodes.\"

You can see the options for qsub by looking at the qsub man page.  To
delete a job from the queue before it runs, or after it\'s begun
running, use \"qdel \<jobid>\". 

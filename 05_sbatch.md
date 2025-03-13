# Introduction to SLURM

**SLURM** (Simple Linux Utility for Resource Management) is a powerful open-source job scheduler used for managing and allocating resources (CPU, memory, storage, etc.) in a High-Performance Computing (HPC) cluster. It is widely used in research institutions, universities, and data centers to manage and schedule jobs that need to be run on shared computing resources.

## Key Concepts

Before we dive into the commands, let's first understand the basic SLURM concepts:

- **Job**: A task that you submit to the cluster to be run.
- **Job Script**: A script (usually a bash script) that contains the commands to be executed and the resource requirements.
- Node: A single computer within the cluster.
- **Partition**: A set of nodes with specific characteristics, like a queue of jobs that have different priorities or different resource availability.
- **Queue**: A line where jobs wait for available resources.

## Common SLURM Commands

### Run a job

`srun` is used to run a job directly, without submitting a job script.

Example:

```bash
srun -n 4 my_command
```

This command will run `my_command` on 4 CPUs. The `-n` option specifies the number of tasks (CPUs).


## Submit a job script

Most jobs are submitted to SLURM via the `sbatch` command, which is used to submit a script that specifies the job to be run and its resource requirements.


Example Job Script (`job_script.sh`):

```bash
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --output=job_output.out
#SBATCH --ntasks=1
#SBATCH --time=01:00:00
#SBATCH --partition=short

# Commands to run
./my_program
```

Submit the job using sbatch:

```bash
sbatch job_script.sh
```

This will submit the job script `job_script.sh` to the SLURM scheduler, which will allocate the necessary resources and run the job.

## Check the status of jobs

`squeue` shows the status of jobs in the queue, including jobs that are waiting, running, or finished.

```bash
squeue
```

Output:

```
JOBID   PARTITION   NAME   USER   ST   TIME   NODES   NODELIST(REASON)
12345   lab         my_job user   R    5:00   1       node01
```

Here, you can see the job ID, the partition, job name, user, status, and nodes being used.

You can also see all jobs you have submitted by using the `-u` option followed by your username:

```bash
scancel -u $USER
```

## Cancel a job

If you need to cancel a job, you can use the `scancel` command followed by the job ID.

Example:

```bash
scancel 12345
```

This will cancel the job with ID `12345`.

You can cancel multiple jobs by specifying multiple job IDs:

```bash
scancel 12345 12346 12347
```

You can also cancel all jobs you have submitted by using the `-u` option followed by your username:

```bash
scancel -u $USER
```

## View job accounting information

`scct` provides detailed information on the status and history of jobs. This includes the start and end time, job state, and resource usage.

Example:

```bash
sacct -j 12345
```

Ouput:

```
       JobID    JobName  Partition    AllocCPUs      State ExitCode 
------------ ---------- ---------- ------------    -------- --------
12345        my_job    short       1              Completed      0:0
```

This command provides useful information about past jobs, especially if you want to troubleshoot or track resource usage.

##  View node and partition status

`sinfo` shows detailed information about available nodes and partitions in the cluster.

```bash
sinfo
```

Output:

```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
lab*         up   infinite      1  drain bipbip
lab*         up   infinite      3   idle derrick,larry,vega
lab*         up   infinite      1   down studio
web          up   12:00:00      1  drain bipbip
web          up   12:00:00      1   down studio
annotsv      up   12:00:00      1   idle vega
low          up   infinite      1  drain bipbip
low          up   infinite      1   idle vega
low          up   infinite      1   down studio
larry        up   infinite      1   idle larry
```

For the CSTB server, use the `lab` or `low` partition. 

## SLURM directives

When writing a job script, SLURM directives are used to request resources (e.g., memory, time) and control job execution.

Common Directives:

- `--job-name`: Specifies the name of the job.
- `--ntasks`: Specifies the number of tasks to run.
- `--time`: Specifies the maximum time your job can run.
- `--partition`: Specifies which partition to submit the job to.
- `--mem`: Specifies the memory needed for the job.
- `--output`: Redirects the output to a file.
- `--error`: Redirects the error messages to a file.

Example:

```bash
#!/bin/bash
#SBATCH --job-name=my_analysis
#SBATCH --ntasks=4
#SBATCH --time=02:00:00
#SBATCH --mem=8GB
#SBATCH --output=output.txt
#SBATCH --partition=lab
```

This script will request 4 tasks, 8GB of memory, a time limit of 2 hours, and submit to the `lab` partition.

Example of full job script

```bash
#!/bin/bash
#SBATCH --job-name=simulation
#SBATCH --ntasks=8
#SBATCH --time=04:00:00
#SBATCH --mem=16GB
#SBATCH --partition=lab
#SBATCH --output=simulation_output.txt

module load busco

busco -i input.fasta -o output -l eukaryota_odb10 -m genome
```

## SLURM job array

A **job array** is a way to submit multiple similar jobs (e.g., running a simulation on different datasets) at once. Each job in the array is treated as a separate job.

Example:

```bash
#!/bin/bash
#SBATCH --job-name=simulation_array
#SBATCH --array=1-10
#SBATCH --ntasks=1
#SBATCH --time=02:00:00
#SBATCH --mem=8GB

module load python/3.8

python simulation.py $SLURM_ARRAY_TASK_ID
```

The `$SLURM_ARRAY_TASK_ID` variable is used to access the specific task ID for each job in the array.

Submit the array with:

```bash
sbatch job_array_script.sh
```

This will submit 10 jobs (1 to 10), each running `simulation.py` with the task ID as an argument.

## SLURM tips for efficient job submission

- **Use job arrays** when you need to run the same job on multiple datasets.
- **Check partition availability **with sinfo to make sure resources are available for your job.
- **Use time limits wisely** to avoid long-running jobs using unnecessary resources.
- **Monitor job status** with squeue and sacct to track the progress of your jobs.
- **Cancel unnecessary jobs** with scancel to free up resources for other users.
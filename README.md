# slurm_cheatsheet
Some useful commands to interact with the SLURM job scheduler

### Submitting and cancelling jobs

```bash
# Submit a job script
sbatch 00-script/02.stacks.sh

# Cancel a task with the related JOB_ID
scancel 26666

# Launch 'test.sh' with 4 CPUs and 10 Go or RAM
srun -c4 test.sh --mem 10000

# Use srun for any long jobs, even cp or rsync
srun rsync

# Launch interactive job with 2 CPUs and 8 Go of RAM
srun -c 2 --mem 8000 --pty bash
```

### Find information about partitions and jobs

```bash
# Display submitted jobs
squeue

# Improved format
squeue -o "%.18i %.9P %.8j %.4C %.8u %.2t %.10M %.6D %.12R"

# List infos about running jobs
sacct --format="CPUTime,MaxRSS,AveRSS,JobName,Timelimit,Start,Elapsed"

# Display available partitions
sinfo

# Display available partitions with more informations
sacctmgr list qos format="Name,MaxWall,MaxTRESPerUser%20,GrpTRES" | column -t

# List jobs that ran today
sacct -l

# List jobs that ran in the past
sacct -S 2016-01-01

# Visualize partitions (equal to qinfo in SGE)
sacctmgr -p list qos

# List partitions you have access to
sacctmgr list associations where user=$USER

# Get help about sacct command
sacct --helpformat
```

### Job dependencies

#### Basic

```bash
# Launch job2 if job1 finished OK
sbatch --dependency=afterok:SLURM_JOB_ID job2.sh

```

#### Advanced

```bash
#! /bin/bash

# first job - no dependencies
jid1=$(sbatch  --mem=12g --cpus-per-task=4 job1.sh)

# multiple jobs can depend on a single job
jid2=$(sbatch  --dependency=afterany:$jid1 --mem=20g job2.sh)
jid3=$(sbatch  --dependency=afterany:$jid1 --mem=20g job3.sh)

# a single job can depend on multiple jobs
jid4=$(sbatch  --dependency=afterany:$jid2:$jid3 job4.sh)

# swarm can use dependencies
jid5=$(swarm --dependency=afterany:$jid4 -t 4 -g 4 -f job5.sh)

# a single job can depend on an array job
# it will start executing when all arrayjobs have finished
jid6=$(sbatch --dependency=afterany:$jid5 job6.sh)

# a single job can depend on all jobs by the same user with the same name
jid7=$(sbatch --dependency=afterany:$jid6 --job-name=dtest job7.sh)
jid8=$(sbatch --dependency=afterany:$jid6 --job-name=dtest job8.sh)
sbatch --dependency=singleton --job-name=dtest job9.sh

# show dependencies in squeue output:
squeue -u $USER -o "%.8A %.4C %.10m %.20E"
```

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
srun -c2 --mem 8000 -pty bash
```

### Find information about partitions and jobs

```bash
# Display submitted jobs
squeue

# List infos about running jobs
sacct --format="CPUTime,MaxRSS,AveRSS,JobName,Status,Timelimit,Start,Elapsed"

# Display available partitions
sinfo

# List jobs that ran today
sacct -l

# List jobs that ran in the past
sacct -S 2016-01-01

# Visualize partitions (equal to qinfo in SGE)
sacctmgr -p list qos

# List partitions you have access to
sacctmgr list associations where user=$USER
```

### More advanced tricks

```bash
# Launch job2 if job1 finished OK
sbatch --dependency=afterok:SLURM_JOB_ID job2.sh

# Get help about sacct command
sacct --helpformat
```

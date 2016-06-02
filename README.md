# slurm_cheatsheet
Some useful commands to interact with the SLURM job scheduler

### Submit a job script
`sbatch 00-script/02.stacks.sh`

### Display submitted jobs
`squeue`

### List jobs that ran today
`sacct -l`

### List jobs that ran in the past
`sacct -S 2016-01-01`

### Launch 'test.sh' with 4 CPUs and 10 Go or RAM
`srun -c4  test.sh --mem 10000`

### Use srun for any long jobs, even cp or rsync
`srun rsync`

### Visualize partitions (equal to qinfo in SGE)
`sacctmgr -p list qos`

### List partitions you have access to
`sacctmgr list associations where user=$USER`

### Launch job2 if job1 finished OK
`sbatch --dependency=afterok:SLURM_JOB_ID job2.sh``

### List infos about running jobs
`sacct --format="CPUTime,MaxRSS,AveRSS,JobName,Timelimit,Start,Elapsed"`

### Get help about sacct command
`sacct --helpformat`

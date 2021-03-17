# The Slurm job scheduler

Sophia uses the [Slurm cluster management and job scheduling system](https://slurm.schedmd.com/overview.html).
Computations on Sophia are executed through interactive Slurm sessions
or via batch compute job scripts.

## Job *queues* a.k.a. *partitions*

Usage of particular computation resources available on Sophia is governed by
specification of a job *queue*, or *partition* in Slurm lingo, which can be
done either on the commandline as argument to the `srun` command or via a
batch job script that is submitted to the scheduler with the `sbatch` command.

Sophia [compute nodes](hardware.md#compute-nodes) are organised in the following job queues/partitions:

| Name  | Description |
| ----- | ----------- |
| workq | General, open for all Sophia users |
| fatq  | Double random access memory capacity |
| gpuq  | 1 Nvidia Quadro P4000 GPU per node |
| v100  | 1 Nvidia Tesla V100 per node |
| windq | Exclusive access for [DTU Wind Energy](https://windenergy.dtu.dk/english) staff |
| windfatq | Double random access memory capacity, exclusive access for [DTU Wind Energy](https://windenergy.dtu.dk/english) staff |

Use the `sinfo` command to list information about the Slurm partitions configured and `squeue` 
to list compute jobs on queue.


## Interactive jobs

For code testing purposes an interactive terminal session is convenient and can be
requested using the `srun` command. The following
example illustrates the procedure;
```md
srun --partition windq --time 06:00:00 --nodes 2 --ntasks-per-node 32 --pty bash
```
which requests an *interactive job* with 2 compute nodes from the `windq` partition,
and all 32 physical cores available on each, for 6 hours. The session is granted when
resources become available and ends when the user exits the terminal or once the time
limit - here 6 hours - is reached.


## Batch jobs

Once the simulation workflow has been tested via interactive jobs one may wish to run
a number of jobs unsupervised. To dispatch jobs programmatically a job script must be
prepared;

!!! info "Slurm job script example"
    `[<username>@sophia1 ~]$ cat slurm.job`

              #!/bin/bash
              #SBATCH --time=2:00:00
              #SBATCH --partition=workq
              #SBATCH --nodes=1
              #SBATCH --ntasks-per-node=1
              echo "hello, Sophia!"


which can then be submitted with the `sbatch` command,
```md
[<username>@sophia1 ~]$ sbatch slurm.job
```

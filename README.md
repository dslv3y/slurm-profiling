# slurm-profiling
Profiling slurm scripts with `--profile=task` flag

# Motivation
Thanks to Attila Fekete from HPC we now have this opportunity.

# Steps to profile
## 1. Running the job with the `--profile` flag
Currently only the "task" profiling is available and the frequency was set to 120 seconds.
In order to use the profiling you need to include the following lines in your sbatch script:
#SBATCH --profile=task
#SBATCH --acctg-freq=2 # Don't use this if you don't really need it.
...or just run `sbatch` or `srun` command with these flags.
## 2. Merging the data
The slurm write your h5 files `/mnt/beegfs/slurm_profiling/your_user_name` directory and you are the owner of this directory so you can manually digest the data or you can use: `sh5util -j job_number` to merge the node files 
## 3. Getting the pandas df
With this simple script you can obtain a pandas df to futher analyse.
```
import h5py
import pandas as pd

file_path = ''
with h5py.File(file_path, 'r') as f:
    node = np.array(f['Steps']["batch"]["Nodes"])[0]    
    df = pd.DataFrame(np.array(f['Steps']["batch"]["Nodes"][node]["Tasks"]['0']))
```

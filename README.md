# Walkthrough
This page holds tips and tricks for efficient cluster usage


## Table of Contents
1. [Compressing/Extracting data](#compress)
2. [Links](#links)

<a name="compress"></a>
## Compressing and extracting data
Let's say you want to compress `/scratch/$USER/mydataset` into `/scratch/$USER/mydata.zip`
```bash
zip -r /scratch/$USER/mydata.zip /scratch/$USER/mydataset
```
You can also use `7z`:
```bash
7z a /scratch/$USER/mydata.zip /scratch/$USER/mydataset
```

If this is for a large dataset, you can either run this inside `tmux`, or schedule a compute job to do it, for example save the following file:
```bash
#!/bin/bash
#SBATCH --account=ACCOUNT
#SBATCH --mem=32G
#SBATCH --cpus-per-task=4
#SBATCH --time=18:00:00
#SBATCH --mail-user=EMAIL
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL
#SBATCH --mail-type=REQUEUE
#SBATCH --mail-type=ALL

set -euo pipefail


NOW=$(date +"%m_%d_%Y_HH%I_%M")
echo "Starting compression at $NOW"

# Change what needs to be zipped
zip -r /scratch/$USER/mydata.zip /scratch/$USER/mydataset

NOW=$(date +"%m_%d_%Y_HH%I_%M")

echo "DONE at ${NOW}"         
```

- Modify the ACCOUNT, EMAIL, and dataset fields (marked by line 'change')
- Save in zip.sh
Schedule
```bash
sbatch zip.sh
```
You'll be notified by email when it's completed.

### Unzip
You can use `unzip` or `7z`
```bash
unzip myzipfile.zip
```
or to extract to a different directory
```bash
unzip myzipfile.zip -d /scratch/$USER/outputfolder
```
Using 7z as an alternative:
```bash
7z x myzipfile.zip
```


<a name="links"></a>
## Links
[Cluster Wiki](https://docs.alliancecan.ca/wiki/Technical_documentation)

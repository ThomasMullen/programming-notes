#

## Example condor submit files
Here is a [link](https://htcondor.readthedocs.io/en/v8_8/man-pages/condor_submit.html#examples) to documentation.

```bash
universe = docker
docker_image = sha256:63fc7c2504f967f643e27c66ff2d4e10ae5e2b4ee369fc195ec19ef98741640c
executable = shell_script.sh

# File transfer
# transfer_input_files = requirements.txt, run.py
transfer_output_files = /nfs/tank/orger/users/thomas.mullen/scape/20210531/fish1

# Change according to your app needs
request_cpus = 64
request_memory = 70G
request_gpus = 0

# Logging
stream_output = True # If you dont need realtime status set to False
output = output.txt
error = error.txt
log = log.txt

queue 1
```

## Canceling jobs

```bash
# condor status
condor_status

# list all jobs
condor_q nobatch
# remove id
condor_rm id


```
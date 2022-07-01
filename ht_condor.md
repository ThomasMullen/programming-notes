#

# Instructions
1. Create Dockerfile which includes the environment builds
2. Build the Dockerfile locally. i.e. in the location of the Dockerfile:
```bash
docker build -t thomasmullen/[repo]:[tag_name]
```
3. Run the Docker locally
```bash
# note: -it is with interactive terminal
docker run -it scape:light_test /bin/bash
```
4. Push the Docker container in Dockerhub
```bash
docker push thomasmullen/scape:fish_test1 
```
5. SSH into username@HTCondor
6. Go to home area and upload a submission file
```bash
vim htc-scape-fish-test
```
Should link the docker hub file with the docker_image variable
e.g.
```bash
universe = docker
docker_image = docker.io/thomasmullen/scape:light_testv6
executable = /application/run.sh
arguments =  -pD $(fish_path) -m 3 4 -dZ 5 -c 1 3 -r 0.1 4 4 -theta 76.2 -uvS 0.01 -p 0 -n 1 -ray 1

request_cpus = 1
request_memory = 4096
request_gpus = 1

output = $(fish_path)/output.txt
error = $(fish_path)/error.txt
log = $(fish_path)/log.txt

queue fish_path from (
    /nfs/tank/orger/users/thomas.mullen/data/SCAPE/20210601/20210531_6dpf_HUC_H2B_fish3_run1
)
```
7. Submit the Condor job
```bash
condor_submit htc-scape-fish-test
```

8. Check it is register
```bash
condor_status
```


## Example condor submit files
Detailed univeristy [documentation](https://chtc.cs.wisc.edu/uw-research-computing/guides.html) on running tasks with HT-Condor.
Here is a [link](https://htcondor.readthedocs.io/en/v8_8/man-pages/condor_submit.html#examples) to documentation.
Condor submission [link](https://htcondor.readthedocs.io/en/latest/man-pages/condor_submit.html#description)
[Link](https://htcondor.readthedocs.io/en/latest/users-manual/docker-universe-applications.html) for connecting docker with condor

```bash
universe = docker
docker_image = docker.io/thomasmullen/scape:light_testv6
executable = shell_script.sh

# File transfer
# transfer_input_files = requirements.txt, run.py
# transfer_output_files = /nfs/tank/orger/users/thomas.mullen/scape/20210531/fish1

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


## Example condor submit files
Here you can queue many docker files but with a slight chang to the directory parameters.

```bash
universe = docker
docker_image = docker.io/thomasmullen/scape:light_testv6
executable = /application/run.sh
arguments =  -pD $(fish_path) -m 3 4 -dZ 5 -c 1 3 -r 0.1 4 4 -theta 76.2 -uvS 0.01 -p 0 -n 1 -ray 1
#executable = /usr/bin/python3
#arguments =  /application/run.py -pD $(fish_path) -r 0.1 400 400

#$ENV(_CONDOR_SCRATCH_DIR)

request_cpus = 1
request_memory = 4096
request_gpus = 1

output = $(fish_path)/output.txt
error = $(fish_path)/error.txt
log = $(fish_path)/log.txt

queue fish_path from (
    /nfs/tank/orger/users/thomas.mullen/data/SCAPE/20210601/20210531_6dpf_HUC_H2B_fish3_run1
)

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
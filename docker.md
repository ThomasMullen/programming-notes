# Dockerfile

[Docker-hub](https://hub.docker.com/) username: thomasmullen

Official docker [link](https://docs.docker.com/engine/reference/builder/#cmd) to dockerfile commands

Useful [notes](https://mehlj.github.io/Dockerfile/#:~:text=As%20explained%20in%20another%20post,Docker%20image%20using%20docker%20build%20.) on dockerfile anatomy:



## Base image

* Want to start with a baseIMage which is a template of the operating system.
* Look on [dockerhub](https://hub.docker.com/).
	* Example: [Fedora](https://hub.docker.com/_/fedora)
	* Example: [Python]()

```bash
FROM fedora:latest
# or
FROM python:3.9
```

## Execute bash commands
To execute bash commands use the `RUN` function. Any `RUN` command will execute on top of the current image.

Either use `RUN <command>` or `RUN ["executable", "param1", "param2"]`

```bash
RUN mkdir ./SCAPE-Toolbox

```
THis will be where we copy the python preprocessing files to.

### Installing python using a non-python base image

```bash
# # Install Python pip via fedora (dnf)
RUN dnf -y update && \
    dnf install -y python3-pip

# Install python venv
COPY requirements_v.txt .
RUN pip3 install -r requirements_v.txt
```

## Copying files to container

```bash
# make application dir
RUN mkdir /application
# move files from host root area
COPY ./src /application/src
COPY ./utils /application/utils
COPY ./visualisation /application/visualisation
COPY ./run.py /application
```

## Defining working directory
Sets the working directory for any `RUN`, `ENTRYPOINT`, etc. command that follow in the dockerfile.

```bash
WORKDIR /application

```

## Adding drives to your docker
To mount a volume to your docker you have to specify during the `run` of your dockerfile. You might want to do this so you don't have to build your docker with very large data files inside it and it will be more lightweight. You can use either the `-v` or the `--mount` in the command line to map the host directory to docker directory.

The docker run command should look something like
```bash
docker build -t test_python .
docker -it run -v /Volumes/TomOrgerLab/experimentSCAPE/20210628_fish1:/application/data test_python /bin/bash
```

Then in your dockfile you should point where the volume is mapped to from the run.
```bash
VOLUME ["/application/data"]
```

## Interacting with `CMD` in your dockerfile using `ENTRYPOINT`

This will be where command will be executed but all following argments passed in the command line will overwrite and append tothe `CMD` execution in the dockerfile. it is a way to interact with the docker image from the host terminal.

### Simple example
dockerfile:
```bash
RUN mkdir /application
WORKDIR /application
RUN mkdir "/application/data"
# Mounted -v cmd to datapoint
VOLUME ["/application/data"]

# CMD ls but now taking next args from commandline
ENTRYPOINT["/bin/ls"]

```

Example 1 command line:
```bash
docker build -t <tagname> .
docker run -v /Volumes/TomOrgerLab/experimentSCAPE/20210628_fish1:/application/data <tagname> /data

>>
20210628_6dpf_elavl3_H2B_fish1_run1-53_t5749.tiff
Visualisation
corrected_full_vols
correlation_vol.npy
correlation_vol_subset.npy
data.mat
dataSkewCorrected.mat
dump
mat_000000.mat
meta_imgs
motion_correction_shifts
motion_shifts.npy
optovin_frames.npy
reducedVolumes.zip
roiData
test_correlation_vol_subset.png
trial_0.zarr
trial_0.zarr_mc.zarr
trial_1.zarr
trial_2.zarr
trial_3.zarr
trial_4.zarr
trial_5.zarr
trial_6.zarr
trial_7.zarr
trial_8.zarr
trials_partition
```
Example 2 command line:
```bash
docker build -t <tagname> .
docker run -v /Volumes/TomOrgerLab/experimentSCAPE/20210628_fish1:/application/data <tagname> -lrt /data

>>
total 145045504
-rwxrwxrwx 1 root root    50505606 Aug  8  2021 unCor_20210628_6dpf_elavl3_H2B_fish1_run1_vol5748.tiff
-rwxrwxrwx 1 root root 70450042476 Aug  8  2021 data.mat
-rwxrwxrwx 1 root root 76643324001 Aug  8  2021 dataSkewCorrected.mat
-rwxrwxrwx 1 root root    87248806 Aug  8  2021 20210628_6dpf_elavl3_H2B_fish1_run1-53_t5749.tiff
drwxrwxrwx 1 root root     1048576 Oct 10  2021 corrected_full_vols
-rwxrwxrwx 1 root root       91232 Dec  4  2021 motion_shifts.npy
-rwxrwxrwx 1 root root         192 Dec  4  2021 optovin_frames.npy
drwxrwxrwx 1 root root     1048576 Dec 14  2021 dump
-rwxrwxrwx 1 root root     1771688 Dec 21  2021 correlation_vol_subset.npy
-rwxrwxrwx 1 root root       38491 Dec 21  2021 test_correlation_vol_subset.png
-rwxrwxrwx 1 root root  1057573945 Jan 13 09:47 reducedVolumes.zip
drwxrwxrwx 1 root root     1048576 May 17 21:16 Visualisation
-rwxrwxrwx 1 root root    12260471 Jun 24 12:46 mat_000000.mat
drwxrwxrwx 1 root root     1048576 Jun 26 22:49 trials_partition
drwxrwxrwx 1 root root     1048576 Jun 26 22:49 roiData
drwxrwxrwx 1 root root     1048576 Jun 26 22:49 motion_correction_shifts
drwxrwxrwx 1 root root     1048576 Jun 26 22:49 meta_imgs
drwxrwxrwx 1 root root     1048576 Jun 27 11:02 trial_0.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:03 trial_1.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:06 trial_2.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:09 trial_3.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:12 trial_4.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:15 trial_5.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:17 trial_6.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:20 trial_7.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:23 trial_8.zarr
drwxrwxrwx 1 root root     1048576 Jun 27 11:31 trial_0.zarr_mc.zarr
-rwxrwxrwx 1 root root   199113328 Jun 27 12:30 correlation_vol.npy

```



# docker commands & debugging dockerfile

To debug in the dockfile run use the arg `-it` and end the run in the `/bin/bash` this will allow you to access the bash terminal inside the docker so you can see how your docker is organised and what commands you have.
```bash
docker -it run <dockerfile_tag> /bin/bash
```

Build dockerfile image:
```bash
docker build -t <tagname> .
```

Show all local docker images:
```bash
docker image ls
```

Push local docker image to repo:
```bash
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
```

Debug as an external user `nobody`
```bash
docker run -e _CONDOR_SCRATCH_DIR=/tmp  --user nobody -it thomasmullen/scape:light_testv3 /bin/bash
```

# Save and Run docker without remote repo
1. First ssh to your server that has to docker image
```bash
sudo docker save -o /home/your_image.tar your_image_name
# determine the size of your compressed tar file
ls -lah /home/your_image.tar
```
2. Copy the file over to the host server using rsync or cp
```bash
rsync -avz /home/sammy/your_image.tar username@remote_server_ip_address:destination_directory
```

3. SSH to the new server and load the repo
```bash
sudo docker load -i your_image.tar
sudo docker images
```

# Example:
## Dockerfile
```bash
FROM fedora:33

# # Install Python
RUN dnf -y update && \
    dnf install -y python3-pip

# Install python venv
COPY requirements_v.txt .
RUN pip3 install -r requirements_v.txt

# make application dir
RUN mkdir /application

# Add to contain the project to application
COPY ./src /application/src
COPY ./utils /application/utils
COPY ./visualisation /application/visualisation
COPY ./run.py /application

WORKDIR /application

RUN mkdir "/application/data"

# Mounted -v cmd to datapoint
VOLUME ["/application/data"]

# Additional arguments appended after entrypoint
ENTRYPOINT ["python", "run.py", "-pD /application/data"]
```
## Terminal command

```bash
docker build -t test_python .
docker run -v /Volumes/TomOrgerLab/experimentSCAPE/20210628_fish1:/application/data test_python -m 3 40 -dZ 5 -c 1 30 -r 0.1 400 400 -theta 76.2 -uvS 0.01
```



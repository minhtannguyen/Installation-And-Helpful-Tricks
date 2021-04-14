# Installation-And-Helpful-Tricks
A list of installation and helpful tricks related to the DSP group projects. <br>
Any suggestions and pull requests are welcome. 

# Bookmarks
  * [Using Jupyter Notebook inside the Docker](#using-jupyter-notebook-inside-the-docker)
  * [Using Standalone Tensorboard inside the Docker](#using-standalone-tensorboard-inside-the-docker)
  * [Useful Docker Commands](#useful-docker-commands)
  * [Useful Commands to Transferring Files](#useful-commands-to-transferring-files)
  * [Useful Commands to Monitor the Server](#useful-commands-to-monitor-the-server)
  * [Tricks to Work with Screen](#tricks-to-work-with-screen)
  * [Recover Deleted Files on the Server](#recover-deleted-files-on-the-server)
  * [Other Useful Tricks for Working with Remote Servers](#other-useful-tricks-for-working-with-remote-servers)
  * [Tricks to Submit on arXiv](#tricks-to-submit-on-arxiv)
  * [Download and Upload from Google Drive when Using Terminal on a Remote Server](#download-and-upload-from-google-drive-when-using-terminal-on-a-remote-server)
  * [Kill background processes](#kill-background-processes)
  * [Log all data from Tensorboard](#log-all-data-from-tensorboard)
  * [Rsync files of a certain extension recursively from a folder](#rsync-files-of-a-certain-extension-recursively-from-a-folder)
  
## Using Jupyter Notebook inside the Docker
Log into the cluster from your local computer:
```
ssh -L 127.0.0.1:8887:127.0.0.1:8887 -L 127.0.0.1:8888:127.0.0.1:8888 -X tan@terminator1.ece.rice.edu
```
On the cluster, create a docker container:
```
nvidia-docker run -p 0.0.0.0:8887:8887 -p 0.0.0.0:8888:8888 --rm -ti --name tanpytorch --hostname insideDocker -v /home/tan/:/root/ -v /mnt/project2/tanData/:/tanData/ tannguyen1989/efficientcnn:latest /bin/bash
```
Inside the docker container, launch jupyter notebook
```
jupyter notebook --port 8887 --ip 0.0.0.0 --allow-root
jupyter notebook --port 8888 --ip 0.0.0.0 --allow-root
```
Copy the link and paste it into your local internet browser

If you would like to use JupyterLab instead of the notebook, use the following command to launch the lab
```
jupyter lab --port 8887 --ip 0.0.0.0 --allow-root
jupyter lab --port 8888 --ip 0.0.0.0 --allow-root
```
You can find instruction to install JupyterLab here: https://github.com/jupyterlab/jupyterlab. The easiest way to install JupyterLab is to use pip.
```
pip install jupyterlab
```
If you are interested in using Jupyter Notebook and Jupyter Lab, the following tutorials will be useful.
* Jupyter Notebook tutorial: https://www.youtube.com/watch?v=k7WXVWej-NY
* Jupyter Lab tutorial: https://www.youtube.com/watch?v=w7jq4XgwLJQ
## Using standalone Tensorboard inside the Docker
Add the `-p 0.0.0.0:6006:6006` parameter to the `nvidia-docker run` command. For example, 
```
nvidia-docker run -p 0.0.0.0:6006:6006 --rm -ti --name tanmxnet4 --hostname insideDocker -v /home/tan/:/root/ -v /mnt/project2/tanData/:/tanData/ tannguyen1989/ld-drm:latest /bin/bash
```

Once that is done, make a directory named `logs` and run 
```
tensorboard --logdir=./logs --port 6006 --host 0.0.0.0
```
Then, go into a (local) browser and type 
```
http://username@terminator1.ece.rice.edu:6006/
```

## Useful Docker Commands
Commit contents in container de020b9b1da7 to the docker image tannguyen1989/ld-drm:latest
```
docker commit de020b9b1da7 tannguyen1989/ld-drm:latest
```
Push the docker image tannguyen1989/ld-drm:latest to docker hub
```
docker push tannguyen1989/ld-drm:latest
```
Kill a docker container tanmxnet
```
docker kill tanmxnet
```
List all docker containers
```
docker ps
```
Attach to a docker container
```
docker exec -it tanmxnet /bin/bash
```

## Useful Commands to Transferring Files
```
scp -r /Users/Owl/Downloads/Dockerfile.rtf tan@terminator1.ece.rice.edu:/home/tan/dockerfiles
Dockerfile.rtf
```

## Useful Commands to Monitor the Server
Find RAM, Swap memory, CPU usage
```
top OR htop
```
Users logged in
```
w
```
Login History
```
last
```
Monitor GPU usage every 2 seconds, default is 2 seconds
```
watch -n 2 nvidia-smi
```

## Tricks to Work with Screen
While the screen is running, if we would like to start logging the output from that screen, do the following:
```
Ctrl + a:logfile  <name-of-log-file>
Ctrl + a Shift + h
```
If we want to copy the current output of a screen to a file, do the following:
```
Ctrl + a h
```
or
```
Ctrl + a:hardcopy -h <filename>
```
If we would like to copy the whole rollback buffer, then do the following:
```
Ctrl + a:writebuf <filename>
```
We can also log the screen from its start by doing the following:
```
screen -S <screenname> -L -Logfile <log_filename>
```
## Recover Deleted Files on the Server
Install testdisk
```
sudo apt install testdisk
```
To recover files run testdisk /dev/sdX where sdX is the disk's name (type df to see all the disks). Select your partition table type (table type is None for the terminator1). After this, select [ Advanced ] Filesystem Utils , then choose your partition. Now you can browse and select deleted files and copy them to another location in your filesystem by following the instructions given by testdisk at the bottom of your window.

## Other Useful Tricks for Working with Remote Servers
Keep the connection between the terminal and the server alive 
* In ~/.ssh/config, add the following:
```
Host *
ServerAliveInterval 240
```
* Then do
```
chmod 600 ~/.ssh/config
```

## Tricks to Submit on arXiv
use \cite{} and do not use package natbib

## Download and Upload from Google Drive when Using Terminal on a Remote Server
```
https://www.howtogeek.com/451262/how-to-use-rclone-to-back-up-to-google-drive-on-linux/
```

## Kill background processes
```
ps -eaf
sudo kill -9 PID
More at: https://www.baeldung.com/linux/kill-background-process
```

## Log all data from Tensorboard
```
https://gemst1.github.io/1-Tensorboard-log-data
The application file can be found here: /opt/conda/lib/python3.6/site-packages/tensorboard/backend
```

## Rsync files of a certain extension recursively from a folder
```
rsync -rav -e ssh --include '*/' --include='*.rigveda' --exclude='*' tanmnguyen@medusa.sci.utah.edu:/home/collab/tanmnguyen/results/momentum_transformer/nonauto /Users/Owl/Documents/medusa-files/nonauto
```


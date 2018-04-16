# Installation-And-Helpful-Tricks
A list of installation and helpful tricks related to the DSP group projects. <br>
Any suggestions and pull requests are welcome. 

# Bookmarks
  * [Using Jupyter Notebook inside the Docker](#using-jupyter-notebook-inside-the-docker)
  * [Using Standalone Tensorboard inside the Docker](#using-standalone-tensorboard-inside-the-docker)
  * [Useful Docker Commands](#useful-docker-commands)
  * [Useful Commands to Monitor the Server](#useful-commands-to-monitor-the-server)
  * [Other Useful Tricks for Working with Remote Servers][#other-useful-tricks-for-working-with-remote-servers]
  
## Using Jupyter Notebook inside the Docker
Log into the cluster from your local computer:
```
ssh -L 127.0.0.1:8888:127.0.0.1:8888 -X tan@terminator1.ece.rice.edu
```
On the cluster, create a docker container:
```
nvidia-docker run -p 127.0.0.1:8888:8888 --rm -ti --name tanpytorch --hostname insideDocker -v /home/tan/:/root/ -v /mnt/project2/tanData/:/tanData/ tannguyen1989/efficientcnn:latest /bin/bash
```
Inside the docker container, launch jupyter notebook
```
jupyter notebook --port 8888 --ip 0.0.0.0 --allow-root
```
Copy the link and paste it into your local internet browser
  
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

## Useful Commands to Monitor the Server
```
top
```
```
w
```
```
last
```

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

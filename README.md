# Installation-And-Helpful-Tricks
A list of installation and helpful tricks related to the DSP group projects. <br>
Any suggestions and pull requests are welcome. 

# Bookmarks
  * [Using Jupyter Notebook inside the Docker](#using-jupyter-notebook-inside-the-docker)
  * [Using Standalone Tensorboard inside the Docker](#using-standalone-tensorboard-inside-the-docker)
  
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
alias startdocker='nvidia-docker run -p 0.0.0.0:6006:6006 --rm -i -t \
--name Container \
--hostname inDocker \
-v /home/user123/:/root/ \
-v /mnt/project5/dataFolder/:/data/ \
image/stuff:latest \
/bin/bash'
export PATH="/mnt/project5/dataFolder:$PATH"
```

Once that is done, make a directory named `logs` and run 
```
tensorboard --logdir=./logs --port 6006 --host 0.0.0.0
```
Then, go into a (local) browser and type 
```
http://username@terminator1.ece.rice.edu:6006/
```

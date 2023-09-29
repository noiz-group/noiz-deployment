# Noiz deployment files 

This is a repository that contains several files and information about how to properly deploy noiz.

It will contain no code, but it will rather be focused on properly docker-compose and pulling images of noiz and noiz-airflow.


## Requirements

docker
And access to internet so you can connect to the docker image repository

## Running

First of all, adjust mount paths in the `docker-compose.yml`.
You need to specify three directories on your local machine :

- Directory for hosting all the files referenced in the Noiz database (datchunks and processing results). 
Make sure this path is on a disk with enough space to host such a volume of data.
In our example, we create an empty directory, which will serve as mount path :
/home/alex/noiz_databases/processed-data-dir-tutorial

- Directory for hosting the input raw data (miniseed files, station.xml, state-of-health files).
In our example we use the tutorial dataset :
```shell script
git clone git@gitlab.com:noiz-group/noiz-tutorial-dataset.git /home/alex/programs/
``` 
The mount path in this case will be :
/home/alex/programs/noiz-tutorial-dataset/dataset

- Directory for hosting the Noiz source codes. This must be an empty directory, since the source code of Noiz will automatically be pulled from the source repository when building the docker container.
In our example, the mount path will be :
/home/alex/programs/noiz

Specify these mounts as following at the end of your docker-compose.yml
```
volumes:
  - /home/alex/noiz_databases/processed-data-dir-tutorial:/processed-data-dir
  - /home/alex/programs/noiz-tutorial-dataset/dataset:/SDS
  - /home/alex/programs/noiz:/noiz
```

After you do that you can run in terminal:
```shell script
docker-compose pull
``` 

It will download all required docker images from the repository.
After download is finished you can run:

```shell script
docker-compose up
```

This will run the whole stack, without detaching it from your terminal.
It's good to test but not to run it long term.
After you see that everything boots up properly, click `CTRL+C` that will stop everything.
Now run:
```shell script
docker-compose up -d
```
It will run the same stack in a detached mode. 
It means that it will run even if you close the terminal.
To see the logs, run
```shell script
docker-compose logs -f -t     
```

It will print out of the logs and will keep following them, as if you were attached to the process. 
If you don't want to print the whole history, you need to add a flag `--tail 50` that will show only last 50 lines for each of the containers.
Find the line where the jupyterlab token is given, it looks like :
```
http://127.0.0.1:8888/lab?token=61b38931e49123aeaf86419b03e1734956f6b5c45d512e51
```
Save the token in some separate file : you will need it for using the Jupyter server.

Now, open new terminal.
In the new terminal, type:
```shell script
docker exec  -it noiz-deployment_noiz_1 /bin/bash     
```

You are ready to run the Noiz core processing of your dataset : example here https://noiz-group.gitlab.io/noiz/content/tutorials/notebook_tutorial.html
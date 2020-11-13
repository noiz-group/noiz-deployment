# Noiz deployment files 

This is a repository that contains several files and information about how to properly deploy noiz.

It will contain no code, but it will rather be focused on properly docker-compose and pulling images of noiz and noiz-airflow.


## Requirements

docker
And access to internet so you can connect to the docker image repository

## Setting up

You need to login to your gitlab repository. 
To do that, go to your terminal and type:

```shell script
docker login registry.gitlab.com 
```

It will prompt you for giving user and password. 
You can either use your gitlab credentials or use something called `deploy token` that is delivered by gitlab.
I higly recommend using the latter one since they are going to be stored in plain text file.

## Running

First of all, adjust mount paths in the `docker-compose.yml`. 
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

Now, open new terminal.
In the new terminal, type:
```shell script
docker exec  -it noiz-deployment_noiz_1 /bin/bash     
```
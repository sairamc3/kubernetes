# How to create your own image?

1. OS - Ubuntu
2. Update apt repo
3. Install dependencies using apt
4. Install python dependencies using pip
5. Copy source code to /opt folder
6. Run the web server using `flask` command

## Docker commands

```
docker run ubuntu
```

The below command will show the running containers
```
docker ps
```

The below command will show the container whicha are exited but not pruned.
```
docker ps -a
```
The below command will run a conatiner with ubuntu image and run `sleep 5` command inside the conatainer
```
docker run ubuntu sleep 5
```

## Difference between **CMD** and **ENTRYPOINT**
### CMD
* CMD defined will be an array and there is no run time command to replace specific part of the command
* You can replace the comand while running the container

### ENTRYPOINT
* ENTRYPOINT can have runtime arguments passed while running the container to be passed to the conatainer.
* Which mandates a new variable to the existing command
* If needed a default value, then ENTRYPOINT and CMD can be used together.

## Commands and Arguments in a kuberentes pod
 


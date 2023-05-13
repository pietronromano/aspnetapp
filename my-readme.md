# 13-May-2023 
# DevOps Tests
# ORIGINAL SOURCE: https://github.com/dotnet/dotnet-docker.git
# See Source for other Dockerfile images



# Git
.gitignore
bin/
obj/

git init
git commit -m 'First Local Commit'
git status


## remote
git remote -v
git remote add githubrepo https://github.com/pietronromano/aspnetapp
git push githubrepo main

# Asp.Net
dotnet restore
dotnet build
## Creates folder bin/publish
dotnet publish

## Run on http://localhost:5011
dotnet run
## Environment variables
http://localhost:5011/Environment
## Health Check
http://localhost:5011/healthz


# Docker #####################################
--------------------------------------------------------------
# DEBUG
## BEST WAY IS TO Initialize Docker from VS Code: Creates Dockerfile that can be Debugged in the container
## VS Code - > Run -> Add Configuration -> "Add Docker Files to Workspace"
## Build
docker build -t aspnetapp-win -f Dockerfile .
## Run
docker container run  --name aspnetapp -it -p 8000:5011 aspnetapp-win
http://localhost/8000

# Debug -> "Docker .NET Launch"
F5
http://localhost:32768

# Build for ubuntu
docker build --pull -t aspnetapp-ubuntu -f Dockerfile.ubuntu-x64 .

# Run
docker container run  --name aspnetapp -it -p 8000:80 aspnetapp

# Run bash: NOTE: that curl isn't installed on linux by default
docker exec -it aspnetapp bash
# curl http://localhost:8000/Environment


# --------------------------------------------------------------------
# BINDS
## DID WORK ON LINUX!

## DIDN'T WORK ON WINDOWS!!: Didn't do anything - container wasn't watching host folder
docker container run  --name aspnetapp -it -p 8000:80 -v $(pwd):/app  aspnetapp

# VS Code Git Bash didn't work, nor did it from WSL or Terminal
## DIDN'T WORK: /w argument caused this error:
## docker: Error response from daemon: the working directory 'C:/Program Files/Git/app' is invalid, it needs to be an absolute path.
docker container run  --name aspnetapp -it -p 8000:80 -v $(pwd):/app -w "/app" aspnetapp

## DIDN'T WORK: https://docs.docker.com/get-started/06_bind_mounts/
## ERROR: Error response from daemon: failed to create shim task: OCI runtime create failed:
docker container run  --name aspnetapp -it -p 8000:80 --mount src="$(pwd)", target=app aspnetapp bash


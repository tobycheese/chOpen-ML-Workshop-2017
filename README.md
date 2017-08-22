# chOpen-ML-Workshop-2017

This is the repository for my workshop [Machine Learning for Software Developers](https://workshoptage.ch/workshops/machine-learning-fuer-software-entwickler/) at CH Open Workshop Days 2017.

Here you will find the environment to follow the workshop and to run the exercises and the exercises themselves along with suggested solutions. After the workshop is over, I'll upload the slides as well.

## Prepare environment

1. Install [Docker](https://www.docker.com/)
1. Start Docker Service
1. Create a directory to hold workshop files

## Run environment
### Linux
1. Open terminal
1. Execute `docker run -d --init --rm --mount type=bind,src=/home/yourname/your-dir-from-pt-3-above,dst=/data -p 127.0.0.1:8888:8888 tobycheese/workshop2017`

    (the first time you do this, the docker image will be downloaded, which will take a while)

### MacOs
1. Open terminal
1. As above but replace `/home` with `/Users`

### Windows (NEEDS VERIFICATION)
1. Run cmd.exe
1. As above but replace `/home/yourname/your-dir-from-pt-3-above` with `c:\path\to\your-dir-from-pt-3-above`

## Access environment
* Browse to http://127.0.0.1:8888/

## Stop environment
1. Save all your open notebooks
1. Find container id: `docker ps`
1. Stop container: `docker stop <container-id>`
1. Stop Docker service

## Update environment
1. Stop container if running: `docker stop <container-id>`
1. Download updated image: `docker pull tobycheese/workshop2017`

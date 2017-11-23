# Docker

> docker use images to create a container.
>
> Image is the Class - Container is the Object

## BUILD

### Existing images
Pulling an image is to store it on your computer. By default when you start using an image it is automatically pulled.

```bash
docker pull [author/]image[:version]
```
**default version is "latest"**. every version can be found on the docker hub ( hub.docker.com ) or on special repositories.

examples :
Latest PHP Version (PHP is an official library so it doesn't need author)
```
  docker pull php
```

Elastic search version 5.6.2
```
  docker pull docker.elastic.co/elasticsearch/elasticsearch:5.6.2
```

### Create custom image
This is a sample structure of a file to create your own docker image. The file should be named ```Dockerfile``` and for easier usages, be in its folder. Add any needed file/folder for the build at the same level or in a subfolder.

```
# a docker is always built from an existing image.
# it can be a simple image like ubuntu or
# some images more complex like php.
# ALWAYS ON TOP

FROM ubuntu:artful     

# set the environment variable myName with the value.
# can be chained
ENV myName="Henri L." myDish=fish\ and\ chips
ENV myGender Male
ENV myTeam Aboutgoods Company

# Any metadata you require to define your image
# useless 99% of the time
LABEL maintainer="SvenDowideit@home.org.au"

# copy a file from your computer inside the docker
# before running it - for any purpose.
COPY scripts/run.sh /usr/bin/run.sh
# can run a command.
RUN chmod 777 /usr/bin/run.sh & /usr/bin/run.sh
RUN mkdir /data


# change directory. don't use CD.
# used before RUN, CMD, ENTRYPOINT,COPY, ADD
WORKDIR /data

# same as copy but can unzip a file or download an url
ADD https://example.com/script.sh entrypoint.sh

# create a mounting point
VOLUME ["/data"]


# When you expose ports, the person who will
# use your image can bind them to his system.
EXPOSE 8082 8083

# if you need to change shell entry
SHELL ["zsh","-s"]

# the command to run when the docker starts
ENTRYPOINT ["data.sh"]

# if entrypoint fails you can specify a RUN command
RUN /usr/bin/run.sh
```
### Build your image
To build your image :
```
docker build .
```

to specify name for the build :
```
docker build -t my-image[:version] .
```

### Manage images
list images :
```
docker image ls
```

know more about image :
```
docker image inspect image[:version]
```

delete image :
```
docker rm image[:version]
```

## RUN
```
docker run [OPTIONS] IMAGE[:TAG] [COMMAND [ARG...]]
```
Run `docker run hello-world` to test if your docker instance works.

every arguments are listed after. The star marks the most important ones.

- attach (full example)
```
docker run -a stdin -a stdout -i -t ubuntu /bin/bash
```
- Detach
```
-d
```
- Name (useful to define wich container is used instead of random name)
```
docker run -name containerName
# docker run -name hw hello-world
```
- Restart
```
--restart=always
```
- Expose ports
```
-p host:container -p 8080:80
```
- Set environment variables
```
-e "VAR=value" -e "VAR2=value"
```
- Bind mount a volume.
```
-v my-folder:/var -v host:container
```

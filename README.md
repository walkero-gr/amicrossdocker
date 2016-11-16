# AmiCrossDocker

Use this Docker compose file to spin up local crosscompiler environment for AmigaOS 4 with a native Docker app on Linux, Mac OS X and Windows.

AmiCrossDocker is designed to be used for local C/C++ crosscompiling development environment, using GGC for PPC CPUs.

It is based on an Ubuntu container, using the phusion/baseimage with tag 0.9.19. This image is based on Ubuntu 16.04 LTS official image. More info at https://hub.docker.com/r/phusion/baseimage/

AmiCrossDocker is based on the Nicolas Mendoza guide on setting up a crosscompiling development environment.

There are the following tags available

latest  Using GGC v4.4.3
4.4.3   Using GGC v4.4.3
4.2.4   Using GGC v4.2.4


## Container's contents

| Application | Version |
| ----------- | ------- |
| [AmigaOS 4 SDK](http://hyperion-entertainment.biz/index.php/news/36-amigaos-4x/163-aos4fe-sdk) | 53.30 |
| GNU Make | 4.1 |
| ppc-amigaos-gcc | 4.4.3 / 4.2.4 |
| git | 2.7.4 |

## Usage instructions

1\. Install docker for <a href="https://docs.docker.com/engine/installation/" target="_blank">Linux</a>, <a href="https://docs.docker.com/engine/installation/mac" target="_blank">Mac OS X</a> or <a href="https://docs.docker.com/engine/installation/windows" target="_blank">Windows (10+ Pro)</a>. For Mac and Windows make sure you're installing native docker app version 1.12, not docker toolbox.

For Linux additionally install <a href="https://docs.docker.com/compose/install/" target="_blank">docker compose</a>

2\. Download <a href="https://raw.githubusercontent.com/walkero-gr/amicrossdocker/master/docker-compose.yml" target="_blank">the compose file</a> from this repository and put it to your project codebase (you might want to add docker-compose.yml to .gitignore).

3\. Since containers <a href="https://docs.docker.com/engine/tutorials/dockervolumes/" target="_blank">do not have a permanent storage</a>, directories from the host machine (volumes) should be mounted: one with code of projects. Create a folder named "projects" in the same directory.

By default, the directory named 'projects' will be mounted to UBU container, assuming it's your codebase directory. Do not forget to add `projects` to your .gitignore file. The `projects` folder is mounted under `/home/projects/` inside the container.

You may also change the folder name, but this change must be edited in the `docker-compose.yml` file, as well.

4\. Now, let's run the compose file. It will download the images and run the containers:
```bash
$ docker-compose up -d
```

5\. Make sure all containers are running by executing:

```bash
$ sudo docker-compose ps
```

6\. That's it!

## Accessing the container

You can connect to the container by executing the following command:
```bash
$ sudo docker-compose exec ubu /bin/bash
```

To test the GCC installation, connect to the container and execute the following commands
```bash
$ ppc-amigaos-gcc --version
$ echo "int main(void) {return 0;}" | ppc-amigaos-gcc -xc - -o /tmp/ppc-amigaos-test-exec
$ file /tmp/ppc-amigaos-test-exec
```

The above should give the following result:
/tmp/ppc-amigaos-test-exec: ELF 32-bit MSB executable, PowerPC or cisco 4500, version 1 (SYSV), statically linked, not stripped

More info about the compiler you can get by executing the following command
```bash
$ ppc-amigaos-gcc -dumpspecs
```

## Status

We're actively working on these containers. More options will be added soon. If you have a feature request or found a bug please submit an issue.

We update the containers from time to time, to get the latest changes simply run again:
```
$ sudo docker-compose up -d
```




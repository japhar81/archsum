
TODO: SSH Instructions Go Here

## Hello World

A docker container is basically an application. So running a basic container in Docker is as easy as executing any other application;
```bash
docker run hello-world
```

You should see the container be downloaded, and then a Hello World message will appear. What just happened;
1. Docker was asked to run an image named `hello-world` which did not exist on the machine
2. By default, Docker searches DockerHub (https://hub.docker.com) for images — so `hello-world` was really [https://hub.docker.com/\_/hello-world][1]
3. Containers are versioned using `tags`. Because one was not specified in the command, it defaults to `latest`
4. This container was pulled (downloaded) locally — run `docker images` to see all images that are local
5. Then it was executed from the local download

## Pulling a Base Container

Most containers you create will start with another container as a base. You will essentially extend them. Go ahead and pull CentOS 7.6.1810 now so it’s available for the rest of the lab;
```bash
docker pull centos:centos7.6.1810
```

Verify it’s there;
```bash
docker images
```

## Running a Container
Run a test command in CentOS;
```bash
docker run centos:centos7.6.1810 echo "Test"
```

Check for the container;
```bash
docker ps
```
There’s nothing there — once your echo command ended, the container exited.
```bash
docker ps -a
```
Using `-a` shows you exited containers as well.

The important thing to note is that there is no init system in a Container. `docker run` will start a single process. Once it exits, so does the container. This is commonly referred to as the `entrypoint`. Keep in mind that while there is a single `entrypoint`, there is nothing stopping you from forking off multiple long-running processes.

Start an interactive long-running process (bash);
```bash
docker run -it centos:centos7.6.1810 bash
```
Make sure the prompt changes to: `[root@e089e5dc1dd8 /]# `

You’re now in a shell inside the container. Just like SSH. Let’s go crazy;
```bash
ls
rm -rf / --no-preserve-root
ls
```
Well, that’s all hosed. Lets kill it;
```bash
exit
```

Now, run it again;
```bash
docker run -it centos:centos7.6.1810 bash
```
Make sure the prompt changes to: `[root@e089e5dc1dd8 /]# ` and run;
```bash
ls
```
Everything is back!

A container’s filesystem is writeable, but **only** for the lifetime of that container. Once it exits, all changes are lost, and the next run will start from the same point in time (e.g. `tag`)

## Cleanup
Exited containers continue to live on disk. It’s handy to clean them up on occasion;
```bash
docker rm $(docker ps -a -q -f status=exited)
```

## Building A Container
*NOTE: Because ports and containers are shared between users on a VM, please use your username and pick a random port # between 1024 and 65535 for this example.*
```bash
cd 01_dockerfile
docker build -t nginxtest:<your username> .
docker run -p <random number>:80 nginxtest:<your username>
```

In a separate tab, test by doing `curl localhost:<random number>`

Take a look at `Dockerfile` to see what has happened. Essentially, Docker was told to start with a container that had nginx pre-installed, copy over some content and an entry point script, and then start that entry point. Take special note of the `RUN` command. For the most part, if you can do it in `bash`, you can do it in `RUN`. This includes things like `rpm -i` and `yum`.

[1]:	https://hub.docker.com/_/hello-world
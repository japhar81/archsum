## Accessing the Lab Server

All lab exercises will require you to be SSHd into a lab server we have set up with all the tools you need.

```bash
ssh openshift-lab.secureworkslab.net
```

Use your lab credentials to authenticate.

## Hello World

A docker container is basically an application. So running a basic container in Docker is as easy as executing any other application;

Run this command:

```bash
docker run hello-world
```

You just told docker to run a _container_ based on an _image_ named `hello-world`. You may see the image be downloaded (unless it has already been downloaded to your machine), and then a Hello World message will appear. Here's what just happened behind the scenes;

1. Docker was asked to run an image named `hello-world` which did not exist on the machine
2. By default, Docker searches DockerHub (https://hub.docker.com) for images — so `hello-world` was really [https://hub.docker.com/\_/hello-world][1]
3. Containers are versioned using `tags`. Because one was not specified in the command, it defaults to `latest`
4. This container was pulled (downloaded) locally — run `docker images` to see it and all other images that are downloaded locally
5. The local image was executed (run as a container)
6. The container printed its message and exited.

## Pulling a Base Container

Most images you create will start with another image as a base. You will essentially extend the base image by adding more configuration commands. Go ahead and pull (download to your local machine) CentOS 7.6.1810 now so it’s available for the rest of the lab;

```bash
docker pull centos:centos7.6.1810
```

Verify it’s there;

```bash
docker images
```

## Running a Container

When you execute a docker container, it must be given a single command to run.

Run a test command in a container using the CentOS image you pulled;

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

The important thing to note is that there is no init system in a container. `docker run` will execute a single process. Once it exits, so does the container. This is commonly referred to as the `entrypoint`. Keep in mind that while there is a single `entrypoint`, there is nothing stopping you from forking off multiple long-running processes.

Start an interactive long-running process (bash);

```bash
docker run -it centos:centos7.6.1810 bash
```

Make sure the prompt changes to: `[root@############ /]#`

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

Make sure the prompt changes to: `[root@e089e5dc1dd8 /]#` and run;

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

_NOTE: Because ports and containers are shared between users on a VM, please use your username and pick a random port # between 1024 and 65535 for this example._

For this next exercise, you will need some file from the git repository. They have been cloned onto the lab server. Use the following command to copy them to your home directory;

```bash
cp -r /repo/01_dockerfile ~/
ls ~/01_dockerfile/
```

Look in this directory. By default, Docker looks for a file named `Dockerfile` to give it instructions on how to build a new image. Use this command to build a new docker image using the given Dockerfile;

```bash
cd ~/01_dockerfile
docker build -t nginxtest:<your username> .
docker run -d --name lab_nginx-<your username> -p <random number>:80 nginxtest:<your username>
```

_If you get an error that the random port you chose is in use, choose another. Someone else on the server must be running something on the originally chosen port._

The `-d` makes the container run in the background, or _daemoinzed_ mode. This allows you to use the same terminal to test. The `--name` gives docker a name to refer to this container by. Test by:

```bash
curl localhost:<random number>
```

You will see that nginx has responded to your request. Look at the resulting logs in the running container with:

```bash
docker logs lab_nginx-<your username>
```

You should see a reference to the GET request you issued with curl.

Take a look at the contents of `Dockerfile` to see what has happened. Essentially, Docker was told to start with an image that had nginx pre-installed, copy over some content and an entry point script, and then start that entry point. Take special note of the `RUN` command. For the most part, if you can do it in `bash`, you can do it in `RUN`. This includes things like `rpm -i` and `yum`.

Time to clean up after yourself. Stop your container before moving on to the next lab:

```bash
docker stop lab_nginx-<your username>
```

Remember earlier, when we said exited containers continue to live on disk? Use `docker ps -a` to see your stopped, but still existing, container. Then remove it from disk using:

```bash
docker rm -f lab_nginx-<your username>
```

...and move on to lab 2.

[1]: https://hub.docker.com/_/hello-world

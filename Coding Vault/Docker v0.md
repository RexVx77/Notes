CH1: Install

# What Is Docker?

> Docker makes development efficient and predictable
> 
> Docker takes away repetitive, mundane configuration tasks and is used throughout the development lifecycle for fast, easy and portable application development – desktop and cloud. Docker's comprehensive end to end platform includes UIs, CLIs, APIs and security that are engineered to work together across the entire application delivery lifecycle.
> 
> -- The [Docker](https://www.docker.com/) team

Put simply: Docker allows us to deploy our applications inside "containers", which are kind of like _very_ lightweight virtual machines. Instead of just shipping an application, we can ship an application _and the environment it runs in_.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/FGpjTCQ-422x585.jpg)

# Docker Hub

[Docker Hub](https://hub.docker.com/) is the official cloud service for storing and sharing Docker images (we'll talk about images later, chill plz).

We'll use Docker Hub in this course, but there are other popular alternatives, and they're usually coupled with cloud service providers. For example,

- [AWS ECR](https://aws.amazon.com/ecr/)
- [GCP Artifact Registry](https://cloud.google.com/artifact-registry/docs)
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)

For most of my career, if my company used AWS to _deploy_, we used AWS to _host our images_. If we used GCP to deploy, we hosted images on GCP. I'd usually just use whatever's most convenient and cost effective, the features are very similar between providers.

---

CH2: Containers

# Containers

> A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.
> 
> -- [Docker](https://www.docker.com/resources/what-container/)

We've had virtual machines (like [VirtualBox](https://www.virtualbox.org/wiki/Downloads)) for a _long_ time. The trouble with virtual machines is that _they're slow as h`*`ck_. Booting one up usually takes _longer_ than a physical machine.

Containers, on the other hand, gives us 90% of the benefits of virtual machines, but are _super_ lightweight. _Containers boot up in seconds, while virtual machines can take minutes._

## Virtual Machine Architecture

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/diagrams/learn-docker/2-containers/1-containers/how_vms_work.png)

## Container (Docker) Architectures

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/diagrams/learn-docker/2-containers/1-containers/how_docker_containers_work.png)

## Why Are Containers Lightweight?

Virtual machines virtualize _hardware_, they emulate what a physical computer does at a low level. Containers virtualize at the _operating system_ level. Isolation between containers that are running on the same machine is still _really good_. For the most part, each container _feels like_ it has its own operating system and filesystem. In reality, a lot of resources are being shared, but they're being shared securely through [namespaces](https://docs.docker.com/engine/security/userns-remap/).

# Images

So a "container" is kinda like a lightweight VM, great... so what's an _image_?

- **Image**: A read-only _definition_ of a container
- **Container**: an _instance_ of a virtualized read-write environment

A container is basically an image that's **actively running**. In other words, you boot up a container _from_ an image. You can create multiple separate containers all from the same image (it's _kinda_ like the relationship between classes and objects).

# Run a Container

Now that you've downloaded the [getting started image](https://hub.docker.com/r/docker/getting-started), let's use it to _run_ a new container. The `docker run` command starts a new container from an image. Let's break down the syntax:

```bash
# this is just an example, don't run this
docker run -d -p hostport:containerport namespace/name:tag
```

- `-d`: Run in detached mode (doesn't block your terminal)
- `-p`: Publish a container's port to the host (forwarding)
- `hostport`: The port on your local machine
- `containerport`: The port inside the container
- `namespace/name`: The name of the image (usually in the format `username/repo`)
- `tag`: The version of the image (often `latest`)

## Assignment

1. [ ] Use the `run` command to start a new container from the "getting started" image:

```bash
docker run -d -p 8965:80 docker/getting-started:latest
```

2. [ ] You should see the container running in the "Containers" tab of Docker Desktop. Run this on the command line to see the running containers:

```bash
docker ps
```

On one of the columns you should see this:

```
PORTS
0.0.0.0:8965->80/tcp
```

This is saying that port `8965` on your local "host" machine is being forwarded to port `80` on the running container. Port `80` is conventionally used for HTTP web traffic. Navigate to `http://localhost:8965` and you should see a webpage served from the container!

# Stop a Container

Okay, now you know how to start a new instance of a container from an image... but how do you _stop_ it? There are two primary ways:

- `docker stop`: This stops the container by issuing a `SIGTERM` signal to the container. You'll typically want to use `docker stop`.
- `docker kill`: This stops the container by issuing a `SIGKILL` signal to the container. This is a more forceful way to stop a container, and should be used as a last resort.
- `docker rm CONTAINER_ID`: This will delete the container.
-  `docker restart CONTAINER_ID`: This will restart the container; same as running stop and then run with the same command.

---

CH3: Storage

# Volumes

By default, Docker containers don't retain any state from _past_ containers. For example, if I:

1. Start a container from an image
2. Make some changes to the filesystem (like installing a new package) in that container
3. Stop the container
4. Start a new container from the same image
5. The new container does _not_ have the changes I made in step 2.

However, if I restart the _stopped_ container, it _will_ have the changes I made. This is only worth mentioning because sometimes developers think that killing an old container and starting a new one is the same as _restarting a process_ - but that's not true... it's more like resetting the state of the _entire machine_ to the original image.

All this said, Docker _does_ have ways to support "persistent state" through [storage volumes](https://docs.docker.com/storage/volumes/). They're basically a filesystem that lives outside of the container, but can be accessed by the container.

Click to hide video

## Assignment

[Ghost](https://ghost.org/) is an open-source blogging software, kind of like [WordPress](https://wordpress.org/). As you can imagine, blogging software that doesn't save your blog posts, would be pretty useless, so we'll install Ghost and use volumes to persist our data!

1. [ ] Create a new empty volume called `ghost-vol`:

```bash
docker volume create ghost-vol
```

2. [ ] Make sure it worked:

```bash
docker volume ls
```

3. [ ] Inspect the volume to see where it is on your local machine:

```bash
docker volume inspect ghost-vol
```

# Run Ghost

Cool, we've got a named volume ready to go. Time to run Ghost in Docker.

## Assignment

Docker hosts an official image for Ghost on [Docker Hub](https://hub.docker.com/_/ghost).

1. [ ] Pull the Ghost image from Docker Hub:

```bash
docker pull ghost
```

2. [ ] Run the Ghost image in a new container:

```bash
docker run -d -e NODE_ENV=development -e url=http://localhost:3001 -p 3001:2368 -v ghost-vol:/var/lib/ghost ghost
```

- `-d` runs the image in detached mode to avoid blocking the terminal.
- `-e NODE_ENV=development` sets an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) within the container. This tells Ghost to run in "development" mode (rather than "production", for instance)
- `-e url=http://localhost:3001` sets another environment variable, this one tells Ghost that we want to be able to access Ghost via a URL on our host machine.
- We've used `-p` before. `-p 3001:2368` does some [port-forwarding](https://en.wikipedia.org/wiki/Port_forwarding) between the container and our host machine.
- `-v ghost-vol:/var/lib/ghost` mounts the `ghost-vol` volume that we created before to the `/var/lib/ghost` path in the container. Ghost will use the `/var/lib/ghost` directory to persist stateful data (files) between runs.

3. [ ] Navigate to `http://localhost:3001/` in your browser, you should see your new Ghost CMS!

# Persist Quiz

- A _container's_ file system is _read-write_, but when you delete a container, and start a new one from the same image, that new container starts from scratch again with a copy of the image. All stateful changes are lost.
- A _volume's_ file system is _read-write_, but it lives _outside_ a single container. If a container uses a volume, then stateful changes can be persisted to the volume even if the container is deleted.

Volumes are often used by applications like Ghost, Grafana, or WordPress to persist data so that when a container is deleted and a new one is created the state of the application isn't lost. Containerized applications are typically thought of as _ephemeral_ (temporary). If your application breaks just because you deleted and recreated a container... it's not a very good containerization!

# Delete a Volume

Now that we're done playing with Ghost, let's save the space on our host machine by deleting the volume.

## Assignment

1. [ ] Use `docker ps -a` to see _all_ containers, even those that aren't running.
2. [ ] Stop the running Ghost container
3. [ ] Remove the ghost container. Use `docker --help` to find the right command.
4. [ ] Remove the `ghost-vol` volume. Use `docker volume --help` to find the right command.

Now that it's gone, let's see what happens if we try to start the Ghost container back up and attach it to a volume that doesn't exist.

```bash
docker run -d -e NODE_ENV=development -e url=http://localhost:3001 -p 3001:2368 -v ghost-vol:/var/lib/ghost ghost
```

Navigate to `http://localhost:3001/` in your browser, and you should see a fresh CMS. That's weird, why no errors?

Run:

```bash
docker volume ls
```

The `ghost-vol` is back from the dead!?! It turns out the `-v ghost-vol:/var/lib/ghost` flag binds to a "ghost-vol" volume if it exists, otherwise, it creates it automatically!

So, we now have a fresh installation. Our post that was on the old volume is gone, but this new volume will persist if we don't delete it.

---

CH4: Execute

# Help

Like most CLI applications, `docker` has a help menu, either of these will work:

- `docker --help`
- `docker help`

# Exec

When it comes to _deploying_ applications with Docker, you'll usually just let the container do its thing. For example, the Ghost container we ran in the last chapter started up its own web server (based on the image configuration). We didn't need to run any manual commands in addition to just starting the container.

That said, it _is_ possible to run commands inside a running container! It's kinda like the container version of [ssh](https://www.ssh.com/ssh/)ing into a remote server and running a command.

## Assignment

1. [ ] List your running containers:

```bash
docker ps
```

2. [ ] Start up the "getting started" container again:

```bash
docker run -d -p 8965:80 docker/getting-started
```

3. [ ] Ensure that it's running:

```bash
docker ps
```

4. [ ] Run an `ls` command _from inside the container_ using the `docker exec` command:

```bash
docker exec CONTAINER_ID ls
```

5. [ ] Create a new `hacker.log` file in the working directory of the container by running `touch hacker.log` inside the container.
6. [ ] Run the `ls` command again to make sure that the file was created.

You should get a list of all the files and directories in the working directory (which happens to be the root in this case) of the container!

# Exec Netstat

I'm curious about what software the "getting started" container is using to serve a webpage.

## Assignment

The [netstat](https://manpages.ubuntu.com/manpages/noble/man8/netstat.8.html) command shows us which programs are bound to which ports. We're looking for the process bound to port `80`, that is, the one serving the webpage.

Run the `netstat -ltnp` command inside the container.

# Live Shell

Being able to run one-off commands is nice, but it's often more convenient to start a shell session running within the container itself. Thats where the `-i` and `-t` flags come in:

- `-i` makes the `exec` command interactive
- `-t` gives us a [tty (keyboard) interface](https://en.wikipedia.org/wiki/Tty_\(Unix\))
- Running `/bin/sh` gives us a shell session inside the container

For example:

```bash
docker exec -it CONTAINER_ID /bin/sh
```

## Assignment

1. [ ] Start a new shell session inside the getting started container.
2. [ ] Change into the `usr/share/nginx/html/` directory. If you're in the right spot, this is the root of files served by the nginx web server!
3. [ ] Open the website in your browser at `http://localhost:8965`. Notice it automatically redirects you to the `/tutorial` page.
4. [ ] `cd` into the tutorial directory.
5. [ ] Let's "hack" the site! Overwrite the contents of `index.html` with the string `I hacked you!`:
    
    ```bash
    echo "I hacked you!" > index.html
    ```
    
6. [ ] Refresh the page to make sure it worked.
7. [ ] Exit the container shell session with the `exit` command.

---

CH5: Networks

# Offline

We've already done a bit of networking in Docker. Specifically, we've exposed containers to the host network on various ports and accessed web traffic.

Now, let's force a container into _offline_ mode!

You might be thinking, "why would I want to turn off networking?!?" Well, usually it's for security reasons. You might want to remove the network connection from a container in one of these scenarios:

- You're running 3rd party code that you don't trust, and it shouldn't need network access
- You're building an e-learning site, and you're allowing students to execute code on your machines
- You know a container has a virus that's sending malicious requests over the internet, and you want to do an audit

## Network None

The `docker run` command has a `--network none` flag that makes it so that the container can't network with the outside world, which is super useful for isolating containers. It stops the container from connecting to any external networks.

# Break the Network

Let's quarantine a container and make sure that we _can't_ reach the outside world.

The `ping` command allows you to check for connectivity to a host. For example we can ping `google.com`:

```bash
ping google.com
```

If everything is working, we'll see a response like:

```
64 bytes from 142.250.189.14: icmp_seq=0 ttl=119 time=13.636 ms
64 bytes from 142.250.189.14: icmp_seq=1 ttl=119 time=17.735 ms
64 bytes from 142.250.189.14: icmp_seq=2 ttl=119 time=17.691 ms
```

You can kill the `ping` command with Ctrl+C.

## Assignment

1. [ ] Use `docker ps`, `docker stop`, and `docker rm` to stop and remove the "getting started" container if it's running.
2. [ ] Start a new "Getting Started" container in `--network none` mode:

```bash
docker run -d --network none docker/getting-started
```

3. [ ] Run the `ping` command with a timeout of 2 seconds inside the container:

```bash
docker exec CONTAINER_ID ping google.com -W 2
```

If all goes well, the program should hang for 2 seconds, then **report an error message**, because you don't have internet access!

# Load Balancers

Let's try something a bit more complex: configuring a [load balancer](https://www.cloudflare.com/learning/performance/what-is-load-balancing/)!

A load balancer behaves as advertised: it balances a load of network traffic across some number of servers. Think of a _huge_ website like `Google.com`. There's _no way_ that a single server (literally a single computer) could handle all of the Google searches for the entire world. Google uses load balancers to route requests to different servers.

A central server, called the "load balancer", receives traffic from users (aka clients), then routes those requests to different back-end application servers. In the case of Google, this splits the world's traffic across potentially many different thousands of computers.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/diagrams/learn-docker/5-networks/3-load-balancers/load_balancer.png)

A _good_ load balancer sends new traffic to servers that have lower current resource utilization (CPU and memory). The goal is to "balance the load" so that no single backend server becomes overwhelmed. There are [many strategies](https://www.cloudflare.com/learning/performance/types-of-load-balancing-algorithms/) that load balancers use, but a simple strategy is the "round robin" where requests are simply routed one after the other to different back-end servers:

- Request 1 -> Server 1
- Request 2 -> Server 2
- Request 3 -> Server 3
- Request 4 -> Server 1
- Request 5 -> Server 2
- ...

# Application Servers

First, we need to start some application servers so that our load balancer has somewhere to send the traffic! We'll use [Caddy](https://caddyserver.com/), an awesome open-source load balancer _and_ web server. [Nginx](https://www.nginx.com/) and [Apache](https://httpd.apache.org/) are other popular alternatives that do similar things, but Caddy is a modern version written in Go, so it'll be fun to use.

## What Will Our Servers _Do_?

Each application server will serve a _slightly different_ HTML webpage. We'll make them different so that we can see load balancing in action!

## Assignment

1. [ ] Pull down the [official `caddy`](https://hub.docker.com/_/caddy) image:

```bash
docker pull caddy
```

2. [ ] Create an `index1.html` file in your working directory:

```html
<html>
  <body>
    <h1>Hello from server 1</h1>
  </body>
</html>
```

3. [ ] Create an `index2.html` file in your working directory:

```html
<html>
  <body>
    <h1>Hello from server 2</h1>
  </body>
</html>
```

4. [ ] Run a container for `index1.html` on port `8881`:

```bash
docker run -d -p 8881:80 -v $PWD/index1.html:/usr/share/caddy/index.html caddy
```

5. [ ] Run a container for `index2.html` on port `8882`:

```bash
docker run -d -p 8882:80 -v $PWD/index2.html:/usr/share/caddy/index.html caddy
```

6. [ ] Navigate to `localhost:8881` in a browser. You should see "Hello from server 1".
7. [ ] Navigate to `localhost:8882` in a browser. You should see "Hello from server 2".

# Custom Network

We can create custom [bridge](https://docs.docker.com/network/bridge/) networks so that containers can communicate with each other if we want them to, but still otherwise remain isolated. Let's build a system where our application servers are hidden within a custom network, and only our load balancer is exposed to the host.

This is a very common setup in backend architecture. The load balancer is exposed to the public internet, but the application servers are only accessible _via_ the load balancer.

## Assignment

1. [ ] Let's create a custom bridge network called "caddytest".

```bash
docker network create caddytest
```

2. [ ] See if it worked by listing all the networks:

```bash
docker network ls
```

3. [ ] Stop the containers and spin up new ones with the same `docker run` command from the previous lesson, but this time, make sure you attach them to the `caddytest` network.
    - Use the `--network caddytest` flag to attach them to the network
    - Use the `--name` flag to name them `caddy1` and `caddy2` respectively so it's easier to reference them later
    - Place these flags before the image name (`caddy`) in your `docker run` command. For example:
        
        ```bash
           docker run -d --name caddy1 --network caddytest -v $PWD/index1.html:/usr/share/caddy/index.html caddy
        ```
        
    - Do _not_ use the `-p` flag to expose ports. We don't want these accessible from the host machine.
4. [ ] Create another "getting started" container on the same network and start a shell session within it:

```bash
docker run -it --network caddytest docker/getting-started /bin/sh
```

By giving our containers some names, `caddy1` and `caddy2`, and providing a bridge network, Docker has set up name resolution for us! The container names resolve to the individual containers from all other containers on the network.

5. [ ] Within your `docker/getting-started` container shell, [curl](https://curl.se/) the first container:

```bash
curl caddy1
```

6. [ ] Also `curl` the second container:

```bash
curl caddy2
```

7. If you get the HTML responses that you expect, `exit` out of your shell session within the "getting started" container.

**Run and submit** the CLI tests.

If you need to restart your caddy application servers after naming them, you can use: `docker start caddy1` and `docker start caddy2`.


# Configuring the Load Balancer

We've confirmed that we have 2 application servers (Caddy) working properly on a custom bridge network. Let's create a load balancer that balances network requests between the two! We'll use a round-robin balancing strategy, so each request should route back and forth between the servers.

## Caddyfiles

Caddy works great as a file server, which is what our little HTML servers are, but it also works great as a load balancer! To use Caddy as a load balancer we'll need to create a custom [Caddyfile](https://caddyserver.com/docs/caddyfile) to tell Caddy how we want it to balance the traffic. It's just a config file for Caddy.

## Assignment

1. [ ] Stop and remove any containers that aren't the 2 caddy servers we're working with currently.
2. [ ] Create a new file in your local directory called `Caddyfile`:

```
localhost:80

reverse_proxy caddy1:80 caddy2:80 {
	lb_policy       round_robin
}
```

This tells Caddy to run on `localhost:80`, and to round robin any incoming traffic to `caddy1:80` and `caddy2:80`. Remember, this only works because we're going to run the loadbalancer _on the same network_, so `caddy1` and `caddy2` will automatically resolve to our application server's containers.

3. [ ] Start the load balancer container on port `8880`. Instead of an `index.html`, give it our custom `Caddyfile`:

```bash
docker run -d --network caddytest -p 8880:80 -v $PWD/Caddyfile:/etc/caddy/Caddyfile caddy
```

4. [ ] Hit the load balancer on `http://localhost:8880/`! You should either get a response from server 1 or server 2, and if you hard refresh the page, it should swap back and forth.

If it's not swapping properly, try using `curl` instead. Your browser might be caching the HTML.

```bash
curl http://localhost:8880/
```

---

CH6: Dockerfiles

# Dockerfiles

Docker isn't _only_ useful for running _other_ people's software (as we've been doing so far). It's also a great way to build and package our own software.

I've used Docker both ways. As a DevOps/platform engineer I'm usually using other's images, but as a backend developer I was usually building images for our own servers.

Docker images are built from _Dockerfiles_. A Dockerfile is just a text file that contains all the commands needed to assemble an image. It's essentially the ["Infrastructure as Code"](https://en.wikipedia.org/wiki/Infrastructure_as_Code) (IaC) for an image. It runs commands from top to bottom, kind of like a shell script.

Instead of manually installing dependencies on servers and making updates manually, we can check a Dockerfile into source control and build it automatically. Mhmmmm, _automation_.

## Assignment

1. [ ] Create a file called `Dockerfile` in your working directory. If you're using VS Code, I'd recommend installing the [Docker extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker). It will give you some nice syntax highlighting.
2. [ ] Inside the Dockerfile add these lines of text:

```dockerfile
# This is a comment

# Use a lightweight debian os
# as the base image
FROM debian:stable-slim

# execute the 'echo "hello world"'
# command when the container runs
CMD ["echo", "hello world"]
```

3. [ ] Build a new image from the Dockerfile and call it `helloworld`:

```bash
docker build . -t helloworld:latest
```

The `-t helloworld:latest` flag tags the image with the name "helloworld" and the "latest" tag. Names are used to organize your images, and tags are used to keep track of different [versions](https://en.wikipedia.org/wiki/Software_versioning).

4. [ ] Run your image in a new container:

```bash
docker run helloworld
```

_If all went well, you'll see "hello world" printed to the console!_

5. Run `docker ps`. You'll notice that your container is _not_ running anymore! All it did was print and exit. Just like regular programs, docker containers can execute simple commands that exit quickly, or they can execute servers that run until killed. It just depends on the command you give it.
    
6. See the stopped container with `docker ps -a`.
    
7. Delete the Dockerfile, we don't need it anymore.

# Dockerizing the Server

Now that you know how to run your server manually, let's run it in Docker! The steps are simple:

1. Build the Server
2. Create a Dockerfile
3. Build an image using the Dockerfile (which will copy in the built server)
4. Run the image in a container

## Assignment

1. [ ] Create a `Dockerfile` in the root of your server's repo. Let's start with a simple lightweight [Debian Linux OS](https://www.debian.org/):

```dockerfile
FROM debian:stable-slim
```

2. [ ] Add a [`COPY`](https://docs.docker.com/engine/reference/builder/#copy) command on the next line of your `Dockerfile`. In the case of a simple compiled Go server, all we need is the compiled program itself!

```dockerfile
# COPY source destination
COPY goserver /bin/goserver
```

Replace the first "goserver" with the name of _your_ server executable if it's different.

The [ADD](https://docs.docker.com/engine/reference/builder/#add) command would also work here, but `COPY` is fine because we don't need the extra functionality that `ADD` offers.

3. [ ] Add a `CMD` command as the last line in the `Dockerfile`. This automatically starts the server process in the container when we run it.

```dockerfile
CMD ["/bin/goserver"]
```

4. [ ] Build your Dockerfile into an image.

```bash
docker build . -t goserver:latest
```

5. [ ] Start a new container from the image. Be sure to forward the ports to your host machine.

```bash
docker run -p 8010:8010 goserver
```

If you get an `exec format error`, it's probably because you built the go server for your local architecture, but you're trying to run it on a Linux OS! To fix it, rebuild the binary (and then the Dockerfile) with these environment variables:

`GOOS=linux GOARCH=amd64 go build`

6. [ ] You should be able to access your server from the browser just like before, but this time it's running inside of Docker!

# Creating an Environment

You may be thinking, "What's the point of dockerizing this simple service"? Well, at the moment, there are only a couple of benefits:

- Anyone with Docker can run your image, regardless of their OS
- You can easily deploy containers of your image on any cloud service that uses images (most of them) or on an orchestration server like [Kubernetes](https://kubernetes.io/).
- If your server were written in a language like Python or JavaScript, you could bundle the interpreter and dependencies inside the image so that you don't need to reconfigure them on the server.

That said, because our app is so simple, there's just not much environment required, and one of the best things about Docker is that it allows you to ship an entire environment.

So... let's make it more interesting!

## Assignment

We're going to make the port that our server binds to configurable: it will be set by an environment variable.

1. [ ] In `main.go`, find the line that sets `port` to a hard-coded value of `8010` and update it so that it reads an environment variable called `PORT`. You can use [`os.Getenv`](https://pkg.go.dev/os#Getenv):

```go
port := os.Getenv("PORT")
```

Make sure that the `os` package is imported:

```go
import (
	"fmt"
	"log"
	"net/http"
	"os"
	"time"
)
```

2. [ ] Change the port to 8999 by setting an environment variable in your shell:

```bash
export PORT="8999"
```

3. [ ] Rebuild and run your Go program and make sure it serves on port 8999. If you still have `GOOS` and `GOARCH` set from the last lesson, unset them first. Remember to replace `goserver` with the name of your binary.

```bash
go build
./goserver
```

4. [ ] Add an [ENV command](https://docs.docker.com/engine/reference/builder/#env) to your Dockerfile to set the port within the image. You'll need to do it _before_ the `CMD` command so that the environment variable is set before the server starts.

```dockerfile
ENV PORT=8991
```

5. [ ] Rebuild your Docker image:

If you're not on Linux, rebuild the Go binary for Linux first: `GOOS=linux GOARCH=amd64 go build`

```bash
docker build . -t goserver:latest
```

6. [ ] Rerun your Docker container, be sure to expose the correct port:

```bash
docker run -p 8991:8991 goserver
```

**Run and submit** the CLI tests from **your working directory**.

# Dockerizing Python Error

Let's Dockerize the Python script!

## Assignment

1. [ ] Create a new Dockerfile, call this one `Dockerfile.py` instead:

```dockerfile
FROM debian:stable-slim
COPY main.py main.py
COPY books/ books/
CMD ["python", "main.py"]
```

Notice that we're moving the Python code (because it's not a compiled program) as well as the data files into the image.

2. [ ] Build the image, be sure to specify the Dockerfile name because we're not using the default `"Dockerfile"` name:

```bash
docker build -t bookbot -f Dockerfile.py .
```

3. [ ] Run the image in a container:

```bash
docker run bookbot
```

Ahh! You _should_ get an error here.

# Dockerizing Python

Okay, so our first attempt at Dockerizing the Python script didn't work... let's fix it!

The problem is our image doesn't have the `python` interpreter installed, so it can't run the script.

## Assignment

1. [ ] Update the `Dockerfile.py` to use [RUN](https://docs.docker.com/engine/reference/builder/#run) commands to install the necessary dependencies before the script is started. If you're up for the challenge, figure out how to install Python on your own. (If not, I've included the Dockerfile below in the `tip` section.)
2. [ ] Rebuild the image:

```bash
docker build -t bookbot -f Dockerfile.py .
```

It might take a few minutes to build the image... we're installing an entire Python interpreter after all!

Maybe now you can see why I like Go so much...

3. [ ] Run the image in a new container:

```bash
docker run bookbot
```

If Bookbot ran, then you did it correctly! We've bundled up the Python script, its required data, and its required runtime all into a nice little container image!

**Run and submit** the CLI tests.

Common technologies often have images available for ease of use. See the [Python Official Image](https://hub.docker.com/_/python) from [DockerHub Official Images](https://hub.docker.com/search?image_filter=official&q=).

## Tip

```dockerfile
# Build from a slim Debian/Linux image
FROM debian:stable-slim

# Update apt
RUN apt update
RUN apt upgrade -y

# Install build tooling
RUN apt install -y build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev

# Download Python interpreter code and unpack it
RUN wget https://www.python.org/ftp/python/3.10.8/Python-3.10.8.tgz
RUN tar -xf Python-3.10.*.tgz

# Build the Python interpreter
RUN cd Python-3.10.8 && ./configure --enable-optimizations && make && make altinstall

# Copy our code into the image
COPY main.py main.py

# Copy our data dependencies
COPY books/ books/

# Run our Python script
CMD ["python3.10", "main.py"]
```

---

CH7: Debug

# Docker Logs

When containers are running in detached mode with the `-d` flag, you don't see any output in your terminal, which is nice for keeping your terminal clean, but what if something goes _wrong_?

_Enter the `docker logs` command_.

```bash
docker logs [OPTIONS] CONTAINER
# CONTAINER can be an id or name
```

## Assignment

1. [ ] Let's run the Linux `alpine` image in a new container in detached mode, and give it a simple command to run to generate some standard output:

```bash
docker run -d --name logdate alpine sh -c 'while true; do echo "LOGGING: $(date)"; sleep 1; done'
```

The `sh -c 'while true; do echo "LOGGING: $(date)"; sleep 1; done'` part is just a simple shell script to execute inside the container that prints the current date and time every second.

2. [ ] Find the ID of the running container with `docker ps`.
3. [ ] Use the `docker logs` command to view the logs of the container.

Notice that if you run `docker logs` over and over again, you will get different output. That's because you're only getting the most recent logs each time.

4. [ ] Add the `-f` flag to the `docker logs` command to follow the logs in real-time.
5. [ ] Exit the logs with Ctrl+C, then view only the most recent 5 logs with the `--tail` option:

```bash
docker logs --tail 5 CONTAINER
```

**Run and submit** the CLI tests, then stop and remove the container.

# Stats

Okay, we know how to inspect a containers logs, but what if we want to see the resource utilization?

It's common to spin up some Docker containers, forget about them, and then wonder why your host machine has gotten really slow. It's really nice to see how much RAM/CPU each container is using, and it's _critical_ in production environments.

The `docker stats` command gives you a live data stream of resource usage for running containers.

```bash
docker stats [OPTIONS] [CONTAINER...]
```

## Assignment

1. [ ] The pre-built [stress-ng](https://hub.docker.com/r/alexeiled/stress-ng) image is a nice little image that we can use to create a container that artificially uses a lot of CPU/memory resources. Start a container that uses a full CPU core:

```bash
docker run -d --name cpu-stress alexeiled/stress-ng --cpu 2 --timeout 10m
```

This is going to slow down your machine somewhat while it's running, but don't worry we'll kill it soon. We added a timeout of 10 minutes just in case you forget to kill it later.

2. [ ] Start another container that uses some memory:

```bash
docker run -d --name mem-stress alexeiled/stress-ng --vm 1 --vm-bytes 1G --timeout 10m
```

This one will allocate a full gigabyte of memory for the container, which is a lot for a single container. Again, we'll kill it soon.

3. [ ] Run `docker stats` to see the live resource usage of the two containers. _Take a good look_! You should see a table with CPU, memory, network I/O, and block I/O usage for each container. Press Ctrl+C to exit the stats view.

# Top

The `docker top` command shows the running _processes inside_ a container.

```bash
docker top CONTAINER [ps OPTIONS]
```

Use `stats` for _entire containers_ and `top` for _processes in a container_.

## Assignment

Make sure you still have your `cpu-stress` and `mem-stress` containers running.

1. [ ] Check which processes are running inside the CPU-intensive container, then compare it to the memory-intensive container:

```bash
# Check the processes in the CPU-intensive container
docker top cpu-stress

# Check the processes in the memory-intensive container
docker top mem-stress
```

You should notice that the CPU intensive container has two processes with high CPU usage (The "C" column), while the memory intensive container only has one.

2. [ ] Kill and remove both containers so they're no longer using your resources.

# Resource Limits

When you notice a container's using too many resources, if you don't have the time or the ability to "fix" the code, you can limit the resources the container has available. The `docker run` command has a few options for limiting resources:

- `--memory`: Limit the memory available to the container
- `--cpus`: Limit the CPU time available to the container

## Assignment

1. [ ] Run the same `alexeiled/stress-ng` container as before, but this time limit the CPUs to only `0.25` (1/4th of a CPU core):
    
    ```bash
    docker run -d --cpus="0.25" --name cpu-stress alexeiled/stress-ng --cpu 2 --timeout 10m
    ```
    
    Here, `--cpus="0.25"` is a Docker flag. `--cpu 2` comes after the image name, so it's passed to `stress-ng`.
    
2. [ ] Run `docker stats` again to see the resource usage. You should notice that it's using a quarter of a core rather than a full CPU core.

Being able to monitor container resources is crucial for maintaining healthy container environments, especially in production settings.

---

CH8: Publish

# Publishing to Docker Hub

You should have already created a Docker Hub account, but let's review a bit about what Docker Hub _is_.

[Docker Hub](https://hub.docker.com/) is the official cloud service for storing and sharing Docker images. We call these kinds of services "registries". Other popular image registries include:

- [AWS ECR](https://aws.amazon.com/ecr/)
- [GCP Artifact Registry](https://cloud.google.com/artifact-registry/docs)
- [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [Harbor](https://goharbor.io/)
- [Azure ACR](https://azure.microsoft.com/en-au/products/container-registry/)

# Publishing to Docker Hub

Let's publish the Go server we Dockerized up to Docker Hub.

## Assignment

1. [ ] Rebuild the Go binary:

```bash
GOOS=linux GOARCH=amd64 go build
```

2. [ ] Rebuild the image. You'll need to use a name that corresponds to _your_ namespace on Docker Hub. Swap out `USERNAME` for _your_ Docker Hub username.

```bash
docker build . -t USERNAME/goserver
```

3. [ ] Run your image in a container to make sure it still works:

```bash
docker run -p 8991:8991 USERNAME/goserver
```

4. [ ] Push the image to Docker Hub:

```bash
docker push USERNAME/goserver
```

If everything worked, you should be able to refresh your [repositories page](https://hub.docker.com/repositories) and see your new image! By default, Docker Hub makes your images public, so anyone can pull them.

`docker push` prints the upload status of an image layer by layer

# Delete and Pull

Let's delete our local copy of the image, then pull it back down from Docker Hub. Just like with GitHub, the nice thing about having images in the cloud is that if something happens to your computer, or you're working on another machine, you can always pull down your images.

## Assignment

1. [ ] Remove your local `USERNAME/goserver` image:

```bash
docker image rm USERNAME/goserver
```

2. [ ] Pull it back down from Docker Hub:

```bash
docker pull USERNAME/goserver
```

3. [ ] Run it to make sure it works:

```bash
docker run -p 8991:8991 USERNAME/goserver
```

# Tags

Let's publish a new _version_ of our web server. With Docker, a tag is a label that you can assign to a specific version of an image, similar to a tag in Git.

The `latest` tag is the default tag that Docker uses when you don't specify one. It's a convention to use `latest` for the most recent version of an image, but it's also common to include other tags, often [semantic versioning](https://semver.org/) tags like `0.1.0`, `0.2.0`, etc.

## Deployment Pipelines

Publishing new versions of Docker images is a _very common_ method of deploying cloud-native back-end servers. Here's a diagram describing the deployment pipeline of many production systems (including the server that powers the Boot.dev site you're on currently).

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/cPNJU9E-1080x762.png)

## Assignment

1. [ ] Update the Go code of your server:
    - Replace: `<p> Hello from Docker! I'm a Go server. </p>`
    - With: `<p> Hi Docker, I pushed a new version. </p>`
2. [ ] Compile the new go binary: `GOOS=linux GOARCH=amd64 go build`
3. [ ] Rebuild the Docker image. Make sure to use the same name as before, but this time, add a new tag to the end of the image name:

```bash
docker build . -t USERNAME/goserver:0.2.0
```

4. [ ] Run the new image:

```bash
docker run -p 8991:8991 USERNAME/goserver:0.2.0
```

5. [ ] Open your browser and navigate to `http://localhost:8991`. You should see the new version of the server running!
6. [ ] Push the new image up to Docker Hub. This time, you need to specify the tag as well:

```bash
docker push USERNAME/goserver:0.2.0
```

7. [ ] Navigate to your repo on DockerHub, and now you should see 2 images with different tags.
8. [ ] Stop and remove the new container you just started.
9. [ ] Delete the local images so that you have no reference on your local machine. Pull and run the new version from Docker Hub.

```bash
docker pull USERNAME/goserver:0.2.0
docker run -p 8991:8991 USERNAME/goserver:0.2.0
```

# Latest

If you look closely, you'll notice that your _old_ version is tagged "latest"... that's a bit confusing. As it turns out, the `latest` tag doesn't always indicate that a specific tag is the latest version of an image. In reality, `latest` is just the _default_ tag that's used if you don't explicitly supply one. We didn't use a tag on our first version, that's why it was tagged with "latest".

## Should I Use “latest”?

The convention I'm familiar with is to use [semantic versioning](https://semver.org/) on all your images, but to _also_ push to the "latest" tag on your most recent image. That way you can keep all of your old versions around, but the `latest` tag still always points to the latest version.

So, for example, if I were updating an application to version 5.4.6, I would probably do it like this:

```bash
docker build -t bootdotdev/awesomeimage:5.4.6 -t bootdotdev/awesomeimage:latest .
docker push bootdotdev/awesomeimage --all-tags
```

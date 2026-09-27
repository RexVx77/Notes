 CH1: Install
# Welcome to “Learn Kubernetes”

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/FFFgX7n-1096x514.png)

This course is a bit different than the other courses of Boot.dev. We'll be doing _very little_ coding in the browser. [Kubernetes](https://kubernetes.io/) is a distributed system of servers that host software applications, and you interact with it primarily through your local command line - it's not a programming language.

# What Is Kubernetes?

> Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.
> 
> -- The [Kubernetes](https://kubernetes.io/) team

Kubernetes orchestrates and manages collections of containers (often using container runtimes like containerd). It takes care of scaling, distribution, and connectivity among these containers. Think of it as a _system to manage many containers and the infrastructure they run on_.

For example, you _could_ install Docker on a single server, and route traffic directly to it. That's fairly simple to set up, but what if you want 10 instances of that server? What about 1000 instances? What if you want to deploy many different services, each scaling up with more instances depending on load? Those are the problems that Kubernetes solves.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/sW9VfZO-680x216.jpg)

-- [Source: Dilbert comic](https://en.wikipedia.org/wiki/Dilbert)

# Kubectl

The Kubernetes command-line tool, `kubectl`, allows you to run commands against Kubernetes clusters. It's a client that communicates with a Kubernetes API server.

## Install

Follow the official [installation instructions for kubectl](https://kubernetes.io/docs/tasks/tools/).

## Verify Installation

Run `kubectl version` to verify that kubectl is installed correctly. You won't see a server version yet because we haven't set up a cluster, but that's okay.

# Minikube

During this course, we'll be using [Minikube](https://minikube.sigs.k8s.io/docs/) to practice with Kubernetes. In production, you probably wouldn't use Minikube, you would use a cluster of servers, probably in the cloud. That's expensive! Minikube is a fantastic tool that allows you to run a single-node Kubernetes cluster on your local machine.

## Assignment

### Install

Follow the official [installation instructions for Minikube](https://minikube.sigs.k8s.io/docs/start/). Notice at the top the "what you'll need" section. If you don't have the system requirements, you'll have a hard time getting everything up and running, unfortunately.

### Verify Installation

Run `minikube version` to verify that Minikube is installed correctly.

### Run Minikube

We'll be using Kubernetes with Docker, which is arguably the most common way to use Kubernetes. Make sure your Docker daemon is running before starting Minikube. _If you haven't yet taken our [Docker course](https://boot.dev/courses/learn-docker), you should do that first._

Next, run:

```bash
minikube start --extra-config="apiserver.cors-allowed-origins=['http://boot.dev']"
```

This will take a few minutes to complete the first time. The extra configuration is just so we can hit your cluster from Boot.dev. You should see a message like "kubectl is now configured to use "minikube" cluster and "default" namespace by default".

# Deploying Synergychat

You've been hired by SynergyChat! [SynergyChat](https://github.com/bootdotdev/synergychat) is a metaverse-enabled chat app that provides data-driven insights through cutting-edge AI models that run on Web 3 infrastructure. It's truly the pinnacle of Silicon Valley innovation and culture.

_All that to say, it's like Discord but with extra features for businesses._

For the rest of this course, we'll be deploying SynergyChat web services to Kubernetes!

## Deploying an Image

The `kubectl create deployment` command will create a "deployment" for us. We'll talk more about the nuances of "deployments" later. But to put it simply, we only need to provide two things:

1. The name of the deployment (this can be anything, it's used to identify the deployment)
2. The ID of the Docker image we want to deploy (it would be a full URL if we weren't hosting the image on Docker Hub, which is the default)

```bash
kubectl create deployment synergychat-web --image=docker.io/bootdotdev/synergychat-web:latest
```

This command will deploy a container built from [this Docker image](https://hub.docker.com/r/bootdotdev/synergychat-web) to your local k8s cluster.

## Viewing Deployments

To make sure the deployment was successful, run:

`kubectl get deployments`

## Accessing the Web Page

By default, resources inside of Kubernetes run on a private, isolated network. They're visible to other resources within the cluster, but not to the outside world.

In order to access the application from your local network, you'll need to use `kubectl` to do some port forwarding. First, run:

```bash
kubectl get pods
```

We'll talk more about pods later, but for now, a pod is an abstraction over a container, and remember, a container is just a running instance of an image. To oversimplify, **a pod is a running application**.

You should see something like this:

```
NAME                                   READY   STATUS    RESTARTS   AGE
synergychat-web-679cbcc6cd-cq6vx       1/1     Running   0          20m
```

Next, run:

```bash
kubectl port-forward PODNAME 8080:8080
```

Be sure to replace `PODNAME` with _your_ pod's name. In my case, it was `synergychat-web-679cbcc6cd-cq6vx`.

Next, open your browser and navigate to `http://localhost:8080`. You should see a webpage titled "SynergyChat"! Keep in mind, that we haven't configured all the resources the page needs yet, so the forms won't work, but the page should load.

# Minikube vs. Prod

Minikube is a great tool for learning Kubernetes, but it's not a production-scale Kubernetes cluster. The primary difference is that Minikube runs a single-node cluster, whereas production clusters are multi-node distributed systems.

## Distributed Systems Are Complex

Whenever you're dealing with a system that involves multiple machines talking to each other over a network, you're dealing with a distributed system. Distributed systems are inherently complex, and Kubernetes is no exception, but that complexity is generally abstracted away from you as a K8s user. That's what makes Kubernetes so cool! _It does a lot of the hard work for you._

## Resources and Nodes

To zoom way out, Kubernetes' job is to run software applications, and applications require resources. Resources are things like:

- CPU
- Memory
- Disk space

Kubernetes' job is to manage those resources and allocate them to the applications that are running on it. Let's look at an oversimplified example:

### 3 Nodes (Machines)

|Node|RAM|
|---|---|
|Node 1|16GB|
|Node 2|8GB|
|Node 3|8GB|

### 5 Pods (Applications)

|App|Required RAM|
|---|---|
|App 1|12GB|
|App 2|2GB|
|App 3|5GB|
|App 4|4GB|
|App 5|4GB|

Kubernetes looks at the resources required by each application and decides which node to run it on. In this case, it might do something like this:

|Node|Apps|RAM Left Over|
|---|---|---|
|Node 1|App 1 (12GB), App 2 (2GB)|2GB|
|Node 2|App 4 (4GB), App 5 (4GB)|0GB|
|Node 3|App 3 (5GB)|3GB|

What happens if we get a new application that requires 10GB of RAM? The cluster doesn't have enough resources to run it! The solution? Easy. Just add another node to the cluster and let Kubernetes figure out where to run it.

## This Won't Work With Minikube

With Minikube, you only get one node! So once your machine runs out of resources, you're out of luck. That's why Minikube is great for learning, but not for production.

There are Kubernetes clusters running in production that have _thousands_ of nodes. That's a lot of resources to manage! But that's the beauty of Kubernetes.

_If you're interested, you can find some [case studies here](https://www.cncf.io/case-studies/). I liked [this one](https://www.cncf.io/case-studies/bloomberg/) from Bloomberg that shows they run hundreds of clusters with thousands of nodes each._

---

CH2: Pods

# Pods

> "Pods are the smallest deployable units of computing that you can create and manage in Kubernetes."
> 
> -- The [Kubernetes team](https://kubernetes.io/docs/concepts/workloads/pods/)

A Pod is the smallest and simplest unit in the Kubernetes object model that you create or deploy. It represents one (or sometimes more) running container(s) in a cluster. In a simple web application, you might have one single pod: the web server. As traffic grows, you might deploy that same code to multiple pods to handle the increased load. Several pods, one codebase. In a more complex backend system, you might have several pods for the web server and several pods that handle video processing. Multiple pods, multiple codebases.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/dK2fL05-1280x655.png)

Pods are just wrappers around containers. You can think of it as a Docker container with a little extra Kubernetes magic. The container is the actual application, and the Pod is the Kubernetes abstraction that manages the container and the resources it needs to run.

## Assignment

Let's deploy a second pod!

Use the `kubectl get pods` again to see a list of all your running pods. You should still only see the one `synergychat-web` pod. Let's add a second instance!

Run `kubectl edit deployment synergychat-web` to edit the deployment. This will open the deployment in your default text editor. You should see a big 'ol [yaml](https://yaml.org/) file. This is the configuration of your deployment. Under the "spec" section, you should see the `replicas` field set to `1`. Change it to `2`, save the file, and close the editor.

```yaml
spec:
  ...
  replicas: 2
  ...
```

Run `kubectl get pods` again. You should see two pods now!

Which section of the pod name is different for each pod in the same deployment? -> Last

# Ephemeral

Pods die, they die often, and sometimes without warning.

The ephemeral (fancy word for "temporary") nature of Pods is one of the defining features of Kubernetes. Unlike traditional virtual machines (VMs) or physical servers that might run indefinitely (or until hardware failure), Pods are designed to be spun up, torn down, and restarted at a moment's notice.

- **Why are they temporary?** Flexibility and resilience. If a Pod encounters a problem, it can be easily terminated and replaced with a new, healthy instance. This model not only allows for high availability but also promotes immutability. Instead of manually patching or updating _existing_ environments, you spin up new versions of the entire environment.
- **How does it affect me?** As a developer, it's crucial to understand that it's rarely a good idea to store persistent data on a Pod. They can be terminated and replaced, and any locally saved data will be lost. Plan on your image restarting from scratch often!

## Assignment

Get a list of your running pods:

```bash
kubectl get pods
```

Print the logs (what the container is printing to stdout) of your _older_ pod:

```bash
kubectl logs PODNAME
```

Kill that _older_ pod (this might take several seconds to complete):

```bash
kubectl delete pod PODNAME
```

Get a list of your running pods again:

```bash
kubectl get pods
```

After deleting a pod, how many pods are running in the cluster? -> Still 2
When a pod is manually deleted... -> A new pod is created from the same image (which kinda feels like a restart)

# Unique IP Addresses

Every Pod in a Kubernetes cluster has a unique internal-to-k8s IP address. By giving each Pod a unique IP, Kubernetes simplifies communication and service discovery within the cluster. Pods within the same Node or across different Nodes can easily communicate.

All the resources inside a k8s cluster are virtualized. So, the IP address of a Pod is not the same as the IP address of the Node it's running on. It's a virtual IP address that is only accessible from within the cluster.

## Assignment

Run this command to get a "wide" output of your pods:

```bash
kubectl get pods -o wide
```

It gives a few more columns of information, including the IP address of each Pod. Notice that each Pod has a unique IP address!

Next, run:

```bash
kubectl proxy
```

This will start a proxy server on your local machine, probably on `127.0.0.1:8001`. Assuming that's the host, navigate to `http://127.0.0.1:8001/api/v1/namespaces/default/pods` in your browser. You should see a big nasty JSON blob that describes the pods that you have running.

---

CH3: Deployments

# Deployments

A _[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)_ provides declarative updates for Pods and ReplicaSets.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/6Pgwx6u-1280x720.png)

You describe your _desired state_ in a Deployment, and the Deployment Controller's job is to make the _current state_ match the _desired state_. You declare your hopes and dreams, and it's Kubernetes' job to make them come true.

## Why Deleting a Pod Doesn't Feel Like a Deletion

Remember when we had you delete a pod, only to see that a new pod was created in its place? It's kinda like chopping heads off of a hydra.

That's because the _desired state_ described in our Deployment says we want 2 pods running at all times. When we delete one, the Deployment Controller sees that the _current state_ doesn't match the _desired state_, so it creates a new pod to make them match again.

## Assignment

Take a look at the YAML file for your current deployment in the CLI:

```bash
kubectl get deployment synergychat-web -o yaml
```

Edit the deployment and change the number of replicas from 2 to 10:

```bash
kubectl edit deployment synergychat-web
```

Make sure you've got 10 pods running:

```bash
kubectl get pod
```

Keep using `kubectl get pod` to check on your pods until all 10 show "1/1" under "READY". Once they are, run:

```bash
kubectl proxy
```

# Replica Sets

A [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) maintains a stable set of replica Pods running at any given time. It's the thing that makes sure that the number of Pods you want running is the same as the number of Pods that are actually running.

You might be thinking, "I thought that's what a Deployment does." Well...yes.

A Deployment is a higher-level abstraction that manages the ReplicaSets for you. You can think of a Deployment as a wrapper around a ReplicaSet. Here's the rub:

_You will probably never use ReplicaSets directly._ I just need to mention what they are because you'll hear the term thrown around, and might even see them referenced in logs and such.

## Look at Your Replica Sets

Let's take a look at the ReplicaSets that are running in your cluster:

```bash
kubectl get replicasets
```

Just like with pods, notice that _we never directly created the replica set_. We created a deployment, and the deployment created the replica set.

# YAML Config

Kubernetes resources are primarily configured using YAML files. We've used the `kubectl edit` command to edit resources in the cluster on-demand, but let's inspect our deployment's YAML file a bit more closely.

Click to hide video

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: what is YAML

## Assignment

First, download a copy of your deployment's YAML file and save it in your current directory:

```bash
kubectl get deployment synergychat-web -o yaml > web-deployment.yaml
```

Then open it in your text editor. There are 5 top-level fields in the file:

- `apiVersion: apps/v1` - Specifies the version of the Kubernetes API you're using to create the object (e.g., apps/v1 for Deployments).
- `kind: Deployment` - Specifies the type of object you're configuring
- `metadata` - Metadata about the deployment, like when it was created, its name, and its ID
- `spec` - The desired state of the deployment. Most impactful edits, like how many replicas you want, will be made here.
- `status` - The current state of the deployment. You won't edit this directly, it's just for you to see what's going on with your deployment.

Inside your editor, change the number of replicas to 3 and save the file. Notice that you're just editing a file on your machine! It won't yet have any effect on the deployment in your cluster.

To apply the changes, run:

```bash
kubectl apply -f web-deployment.yaml
```

You should get a warning that lets you know that you're missing the `last-applied-configuration` annotation. That's okay! we got that warning because we created this deployment the quick and dirty way, by using `kubectl create deployment` instead of creating a YAML file and using `kubectl apply -f`.

However, because we've now _updated_ it with `kubectl apply`, the annotation is now there, and we won't get the warning again.

Download the YAML file again and take a look at it. You should see the annotation now.

Apply the configuration a second time, you won't get the warning. _Save this YAML file in a git repo for this course! We'll be making more configuration files. Kubernetes is an "infra-as-code" tool, so it's important to keep your configuration files in a git repo._

Finally, start the proxy server (if it's not already running):

```bash
kubectl proxy
```

**Run and submit** the CLI tests.

# Thrashing Pods

One of the most common problems you'll run into when working with Kubernetes is Pods that keep crashing and restarting. This is called "thrashing" and it's usually caused by one of a few things:

- The application recently had a bug introduced in the latest image version
- The application is misconfigured and can't start properly
- A dependency of the application is misconfigured and the application can't start properly
- The application is trying to use too much memory and is being killed by Kubernetes

## What Is “CrashLoopBackoff”?

When a pod's status is `CrashLoopBackoff`, that means the container is crashing (the program is exiting with error code `1`).

Because Kubernetes is all about building self-healing systems, it will automatically restart the container. However, each time it tries to restart the container, if it crashes again, it will wait longer and longer in between restarts. That's why it's called a "backoff".

To fix a thrashing pod, you need to find out why it's crashing. We'll do that in a later lesson.

---

CH4: ConfigMaps

# Config Maps

There are several ways to manage environment variables in Kubernetes. One of the most common ways is to use [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/). ConfigMaps allow us to decouple our configurations from our container images, which is important because we don't want to have to rebuild our images every time we want to change a configuration value.

In a Dockerfile we can set environment variables like this:

```dockerfile
ENV PORT=3000
```

The trouble is, that means that everyone using that image will have to use port 3000. It also means that if we want to change the port, we have to rebuild the image.

## Assignment

First, let's take a closer look at our crashing pod and try to figure out why it's crashing.

```bash
kubectl get pods
```

Copy the pod name and then get the logs:

```bash
kubectl logs <pod-name>
```

You should see that a specific environment variable is missing! Let's fix that.

Create a new file. Let's call it `api-configmap.yaml`. Add the following YAML to it:

- `apiVersion`: `v1`
- `kind`: `ConfigMap`
- `metadata/name`: `synergychat-api-configmap`
- `data/API_PORT`: `"8080"`

Next, apply the config map:

```bash
kubectl apply -f api-configmap.yaml
```

Now, we haven't yet connected the config map to our pod, so it should still be crashing. However, for now, let's just validate that the config map was created successfully:

```bash
kubectl get configmaps
```

Run:

```bash
kubectl proxy
```

**Run and submit** the CLI tests.


# Applying the Config Map

Now that we have a config map, we need to connect it to our deployment.

## Assignment

Open up your `api-deployment.yaml` file. We're going to add a few things to it. Under the `containers` section, add the following to the first (and only) entry:

```yaml
env:
  - name: API_PORT
    valueFrom:
      configMapKeyRef:
        name: synergychat-api-configmap
        key: API_PORT
```

This tells Kubernetes to set the `API_PORT` environment variable to the value of the `API_PORT` key in the `synergychat-api-configmap` config map. Reference the [official docs](https://kubernetes.io/docs/concepts/configuration/configmap/) if you're confused about the structure of the yaml.

Next, apply the deployment. Hopefully, you remember the command for this by now.

Once it's applied, you should be able to take a look at the pods and see that a new API pod has been deployed and isn't crashing!

Let's forward the API pod's `8080` port to our local machine so we can test it out.

```bash
kubectl port-forward <pod-name> 8080:8080
```

Make sure it returns a `404` response when you hit the root:

```bash
curl http://localhost:8080
```

# Config Maps Are Insecure

ConfigMaps are a great way to manage innocent environment variables in Kubernetes. Things like:

- Ports
- URLs of other services
- Feature flags
- Settings that change between environments, like `DEBUG` mode

However, they are _not_ cryptographically secure. ConfigMaps aren't encrypted, and they can be accessed by anyone with access to the cluster.

If you need to store sensitive information, you should use [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) or a third-party solution.

# Crawler

We've got one last application to deploy to our cluster: the crawler. This is an application that continuously crawls [Project Gutenberg](https://www.gutenberg.org/) and exposes the juicy data that it finds via a JSON API.

## Assignment

### Add a New Config Map

Create a copy of your `api-configmap.yaml` file and call it `crawler-configmap.yaml`. We're going to make a few changes to it.

1. Name it `synergychat-crawler-configmap` instead of `synergychat-api-configmap`.
2. Remove the `API_PORT` environment variable.
3. Add some new environment variables:
    - `CRAWLER_PORT`: `"8080"`
    - `CRAWLER_KEYWORDS`: `love,hate,joy,sadness,anger,disgust,fear,surprise`

It's okay that the `CRAWLER_PORT` in the `crawler` deployment is the same as the `API_PORT` in the `api` deployment. They're in different pods, and these are pod-internal ports.

Here's a [reference](https://github.com/bootdotdev/synergychat/tree/main#crawler-service) to the docs for the SynergyChat microservices on GitHub in case you want additional info about the crawler.

Deploy the config map:

```bash
kubectl apply -f crawler-configmap.yaml
```

### Add a New Deployment

Create a copy of your `api-deployment.yaml` file and call it `crawler-deployment.yaml`. We're going to make a few changes to it.

1. Update all `synergychat-api` references to `synergychat-crawler`.
2. Update the image URL to `bootdotdev/synergychat-crawler:latest`.
3. Update the environment variable references to match the new config map. (See below)

For the `api` service, we used this syntax to connect the config map to the deployment:

```yaml
spec:
  containers:
    - image: bootdotdev/synergychat-api:latest
      name: synergychat-api
      env:
        - name: API_PORT
          valueFrom:
            configMapKeyRef:
              name: synergychat-api-configmap
              key: API_PORT
```

If we use this same format, it gets kinda verbose and repetitive to list out each environment variable:

```yaml
spec:
  containers:
    - image: bootdotdev/synergychat-api:latest
      name: synergychat-api
      env:
        - name: THING_ONE
          valueFrom:
            configMapKeyRef:
              name: synergychat-api-configmap
              key: THING_ONE
        - name: THING_TWO
          valueFrom:
            configMapKeyRef:
              name: synergychat-api-configmap
              key: THING_TWO
        - name: THING_THREE
          ...
```

We can use the `envFrom` key instead of the `env` key to reference the _entire_ config map and make it available to the pods in the deployment:

```yaml
envFrom:
  - configMapRef:
      name: synergychat-crawler-configmap
```

This injects **all keys** from the `synergychat-crawler-configmap` into the container as environment variables.

Once you've updated the deployment, apply it:

```bash
kubectl apply -f crawler-deployment.yaml
```

If the pod isn't "ready", check the logs to see if there's an error. If the error is related to environment variables, debug your config map and deployment files and reapply them.

Once it's ready, forward the pod's `8080` port to your local machine:

```bash
kubectl port-forward <pod-name> 8080:8080
```

---

CH5: Services

# Services

We've spun up pods and connected to them individually, but that's frankly not super useful if we want to distribute real traffic across those pods. That's where services come in.

[Services](https://kubernetes.io/docs/concepts/services-networking/service/) provide a stable endpoint for pods. They are an abstraction used to provide a stable endpoint and load balance traffic across a group of Pods. By "stable endpoint", I just mean that the service will always be available at a given URL, even if the pod is destroyed and recreated.

Click to hide video

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: Services in k8s video explainer

## Assignment

Let's add a service for our 3 `synergychat-web` pods. If you don't have 3 pods running, edit the deployment to have 3 replicas.

Create a file called `web-service.yaml` and add the following:

- `apiVersion`: `v1`
- `kind`: `Service`
- `metadata/name`: `web-service` (we could call it anything, but this is a fine name)
- `spec/selector/app`: I'm going to let you figure out what should be here. This is how the service knows which pods to route traffic to.
- `spec/ports`: An array of port objects. You need one entry:
    - `protocol`: `TCP` ([TCP will allow us to use HTTP](https://qr.ae/pKCJMf))
    - `port`: `80` (this is the port that the service will listen on)
    - `targetPort`: `8080` (this is the port that the pods are listening on)

This creates a new service called `web-service` with a few properties:

- It listens on port `80` for incoming traffic
- It forwards that traffic to pods that are listening on their port `8080`
- Its controller will continuously scan for pods matching the `app: synergychat-web` label selector and automatically add them to its pool

Create the service:

```bash
kubectl apply -f web-service.yaml
```

Now, let's forward the service's port to our local machine so we can test it out.

```bash
kubectl port-forward service/web-service 8080:80
```

Now, if you hit `http://localhost:8080` in your browser, you should see the web app! It's _better_ this time around because now our requests are being load-balanced across 3 pods.

# Service Types

Take a look at the `yaml` that describes your `web-service`.

```bash
kubectl get svc web-service -o yaml
```

_"svc" is a short-hand alias for "service", either will work in kubectl._

You should see a section that looks like this:

```bash
spec:
  clusterIP: 10.96.213.234
  ...
  type: ClusterIP
```

We didn't specify a service type! Why is this here? Well, it's because `ClusterIP` is the default service type.

The `clusterIP` is the IP address that the service is bound to on the internal Kubernetes network. Remember how we talked about how pods get their own internal, virtual IP address? Well, services can too! However, `type: ClusterIP` is just _one type_ of service! There are [several others](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types), including:

- `NodePort`: Exposes the Service on each Node's IP at a static port.
- `LoadBalancer`: Creates an external load balancer in the current cloud environment (if supported, e.g. AWS, GCP, Azure) and assigns a fixed, external IP to the service.
- `ExternalName`: Maps the Service to the contents of the `externalName` field (for example, to the hostname `api.foo.bar.example`). The mapping configures your cluster's DNS server to return a CNAME record with that external hostname value. No proxying of any kind is set up.

The interesting thing about service types is that they typically build on top of each other. For example, a `NodePort` service is just a `ClusterIP` service with the added functionality of exposing the service on each node's IP at a static port (it still has an internal cluster IP).

A `LoadBalancer` service is just a `NodePort` service with the added functionality of creating an external load balancer in the current cloud environment (it still has an internal cluster IP and node port).

An `ExternalName` service is actually a bit different. All it does is a DNS-level redirect. You can use it to redirect traffic from one service to another.

## Which Type Should I Use?

Well, it depends on a lot of things. If you're working in a microservices environment where many services are only meant to be accessed within the cluster, then `ClusterIP` is going to be your go-to. `NodePort` and `LoadBalancer` are used when you want to expose a service to the outside world. `ExternalName` is primarily for DNS redirects (frankly I've never used it).

We'll talk more about exposing services to the outside world in a later chapter.

Check to make sure the service is running:

```bash
kubectl get svc
```

# Change API Service

Remember how I said that `NodePort` and `LoadBalancer` services are used to expose services to the outside world? That's true, but in most cloud-based Kubernetes environments, you'll actually use a Gateway object to expose your services. The Gateway object not only exposes your service to the outside world, but also allows you to do things like:

- Host multiple services on the same IP address
- Host multiple services on the same port (path-based routing)
- Terminate SSL
- Integrate directly with external DNS and load balancers

Because we'll be setting up a Gateway in the next chapter anyway, there's no reason to expose the API service with a `NodePort` service. Let's change it back to a `ClusterIP` service.

---

CH6: Gateway

# Gateway

A [Gateway](https://kubernetes.io/docs/concepts/services-networking/gateway/) resource exposes services to the outside world and is used often in production environments. From the docs:

> A Gateway describes an instance of traffic handling infrastructure. It defines a network endpoint that can be used for processing traffic, i.e. filtering, balancing, splitting, etc. for backends such as a Service. For example, a Gateway may represent a cloud load balancer or an in-cluster proxy server that is configured to accept HTTP traffic.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/yv16w3F-1280x584.png)

In the diagram above, the "client" can be anything. It doesn't live inside k8s. It might just be a web browser or a mobile app. The "Gateway-managed load balancer" can be a bit confusing, we'll talk about it more later. For now, just know that it's a load balancer that lives outside the cluster and routes traffic through the Gateway to a service.

Gateway is the newer alternative to [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) which you might still come across in production systems.

## Assignment

The Gateway API is a spec that has different implementations. We're gonna use the [Envoy Gateway](https://gateway.envoyproxy.io/docs/concepts/), which you can install with this command:

```bash
kubectl apply --server-side -f https://github.com/envoyproxy/gateway/releases/download/v1.5.1/install.yaml
```

Then, create a file that we'll call `app-gatewayclass.yaml` because we want to create a Gateway for the entire synergychat application, not just a specific service.

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: GatewayClass
metadata:
  name: app-gatewayclass
spec:
  controllerName: gateway.envoyproxy.io/gatewayclass-controller
```

We need this GatewayClass because:

1. Any Gateway we create has to be associated with a GatewayClass
2. The `controllerName` is set to the one created while installing the Envoy Gateway

Next, create the actual Gateway in new file named `app-gateway.yaml`:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: app-gateway
spec:
  gatewayClassName: app-gatewayclass
  listeners:
    - name: http
      protocol: HTTP
      port: 80
```

Next, we need to define the routing rules for our Gateway with [HTTPRoute resources](https://kubernetes.io/docs/concepts/services-networking/gateway/#api-kind-httproute). Set a rule for the web service, which we could save in a file named `web-httproute.yaml`:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: web-httproute
spec:
  parentRefs:
    - name: app-gateway
  hostnames:
    - "synchat.internal"
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /
      backendRefs:
        - name: web-service
          port: 80
```

... and another for the API service in `api-httproute.yaml`:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: api-httproute
spec:
  parentRefs:
    - name: app-gateway
  hostnames:
    - "synchatapi.internal"
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /
      backendRefs:
        - name: api-service
          port: 80
```

This says that any traffic to the `synchat.internal` domain name should be routed to the `web-service` and any traffic to `synchatapi.internal` domain name should be routed to the `api-service`.

# DNS

Now that we've configured the Gateway to route the domains:

- `synchat.internal` to the `web-service`
- `synchatapi.internal` to the `api-service`

We need to configure our local machine to resolve those domains to the Gateway load balancer. We won't be setting up global DNS so that anyone on the internet can access our app! We'll just be configuring our local machine to resolve those domains to the Gateway load balancer.

## Assignment

There is a file called `/etc/hosts` on your local machine that is used to resolve domain names to IP addresses. We can add entries to that file to resolve our domains to the Gateway load balancer.

First, get the Gateway IP address:

```bash
minikube tunnel
kubectl get gateway app-gateway -o custom-columns="IP:.status.addresses[0].value" --no-headers
```

Open `/etc/hosts` in your favorite text editor and add the following lines:

```
<gateway-ip>        synchat.internal
<gateway-ip>        synchatapi.internal
```

Replace `<gateway-ip>` with the IP address from the previous command. You'll probably need to provide your password to save the file.

To make sure it's working, run:

```bash
ping synchat.internal
```

You should see something like:

```
PING synchat.internal (<gateway-ip>): 56 data bytes
64 bytes from <gateway-ip>: icmp_seq=0 ttl=64 time=0.104 ms
64 bytes from <gateway-ip>: icmp_seq=1 ttl=64 time=0.105 ms
64 bytes from <gateway-ip>: icmp_seq=2 ttl=64 time=0.150 ms
...
```

You can kill the ping with Ctrl+C.

If you get "cannot resolve host", then you did something wrong. Be sure to ping the `synchatapi.internal` domain as well.

# Tunnel

In production, once you have a Gateway configured and have pointed your domain name to it (and perhaps its load balancer), you can access your application from anywhere in the world. The trouble is, we're using Minikube and the cluster is running on our local machine, and not only that, it's running in an _isolated virtual_ machine.

Fear not! Minikube has a command that will forward the Gateway to your local machine.

## Assignment

Open a tunnel to your cluster (You might need to enter your password):

```bash
minikube tunnel -c
```

_We'll be using the tunnel a lot in the rest of the course, I recommend you keep it open in a separate terminal window._

The tunnel should expose the Gateway controller's load balancer to your local machine on the Gateway IP you mapped to `synchat.internal` and `synchatapi.internal` in the previous exercise.

While the tunnel is open, navigate to `http://synchat.internal` in your browser. You should see the web app! The JSON API should also be available at `http://synchatapi.internal` (the root of the API should return a 404, but `/healthz` should return a 200).

Once you've confirmed that you can access the web app via a web browser, answer the question on the right.

# Gateway Types

You may have noticed that at the top of all our resources we have this in the YAML:

```yaml
apiVersion: v1
```

This is the API version of the resource, and because those resources are core to Kubernetes, they're in the standard `v1` API group.

However, Gateway isn't a core Kubernetes resource, it's an extension of sorts. That's why it has:

```yaml
apiVersion: gateway.networking.k8s.io/v1
```

You can think of the `networking.k8s.io` API group as a core extension. It's not third-party (it's on `k8s.io` for heaven's sake), but it's not part of the core Kubernetes API either.

## Annotations

The core Kubernetes API is intentionally kept small, but there are a lot of things that people want to do with Kubernetes that aren't part of the core API. So, instead of adding a bunch of new fields to the core API, Kubernetes allows you to add arbitrary annotations to your resources, and then various extensions can read those annotations and do things with them.

For example, the Boot.dev Kubernetes cluster uses a Gateway extension specific to Google Cloud Platform. We use the following annotation so that our controller knows which SSL certificate to use:

```yaml
annotations:
  networking.gke.io/certmap: certmap-name-here
```

If you're curious about the specifics, the [docs are here](https://cloud.google.com/kubernetes-engine/docs/how-to/deploying-gateways). In a nutshell, however, the important take-away is that in most production deployments you'll be using annotations specific to the cloud provider you're using. Each major cloud provider has their own products, so you need to use k8s annotations and extensions specific to that cloud provider.

Now that you understand the basic concepts of Gateway, in the future, it's just a matter of following the documentation for your cloud provider to get it set up.

# Chat

Now that everything is accessible via Gateway (at least while the tunnel is open), let's connect the web application front-end to the API.

## Assignment

1. Create a new ConfigMap for the web service. Add two new environment variables:
    1. WEB_PORT: `8080` (this was already the default, now we're just making it explicit)
    2. API_URL: `http://synchatapi.internal`
2. Update the web application's deployment to use the new ConfigMap.

Once all that's applied to your cluster, you should be able to open the web application and actually _use_ the chat interface (notice it's "synchat", not "synchatapi"):

```
http://synchat.internal
```

Make sure that you can enter a username and a message, and send a message as that user! Assuming it works, that's because the webpage is now sending messages via `fetch` requests to the `api` service. The `api` service saves those messages locally in memory.

To see what I mean, try the following:

1. Create a few messages as different users.
2. Refresh the page
3. The messages should still be there because they're saved in the server's memory.
4. Now, delete the `api` pod
5. Once k8s replaces the deleted pod with a new one, refresh the page again.

Answer the question on the right when you're done.

After the pod has been recreated and you refresh the page, can you still see the first messages you sent? -> No

---

CH7: Storage

# Storage in Kubernetes

By default, containers running in pods on Kubernetes have access to the filesystem, but as we're about to find out, there are some big limitations to this.

## Assignment

The `api` application in SynergyChat can be configured to save its data (the messages) to a file on the filesystem. That way, even if the program is restarted, the messages will still be there.

Update the `api` service's ConfigMap to include a new environment variable:

- `API_DB_FILEPATH`: `/var/lib/synergychat/api/db.json`

Next, update the `api` service's deployment to use the new value.

Take a look at the logs of the new pod (`kubectl logs <podname>`). You should see a message saying that the filesystem will be used for the database. If you do, everything is working as intended!

Now, let's test the persistent storage:

1. Open a webpage to `http://synchat.internal`.
2. Create a few messages as different users.
3. Refresh the page
4. The messages should still be there because they're saved in the server's memory.
5. Now, delete the `api` pod
6. Once k8s replaces the deleted pod with a new one, refresh the page again.

_Perhaps to your surprise_, the messages are gone! What??? Why??? We saved them to the filesystem, didn't we? Well, we did, but in Kubernetes the filesystem is ephemeral. That means that when a pod is deleted, the filesystem is deleted with it.

This has to do with the philosophy behind Kubernetes and even containers in general: when we spin up a new one, it should always be a blank slate, which makes reproducing and debugging issues much easier. No messy state to worry about.

So how do we get persistent storage??? We'll cover that next.

# Ephemeral Volumes

On-disk files in a container are ephemeral as we saw in the last lesson. This presents some problems for applications that want to save long-lived data across restarts. For example, user data in a database.

The Kubernetes [volume](https://kubernetes.io/docs/concepts/storage/volumes/) abstraction solves two primary problems:

1. Data persistence
2. Data sharing across containers

As it turns out, there are _a lot_ of different types of "volumes" in Kubernetes. Some are even ephemeral as well, just like a container's standard filesystem. The primary reason for using an ephemeral volume is to share data between containers in a pod.

## Assignment

It's time to shift our focus back to the [crawler service](https://github.com/bootdotdev/synergychat/#crawler-service). The crawler service continuously crawls Project Gutenberg and exposes the information that it finds via a JSON API. That data is then made available via slash commands in the chat application.

The crawler is _pretty slow_ by default. Each instance only crawls one book every 30 seconds.

To see what I mean, run:

```bash
kubectl logs <crawler-podname>
```

You should see some logs with timestamps that show you the crawler's progress.

We can speed it up by increasing the number of concurrent crawlers. The trouble with scaling up beyond one instance is that each crawler currently stores its data in memory. We need all pods to share the same data so they can each add their findings to the same database.

Let's update the crawler deployment to use a volume that will be shared across all containers in the crawler pod, and scale up the number of containers in the pod.

1. Add a `volumes` section to `spec/template/spec`.

```yaml
volumes:
  - name: cache-volume
    emptyDir: {}
```

2. Add a new `volumeMounts` section to the container entry. This will mount the volume we just created at the `/cache` path.

```yaml
volumeMounts:
  - name: cache-volume
    mountPath: /cache
```

3. Duplicate the entire first entry in the `containers` list twice (you should now have 3 total containers). Update the name of each:
    1. `synergychat-crawler-1`
    2. `synergychat-crawler-2`
    3. `synergychat-crawler-3`

Now all the containers in the pod will share the same volume at `/cache`. It's just an empty directory, but the crawler will use it to store its data.

4. Add a `CRAWLER_DB_PATH` environment variable to the crawler's ConfigMap. Set it to `/cache/db`. The crawler will use a directory called `db` inside the volume to store its data.

When you run the next step, your pods are going to crash. Don't worry, we'll fix it!

5. Apply the new ConfigMap and Deployment, and use `kubectl get pod` to see the status of your new pod.

You should notice that there's a problem with the pod! Only 1/3 of containers should be "ready". Use the `logs` command to get the logs for all 3 containers:

```bash
kubectl logs <podname> --all-containers
```

You should see something like this:

```
listen tcp :8080: bind: address already in use
```

Because pods share the same network namespace, they can't all bind to the same port! Hmm... let's put a band-aid on this by binding each container to a different port. `8080` is the only one that will be exposed via the service, but that's okay for now. We can add redundancy later.

6. Add two new values to the crawler's ConfigMap:
    1. `CRAWLER_PORT_2`: `8081`
    2. `CRAWLER_PORT_3`: `8082`
7. Update the crawler deployment:

Change the second and third containers to map `CRAWLER_PORT_2` -> `CRAWLER_PORT` and `CRAWLER_PORT_3` -> `CRAWLER_PORT` respectively (the Docker image expects a variable named "CRAWLER_PORT"). I'm not going to give you the code, but know that it's gonna be a bit tedious because you need to use `env:` instead of `envFrom:` for the second and third containers. Don't forget to continue exposing the `CRAWLER_KEYWORDS` and `CRAWLER_DB_PATH` environment variables for all containers.

When you're done, apply the changes and run `kubectl get pods` again. All three containers should be ready, each serving on a different port, with only the first exposed via the service.

Run:

```bash
kubectl proxy
```

**Run and submit** the CLI tests.

# Containers in Pods

Run the following command:

```bash
kubectl get pods
```

You should see something like this:

```
synergychat-api-6c7944b5c4-rp2k4      1/1     Running   0          160m
synergychat-crawler-cd4947995-ftqg4   3/3     Running   0          151m
synergychat-web-846d86c444-2m6x7      1/1     Running   0          21h
synergychat-web-846d86c444-gxztt      1/1     Running   0          21h
synergychat-web-846d86c444-s88rz      1/1     Running   0          21h
```

It's important to remember that while it's common for a pod to run just a single container, multiple containers can run in a single pod. This is useful when you have containers that need to share resources. In other words, we can scale up the instances of an application either at the container level or at the pod level.

# Persistence

All the volumes we've worked with so far have been ephemeral, meaning when the associated pod is deleted the volume is deleted as well. This is fine for some use cases, but for most CRUD apps we want to persist data even if the pod is deleted.

If you think about it, it's not even just when pods are explicitly deleted with `kubectl` that we need to worry about data loss. Pods can be deleted for several reasons:

- The node they're running on could fail
- A new version of the image was published (code was updated, etc)
- A new node was added to the cluster and the pod was rescheduled

In all of these cases, we want to make sure that our data is still available. [Persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) allow us to do this.

## Persistent Volumes (PV)

Instead of simply adding a volume to a deployment, a persistent volume is a cluster-level resource that is created separately from the pod and then attached to the pod. It's similar to a ConfigMap in that way.

PVs can be created statically or dynamically.

- Static PVs are created manually by a cluster admin
- Dynamic PVs are created automatically when a pod requests a volume that doesn't exist yet

Generally speaking, and especially in the cloud-native world, we want to use dynamic PVs. It's less work and more flexible.

## Persistent Volume Claims (PVC)

A [persistent volume claim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) is a _request_ for a persistent volume. When using dynamic provisioning, a PVC will automatically create a PV if one doesn't exist that matches the claim.

The PVC is then attached to a pod, just like a volume would be.

## Assignment

Create a new file called `api-pvc.yaml` and add the following:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: xxx
spec:
  accessModes:
    - xxx
  resources:
    requests:
      storage: xxx
```

Add the following properties:

- `metadata/name`: `synergychat-api-pvc`
- `spec/accessModes`: An array with one entry:
    - `ReadWriteOnce`
- `spec/resources/requests/storage`: `1Gi`

This creates a new PVC called `synergychat-api-pvc` with a few properties that can be read from and written to by multiple pods at the same time. It also requests 1GB of storage.

Apply the PVC.

Run both of these commands:

```bash
kubectl get pvc
kubectl get pv
```

You should see that a new PV was created automatically!

Now _delete_ the PVC:

```bash
kubectl delete pvc <pvc-name>
```

Make sure both the PVC and PV are gone:

```bash
kubectl get pvc
kubectl get pv
```

Now recreate the PVC. Dynamic provisioning is awesome! Run:

```bash
kubectl proxy
```

**Run and submit** the CLI tests.

# Attach Persistence

So far all we've done is create an empty persistent volume. Let's get the `api` application to use it.

## Assignment

Use your `crawler` deployment and the [docs](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) as a reference. Create a new volume in the api-deployment referencing your pvc:

```yaml
volumes:
  - name: synergychat-api-volume
    persistentVolumeClaim:
      claimName: synergychat-api-pvc
```

Then mount it in the container under the `/persist` directory:

```yaml
volumeMounts:
  - name: synergychat-api-volume
    mountPath: /persist
```

Update the `API_DB_FILEPATH` environment variable you added earlier to instead use the new mount path: `/persist/db.json`

Apply the changes, then check to make sure all your pods are healthy:

```bash
kubectl get pods
```

With your tunnel running (`minikube tunnel -c`), open `http://synchat.internal/` in your browser.

1. Send some messages.
2. Delete the `api` pod.
3. Once the new pod is running, refresh the page and make sure your messages are still there. If they are, your persistent volume is working!

# Databases

Now that you know all about volumes, you might be thinking, "Awesome! I'll host my CRUD app on Kubernetes and use a volume to store my PostgreSQL database data!".

That's certainly possible, but frankly, it's not always the best idea. For example, the Boot.dev system that powers this website has the following components:

- A web application, currently served by Cloudflare (this could easily be a Kubernetes deployment if we cared to move it)
- Several backend microservices, all running on Kubernetes in Google Cloud
    - Our main JSON CRUD API
    - A Discord bot
    - A service that compiles student's Go code to WASM
    - etc
- A managed PostgreSQL database, hosted by Cloud SQL (GCP)

Why do we do this? Well, Kubernetes isn't always the _simplest_ way to get a job done. We could certainly host a PostgreSQL database on Kubernetes, but it would require a lot of extra work to get it to work well. For example, we'd need to manually build all the configurations to:

- Create a persistent volume
- Handle Postgres version updates
- Set resource limits
- Set up automated backups

For that reason, when I need an SQL database, I typically use a managed service like Cloud SQL or RDS. There are exceptions to that rule, but it's a good rule of thumb.

## When Would You Use a Database on Kubernetes?

I have used databases on Kubernetes in the past, but I've usually done it when the deployment wasn't exactly mission-critical. For example, I've deployed [Grafana](https://grafana.com/) and [Prometheus](https://prometheus.io/) on Kubernetes, and they both have out-of-the-box support for in-cluster databases. I didn't care too much about backups and automatic upgrades for my telemetry data, and I knew the data set was small and static, so it was a good fit.

---

CH8: Namespaces

# Namespaces

To quote the [Zen of Python](https://www.python.org/dev/peps/pep-0020/):

> Namespaces are one honking great idea -- let's do more of those!

[Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) are a way to isolate cluster resources into groups. They're a bit like directories on your computer, but instead of containing files, they contain Kubernetes objects. As you've already learned, every resource in Kubernetes has a name. Some of our names include:

- synergychat-api-configmap
- api-service
- api-deployment
- web-deployment
- ...

You can only use a name once. It is a unique identifier. That's how `kubectl apply` knows when it should create a new resource and when it should update an existing one. Namespaces allow us to use the same name for different resources, as long as they're in different namespaces.

Run the following command:

```bash
kubectl get namespaces
```

or the shorter version:

```bash
kubectl get ns
```

# Moving Namespaces

Up until this point, we've been working in the `default` namespace. When using `kubectl` commands, you can specify the namespace with the `--namespace` or `-n` flag. If you don't, it will use the `default` namespace.

```
wagslane@MacBook-Pro courses % kubectl get pod
NAME                                  READY   STATUS    RESTARTS   AGE
synergychat-api-646c6fd585-dk5db      1/1     Running   0          28m
synergychat-crawler-cd4947995-tcrkn   3/3     Running   0          39m
synergychat-web-846d86c444-d9c8q      1/1     Running   0          28m
synergychat-web-846d86c444-sk6n4      1/1     Running   0          28m
synergychat-web-846d86c444-w2pqg      1/1     Running   0          28m
```

vs.

```
wagslane@MacBook-Pro courses % kubectl -n kube-system get pod
NAME                               READY   STATUS    RESTARTS     AGE
coredns-5d78c9869d-jwcbr           1/1     Running   0            4d
etcd-minikube                      1/1     Running   0            4d
kube-apiserver-minikube            1/1     Running   0            4d
kube-controller-manager-minikube   1/1     Running   0            4d
kube-proxy-j2ssm                   1/1     Running   0            4d
kube-scheduler-minikube            1/1     Running   0            4d
storage-provisioner                1/1     Running   1 (4d ago)   4d
```

The `kube-system` namespace is where all the core Kubernetes components live, it's created automatically when you install Kubernetes. You don't want to mess with it.

## Making a New Namespace

If you have a small cluster with only a few applications, you can probably get away with just using the `default` namespace. However, if you have a large cluster with many applications, and in particular many teams working on those applications, namespaces are a great way to keep things organized.

For the sake of learning, let's assume that we have a separate development team responsible for the `crawler` service at SynergyChat, and they want to have their own namespace.

## Assignment

Create a new namespace called `crawler`:

```bash
kubectl create ns crawler
```

Make sure it was created successfully:

```bash
kubectl get ns
```

Next, add `namespace: crawler` to the `metadata` section of each of the `crawler` resources and apply them. Interestingly, you should see that the resources are "created" not "updated". That's because they're now in a new namespace, and the unique identifier of a resource in Kubernetes is the combination of its name and its namespace.

Make sure your resources are now redeployed in the `crawler` namespace:

```bash
kubectl -n crawler get pods
kubectl -n crawler get svc
kubectl -n crawler get configmaps
```

Then go _delete_ the old resources in the `default` namespace:

```bash
kubectl delete deployment <deployment-name>
kubectl delete service <service-name>
kubectl delete configmap <configmap-name>
```

Next, run:

```bash
kubectl proxy
```

**Run and submit** the CLI tests.

# Intra-Cluster DNS

The front-end of SynergyChat communicates with the `api` application via an external Gateway:

Domain name `http://synchatapi.internal` -> Gateway -> service -> pod

It's now time to connect the `crawler` and `api` applications. The `api` needs to be able to make HTTP requests directly to the `crawler` so that it can get the latest data to power the "stats" slash command.

`front-end` -> `api` -> `crawler`

The HTTP communication between the `api` and `crawler` is strictly _internal_ to the cluster, there's no need for an external domain name or Gateway. That makes it simpler, faster and more secure.

## Slash Command

With the tunnel open (`minikube tunnel -c`) open `http://synchat.internal/` in your browser. Post a new message with `/stats` as the message text. You should see a response that says:

```
crawler-bot: Crawler worker not configured
```

That's because the `api` doesn't know how to communicate with the `crawler` yet. Let's fix that.

## DNS

Kubernetes automatically creates [DNS entries](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/) for each service that can be used to route HTTP traffic between services. The format is:

```
<service-name>.<namespace>.svc.cluster.local
```

## Assignment

Update the `api` application's ConfigMap and Deployment to add a new environment variable called `CRAWLER_BASE_URL` its value should be:

```
http://<service-name>.<namespace>.svc.cluster.local
```

If it's not connecting, you may need to include the port:  
`http://<service-name>.<namespace>.svc.cluster.local:80`

Replace `<service-name>` and `<namespace>` with the appropriate values for the `crawler` service. This will allow the `api` to make HTTP requests to the `crawler` service.

Apply your changes, restart the pod, then post a new message to SynergyChat with `/stats` as the message text and answer the question on the right.

# Intra-Cluster DNS

The front-end of SynergyChat communicates with the `api` application via an external Gateway:

Domain name `http://synchatapi.internal` -> Gateway -> service -> pod

It's now time to connect the `crawler` and `api` applications. The `api` needs to be able to make HTTP requests directly to the `crawler` so that it can get the latest data to power the "stats" slash command.

`front-end` -> `api` -> `crawler`

The HTTP communication between the `api` and `crawler` is strictly _internal_ to the cluster, there's no need for an external domain name or Gateway. That makes it simpler, faster and more secure.

## Slash Command

With the tunnel open (`minikube tunnel -c`) open `http://synchat.internal/` in your browser. Post a new message with `/stats` as the message text. You should see a response that says:

```
crawler-bot: Crawler worker not configured
```

That's because the `api` doesn't know how to communicate with the `crawler` yet. Let's fix that.

## DNS

Kubernetes automatically creates [DNS entries](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/) for each service that can be used to route HTTP traffic between services. The format is:

```
<service-name>.<namespace>.svc.cluster.local
```

## Assignment

Update the `api` application's ConfigMap and Deployment to add a new environment variable called `CRAWLER_BASE_URL` its value should be:

```
http://<service-name>.<namespace>.svc.cluster.local
```

If it's not connecting, you may need to include the port:  
`http://<service-name>.<namespace>.svc.cluster.local:80`

Replace `<service-name>` and `<namespace>` with the appropriate values for the `crawler` service. This will allow the `api` to make HTTP requests to the `crawler` service.

Apply your changes, restart the pod, then post a new message to SynergyChat with `/stats` as the message text and answer the question on the right.

To restart a pod in Kubernetes, the most common and "correct" way is to perform a rollout restart of the deployment that manages the pod. This ensures that new pods are spun up before the old ones are terminated, maintaining availability.

You can use the following command:

```bash
kubectl rollout restart deployment <deployment-name>
```

In the context of this lesson, if you wanted to restart the `api` application, you would run:

```bash
kubectl rollout restart deployment api
```

### Alternative Methods

1. **Deleting the Pod**: If you delete a pod that is managed by a Deployment or ReplicaSet, Kubernetes will notice the pod is missing and automatically create a new one to match the desired state.
    
    ```bash
    kubectl delete pod <pod-name>
    ```
    
2. **Scaling**: You can scale the deployment down to zero and then back up to your desired number of replicas.
    
    ```bash
    kubectl scale deployment <deployment-name> --replicas=0
    kubectl scale deployment <deployment-name> --replicas=1
    ```
    

The `rollout restart` is generally preferred because it is a clean, built-in way to refresh your application pods without manual scaling or risk of downtime.

# Namespace and Routing Review

Namespaces are used to separate resources into logical groups. They also impact how internal DNS works.

At Boot.dev, we have all of our production backend services in a namespace, let's pretend it's called "backend". Each time I use `kubectl` to work with the backend services, I have to specify the namespace:

```bash
kubectl -n backend get pods
```

## Internal Routing

Kubernetes makes it _really easy_ for pods to communicate with each other. It does this by automatically creating DNS entries for each service. The format is:

```
<service-name>.<namespace>.svc.cluster.local
```

In reality, the `.svc.cluster.local` isn't needed in most scenarios. If you just use `http://<service-name>.<namespace>` for the api->crawler communication, it will work. When working in the same namespace, you can even just use `http://<service-name>`. That wouldn't work for us in our scenario just because the `crawler` is in its own separate namespace.

## Internal Is Better Than External

Unless a service really needs to be made available to the outside world, it's better to keep it internal to the cluster. Internal communications are great because:

- It's faster (assuming nodes are close to each other physically)
- No public DNS is required
- Communication is inherently more secure because it runs on an internal network (usually don't even need HTTPS)

The architecture of SynergyChat is a good example of this. We expose a single JSON API to the outside world, and if the pod that serves those HTTP requests doesn't have all the info it needs locally, it makes internal HTTP requests to other services.

---


CH9: Scaling

# Top

We've already learned how to look at logs for k8s pods, but sometimes that's not enough when it comes to debugging. Sometimes we want to know about the _resources_ that a pod is using.

To get metrics working, we need to enable the `metrics-server` addon. Run:

```bash
minikube addons enable metrics-server
```

Take a look inside the `kube-system` namespace:

```bash
kubectl -n kube-system get pod
```

You should see a new "metrics-server" pod. _It might take a couple of minutes to get started_, but once that pod is ready, you should be able to run:

```bash
kubectl top pod
```

You should see something like this:

```
NAME                               CPU(cores)   MEMORY(bytes)
synergychat-api-76b796b58d-x5wpk   1m           14Mi
synergychat-web-846d86c444-d9c8q   1m           15Mi
synergychat-web-846d86c444-sk6n4   1m           15Mi
synergychat-web-846d86c444-w2pqg   1m           15Mi
```

The `kubectl top` command (just like the [unix top command](https://en.wikipedia.org/wiki/Top_\(software\))) will show you the resources that each pod is using. In the example above, each pod is using about 1 milliCPU and 15 megabytes of memory.

# Vertical and Horizontal Scaling

Generally speaking, there are two ways to scale an application: vertically and horizontally. When I say "Scaling", I'm talking about increasing the capacity of an application. For example, maybe we have a web server, and to handle roughly 1000 requests per second, it uses about:

- 1/2 of a CPU core
- 1 GB of RAM

If we want to "scale up" to handle 2000 requests per second, we could double the CPU and RAM:

- 1 CPU core
- 2 GB of RAM

This is called "vertical scaling" because we're increasing the capacity of the application by increasing the resources available to it. We're scaling up. Scaling up works until it doesn't. You can only scale up as much as your hardware will allow (the maximum number of CPUs and amount of RAM your node has).

The other way to scale is horizontally. Instead of increasing the resources available to the application, we increase the number of _instances_ of the application (pods). Pods can be distributed _across_ nodes, so we can scale horizontally until we run out of nodes. When working in a system like Kubernetes, it's generally better to scale horizontally than vertically.

# Resource Limits

None of our current deployments have any resource limits set. We have very little traffic, so it's not currently an issue, but in a production environment, we would want to set resource limits to ensure that our pods don't consume too many resources.

We wouldn't want a pod to hog all the CPU and RAM on its node, suffocating all of the other pods on the node.

## Setting Limits

We can set [resource limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) in our deployment files. Here's an example:

```yaml
spec:
  containers:
    - name: <container-name>
      image: <image-name>
      resources:
        limits:
          memory: <max-memory>
          cpu: <max-cpu>
```

Memory is measured in bytes, so we can use the suffixes `Ki`, `Mi`, and `Gi` to specify [kibibytes, mebibytes, and gibibytes](https://en.wikipedia.org/wiki/Byte#Multiple-byte_units), respectively. For example, `512Mi` is 512 mebibytes.

CPU is [measured in cores](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#meaning-of-cpu), so we can use the suffix `m` to specify milli-cores. For example, `500m` is 500 milli-cores, or 0.5 cores.

## Assignment

It would be really hard to test resource limits with our SynergyChat web application because we have no production traffic. Instead, I've created a couple of custom applications we can use to test and debug resource limits.

The `bootdotdev/synergychat-testcpu:latest` image on Docker Hub is an application that simply consumes as much CPU power as it can.

Create a new file called `testcpu-deployment.yaml` with the following:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: synergychat-testcpu
  name: synergychat-testcpu
spec:
  replicas: 1
  selector:
    matchLabels:
      app: synergychat-testcpu
  template:
    metadata:
      labels:
        app: synergychat-testcpu
    spec:
      containers:
        - image: bootdotdev/synergychat-testcpu:latest
          name: synergychat-testcpu
```

Add a CPU limit of `50m` to the deployment.

Apply the deployment, then make sure the pod is running:

```bash
kubectl get pod
```

It might take a minute or so, but soon you should be able to see its metrics with `top`:

```bash
kubectl top pod
```

Assuming everything is working properly, you should see that the pod is using about 50 milli-cores of CPU. That's because k8s is throttling the pod to ensure that it doesn't use more than 50 milli-cores.

# Limits - RAM

We've successfully throttled the CPU usage of our `testcpu` pod, but what about RAM?

Create a new file called `testram-deployment.yaml` with the following:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: synergychat-testram
  name: synergychat-testram
spec:
  replicas: 1
  selector:
    matchLabels:
      app: synergychat-testram
  template:
    metadata:
      labels:
        app: synergychat-testram
    spec:
      containers:
        - image: bootdotdev/synergychat-testram:latest
          name: synergychat-testram
```

Add a memory limit of `256Mi` (256 Megabytes) to the deployment. Remember, this is the [syntax](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) to do so:

```yaml
spec:
  containers:
    - name: <container-name>
      image: <image-name>
      resources:
        limits:
          memory: <max-memory>
          cpu: <max-cpu>
```

Additionally, create a ConfigMap called `testram-configmap.yaml` with the name "synergychat-testram-configmap" for this deployment that specifies a single environment variable:

- `MEGABYTES`: `"200"`

That will tell the application to allocate 200 megabytes of memory. Update the deployment to use the config map, then apply both.

Make sure the pod is healthy:

```bash
kubectl get pods
```

After a minute or so, you should be able to see the memory usage of the pod:

```bash
kubectl top pods
```

The memory usage should be a little over 200 megabytes, but not more than 256 megabytes.

# Breaking the Limits

You may have noticed that with the `testcpu` application, we never "told" the application how much CPU to use. That's because generally speaking, applications don't know how much CPU they should use. They just go as "fast" as they can when they're doing computations.

Memory is different, applications allocate memory based on a variety of factors, and while an application can have its CPU throttled and just "go slower", if an application runs out of available memory, it will crash.

Let's test that!

Update the `MEGABYTES` environment variable for the `testram` application to `500` and apply the change.

Delete the `testram` pod so that the new environment variable takes effect. Assuming you did everything correctly, the pod should crash. You'll be able to check with:

```bash
kubectl get pods
```

Then describe the individual pod:

```bash
kubectl describe pod <pod-name>
```

Look for a section in the output that looks like this:

```
Containers:
  synergychat-testram:
    Container ID:   docker://453facc1515e05ec553ad755c6a0edffd3e67b62c14b4ddb328cc0f8d5c67250
    Image:          bootdotdev/synergychat-testram:latest
    Image ID:       docker-pullable://bootdotdev/synergychat-testram@sha256:a127779899f29d7b2e1fc80ed75e001eaed8e7cec0985707a802319fcdd9bec1
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       XXX
```

Answer the question on the right about the reason for the crash.

# Fix the Limits

I don't want the pod to be consuming too much of your machine's resources, nor do I want you to have a constantly crashing pod, so before moving on, let's just reduce the memory usage of the `testram` pod.

Set the `MEGABYTES` environment variable to `10` and apply the change, then delete the pod so that the new environment variable takes effect.

Use `get pods` and `top pods` to make sure the pod is healthy and is using around 10 megabytes of memory.

# Horizontal Pod Autoscaling (HPA)

A [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) can automatically scale the number of Pods in a Deployment based on observed CPU utilization or other custom metrics. It's very common in a Kubernetes environment to have a low number of pods in a deployment, and then scale up the number of pods automatically as CPU usage increases.

## Assignment

First, delete the `replicas: 1` line from the `synergychat-testcpu` deployment. This will allow our new autoscaler to have full control over the number of pods.

Create a new file called `testcpu-hpa.yaml`. Add the following YAML to it:

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: testcpu-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: x
  minReplicas: x
  maxReplicas: x
  targetCPUUtilizationPercentage: x
```

Set the following values:

- `name`: The name of the `synergychat-testcpu` deployment
- `minReplicas`: `1`
- `maxReplicas`: `4`
- `targetCPUUtilizationPercentage`: `50`

This hpa will monitor the CPU usage of the pods in the `synergychat-testcpu` deployment. Its goal is to scale up or down the number of pods in the deployment so that the average CPU usage of all pods is around 50%. As CPU usage increases, it will add more pods. As CPU usage decreases, it will remove pods. You can find the algorithm it uses [here](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#algorithm-details) if you're interested.

Apply the hpa, then run the following commands every few seconds to watch as the number of pods scales up:

```bash
kubectl get pods
kubectl top pods
```

An `hpa` is just another resource, so you can also use `kubectl get hpa` to see the current state of the autoscaler.

If `kubectl get hpa` shows `<unknown>` for CPU after a reboot, make sure your local cluster and metrics server are running again before debugging the YAML.

# HPA - Web

Now that you've seen how an application that chews through CPU will quickly scale up from a single pod to multiple pods, let's see what happens with an application that doesn't have much going on in terms of compute resources.

## Assignment

Delete the line "replicas: 3" from the `web` deployment. This will allow our new autoscaler to have full control over the number of pods.

Copy your `testcpu-hpa.yaml` file and call it `web-hpa.yaml`. Update the following values:

- `name: web-hpa`
- Target the "web" deployment
- Keep the scaling values the same

Apply the hpa, then use the following commands to see if any scaling happens:

```bash
kubectl get pods
kubectl top pods
```

---

CH10: Nodes

# Nodes

We've talked about how in a production environment, you'll have multiple nodes in your cluster. For this course, we've been using a single node cluster with `minikube`. The nice thing about Kubernetes is that almost everything you do with it is abstracted away from the underlying infrastructure with the `kubectl` CLI.

All the commands we've been using locally will work the same way on a production cluster.

## Ways to Deploy to Production

There are several popular ways to deploy a Kubernetes cluster to production:

- [GKE (Google Kubernetes Engine) / Autopilot](https://cloud.google.com/kubernetes-engine)
- [EKS (Amazon Elastic Kubernetes Service)](https://aws.amazon.com/eks/)
- [AKS (Azure Kubernetes Service)](https://azure.microsoft.com/en-us/products/kubernetes-service)
- [Manual Deployment](https://kubernetes.io/docs/setup/production-environment/)

### Gke, Eks, Aks

These are all managed Kubernetes services, offered by the big 3 cloud providers. They're all pretty similar, though I believe GKE is generally the most feature-rich of the three. GKE also has a cool [auto-pilot mode](https://cloud.google.com/kubernetes-engine/docs/concepts/autopilot-overview) that makes it so that you don't have to worry about managing nodes at all.

The nice thing about a managed offering is that it can be configured to handle autoscaling at the node level. This means that you can set up your cluster to automatically add and remove nodes based on the load of your cluster.

### Manual

You can also set up your own cluster manually. I've worked on teams where a cloud engineering team has custom scripts that configure a cluster on top of standard EC2 instances. Then they have their own autoscaling scripts that add and remove nodes based on the load of the cluster. I happened to work at a company that manually deployed k8s to a group of AWS virtual machines, but you could also do the same thing on physical machines.

Deploying manually was more popular before the managed services were released, but it's still a viable option if you want more control over your cluster.

# Node Types

Broadly speaking, there are two types of machines in a production Kubernetes cluster:

- Control Plane
- Worker Nodes

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/6vd1h1w-1280x605.png)

The control plane is responsible for managing the cluster. It's where the API server, scheduler, and controller manager live. The control plane used to be called "master nodes", but that term is deprecated now.

When you hear the word "node" used in isolation it's _usually_ referring to worker nodes. In fact, because I'm using GKE for Boot.dev, when I run:

```bash
kubectl get nodes
```

I get back:

```
NAME                                       STATUS   ROLES    AGE     VERSION
gk3-kube-prod-nap-1cmjprlt-92e1dc91-jmi8   Ready    <none>   5d21h   v1.25.12-gke.500
gk3-kube-prod-nap-1cmjprlt-dfe4d6de-cq0k   Ready    <none>   5d21h   v1.25.12-gke.500
```

These are my worker nodes. They're the machines that are actually running my containers. The control plane is fully managed by GKE, so I don't have to worry about it. That's usually fine because as the load on my cluster increases, I'm mostly concerned with scaling out my worker nodes and making sure they're healthy. The control plane is fairly static.

# Resource Requests

We talked about resource _limits_, but there's another critical concept to understand: resource _requests_.

A resource request is the amount of a resource that a pod _requests_ from the node it's running on. A resource limit, on the other hand, is the _maximum_ amount of a resource that a pod is allowed to consume before it's throttled or killed.

## Why Do We Need Requests?

Let's say we have 2 nodes:

|Node|RAM|
|---|---|
|Node 1|8GB|
|Node 2|8GB|

And we have 4 pods:

|Pod|Node|RAM|
|---|---|---|
|Pod 1|Node 1|3GB|
|Pod 2|Node 1|3GB|
|Pod 3|Node 2|3GB|
|Pod 4|Node 2|3GB|

This is valid. Currently, only 6/8 GB of RAM is being used on each node. The trouble is, even though each node has 2GB of RAM left, if we try to add another pod, and it ends up utilizing more than 2GB of RAM (like the 3GB it will likely need), it will crash.

_Resource requests solve this_.

If we add a resource request of 3GB, Kubernetes will know that each pod needs 3GB of RAM to run. If we try to [schedule](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#how-pods-with-resource-requests-are-scheduled) a new pod with the request in place, k8s will gracefully tell us it doesn't have enough resources to do so, or it will use a node in the cluster that has at least 3GB of RAM available.

## Assignment

Let's set an absurdly high resource request for our `synergychat-testram` pods.

1. [ ] Update the `resources` section of its deployment. I used 40GB:

```yaml
resources:
  limits:
    memory: 40000Mi
  requests:
    memory: 40000Mi
```

Apply the deployment, then check on your pods:

```bash
kubectl get pods
```

You _should_ see that the pod is "Pending". If it worked... well you have more money than me to spend on hardware. Go even higher!

2. [ ] Once you've got it in a pending state, `describe` the pod and look at the "Events" section of the output to see what's going on:

```bash
kubectl describe pod <pod-name>
```

# Requests

One of the most important things to get right when working with pod autoscalers in Kubernetes are the resource requests and limits. If you don't set them correctly, you can end up with a situation where your pods are crashing, or your autoscaler is scaling up too many pods.

Generally speaking, my rule of thumb is:

- Set memory _requests_ ~10% higher than the average memory usage of your pods
- Set CPU _requests_ to 50% of the average CPU usage of your pods
- Set memory _limits_ ~100% higher than the average memory usage of your pods
- Set CPU _limits_ ~100% higher than the average CPU usage of your pods

Why these numbers? Consider several points:

## Memory Is Scarier

Memory is the scariest resource to run out of. If you run out of CPU, your pods will just slow down. If you run out of memory, your pods will crash. For that reason, it's more important to add a buffer to your memory requests than your CPU requests.

## Limits Are for Protection

Limits should only take effect when a pod is using more resources than it should. Limits are like a safety net. If your limits are constantly being hit, you should either increase them or fix your application code so that it uses fewer resources.

As such, limits should generally be set higher than requests.

## Requests Are for Scheduling

Because requests are used to schedule pods, you want to make sure that your requests are high enough that once scheduled, your pods will have the resources, but not so high that you're wasting resources. If you set your requests too high, you'll end up with a situation where you can't schedule pods because k8s _thinks_ it doesn't have enough resources, even though it does.

## It All Depends!

These are just rules of thumb! At the end of the day, you always need to understand how your applications work, and what resources they need. The right numbers for your applications might be drastically different than the numbers I've suggested here.
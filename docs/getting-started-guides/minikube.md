---
---

The following instructions show you how to set up a simple, single node Kubernetes cluster using Minikube.

Here's a diagram of what the final result will look like:

![Kubernetes Single Node on Minikube](/images/docs/minikube-hello-world-diagram.svg)

* TOC
{:toc}

## Prerequisites

1. Minikube must installed on your machine.  To install minikube follow the steps listed at:
[minikube's github releases page here](https://github.com/kubernetes/minikube/releases)

2. Virtualbox must be installed on your machine.  Virtualbox can be installed from: [the Virtualbox website here](https://www.Virtualbox.org/wiki/Downloads) (also can be installed on Linux via apt-get with `sudo apt-get install Virtualbox`)

3. The `kubectl` command must be installed on your machine.  `kubectl` can be installed from the [gcloud page which has installation instructions here](https://cloud.google.com/container-engine/docs/quickstart#install_the_gcloud_command-line_interface)

4. (TODO: optional?) VT-x/AMD-v virtualization must be enabled in BIOS

### Verify that minikube is installed (TODO: other software as well?)

Assuming that you have correctly installed minikube, the following command `minikube version` should run and display the installed minikube version

```shell
$ minikube version
minikube version: <installed-version-of-minikube>
```

### Starting Minikube
To start minikube, the `minikube start` command is used.  This command launches a Virtualbox vm with a lightweight iso image capable of running the kubernetes api which your local machine can interact with via `kubectl`

```shell
$ minikube start
Starting local Kubernetes cluster...
Running pre-create checks...
Creating machine...
Starting local Kubernetes cluster...
Kubernetes is available at https://192.168.99.100:443.
```
And then to setup the environment variables that minikube needs to run commands against your Virtualbox vm:

```shell
$ eval $(minikube docker-env)
$ minikube stop
Stopping local Kubernetes cluster...
Stopping "minikubeVM"...
```


### Run a echo server application
Now we will run an echo server container image using `kubectl`:

```shell
$ kubectl run echo-server --image=gcr.io/google_containers/echoserver:1.4 --hostport=8000 --port=8080
deployment "hello-minikube" created
Successfully built d16fe85e1abe
```

### Accessing our echo-server pod
To access our echo-server we can use the following command:

```shell
curl http://$(minikube ip):8000
CLIENT VALUES:
client_address=192.168.99.1
command=GET
real path=/
...
```

This server 'echo's' information about the client request that has accessed it.

<!-- ### Expose it as a service (TODO: this has never worked for me) -->

<!-- ```shell -->
<!-- kubectl expose deployment nginx --port=80 -->
<!-- ``` -->

### Turning down your cluster
There are two options for turning down your cluster
1. The `minikube stop` command which stops your local kubernetes cluster running in Virtualbox. This command stops the VM itself, leaving all files intact. The cluster can be started again with the "start" command."

```shell
$ minikube stop
Stopping local Kubernetes cluster...
Machine stopped.
```
2. The `minikube delete` command which will delete your local kubernetes cluster. This command deletes the VM, and removes all associated files.

```shell
$ minikube delete
Deleting local Kubernetes cluster...
Machine deleted.
```

### Troubleshooting
If you encounter any errors, questions can be asked and solutions can be found at the minikube issues page [here](https://github.com/kubernetes/minikube/issues/)

## Support Level

IaaS Provider        | Config. Mgmt | OS     | Networking  | Docs                                              | Conforms | Support Level
-------------------- | ------------ | ------ | ----------  | ---------------------------------------------     | ---------| ----------------------------
Minikube Single Node   | custom       | N/A    | local       | [docs](/docs/getting-started-guides/minikube)                                 |          | Project ([@brendandburns](https://github.com/brendandburns))


For support level information on all solutions, see the [Table of solutions](/docs/getting-started-guides/#table-of-solutions) chart.


## Further reading

Please see the [Kubernetes docs](/docs/) for more details on administering
and using a Kubernetes cluster.

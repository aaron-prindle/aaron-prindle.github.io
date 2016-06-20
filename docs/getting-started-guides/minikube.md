---
---

The following instructions show you how to set up a simple, single node Kubernetes cluster using Minikube.

Here's a diagram of what the final result will look like:

![Kubernetes Single Node on Minikube](/images/docs/minikube-hello-world-diagram.svg)

* TOC
{:toc}

## Prerequisites

1. Minikube must installed on your machine.  To install minikube follow the steps listed at minikube's github releases page
[here](https://github.com/kubernetes/minikube/releases)

2. Virtualbox must be installed on your machine.  Virtualbox can be installed from the virtualbox website [here](https://www.virtualbox.org/wiki/Downloads) (also can be installed on Linux via apt-get with `sudo apt-get install virtualbox`)

3. The `kubectl` command must be installed on your machine.  `kubectl` can be installed by following the instructions [here](https://cloud.google.com/container-engine/docs/quickstart#install_the_gcloud_command-line_interface)

4. (TODO: optional?) VT-x/AMD-v virtualization must be enabled in BIOS

### Verify that minikube is installed (TODO: other software as well?)

Assuming that you have correctly installed minikube, the following command `minikube version` should run and display the installed minikube version

```shell
$ minikube version
minikube version: <installed-version-of-minikube>
```

### Starting Minikube
To start minikube, the `minikube start` command is used.  This command launches a virtualbox vm with a lightweight iso image capable of running a kubernetes api that your local machine can interact with via `kubectl`

```shell
$ minikube start
Starting local Kubernetes cluster...
Running pre-create checks...
Creating machine...
Starting local Kubernetes cluster...
Kubernetes is available at https://192.168.99.100:443.
```
And then to setup the environment variables that minikube needs to run commands against your virtualbox vm:
```shell
$ eval $(minikube docker-env)
$ minikube stop
Stopping local Kubernetes cluster...
Stopping "minikubeVM"...
```


### Run an application
Now we will run a hello-world container image inside of a hello-minikube deployment inside of minikube:

```shell
$ kubectl run hello-minikube --image=tutum/hello-world (TODO: change this image)
deployment "hello-minikube" created
Successfully built d16fe85e1abe
```

### Accessing our hello-world pod

```shell
$ kubectl port-forward hello-minikube-3405552688-96yyv 8080:80 (TODO: explain how to get pod name)
I0616 16:48:59.585906   73205 portforward.go:213] Forwarding from 127.0.0.1:8080 -> 80
I0616 16:48:59.586069   73205 portforward.go:213] Forwarding from [::1]:8080 -> 80
...
```
And in a different shell:
```shell
$ curl localhost:8080
<hello-world page>
...
```
Or alternativelt you can view the page in you browser at the url:
`localhost:8080`


### Expose it as a service (TODO: this has never worked for me)

```shell
kubectl expose deployment nginx --port=80
```
### Turning down your cluster

1. The `minikube delete` command

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

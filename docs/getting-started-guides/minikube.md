---
---

The following instructions show you how to set up a simple, single node Kubernetes cluster using Minikube.

Here's a diagram of what the final result will look like:

![Kubernetes Single Node on Minikube](/images/docs/k8s-minikube.png)

* TOC
{:toc}

## Prerequisites

1. Minikube must installed on your machine.  To install minikube follow the steps listed at minikube's github releases page
[here](https://github.com/kubernetes/minikube/releases)

2. Virtualbox must be installed on your machine.  Virtualbox can be installed from the virtualbox website [here](https://www.virtualbox.org/wiki/Downloads)

3. VT-x/AMD-v virtualization must be enabled in BIOS

4. The `kubectl` command must be installed on your machine.  `kubectl` can be installed by following the instructions [here](https://cloud.google.com/container-engine/docs/quickstart#install_the_gcloud_command-line_interface)

### Verify that minikube(other software as well?) is installed

Assuming that you have correctly installed minikube, the following command `minikube version` should run and display minikube's version
```shell
$ minikube version
minikube version: <installed-version-of-minikube>
```

### Starting Minikube
In order to start minikube, the `minikube start` command is run.  This command launches a virtualbox vm with a running kubernetes api that your machine can interact with via `kubectl`

```shell
$ minikube start
Starting local Kubernetes cluster...
Running pre-create checks...
Creating machine...
Starting local Kubernetes cluster...
Kubernetes is available at https://192.168.99.100:443.
```

```shell
$ eval $(minikube docker-env)
$ minikube stop
Stopping local Kubernetes cluster...
Stopping "minikubeVM"...
```


### Run an application

```shell
$ docker build -t helloworld .
Successfully built d16fe85e1abe
$ kubectl run hello-minikube --image=helloworld --hostport=8000 --port=8080 --generator=run-pod/v1
pod "hello-minikube" created
$ curl http://$(minikube ip):8000
Hello World!
```

### Expose it as a service

```shell
kubectl expose deployment nginx --port=80
```

Run the following command to obtain the cluster local IP of this service we just created:

```shell{% raw %}
ip=$(kubectl get svc nginx --template={{.spec.clusterIP}})
echo $ip
{% endraw %}```

Hit the webserver with this IP:

```shell{% raw %}
kubectl get svc nginx --template={{.spec.clusterIP}}
{% endraw %}```

On OS X, since minikube is running inside a VM, run the following command instead:

```shell
minikube-machine ssh `minikube-machine active` curl $ip
```

## Deploy a DNS

See [here](/docs/getting-started-guides/minikube-multinode/deployDNS/) for instructions.

### Turning down your cluster

1. Delete all the containers including the kubelet:

Many of these containers run under the management of the `kubelet` binary, which attempts to keep containers running, even if they fail.
So, in order to turn down the cluster, you need to first kill the kubelet container, and then any other containers.

You may use `minikube rm -f $(minikube ps -aq)`, note this removes _all_ containers running under Minikube, so use with caution.

2. Cleanup the filesystem:

On OS X, first ssh into the minikube VM:

```shell
minikube-machine ssh `minikube-machine active`
```

```shell
sudo umount `cat /proc/mounts | grep /var/lib/kubelet | awk '{print $2}'` 
sudo rm -rf /var/lib/kubelet
```

### Troubleshooting

## Support Level


IaaS Provider        | Config. Mgmt | OS     | Networking  | Docs                                              | Conforms | Support Level
-------------------- | ------------ | ------ | ----------  | ---------------------------------------------     | ---------| ----------------------------
Minikube Single Node   | custom       | N/A    | local       | [docs](/docs/getting-started-guides/minikube)                                 |          | Project ([@brendandburns](https://github.com/brendandburns))


For support level information on all solutions, see the [Table of solutions](/docs/getting-started-guides/#table-of-solutions) chart.


## Further reading

Please see the [Kubernetes docs](/docs/) for more details on administering
and using a Kubernetes cluster.


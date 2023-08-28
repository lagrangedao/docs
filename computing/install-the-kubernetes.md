---
description: The Kubernetes version should be v1.24.0+
---

# Install the Kubernetes

### Install Container Runtime Environment

If you plan to run a Kubernetes cluster, you need to install a container runtime into each node in the cluster so that Pods can run there, refer to [here](https://kubernetes.io/docs/setup/production-environment/container-runtimes/). And you just need to choose one option to install the `Container Runtime Environment`

#### **Option 1: Install the `Docker` and `cri-dockerd` （Recommended）**

To install the `Docker Container Runtime` and the `cri-dockerd`, follow the steps below:

* Install the `Docker`:
  * Please refer to the official documentation from [here](https://docs.docker.com/engine/install/).
* Install `cri-dockerd`:
  * `cri-dockerd` is a CRI (Container Runtime Interface) implementation for Docker. You can install it refer to [here](https://github.com/Mirantis/cri-dockerd).

#### **Option 2: Install the `Containerd`**

`Containerd` is an industry-standard container runtime that can be used as an alternative to Docker. To install `containerd` on your system, follow the instructions on [getting started with containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md).

### Optional-Setup a docker registry server

**If you are using the docker and you have only one node, the step can be skipped**.

If you have deployed a Kubernetes cluster with multiple nodes, it is recommended to set up a **private Docker Registry** to allow other nodes to quickly pull images within the intranet.

* Create a directory `/docker_repo` on your docker server. It will be mounted on the registry container as persistent storage for our docker registry.

```bash
sudo mkdir /docker_repo
sudo chmod -R 777 /docker_repo
```

* Launch the docker registry container:

```bash
sudo docker run --detach \
  --restart=always \
  --name registry \
  --volume /docker_repo:/docker_repo \
  --env REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/docker_repo \
  --publish 5000:5000 \
  registry:2
```

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

*   Add the registry server to the node

    * If you have installed the `Docker` and `cri-dockerd`(**Option 1**), you can update every node's configuration:

    ```bash
    sudo vi /etc/docker/daemon.json
    ```

    ```
    ## Add the following config
    "insecure-registries": ["<Your_registry_server_IP>:5000"]
    ```

    Then restart the docker service

    ```bash
    sudo systemctl restart docker
    ```

    * If you have installed the `containerd`(**Option 2**), you can update every node's configuration:

```bash
[plugins."io.containerd.grpc.v1.cri".registry]
  [plugins."io.containerd.grpc.v1.cri".registry.mirrors]
    [plugins."io.containerd.grpc.v1.cri".registry.mirrors."<Your_registry_server_IP>:5000"]
      endpoint = ["http://<Your_registry_server_IP>:5000"]

[plugins."io.containerd.grpc.v1.cri".registry.configs]
  [plugins."io.containerd.grpc.v1.cri".registry.configs."<Your_registry_server_IP>:5000".tls]
      insecure_skip_verify = true                                                               
```

Then restart `containerd` service

```bash
sudo systemctl restart containerd
```

**\<Your\_registry\_server\_IP>:** the intranet IP address of your registry server.

Finally, you can check the installation by the command:

```bash
docker system info
```

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Create a Kubernetes Cluster

To create a Kubernetes cluster, you can use a container management tool like `kubeadm`. The below steps can be followed:

* [Install the kubeadm toolbox](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/).
* [Create a Kubernetes cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

### Install the Network Plugin

Calico is an open-source **networking and network security solution for containers**, virtual machines, and native host-based workloads. Calico supports a broad range of platforms including **Kubernetes**, OpenShift, Mirantis Kubernetes Engine (MKE), OpenStack, and bare metal services.

To install Calico, you can follow the below steps, more information can be found [here](https://docs.tigera.io/calico/3.25/getting-started/kubernetes/quickstart).

**step 1**: Install the Tigera Calico operator and custom resource definitions

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/tigera-operator.yaml
```

**step 2**: Install Calico by creating the necessary custom resource

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/custom-resources.yaml
```

**step 3**: Confirm that all of the pods are running with the following command

```
watch kubectl get pods -n calico-system
```

**step 4**: Remove the taints on the control plane so that you can schedule pods on it.

```
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
kubectl taint nodes --all node-role.kubernetes.io/master-
```

If you have installed it correctly, you can see the result shown in the figure by the command `kubectl get po -A`

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

**Note:**

* If you are a single-host Kubernetes cluster, remember to remove the taint mark, otherwise, the task can not be scheduled to it.

```
kubectl taint node ${nodeName}  node-role.kubernetes.io/control-plane:NoSchedule-
```



#### Install the NVIDIA Plugin

If your computing provider wants to provide a GPU resource, the NVIDIA Plugin should be installed, please follow the steps:

* [Install NVIDIA Driver](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#nvidia-drivers).

> Recommend NVIDIA Linux drivers version should be 470.xx+

* [Install NVIDIA Device Plugin for Kubernetes](https://github.com/NVIDIA/k8s-device-plugin#quick-start).

If you have installed it correctly, you can see the result shown in the figure by the command `kubectl get po -n kube-system`

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### Install the Ingress-nginx Controller

The `ingress-nginx` is an ingress controller for Kubernetes using `NGINX` as a reverse proxy and load balancer. You can run the following command to install it:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.1/deploy/static/provider/cloud/deploy.yaml
```

If you have installed it correctly, you can see the result shown in the figure by the command:

* Run `kubectl get po -n ingress-nginx`

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

* Run `kubectl get svc -n ingress-nginx`

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>


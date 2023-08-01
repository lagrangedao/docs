# Installing computing-provider

## Preparation requirements
### Container Runtime Environment
You need to install a container runtime into each node in the cluster so that Pods can run there. 

#### Option 1: Install Docker and cri-dockerd
To install the Docker container runtime and the cri-dockerd, follow the steps below:
* Install Docker:
    - Instructions may vary depending on your operating system. Please refer to the official Docker documentation from [here](https://docs.docker.com/engine/install/).
* Install cri-dockerd:
    - cri-dockerd is a CRI (Container Runtime Interface) implementation for Docker. You can install it refer to [here](https://github.com/Mirantis/cri-dockerd).

#### Option 2: Install Containerd
Containerd is an industry-standard container runtime that can be used as an alternative to Docker. To install containerd on your system, follow the instructions on [getting started with containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md).

### Setup docker registry server
In a Kubernetes cluster with multiple nodes, it is recommended to set up a private Docker Registry to allow other nodes to quickly pull images within the intranet. If there is only a single node, you can ignore this step.
* Create a directory called /docker_repo on your Docker host. We will mount this directory as a volume inside a registry container to work with a persistent data store for our Docker registry.
```bash
mkdir /docker_repo
sudo chmod -R 777 /docker_repo
```
* Launch a Docker registry container with the following command:
```bash
sudo docker run --detach \
  --restart=always \
  --name registry \
  --volume /docker_repo:/docker_repo \
  --env REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/docker_repo \
  --publish 5000:5000 \
  registry:2
```
![image](https://github.com/FogMeta/docs/assets/102578560/2ca8f8c1-7b5e-42c4-8024-c2ba1fbbcc63)

* The following configuration needs to be set on each node in the Kubernetes cluster:
```bash
sudo vi /etc/docker/daemon.json
...
"insecure-registries": ["registryIP:5000"]
...
```
**registryIP:** the intranet IP address of the machine hosting the registry.

* Restart docker service
```bash
systemctl restart docker
```

#### Verify the Installation
```bash
docker system info
```
![image](https://github.com/FogMeta/docs/assets/102578560/eaabbcb2-e7f5-44f8-9e8e-cc94698accba)



### Install Kubernetes
To install Kubernetes, you can use a container management tool like kubeadm or kops. Follow these general steps:
* Install [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/).
* Initialize [Kubernetes master](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) use kubeadm.
   
#### Verify the Installation
After installing Kubernetes, you can verify its installation by running the following command:
```bash
kubectl get po -A
```
Once you have installed it successfully, you will see the screen below:
![image](https://github.com/FogMeta/docs/assets/102578560/c5771aa1-2806-430b-b777-c281b4516b6f)

**Note:**  Remove the node's taint mark.
```bash
kubectl taint node ${nodeName}  node-role.kubernetes.io/control-plane:NoSchedule-
```

### Install Network Plugin: Calico
Calico is a popular Kubernetes networking solution. To install Calico, refer to this [documentation](https://docs.tigera.io/calico/3.25/getting-started/kubernetes/quickstart).

### Install NVIDIA-Plugin
If your cluster requires NVIDIA GPU support, install the NVIDIA-Plugin by following these steps:
* Install the [NVIDIA device driver](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).
* Install the [NVIDIA device plugin](https://github.com/NVIDIA/k8s-device-plugin#quick-start).
#### Verify the Installation
After installing NVIDIA-Plugin, you can verify its installation by running the following command:
```bash
kubectl get po -n kube-system
```
Once you have installed it successfully, you will see the screen below::
![image](https://github.com/FogMeta/docs/assets/102578560/96d49ead-8f33-4bc1-888e-697d680e2519)

### Install Ingress-nginx Controller
ingress-nginx is an Ingress controller for Kubernetes using NGINX as a reverse proxy and load balancer. You can run the following command to install it:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.1/deploy/static/provider/cloud/deploy.yaml
```
#### Verify the Installation
You can verify its installation by running the following command:

```bash
kubectl get po -n ingress-nginx
```
Once you have installed it successfully, you will see the screen below::
![image](https://github.com/FogMeta/docs/assets/102578560/84ed2f2b-3500-4cc7-afea-5e47a6feaa6f)

```bash
kubectl get svc -n ingress-nginx
```
The installation is normal as seen by the following image:
![image](https://github.com/FogMeta/docs/assets/102578560/aa1b64ba-eba1-4c50-b679-23f96c8ab004)

### Configure host nginx
#### Install nginx  to the host
```bash
sudo apt update
sudo apt install nginx
```
#### Configure the local nginx configuration file
```bash
vi /etc/nginx/conf.d/crosschain.conf

# top-level http config for websocket headers
# If Upgrade is defined, Connection = upgrade
# If Upgrade is empty, Connection = close
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
        listen 80;
        listen [::]:80;
        server_name *.example.com;                                           # need to your domain
        return 301 https://$host$request_uri;
        #client_max_body_size 1G;
}
server {
        listen 443 ssl;
        listen [::]:443 ssl;
        ssl_certificate  /etc/letsencrypt/live/example.com/fullchain.pem;     # need to configure
        ssl_certificate_key  /etc/letsencrypt/live/example.com/privkey.pem;   # need to configure

        server_name *.example.com;                                            # need to your domain
        location / {
          proxy_pass http://127.0.0.1:32560; # Need to configure the port number of ingress-nginx-controller 
          proxy_set_header Host $http_host;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection $connection_upgrade;
       }
}
```
**Note:** There must be manual configuration items.
* `server_name`: generic domain name
* `ssl_certificate` and `ssl_certificate_key`: certificate for https.
* `proxy_pass`:  `32560` Port for ingress-nginx controller's 80 port

#### Restart nginx
```bash
sudo nginx -s reload
```

### Install Hardware resource Collector
```bash
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: DaemonSet
metadata:
  namespace: kube-system
  name: resource-exporter-ds
  labels:
    app: resource-exporter
spec:
  selector:
    matchLabels:
      app: resource-exporter
  template:
    metadata:
      labels:
        app: resource-exporter
    spec:
      containers:
      - name: resource-exporter
        image: filswan/resource-exporter:v11.2.2
        imagePullPolicy: IfNotPresent
EOF
```
#### Verify the Installation
You can verify its installation by running the following command:

```bash
kubectl get po -n kube-system
```
The installation is normal as seen by the following image:
![image](https://github.com/FogMeta/docs/assets/102578560/2c2f00da-1724-4fae-971a-d2b7019b6683)

### Install redis service using docker

* The following is the complete configuration file content of redis, which can directly replace the content of the local redis.conf：

```bash
# Redis configuration file example.

################################## NETWORK #####################################
bind 0.0.0.0
protected-mode no
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 300

################################# GENERAL #####################################
daemonize no
pidfile /var/run/redis_6379.pid
loglevel notice
logfile ""
databases 16
always-show-logo no
set-proc-title yes
proc-title-template "{title} {listen-addr} {server-mode}"

################################ SNAPSHOTTING  ################################
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
rdb-del-sync-files no
dir ./

################################# REPLICATION #################################
replica-serve-stale-data yes
replica-read-only yes
repl-diskless-sync yes
repl-diskless-sync-delay 5
repl-diskless-sync-max-replicas 0
repl-diskless-load disabled
repl-disable-tcp-nodelay no
replica-priority 100
acllog-max-len 128

############################# LAZY FREEING ####################################
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no
lazyfree-lazy-user-del no
lazyfree-lazy-user-flush no

################################ THREADED I/O #################################
oom-score-adj no
oom-score-adj-values 0 200 800
disable-thp yes

############################## APPEND ONLY MODE ###############################
appendonly no
appendfilename "appendonly.aof"
appenddirname "appendonlydir"
appendfsync everysec
no-appendfsync-on-rewrite no

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
aof-use-rdb-preamble yes
aof-timestamp-enabled no

slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0

############################# EVENT NOTIFICATION ##############################
notify-keyspace-events Ex

hash-max-listpack-entries 512
hash-max-listpack-value 64
list-max-listpack-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-listpack-entries 128
zset-max-listpack-value 64
hll-sparse-max-bytes 3000
stream-node-max-bytes 4096
stream-node-max-entries 100
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
dynamic-hz yes
aof-rewrite-incremental-fsync yes
rdb-save-incremental-fsync yes
jemalloc-bg-thread yes

```

* Run redis service:

```bash
docker run -v /yourpath/conf:/usr/local/etc/redis -p6379:6379 -d --name myredis redis redis-server /usr/local/etc/redis/redis.conf
```
**Note:**   `/yourpath/conf:` The local directory path where redis.conf is placed

## Clone the Code
Next, clone the computing-provider repository using git:
```bash
git clone https://github.com/lagrangedao/go-computing-provider.git
git checkout mars-testnet
```
## Compile the Code
Inside the `go-computing-provider` directory, compile the code using the build tools specified in the project:
* Install Go
To compile cp, you need Go version 1.19 or above, but no higher than 1.20:
```bash
wget -c https://golang.org/dl/go1.19.7.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc && source ~/.bashrc
```
* Complie computing-provider
```bash
cd go-computing-provider
go mod tidy
go build -o computing-provider main.go
```
## Edit Configuration Files
Edit the necessary configuration files according to your deployment requirements. These files may include settings for the computing-provider components, container runtime, Kubernetes, and other services.

```toml
[API]
Port = 8085                                     # The port number that the web server listens on
MultiAddress = "/ip4/<public_ip>/tcp/<port>"    # The multiAddress for libp2p
Domain = ""                                     # The domain name

RedisUrl = "redis://127.0.0.1:6379"           # The redis server address
RedisPassword = ""                            # The redis server access password

[LAD]
ServerUrl = "https://api.lagrangedao.org"     # The lagrangedao.org API address
AccessToken = ""                              # Access token applied by lagrangedao.org

[MCS]
ApiKey = ""                                   # The MCS API_KEY
AccessToken = ""                              # The MCS API_KEY
BucketName = ""                               # The MCS bucket name
Network = "polygon.mainnet"                   # polygon.mainnet for mainnet, polygon.mumbai for testnet
FileCachePath = ""                            # Cache directory of job task raw data

[Registry]                                    
ServerAddress = ""                            # The docker container image registry address, if only a single node, you can ignore
UserName = ""                                 # The login username, if only a single node, you can ignore
Password = ""                                 # The login password, if only a single node, you can ignore
```
**Note:** Create the path configured for the `FileCachePath` field in the configuration file:
```bash
mkdir ${FileCachePath}
```

## Run computing-provider
Once the code is compiled and the configuration files are edited, you can run computing-provider using the following command:
```bash
nohup ./computing-provider >> cp.log 2>&1 & 
```

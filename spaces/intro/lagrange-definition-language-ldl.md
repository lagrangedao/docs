# Lagrange Definition Language(LDL)

Customers/tenants define the deployment services, datacenters, and requirements, in a "manifest" file (deploy.yaml).&#x20;

The file is written in a declarative language called Lagrange Defintion Language(LDL).&#x20;

LDL is a human-friendly data standard for declaring deployment attributes. The LDL file is a "form" to request resources from the Network. LDL is compatible with the YAML standard and similar to Docker Compose files.&#x20;

Configuration files may end in `.yml` or `.yaml`.A complete deployment has the following sections:

* ​[version​](lagrange-definition-language-ldl.md#version)
* [​services​](lagrange-definition-language-ldl.md#services)
* ​[profiles​](lagrange-definition-language-ldl.md#profiles)
* ​[deployment](lagrange-definition-language-ldl.md#deployment)​

**Networking**

Networking - allowing connectivity to and between workloads - can be configured via the Lagrange Definition Language(LDL) file for a deployment. By default, workloads in a deployment group are isolated - nothing else is allowed to connect to them. This restriction can be relaxed.

### version <a href="#version" id="version"></a>

Indicates the version of the Lagrange configuration file. Currently only `"2.0"` one is accepted.

### services <a href="#services" id="services"></a>

The top-level `services` entry contains a map of workloads to be run on the Lagrange deployment. Each key is a service name; values are a map containing the following keys:

| Name         | Required | Meaning                                                                                                                                                       |
| ------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `image`      | Yes      | <p>Docker image of the container Best practices:</p><ul><li>avoid using <code>:latest</code> image tags as Computing Providers heavily cache images</li></ul> |
| `expose`     | Yes      | Entities allowed to connect to the services. See services.expose​                                                                                             |
| `depends-on` | No       | specifying dependencies for a particular service, indicates that the mentioned service relies on or requires certain other service to function properly       |
| `command`    | No       | Custom command use when executing container                                                                                                                   |
| `args`       | No       | Arguments to custom command use when executing the container                                                                                                  |
| `env`        | No       | Environment variables to set in running container. See services.env​                                                                                          |
| `ready`      | No       | _**NOTE - field is marked for future use and currently has no impact on deployments.**_                                                                       |
| `model`      | No       | A configuration section that defines a list of models for the service                                                                                         |

#### services.depends-on <a href="#services.env" id="services.env"></a>

`depends-on` specifies dependencies for a particular service, indicates that the mentioned service relies on or requires certain other service to function properly

```
    depends-on:
       - db
```

#### services.env <a href="#services.env" id="services.env"></a>

A list of environment variables to expose to the running container.

```
env:
- "GF_PATHS_CONFIG=/opt/grafana/grafana.ini"
```

#### services.expose <a href="#services.expose" id="services.expose"></a>

**Notes Regarding Port Use in the Expose Stanza**

* HTTPS is possible in Lagrange deployments but only self-signed certs are generated.
* To implement signed certs the deployment must be front-ended via a solution such as Cloudflare.&#x20;
* You can expose any other port besides 80 as the ingress port (HTTP, HTTPS) port using as: 80 directive if the app understands HTTP / HTTPS. Example of exposing a React web app using this method:

```
    expose:
      - port: 3000 
        as: 80
```

* In the LDL it is only necessary to expose port 80 for web apps. With this specification, both ports 80 and 443 are exposed.

`expose` is a list describing what can connect to the service. Each entry is a map containing one or more of the following fields:

| Name     | Required | Meaning                                                      |
| -------- | -------- | ------------------------------------------------------------ |
| `port`   | Yes      | Container port to expose                                     |
| `as`     | No       | Port number to expose the container port as                  |
| `accept` | No       | List of hosts to accept connections for                      |
| `proto`  | No       | Protocol type (`tcp, udp, or http`)                          |
| `to`     | No       | List of entities allowed to connect. See services.expose.to​ |

The `as` value governs the default `proto` value as follows:

> _**NOTE**_ - when as is not set, it will default to the value set by the port mandatory directive.

> _**NOTE**_ - when one exposes as: 80 (HTTP), the Kubernetes ingress controler makes the application available over HTTPS as well, though with the default self-signed ingress certs.

| `port`     | `proto` default |
| ---------- | --------------- |
| 80         | http, https     |
| all others | tcp             |

#### services.expose.to <a href="#services.expose.to" id="services.expose.to"></a>

`expose.to` is a list of clients to accept connections from. Each item is a map with one or more of the following entries:

| Name      | Value                        | Default | Description                                               |
| --------- | ---------------------------- | ------- | --------------------------------------------------------- |
| `service` | A service in this deployment | ​       | Allow the given service to connect                        |
| `global`  | `true` or `false`            | `false` | If true, allow connections from outside of the datacenter |

If no service is given and `global` is true, any client can connect from anywhere (web servers typically want this).

If a service name is given and `global` is `false`, only the services in the current datacenter can connect. If a service name is given and `global` is `true`, services in other datacenters for this deployment can connect.

If `global` is `false` then a service name must be given.

#### service.model

`model` is a configuration section that defines a list of models for the service.Each model in the list has the following properties:

* **name**: This property specifies the name of the model.&#x20;
* **url**: This property specifies the URL from which the model's data can be downloaded.&#x20;
* **dir**: This property specifies the directory path within the container where the model's files will be stored after they are downloaded from the specified URL.

Example:

```
services:
  stable-diffusion-ui:
    image: sonic868/stable-diffusion:v1.0
    models:
      - name: illustroV3.safetensors
        url: https://civitai.com/api/download/models/151490
        dir: "/easy-diffusion/models/stable-diffusion"
```

### profiles <a href="#profiles" id="profiles"></a>

The `profiles` section contains named compute and placement profiles to be used in the deployment.

### deployment <a href="#deployment" id="deployment"></a>

The `deployment` section defines how to deploy the services. It is a mapping of service name to deployment configuration.

Each service to be deployed has an entry in the `deployment`. This entry is maps datacenter profiles to compute profiles to create a final desired configuration for the resources required for the service.

Example:

```
deployment:
  minesweeper:
    lagrange:
      count: 1
```

This says that the 20 instances of the `web` service should be deployed to a datacenter matching the `westcoast` datacenter profile. Each instance will have the resources defined in the `web` compute profile available to it.



**The final `deploy.yaml` should look like this:**

```
version: "2.0"

services:
  minesweeper:
    image: creepto/minesweeper
    expose:
      - port: 3000
        as: 80
    
deployment:
  minesweeper:
    lagrange:
      count: 1
```

Check out [here](https://lagrangedao.org/spaces/0x7E0c07e66CD480CDa94dEaaeEB5a84Fa9F8215e6/miner-bomb-bomb/files) to interact with the sample Space.



**Here is another sample `deploy.yaml`:**

```
version: "2.0"

services:
  db:
    image: postgres:11.6-alpine
    env:
      - POSTGRES_USER=codimd
      - POSTGRES_PASSWORD=rootadmin
      - POSTGRES_DB=codimd
    expose:
        - port: 5432
          as: 5432
          to:
            - service: db
    ready-cmd:
        - "psql"
        - "-w"
        - "-U"
        - "codimd"
        - "-d"
        - "codimd"
        - "-c"
        - "SELECT 1"
  codimd:
    image: hackmdio/hackmd:2.4.1
    env:
      - CMD_DB_URL=postgres://codimd:rootadmin@127.0.0.1:5432/codimd
      - CMD_USECDN=false
    depends-on:
      - db
    expose:
        - port: 3000
          as: 3000
          to:
            - global: true

deployment:
  db:
    lagrange:
      count: 1
  codimd:
    lagrange:
      count: 1
```

Check out [here](https://lagrangedao.org/spaces/0x7E0c07e66CD480CDa94dEaaeEB5a84Fa9F8215e6/CodiMD-Test/files) to interact with the sample Space.

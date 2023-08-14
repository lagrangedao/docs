# Lagrange Definition Language(LDL)

Customers/tenants define the deployment services, datacenters, and requirements, in a "manifest" file (deploy.yaml).&#x20;

The file is written in a declarative language called Lagrange Defintion Language(LDL).&#x20;

SDL is a human-friendly data standard for declaring deployment attributes. The LDL file is a "form" to request resources from the Network. LDL is compatible with the YAML standard and similar to Docker Compose files.&#x20;

Configuration files may end in `.yml` or `.yaml`.A complete deployment has the following sections:

* ​[version](https://docs.akash.network/readme/stack-definition-language#version)​
* ​[services](https://docs.akash.network/readme/stack-definition-language#services)​
* ​[profiles](https://docs.akash.network/readme/stack-definition-language#profiles)​
* ​[deployment](https://docs.akash.network/readme/stack-definition-language#deployment)​
* ​[persistent storage](https://docs.akash.network/features/persistent-storage)​

**Networking**

Networking - allowing connectivity to and between workloads - can be configured via the Lagrange Definition Language(LDL) file for a deployment. By default, workloads in a deployment group are isolated - nothing else is allowed to connect to them. This restriction can be relaxed.

### version <a href="#version" id="version"></a>

Indicates the version of the Lagrange configuration file. Currently only `"2.0"` one is accepted.

### services <a href="#services" id="services"></a>

The top-level `services` entry contains a map of workloads to be run on the Lagrange deployment. Each key is a service name; values are a map containing the following keys:

| Name         | Required | Meaning                                                                                                                                                   |
| ------------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `image`      | Yes      | <p>Docker image of the container Best practices:</p><ul><li>avoid using <code>:latest</code> image tags as Akash Providers heavily cache images</li></ul> |
| `depends-on` | No       | _**NOTE - field is marked for future use and currently has no impact on deployments.**_                                                                   |
| `command`    | No       | Custom command use when executing container                                                                                                               |
| `args`       | No       | Arguments to custom command use when executing the container                                                                                              |
| `env`        | No       | Environment variables to set in running container. See [services.env](https://docs.akash.network/readme/stack-definition-language#services.env)​          |
| `expose`     | No       | Entities allowed to connect to the services. See [services.expose](https://docs.akash.network/readme/stack-definition-language#services.expose)​          |

#### services.env <a href="#services.env" id="services.env"></a>

A list of environment variables to expose to the running container.env:- API\_KEY=0xcafebabe- CLIENT\_ID=0xdeadbeef

#### services.expose <a href="#services.expose" id="services.expose"></a>

**Notes Regarding Port Use in the Expose Stanza**

* HTTPS is possible in Akash deployments but only self-signed certs are generated.
* To implement signed certs the deployment must be front-ended via a solution such as Cloudflare.&#x20;
* You can expose any other port besides 80 as the ingress port (HTTP, HTTPS) port using as: 80 directive if the app understands HTTP / HTTPS. Example of exposing a React web app using this method:

\- port: 3000as: 80to:- global: trueaccept:- www.mysite.com

* In the LDL it is only necessary to expose port 80 for web apps. With this specification, both ports 80 and 443 are exposed.

`expose` is a list describing what can connect to the service. Each entry is a map containing one or more of the following fields:

| Name     | Required | Meaning                                                                                                                                        |
| -------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `port`   | Yes      | Container port to expose                                                                                                                       |
| `as`     | No       | Port number to expose the container port as                                                                                                    |
| `accept` | No       | List of hosts to accept connections for                                                                                                        |
| `proto`  | No       | Protocol type (`tcp, udp, or http`)                                                                                                            |
| `to`     | No       | List of entities allowed to connect. See [services.expose.to](https://docs.akash.network/readme/stack-definition-language#services.expose.to)​ |

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

### profiles <a href="#profiles" id="profiles"></a>

The `profiles` section contains named compute and placement profiles to be used in the [deployment](https://docs.akash.network/readme/stack-definition-language#deployment).

#### profiles.placement <a href="#profiles.placement" id="profiles.placement"></a>

`profiles.placement` is map of named datacenter profiles.

Each profile specifies required datacenter attributes and pricing configuration for each [compute profile](https://docs.akash.network/readme/stack-definition-language#profiles.compute) that will be used within the datacenter.&#x20;

It also specifies optional list of signatures of which tenants expects audit of datacenter attributes.

Example:

```
westcoast:
  attributes:
    region: us-west
  pricing:
    web:
      denom: uakt
      amount: 8
    db:
      denom: uakt
      amount: 100
```

This defines a profile named `westcoast` having required attributes `{region="us-west"}`, and with a max price for the `web` and `db` [compute profiles](https://docs.akash.network/readme/stack-definition-language#profiles.compute) of 8 and 15 `uakt` per block, respectively.&#x20;

It also requires that the provider's attributes have been [signed by](https://docs.akash.network/readme/stack-definition-language#profiles.placement.signedby) the accounts `akash1vz375dkt0c60annyp6mkzeejfq0qpyevhseu05` and `akash1vl3gun7p8y4ttzajrtyevdy5sa2tjz3a29zuah`.

#### profiles.placement.signedBy <a href="#profiles.placement.signedby" id="profiles.placement.signedby"></a>

**Optional**

The `signedBy` section allows you to state attributes that must be signed by one or more accounts of your choosing. This allows for requiring a third-party certification of any provider that you deploy to.

### deployment <a href="#deployment" id="deployment"></a>

The `deployment` section defines how to deploy the services. It is a mapping of service name to deployment configuration.

Each service to be deployed has an entry in the `deployment`. This entry is maps datacenter profiles to [compute profiles](https://docs.akash.network/readme/stack-definition-language#profiles.compute) to create a final desired configuration for the resources required for the service.

Example:

```
web:
  westcoast:
    profile: web
    count: 20
```

This says that the 20 instances of the `web` service should be deployed to a datacenter matching the `westcoast` datacenter profile. Each instance will have the resources defined in the `web` [compute profile](https://docs.akash.network/readme/stack-definition-language#profiles.compute) available to it.

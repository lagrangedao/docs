# Build and config the Computing Provider

*   Build the Computing Provider

    Firstly, clone the code to your local:

```bash
git clone https://github.com/lagrangedao/go-computing-provider.git
git checkout mars-testnet
```

Then build the Computing provider follow the below steps:

```bash
cd go-computing-provider

go mod tidy

go build -o computing-provider main.go
```

* Update Configuration The computing provider's configuration sample locate in `./go-computing-provider/config.toml.sample`

```
cp config.toml.sample config.toml
```

Edit the necessary configuration files according to your deployment requirements. These files may include settings for the computing-provider components, container runtime, Kubernetes, and other services.

```toml
[API]
Port = 8085                                     # The port number that the web server listens on
MultiAddress = "/ip4/<public_ip>/tcp/<port>"    # The multiAddress for libp2p
Domain = ""                                     # The domain name

RedisUrl = "redis://127.0.0.1:6379"           # The redis server address
RedisPassword = ""                            # The redis server access password

[LAG]
ServerUrl = "https://api.lagrangedao.org"     # The lagrangedao.org API address
AccessToken = ""                              # Lagrange access token, acquired from “https://lagrangedao.org  -> setting -> Access Tokens -> New token”


[MCS]
ApiKey = ""                                   # Acquired from "https://www.multichain.storage" -> setting -> Create API Key
AccessToken = ""                              # Acquired from "https://www.multichain.storage" -> setting -> Create API Key
BucketName = ""                               # Acquired from "https://www.multichain.storage" -> bucket -> Add Bucket
Network = "polygon.mainnet"                   # polygon.mainnet for mainnet, polygon.mumbai for testnet
FileCachePath = "/tmp"                            # Cache directory of job data

[Registry]                                    
ServerAddress = ""                            # The docker container image registry address, if only a single node, you can ignore
UserName = ""                                 # The login username, if only a single node, you can ignore
Password = ""                                 # The login password, if only a single node, you can ignore
```

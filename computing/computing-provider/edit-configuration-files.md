# Edit Configuration Files

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

# Prerequisites

Before you install the Computing Provider, you need to know there are some resources required:

* Possess a public IP
* Have a domain name (\*.example.com)
* Have an SSL certificate
* `Go` version must 1.19+, you can refer here:

```bash
wget -c https://golang.org/dl/go1.19.7.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local

echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc && source ~/.bashrc
```

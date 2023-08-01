# Compile the Code

Inside the `go-computing-provider` directory, compile the code using the build tools specified in the project:

* Install Go To compile cp, you need Go version 1.19 or above, but no higher than 1.20:

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

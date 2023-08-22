# Build Space

After creating a Space, you'll have an empty repository to develop your code. A configuration file is required - either a `Dockerfile` or `Deploy.yaml`.

* A `Dockerfile` is a text document that contains all the commands a user could call on the command line to assemble an image.&#x20;
* A `Deploy.yaml` is a configuration file, written using [Lagrange Definition Language (LDL)](../intro/lagrange-definition-language-ldl.md)**,** which is used to specify deployment details for a Space on the Lagrange.

You can create/upload each file through the [**Web UI**](option-2-web-interface.md) or via [**Lagrange-cli**](../intro/lagrange-cli.md).

### Dockerfile or Deploy.yaml

In determining the choice of deployment file, consider the following:

* Use a `Dockerfile` if you don't have a pre-built image.
* Use `Deploy.yaml` if you have an existing image to deploy.
* For many files, lagrange-cli is recommended.
* For large files, upload to Multichain.Storage first.

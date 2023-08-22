# Build Space

After creating a Space, you'll have an empty repository to develop your code. A `dockerfile` or `deploy.yaml` is required.

* A `dockerfile` is a text document that contains all the commands a user could call on the command line to assemble an image.&#x20;
* A `deploy.yaml` is a configuration file, written using [Lagrange Definition Language (LDL)](../intro/lagrange-definition-language-ldl.md)**,** which is used to specify deployment details for a Space on the Lagrange.

You can create/upload each file through the [**Web UI**](option-1-create-files.md) or via [**Lagrange-cli**](../intro/lagrange-cli.md).

### `dockerfile` or `deploy.yaml`

In determining the choice of deployment file, consider the following:

* Use a `dockerfile` if you don't have a pre-built image.
* Use `deploy.yaml` if you have an existing image to deploy.
* For many files, [**Lagrange-cli**](../intro/lagrange-cli.md) is recommended to choose to upload them.
* For large files( ＞50MB）, upload to [**Multichain.Storage**](https://www.multichain.storage/) first, and add its IPFS link to your `dockerfile` or `deploy.yaml`.

# Option 1: Create Files

1.Start by selecting the **Files and version** tab, and then clicking **Contribute** → **Create a new file.**

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

2\. And then you can choose a name for your file, add content, and save your file by clicking on "Commit new file".

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
When deploying a Space, provide either a **Dockerfile** or **Deploy.yaml**, or both. If both are provided, **Deploy.yaml** is the main configuration file.
{% endhint %}

* Dockerfile: Defines a custom Docker image for your Space app, including dependencies, environment setup, etc.
* Deploy.YAML: a YAML file that is written using Lagrange's declarative configuration language (LDL).

For more info about LDL, refer to [**Lagrange Definition Language**](../intro/lagrange-definition-language-ldl.md)**.**

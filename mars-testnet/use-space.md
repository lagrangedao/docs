---
description: This page is providing a way create a space for your code.
---

# Docker Space

Space -> Create new space

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Confirm Creation of the space

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

You can upload code now

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### Secret Management

#### Buildtime

In Docker Spaces, the secrets management is different for security reasons. Once you create a secret in the [Settings tab](https://huggingface.co/docs/hub/spaces-overview#managing-secrets), you can expose the secret by adding the following line in your Dockerfile.

For example, if `SECRET_EXAMPLE` is the name of the secret you created in the Settings tab, you can read it at build time by mounting it to a file, then reading it with `$(cat /run/secrets/SECRET_EXAMPLE)`.


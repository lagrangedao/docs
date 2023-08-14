# Creating Space

### Creating a new Space

**To make a new Space**, visit the [Spaces main page](https://lagrangedao.org/spaces) and click on **Create new Space**. Along with choosing a name for your Space, selecting an optional license, and setting your Space’s visibility, you’ll be prompted to choose the **SDK** for your Space. The Hub offers four SDK options:  Docker and static HTML. If you select “Docker” as your SDK, you’ll be navigated to a new repo showing the following page

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Inside your Space, there will be a repository associated with it. This repository holds the code, configuration, and files for your demo.

you could start with hello-world template or upload /create a file

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

How to upload

Click on the \[Files and versions] - \[ Contribute], and select the files you want to upload from your local machine.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Confirm the upload, and the files will be added to your repository.

How to create

Alternatively, you might have the option to create new files directly within the repository interface.

Click on the \[Files and versions] - \[ Contribute]-\[Create a new file], and select the files you want to upload from your local machine.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Look for a button or option to "Create new file" or something similar.

* Specify the filename, extension, and content for the new file.



In the following sections, you’ll learn the basics of creating a Docker Space, configuring it, and deploying your code to it. We’ll create a **Text Generation** Space with Docker that’ll be used to demo the [google/flan-t5-small](https://huggingface.co/google/flan-t5-small) model, which can generate text given some input text, using FastAPI as the server.

You can find a completed version of this hosted [here](https://huggingface.co/spaces/DockerTemplates/fastapi\_t5).

###

###

### Creating a new Space

### Managing secrets

If your app requires secret keys or tokens, don’t hard-code them inside your app! Instead, go to the **Settings** page of your Space repository and enter your secrets there. The secrets will be exposed to your app with [Streamlit Secrets Management](https://blog.streamlit.io/secrets-in-sharing-apps/) if you use Streamlit, and as environment variables in other cases. For Docker Spaces, please check out [secret management with Docker](https://huggingface.co/docs/hub/spaces-sdks-docker#secret-management). Users are warned when `Spaces Secrets Scanner` [finds hard-coded secrets](https://huggingface.co/docs/hub/security-secrets).

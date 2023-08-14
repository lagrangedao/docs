# Docker

Spaces accommodate custom [Docker containers](https://docs.docker.com/get-started/) for apps outside the scope of Streamlit and Gradio. Docker Spaces allow users to go beyond the limits of what was previously possible with the standard SDKs. From FastAPI and Go endpoints to Phoenix apps and ML Ops tools, Docker Spaces can help in many different setups.

### Setting up Docker Spaces

Selecting **Docker** as the SDK when [creating a new Space](https://huggingface.co/new-space) will initialize your Space by setting the `sdk` property to `docker` in your `README.md` file’s YAML block. Alternatively, given an existing Space repository, set `sdk: docker` inside the `YAML` block at the top of your Spaces **README.md** file. You can also change the default exposed port `7860` by setting `app_port: 7860`. Afterwards, you can create a usual `Dockerfile`.

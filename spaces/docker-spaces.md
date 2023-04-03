# Docker Spaces

### Setting up Docker Spaces

Selecting **Docker** as the SDK when creating a new Space will initialize your Space by setting the `sdk` property to `docker` in your `README.md` file’s YAML block. Alternatively, given an existing Space repository, set `sdk: docker` inside the `YAML` block at the top of your Spaces **README.md** file. You can also change the default exposed port `7860` by setting `app_port: 7860`. Afterwards, you can create a usual `Dockerfile`.

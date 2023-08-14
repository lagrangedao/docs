# Docker

Spaces accommodate custom [Docker containers](https://docs.docker.com/get-started/) for apps outside the scope of Streamlit and Gradio.&#x20;

Docker Spaces allow users to go beyond the limits of what was previously possible with the standard SDKs.&#x20;

From FastAPI and Go endpoints to Phoenix apps and ML Ops tools, Docker Spaces can help in many different setups.

### Setting up Docker Spaces

Selecting **Docker** as the SDK when creating a new Space will initialize your Space by setting the `sdk` property to `docker` in your `README.md` file’s YAML block.&#x20;

Alternatively, given an existing Space repository, set `sdk: docker` inside the `YAML` block at the top of your Spaces **README.md** file.&#x20;

You can also change the default exposed port `7860` by setting `app_port: 7860`. Afterward, you can create a usual `Dockerfile`.

Internally you could have as many open ports as you want. For instance, you can install Elasticsearch inside your Space and call it internally on its default port 9200.

If you want to expose apps served on multiple ports to the outside world, a workaround is to use a reverse proxy like Nginx to dispatch requests from the broader internet (on a single port) to different internal ports.

## Let's write your first Dockerfile

In the following sections, We’ll create a **Text Generation** Space with Docker, which can generate text given some input text, using FastAPI as the server.

```
FROM python:3.9

WORKDIR /code

COPY ./requirements.txt /code/requirements.txt

RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "7860"]

```

When the changes are saved, the Space will rebuild and your demo should be up after a couple of seconds!&#x20;




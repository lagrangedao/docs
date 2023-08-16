# Your First Docker Space

## Your First Docker Space: Text Generation with T5

In the following sections, you’ll learn the basics of creating a Docker Space, configuring it, and deploying your code to it. We’ll create a **Text Generation** Space with Docker, which can generate text given some input text, using FastAPI as the server.

You can find a completed version of this hosted here.

### Create a new Docker Space

We’ll start by creating a brand new Space and choosing **Docker** as our SDK.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Selecting **Docker** as the SDK when creating a new Space will initialize your Docker Space by setting the `sdk` property to `docker` in your `README.md` file’s YAML block.

```
sdk: docker
```

You have the option to change the default application port of your Space by setting the `app_port` property in your `README.md` file’s YAML block. The default port is `7860`.

```
app_port: 7860
```

### Add the dependencies

For the **Text Generation** Space, we’ll be building a FastAPI app that showcases a text generation model called Flan T5. We need to start by installing a few dependencies. This can be done by creating a **requirements.txt** file in our repository, and adding the following dependencies to it:

```
fastapi==0.74.*
requests==2.27.*
sentencepiece==0.1.*
torch==1.11.*
transformers==4.*
uvicorn[standard]==0.17.*
```

These dependencies will be installed in the Dockerfile we’ll create later.

### Create the app

Let’s kick off the process with a dummy FastAPI app to see that we can get an endpoint working. The first step is to create an app file, in this case, we’ll call it `main.py`.

```
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World!"}
```

### Create the Dockerfile

The main step for a Docker Space is creating a Dockerfile. You can read more about Dockerfiles here. Although we’re using FastAPI in this tutorial, Dockerfiles give great flexibility to users allowing you to build a new generation of ML demos. Let’s write the Dockerfile for our application

```
FROM python:3.9

WORKDIR /code

COPY ./requirements.txt /code/requirements.txt

RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "7860"]
```

When the changes are saved, the Space will rebuild and your demo should be up after a couple of seconds!&#x20;

### Adding some ML to our app

As mentioned before, the idea is to use a Flan T5 model for text generation. We’ll want to add some HTML and CSS for an input field, so let’s create a directory called static with `index.html`, `style.css`, and `script.js` files. At this moment, your file structure should look like this:

```
/static
/static/index.html
/static/script.js
/static/style.css
Dockerfile
main.py
README.md
requirements.txt
```

Let’s go through all the steps to make this working. We’ll skip some of the details of the CSS and HTML. You can find the whole code in the Files and versions tab of the DockerTemplates/fastapi\_t5 Space.

1. Write the FastAPI endpoint to do inference

We’ll use the `pipeline` from `transformers` to load the google/flan-t5-small model. We’ll set an endpoint called `infer_t5` that receives and input and outputs the result of the inference call

```
from transformers import pipeline

pipe_flan = pipeline("text2text-generation", model="google/flan-t5-small")

@app.get("/infer_t5")
def t5(input):
    output = pipe_flan(input)
    return {"output": output[0]["generated_text"]}
```

2. Write the `index.html` to have a simple form containing the code of the page.

```
<main>
  <section id="text-gen">
    <h2>Text generation using Flan T5</h2>
    <p>
      Model:
      <a
        href="https://huggingface.co/google/flan-t5-small"
        rel="noreferrer"
        target="_blank"
        >google/flan-t5-small
      </a>
    </p>
    <form class="text-gen-form">
      <label for="text-gen-input">Text prompt</label>
      <input
        id="text-gen-input"
        type="text"
        value="German: There are many ducks"
      />
      <button id="text-gen-submit">Submit</button>
      <p class="text-gen-output"></p>
    </form>
  </section>
</main>
```

3. In the `app.py` file, mount the static files and show the html file in the root route

```
app.mount("/", StaticFiles(directory="static", html=True), name="static")

@app.get("/")
def index() -> FileResponse:
    return FileResponse(path="/app/static/index.html", media_type="text/html")
```

4. In the `script.js` file, make it handle the request

```
const textGenForm = document.querySelector(".text-gen-form");

const translateText = async (text) => {
  const inferResponse = await fetch(`infer_t5?input=${text}`);
  const inferJson = await inferResponse.json();

  return inferJson.output;
};

textGenForm.addEventListener("submit", async (event) => {
  event.preventDefault();

  const textGenInput = document.getElementById("text-gen-input");
  const textGenParagraph = document.querySelector(".text-gen-output");

  textGenParagraph.textContent = await translateText(textGenInput.value);
});
```

5. Grant permissions to the right directories

As discussed in the Permissions Section, the container runs with user ID 1000. That means that Space might face permission issues. For example, `transformers` downloads and caches the models in the path under the `HUGGINGFACE_HUB_CACHE` path. The easiest way to solve this is to create a user with the right permissions and use it to run the container application. We can do this by adding the following lines to the `Dockerfile`.

```
# Set up a new user named "user" with user ID 1000
RUN useradd -m -u 1000 user

# Switch to the "user" user
USER user

# Set home to the user's home directory
ENV HOME=/home/user \
	PATH=/home/user/.local/bin:$PATH

# Set the working directory to the user's home directory
WORKDIR $HOME/app

# Copy the current directory contents into the container at $HOME/app setting the owner to the user
COPY --chown=user . $HOME/app
```

The final `Dockerfile` should look like this:

```
FROM python:3.9

WORKDIR /code

COPY ./requirements.txt /code/requirements.txt

RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt

RUN useradd -m -u 1000 user

USER user

ENV HOME=/home/user \
	PATH=/home/user/.local/bin:$PATH

WORKDIR $HOME/app

COPY --chown=user . $HOME/app

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "7860"]
```

Success! Your app should be working now!&#x20;

What a journey! Please remember that Docker Spaces give you lots of freedom, so you’re not limited to using FastAPI.&#x20;




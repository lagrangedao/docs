# Create Space

### Creating a New Space

1\. Visit [https://lagrangedao.org](https://lagrangedao.org) and connect your MetaMask

2\. Once logged in, click on the “Create new Space” button on the Space main page.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

3\. Enter a name for your Space and select a License. Your Space’s URL will be automatically generated based on the name.

4\. Select the SDK you want to use for your Space (Docker, Streamlit, Static).

5\. Set your Space’s visibility to either public or private.

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Building Your First Space

Ok, now that you have an empty space repository, it’s time to write some code.&#x20;

The sample app will consist of the following three files:

* `requirements.txt` — Lists the dependencies of a Python project or application
* `app.py` — A Python script where we will write our FastAPI app
* `Dockerfile` — Sets up our environment, installs `requirements.txt`, then launches `app.py`
* `README.md`— a markdown file that gives other users a detailed description of your Space and will be displayed on the Space card.
*   `Configuration file` - a `Dockerfile` or a YAML file which is written using Lagrange's declarative configuration language (LDL).



You can create each file via the web interface. or upload it via the web interface or lagrange.cli

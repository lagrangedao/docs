# Creating Space

### Creating a new Space

**To make a new Space**, visit the [Spaces main page](https://lagrangedao.org/spaces) and click on **Create new Space**. Along with choosing a name for your Space, selecting an optional license, and setting your Space’s visibility, you’ll be prompted to choose the **SDK** for your Space. The Hub offers four SDK options:  Docker and static HTML. If you select “Docker” as your SDK, you’ll be navigated to a new repo showing the following page

### Creating a new Space

### Managing secrets

If your app requires secret keys or tokens, don’t hard-code them inside your app! Instead, go to the **Settings** page of your Space repository and enter your secrets there. The secrets will be exposed to your app with [Streamlit Secrets Management](https://blog.streamlit.io/secrets-in-sharing-apps/) if you use Streamlit, and as environment variables in other cases. For Docker Spaces, please check out [secret management with Docker](https://huggingface.co/docs/hub/spaces-sdks-docker#secret-management). Users are warned when `Spaces Secrets Scanner` [finds hard-coded secrets](https://huggingface.co/docs/hub/security-secrets).

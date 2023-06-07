# Access Tokens

## User Access Tokens

### &#x20;What are User Access Tokens?

User Access Tokens are the preferred way to authenticate an application or notebook to Lagrange  services. You can manage your access tokens in your [settings](https://lagrangedao.org/personal\_center/setting).

### External Access Tokens

The External Access Tokens  refers to the tokens that computing providers use to access or integrate with external services, such as chatGPT, during the execution of a task. This access token is stored in the computing provider's profile on LagrangeDAO.

Here's a general flow of how it works:

1. **Storing the Access Token:** The computing provider stores the access token on their LagrangeDAO profile. The token is typically obtained from the external service provider (for example, OpenAI for chatGPT) and may need to be refreshed periodically.
2. **Task Assignment:** When the computing provider is assigned a task that requires access to an external service, the access token comes into play. The task might be to run a simulation, process data, or even generate human-like text using an AI model like chatGPT.
3. **Token Retrieval and Environment Setup:** Once the task is assigned, LagrangeDAO retrieves the access token from the computing provider's profile and injects it into the runtime environment of the task as an environment variable.
4. **Task Execution:** The computing provider then executes the task. If the task involves interacting with the external service (like chatGPT), it uses the access token from the environment variable to authenticate and gain necessary permissions.
5. **Accessing the External Service:** The access token is used to authenticate the computing provider's requests to the external service and gain access to the resources or capabilities needed to complete the task.

The usage of access tokens as environment variables can help ensure a secure, efficient, and flexible execution environment. This mechanism allows the computing providers to seamlessly integrate with various external services as required by their tasks, enhancing the capabilities and versatility of the LagrangeDAO network.\
\


<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

In this diagram:

1. The Computing Provider stores the External Access Token (obtained from a service like OpenAI's chatGPT) in their LagrangeDAO Profile under "setting/tokens", which is a part of the LagrangeDAO Hub.
2. The Auction Engine within the LagrangeDAO Hub assigns a task that requires an External Access Token to the Computing Provider.
3. The Computing Provider retrieves the External Access Token from its LagrangeDAO Profile.
4. The Computing Provider loads the External Access Token into the runtime environment as an environment variable.
5. The Computing Provider executes the task using the required resources. If the task involves the external service, it accesses the service using the External Access Token.
6. The External Access Token is used to authenticate the Computing Provider's requests to the External Service and gain access to the resources or capabilities needed to complete the task.


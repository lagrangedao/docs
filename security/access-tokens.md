# Access Tokens

## User Access Tokens

### &#x20;What are User Access Tokens?

User Access Tokens are the preferred way to authenticate an application or notebook to Lagrange  services. You can manage your access tokens in your [settings](https://lagrangedao.org/personal\_center/setting).

### External Access Tokens

The External Access Tokens  refers to the tokens that computing providers use to access or integrate with external services, such as chatGPT, during the execution of a task. This access token is stored in the computing provider's profile on LagrangeDAO.

Here's a general flow of how it works:

1. **Storing the Access Token**: The Computing Provider stores the External Access Token (obtained from a service like OpenAI's chatGPT) in their LagrangeDAO Profile under "setting/tokens", which is a part of the LagrangeDAO Hub.
2. **Task Assignment**: The Auction Engine within the LagrangeDAO Hub assigns a task that requires an External Access Token to the Computing Provider.
3. **Token Retrieval**: The Computing Provider retrieves the External Access Token from its LagrangeDAO Profile.
4. **Environment Setup**: The Computing Provider loads the External Access Token into the runtime environment as an environment variable.
5. **Task Execution**: The Computing Provider executes the task using the required resources. If the task involves the external service, it accesses the service using the External Access Token.
6. **Accessing the External Service**: The External Access Token is used to authenticate the Computing Provider's requests to the External Service and gain access to the resources or capabilities needed to complete the task.\


<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

In this diagram:

1. The Computing Provider stores the External Access Token (obtained from a service like OpenAI's chatGPT) in their LagrangeDAO Profile under "setting/tokens", which is a part of the LagrangeDAO Hub.
2. The Auction Engine within the LagrangeDAO Hub assigns a task that requires an External Access Token to the Computing Provider.
3. The Computing Provider retrieves the External Access Token from its LagrangeDAO Profile.
4. The Computing Provider loads the External Access Token into the runtime environment as an environment variable.
5. The Computing Provider executes the task using the required resources. If the task involves the external service, it accesses the service using the External Access Token.
6. The External Access Token is used to authenticate the Computing Provider's requests to the External Service and gain access to the resources or capabilities needed to complete the task.

The usage of access tokens as environment variables can help ensure a secure, efficient, and flexible execution environment. This mechanism allows the computing providers to seamlessly integrate with various external services as required by their tasks, enhancing the capabilities and versatility of the LagrangeDAO network.

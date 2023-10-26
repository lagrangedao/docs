# How to Integrate Diffuser/Transformer

### How to Fork the Space and Deploy it:

1\. Visit \[[https://lagrangedao.org/spaces](https://lagrangedao.org/spaces)] and log in to your account

2\. Visit the [Base Space](https://lagrangedao.org/spaces/0xFbc1d38a2127D81BFe3EA347bec7310a1cfa2373/api-image-to-text/app), and click on the **\[Fork]** button to create a duplicate of the Base Space.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh6.googleusercontent.com/oRy_T_MNTSJ6bqGTJtiHK_Pw-tzBLhY3r6e1UFzPxxxxD1bh5OImd22rOktVH0Cs1iO-CW1B1-a6J8JYn0GvSy_sZn1PWvuIpNg5yHGTpTI3pgWzxh9Zrm4q2eL-iyW2AAYlgOA29FZR1LN4CGw9t3c" alt=""><figcaption></figcaption></figure>

3\. You can either click on the \[**Just Fork, choose config later**] or select your preferred hardware, and proceed with the payment.

It's recommended to choose "config later" if you anticipate adding more models to your Space in the future to avoid unnecessary redeployments.

<figure><img src="https://lh5.googleusercontent.com/S2daQ_7aiKXdAxCogyBDELByzC282gzCATDRIa9vhx_0xYT8nrqYhAvu5kAvbrE5s-O1kYqzZ1jZYG9pnFYxqK8k-FfD1KFuAMh5g9GJ1GjbYtP5NxueHVCP3QUA3DO25DqUo-jSA63e6hPaJjKizzU" alt=""><figcaption></figcaption></figure>

In your forked Space, you can click the **\[Settings]** button and scroll down to the Rename section to rename your Space.

_**Note:** Please note that after you change the Space name, the Space link will also be updated. Make sure to share the new link with others as needed._

<figure><img src="https://lh4.googleusercontent.com/cdSKmw-kdgN5Zlm2mQ05GiCantlI8hbbfc1Ahc5cp6DcU6Fw4dMlftmYWJCskoq6tAl7oB5sXuUm549WhQ9EcIHxt6RLVcnTVwt-228mqOKEZ5KDiXmZintlNr3CeUKLzw5lTksOslWfjwwRFE7Mb1U" alt=""><figcaption></figcaption></figure>

### How to Change Models

In this tutorial, you will learn how to change models within your Diffuser/Transformer Space to meet your specific needs. We will be using the example of changing a model for the "Image-to-Text" task. Follow these steps:

**Step 1: Choose a New Model**

1.Visit the Hugging Face Model Hub at [https://huggingface.co/models](https://huggingface.co/models).

2.In the library list, select either "Transformers" or "Diffusers," depending on your requirements.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

3.In the task list, choose the specific model type you want. For this tutorial, we'll use "Image-to-Text" as an example.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Refine your search criteria to find a model that meets your requirements. Once you have identified the model you want to use, copy its name.

**Step 2: Update Your Lagrange Space**

1.Go to the Space that you've forked before

2.Navigate to "Files and Versions." Locate the file named "model-setting.json" and open it.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

3.Inside this JSON file, replace the existing model name with the one you copied from the Hugging Face Model Hub.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

4.Save the changes to the JSON file.

5.After updating the model name, redeploy your Space following [build-space.md](../../mars-testnet/build-space.md "mention")

Once the deployment is complete, you can access the new model through the Inference Endpoint, which has been updated with the selected model.

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "https://24lhiquaxn.meta.crosschain.computer"


def query(filename):
    with open(filename, "rb") as f:
        data = f.read()
    response = requests.post(API_URL, data=data)
    return response.json()


output = query("cats.jpg")
print(output)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
async function query(filename) {
    const data = fs.readFileSync(filename);
    const response = await fetch(
        "https://24lhiquaxn.meta.crosschain.computer",
        {
            method: "POST",
            body: data,
        }
    );
    const result = await response.json();
    return result;
}

query("cats.jpg").then((response) => {
    console.log(JSON.stringify(response));
});
```
{% endtab %}

{% tab title="cURL" %}
```url
 curl  https://24lhiquaxn.meta.crosschain.computer \
    -X POST \
    --data-binary '@cats.jpg'
```
{% endtab %}
{% endtabs %}

Click [here](https://lagrangedao.org/spaces/0xFbc1d38a2127D81BFe3EA347bec7310a1cfa2373/api-image-to-text/app) to check the Sample Space.

---
description: >-
  This tutorial guides you on how to integrate the Stable Diffusion via
  inference API into your products.
---

# How to Integrate Stable Diffusion via Inference API

#### Step 1: Obtain the API Endpoint Link

1.Visit the [Stable Diffusion Base Space](https://lagrangedao.org/spaces/0x6091b2f5678952cAfbf02755D78973EBff302e11/Stable-Diffusion-Base-LoRA/app), or fork it to build your own Stable Diffusion Space following this [Guide](../../spaces/fork-space.md).

2.Click on “Click here to deploy in a new tab", and copy the URL.This is your API endpoint link. Save it for later use.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

#### Step 2: Access the API Documentation

The API provides detailed documentation for various image generation tasks. You can access the internal docs via the `/docs` endpoint : `https://<API_ENDPOINT>/docs`

Note: Replace `<API_ENDPOINT>` with the URL obtained in Step 1.

<figure><img src="https://lh6.googleusercontent.com/xnKGJr5ElE3LVRCP3T1IVlbZPEeMZ-BawjCpE5sz8ILddpQHBq4A32Pc38Q-mD5qtLk-fhdCq5ssleS6pRgt0DYt5F1KuRN5brCx7y4r7ztcSedvmIoPojCS4hfCsA9z5RBF8f3SwHZOiIDR2lYYinM" alt=""><figcaption></figcaption></figure>

This enables you to review the API's capabilities and endpoints. The key endpoints to focus on are `/sdapi/v1/txt2img` and `/sdapi/v1/img2img`.&#x20;

<figure><img src="https://lh6.googleusercontent.com/SC53fhku3MyD2V15eO-pLUKIgFP1oNU6bngaV2gxXpg_60tSkqgy2wP1rnbHgi-jdNTBNVrMtBsoiyh_baJclxX0zbIBPmragXyKiKsP3BKsj7120m2Ay_dNlkuVjkBhkPShpiUdq1g60_3GxLpVx6I" alt=""><figcaption></figcaption></figure>

In this guide, we'll focus on `/sdapi/v1/txt2img`.

#### Step 3: Construct Your Payload

When you expand that tab  `/sdapi/v1/txt2img` , it gives an example of a payload to send to the API.

<figure><img src="https://lh3.googleusercontent.com/0xl60Jb99aBorngVJlTckJT5T1s78V6MZ6FPUBmQC9nUjulfoaNwiAzzmlo9zg_v8urKZOKRk1DlaJXpWMHZi0r9w_cAEt09wDiK4IELBfliXiB1uzwQNgovgy_vvnE9un5LWQXKx3F1LzXkShKLT28" alt=""><figcaption></figcaption></figure>

You can include as few or as many parameters as needed in the payload. The API will use defaults for anything not specified.

Here's how you can construct a payload:

```
payload = {
    "prompt": "apple",
    "negative_prompt": "red",
    "sampler_name": "DPM++ 2M Karras",
    "seed": 985454925,
    "cfg_scale": 7,
    "steps": 20,
    "width": 512,
    "height": 512,
}
```

#### Step 4: Make a Request

You can send your payload to the API using a `POST` request. Replace `<API_ENDPOINT>` with the URL obtained in Step 1.

```
import json
import requests
import io
import base64
from PIL import Image

url = "https://<API_ENDPOINT>"
username = 'admin'
password = 'admin1234'

# Encode the username and password in Base64
credentials = f'{username}:{password}'
credentials = base64.b64encode(credentials.encode()).decode()

headers = {
    'Authorization': f'Basic {credentials}',
    'Content-Type': 'application/json'
}

response = requests.post(url=f'{url}/sdapi/v1/txt2img', json=payload, headers=headers)
```

#### Step 5: Retrieve the Image

The API's response contains three entries: images, parameters, and info. To retrieve the generated image, follow these steps:

```
r = response.json()

# 'images' is a list of base64-encoded generated images
image = Image.open(io.BytesIO(base64.b64decode(r['images'][0]))
image.save('output.png')
```

#### Sample Code&#x20;

A sample code that should work can look like this:

```
import json
import requests
import io
import base64
from PIL import Image

url = "https://fk5ge5uhhu.mars.nebulablock.com"
username = 'admin'
password = 'admin1234'

# Encode the username and password in Base64
credentials = f'{username}:{password}'
credentials = base64.b64encode(credentials.encode()).decode()

headers = {
    'Authorization': f'Basic {credentials}',
    'Content-Type': 'application/json'  # Adjust content type if necessary
}

payload = {
    "prompt": "apple",
    "negative_prompt": "red",
    "sampler_name": "DPM++ 2M Karras",
    "seed": 985454925,
    "cfg_scale": 7,
    "steps": 20,
    "width": 512,
    "height": 512,
}
response = requests.post(url=f'{url}/sdapi/v1/txt2img', json=payload, headers=headers)

r = response.json()

image = Image.open(io.BytesIO(base64.b64decode(r['images'][0])))
image.save('output.png')
```



### Additional Notes

Lagrange allows you to switch between different models for image generation. This section outlines how to check the available models and how to change models based on your preferences.

#### 1. Checking Available Models

Before changing models, you should explore the models available. Use the following method to obtain a list of models:

```
import json
import requests
import base64

url = "https://fk5ge5uhhu.mars.nebulablock.com"
username = 'admin'
password = 'admin1234'

# Encode the username and password in Base64
credentials = f'{username}:{password}'
credentials = base64.b64encode(credentials.encode()).decode()

headers = {
    'Authorization': f'Basic {credentials}',
    'Content-Type': 'application/json'  # Adjust content type if necessary
}

response = requests.get(url=f'{url}/sdapi/v1/sd-models', headers=headers)
print(json.dumps(response.json()))
```

The above code sends a `GET` request to the `/sdapi/v1/sd-models` endpoint, providing you with a list of available models.

```
[ { "title":"chilloutmix-Ni.safetensors [7234b76e42]", "model_name":"chilloutmix-Ni", "hash":"7234b76e42", "sha256":"7234b76e423f010b409268386062a4111c0da6adebdf3a9b1a825937bdf17683", "filename":"/stable-diffusion-webui/models/Stable-diffusion/chilloutmix-Ni.safetensors", "config":null }, { "title":"v1-5-pruned-emaonly.safetensors [6ce0161689]", "model_name":"v1-5-pruned-emaonly", "hash":"6ce0161689", "sha256":"6ce0161689b3853acaa03779ec93eafe75a02f4ced659bee03f50797806fa2fa", "filename":"/stable-diffusion-webui/models/Stable-diffusion/v1-5-pruned-emaonly.safetensors", "config":null } ]
```

#### 2. Changing the Model

You can change the model used for image generation by specifying a new model checkpoint using the `/sdapi/v1/options` endpoint.&#x20;

Follow these steps:

**Step1: Define the settings you want to change in the payload.**&#x20;

For example:

```
import json
import requests
import base64

url = "https://fk5ge5uhhu.mars.nebulablock.com"
username = 'admin'
password = 'admin1234'

# Encode the username and password in Base64
credentials = f'{username}:{password}'
credentials = base64.b64encode(credentials.encode()).decode()

headers = {
    'Authorization': f'Basic {credentials}',
    'Content-Type': 'application/json'  # Adjust content type if necessary
}

payload = {
    "sd_model_checkpoint": "v1-5-pruned-emaonly.safetensors [6ce0161689]",
}

```

Step 2: Send a `POST` request to the `/sdapi/v1/options` endpoint with your option payload and appropriate headers:

```
response = requests.post(url=f'{url}/sdapi/v1/options', json=payload, headers=headers)
print(json.dumps(response.json()))
```

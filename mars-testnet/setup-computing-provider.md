---
description: This is a guide to Computing Provider
---

# Computing Provider Setup

🗓️ **Event Period：** 14th August, 00:00 (EST) - 10th September, 23:59 (EST)

## Rules

### **How to Participate:**

1\. Follow the [instructions](https://github.com/lagrangedao/go-computing-provider/tree/mars-testnet) to set up a Computing Provider and keep it online during the Campaign.

* Possess a public IP
* Have a wildcard domain name (\*.example.com)
* Have an SSL certificate
* Have at least one GPU
* At least 8 vCPUs
* Minimum 50GB SSD storage
* Minimum 32GB memory
* Minimum 50MB bandwidth
* Here are the GPUs we recommend:
  * T4, RTX 4090, 3090Ti, 3090, 3080Ti, 3080, 3060Ti, 2060, 2070, 2080, 2080Ti, A100, H100

2\. Check the [Dashboard](https://provider.lagrangedao.org/provider-status) to confirm that your Computing Provider is running during the campaign period.

3\. Provide feedback on the process, documentation, bugs, and improvement ideas in our [Discord Channel](https://discord.gg/qHEEyQTECX).

4\. Submit the [form](https://docs.google.com/forms/d/e/1FAIpQLSf0JRi18xsp\_YCoQKPuE0azYLDDNwAXBMNIeqwXFMgqVljU1Q/viewform?usp=sf\_link) here with the required details.

#### **How to Be Eligible:**

* Eligible Computing Provider should have at least one Space that has been deployed to it and keep the Space running during the Campaign.
* The [form](https://forms.gle/YyzotPhHqx4DmCmy9) must be submitted upon task completion.

#### **Rewards:**

* All eligible Computing Providers can split **500,000 LAG**. Rewards will be assigned based on GPU hours as a percentage of the total GPU hours.
  * Your GPU hours: the sum of the duration of your GPUs that’s been used for running Space
  * Total GPU hours: the sum of the duration of all GPUs that’s been used for running Space
* The Top 1 wins the **Golden CP privilege**

## Tutorial

Refer to [computing](../computing/ "mention")

## FAQ

#### Q: How can I know if the status of the computing provider is normal?

**A**:&#x20;

Run the following command:

{% hint style="info" %}
_**\*Note: Please replace**** ****`<YOUR_MULTI_ADDRESS_IP>:<PORT>`**** ****with your actual multi-address IP and port.**_
{% endhint %}

```
curl --location --request POST 'http://<YOUR_MULTI_ADDRESS_IP>:<PORT>/api/v1/computing/lagrange/jobs' \
--header 'Content-Type: application/json' \
--data-raw '{
"uuid": "5641877b-dc94-469a-bb3b-ecab6d10f7dd",
"name": "Job-5641877b-dc94-469a-bb3b-ecab6d10f7dd",
"status": "Submitted",
"duration": 1800,
"job_source_uri": "https://api.lagrangedao.org/spaces/0x6091b2f5678952cAfbf02755D78973EBff302e11/Minesweeper",
"storage_source": "lagrange",
"task_uuid": "92cd5595-9789-4af3-9100-7c7e4aacb456"
}'
```

After running this command, wait for 3-5 minutes, and then execute&#x20;

<pre><code><strong>kubectl get ing -n ns-0x6091b2f5678952cafbf02755d78973ebff302e11
</strong></code></pre>

Find the hosts corresponding to the name `ing-minesweeper` and ensure that the domain can be accessed in a browser to confirm its normal status.

<img src="../.gitbook/assets/image (21).png" alt="" data-size="original">

#### Q: My node has been running for so long, yet the uptime is 0%.

**A**:

1\. Run the following command:

{% hint style="info" %}
_**Ensure that****  ****`<YOUR_MULTI_ADDRESS_IP>`**** ****is the Public IP.**_
{% endhint %}

```bash
curl http://<YOUR_MULTI_ADDRESS_IP>:<PORT>/api/v1/computing/host/info
```

2\. Compare the returned result with the example provided below. If they are different, you should review your port mappings.

Example result:

```json
{"status":"success","code":"","data":{"swan_miner_version":"","operating_system":"linux","architecture":"amd64","cpu_cores":48}}
```

3\. If your port mappings are correct and the result matches the example, then proceed to check the configuration file of the computing provider.&#x20;

Ensure that the `MultiAddress` is set exactly as `"/ip4/<public_ip>/tcp/<port>"`.

#### Q: Which ports need to be mapped?&#x20;

**A**: Here are the ports you need to map

1\. You need to map the CP's internal IP and its port (default 8085), as well as the public IP and port.

2\. Map your wildcard domain (\*.example.com) to your public IP.&#x20;

3\. Additionally, you need to map port 80 of your internal IP to port 80 of your public IP, as well as port 443 of your internal IP to port 443 of your public IP.

#### **Q: Is it possible to use a port other than 80 and 443 in the wildcard domain(\*.exmaple.com)?**

**A**: No, it is not possible.

#### Q: Is the "pod" used for communication, and "Calico" is used to manage this communication within the cluster?&#x20;

**A**: Both are used for intra-cluster communication. You can use one of these approaches.

#### Q: If someone didn't apply for early bird, can they still join and run the computing provider tasks?

**A**: Of course, they can also follow the [instruction](../computing/) to set up a Computing Provider.

#### Q: Can I move my computing provider to a new one while maintaining my previous server? Will this reset my uptime?

**A**: Yes, you need to move  `.swan_node` to the new server. The uptime will not be reset.

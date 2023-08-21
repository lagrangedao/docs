# Option 2: Upload Code

1\. Start by selecting the **Files and version** tab, and then clicking **Contribute** → **Upload files.**

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Note that, if you prefer, you can also utilize **Lagrange.cli** to upload code files to your Space

#### How to install

**Clone and Install `lagrange-cli`:** Clone the `lagrange-cli` repository and install it using pip:

```bash
git clone git@github.com:lagrangedao/lagrange-cli.git
cd lagrange-cli
pip install.
```

#### **How to use**

**1.Copy Space Link:** After creating your space on Lagrange, copy the link. It will look like this: `https://lagrangedao.org/<type>/<wallet_address>/<name>`.



**2.Clone Your Space:** Open your terminal and run the following command to clone your space repository:

```bash
git clone <space_link>
```

Replace `<space_link>` with the link, you copied in Step 1.

**3.Navigate to Dataset Folder:** Change your directory to the dataset folder created during the clone. Its name matches your space's name:

```bash
cd <space_name>
```

**4.Add Code Files:** Copy your code files and paste them into the dataset folder (same as your space's name).

**5.Add and Commit Files:** Inside the cloned repository, add the code files you want to commit:

```bash
lag add file1 file2 file3 ...
```

To add all files in the current directory and subdirectories:

```bash
lag add .
```

Commit the added files with a descriptive message:

```bash
lag commit -m "commit message"
```

**6.Push Changes to Dataset:** Push your committed changes to your designated dataset using:

```bash
lag push <space_link>
```

7.**Configure API Token:**&#x20;

You will be prompted to set your API token. Go to your Lagrange profile settings, access tokens section, and get your access token.

In your terminal, use the following command to set your API token:

```bash
lag config --api-token <your_access_token>
```

Replace `<your_access_token>` with the token you obtained in the previous step.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>








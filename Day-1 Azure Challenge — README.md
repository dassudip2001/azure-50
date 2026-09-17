# 🚀 50 Days Azure Challenge

A hands-on **50 Days Azure Challenge** focused on Azure Cloud, DevOps, Infrastructure, Security, Networking, Compute, Storage, Monitoring, and automation.

The goal is to learn Azure by completing practical tasks every day and documenting the commands, configuration, and verification steps.

---

## 📅 Day 1 — Create an Azure SSH Key Pair

### 🎯 Objective

Create an Azure SSH key pair with the following requirements:

- **Name:** `xfusion-kp`
- **Key Type:** `RSA`

---

## 🔐 Prerequisites

You need:

- Azure account
- Azure Portal access
- Azure CLI (optional)
- Appropriate Azure subscription
- Resource Group

---

# Method 1 — Azure Portal

### Step 1: Open Azure Portal

Open:

https://portal.azure.com

Sign in with your Azure account.

### Step 2: Search for SSH Keys

In the Azure Portal search bar:

```text
SSH keys
```

Select **SSH keys**.

### Step 3: Create SSH Key

Click:

```text
Create
```

Configure the SSH key:

```text
Subscription: <your subscription>
Resource group: <your resource group>
Region: <your region>
Name: xfusion-kp
Key type: RSA
```

### Step 4: Review and Create

Click:

```text
Review + create
```

Then:

```text
Create
```

The Azure SSH key resource should now be created.

---

# Method 2 — Azure CLI

## Step 1: Check Azure CLI

```bash
az --version
```

---

## Step 2: Login to Azure

```bash
az login
```

A browser window will open for authentication.

---

## Step 3: Check Current Subscription

```bash
az account show -o table
```

List all available subscriptions:

```bash
az account list -o table
```

---

## Step 4: Set Subscription

If multiple subscriptions are available:

```bash
az account set --subscription "<SUBSCRIPTION_ID>"
```

Verify:

```bash
az account show -o table
```

---

## Step 5: List Resource Groups

```bash
az group list -o table
```

Identify the resource group that should contain the SSH key.

---

## Step 6: Create the SSH Key

```bash
az sshkey create \
  --name xfusion-kp \
  --resource-group <RESOURCE_GROUP> \
  --location <REGION>
```

Example:

```bash
az sshkey create \
  --name xfusion-kp \
  --resource-group myResourceGroup \
  --location eastus
```

---

## Step 7: Verify the SSH Key

```bash
az sshkey show \
  --name xfusion-kp \
  --resource-group <RESOURCE_GROUP> \
  -o json
```

Or use a table format:

```bash
az sshkey show \
  --name xfusion-kp \
  --resource-group <RESOURCE_GROUP> \
  -o table
```

---

# 🔎 Verification

Check that the SSH key resource exists:

```bash
az sshkey list \
  --resource-group <RESOURCE_GROUP> \
  -o table
```

Expected resource:

```text
Name
----------------
xfusion-kp
```

The key must be an **RSA** SSH key.

---

# 🧹 Cleanup

If the resource is no longer required:

```bash
az sshkey delete \
  --name xfusion-kp \
  --resource-group <RESOURCE_GROUP>
```

Confirm deletion:

```bash
az sshkey list \
  --resource-group <RESOURCE_GROUP> \
  -o table
```

---

# 📚 What I Learned

- How Azure SSH keys are managed as Azure resources
- How to create an SSH key from the Azure Portal
- How to create an SSH key using Azure CLI
- How to select and manage Azure resource groups
- How SSH keys are used for secure VM authentication
- How to verify Azure resources using the CLI

---

## 🛠️ Technologies

- Microsoft Azure
- Azure Portal
- Azure CLI
- SSH
- RSA
- Cloud Infrastructure
- DevOps

---

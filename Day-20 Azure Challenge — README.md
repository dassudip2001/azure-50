# Day 20: Deploy Azure Resources Using ARM Template

## 📌 Overview
In this challenge, I deployed an Azure Virtual Network (VNet) using an Azure Resource Manager (ARM) template and the Azure CLI. I modified the JSON template to configure the virtual network name, address space, and resource tags.

## 🎯 Objectives
- Modify an existing ARM template.
- Configure the virtual network name and address prefix.
- Add resource tags for identification and environment management.
- Deploy Azure resources using Azure CLI.
- Verify the deployed resource configuration.

## 🛠️ Technologies Used
- Microsoft Azure
- Azure Resource Manager (ARM) Templates
- Azure CLI
- JSON

## ⚙️ Configuration Changes

| Property | Value |
|---|---|
| Resource Type | `Microsoft.Network/virtualNetworks` |
| Virtual Network Name | `arm-vnet-xfusion` |
| Address Prefix | `192.168.0.0/16` |
| `displayName` Tag | `arm-vnet-xfusion` |
| `Environment` Tag | `KKE-xfusion` |
| Location | Resource group location |

## 🚀 Implementation Steps

### 1. Navigate to the ARM Template Directory

```bash
cd /root/arm-templates
```

### 2. Edit the ARM Template

```bash
vi vnet-deployment-template.json
```

Updated the virtual network name, address prefix, and tags in the JSON template.

### 3. Identify the Resource Group

```bash
az group list --query '[].name' --output table | grep 'kml'
```

### 4. Deploy the ARM Template

Replace the resource group name below with the actual resource group returned by Azure CLI.

```bash
az deployment group create \
  --resource-group kml_rg_main-acfe89d084184832 \
  --name arm-vnet-xfusion-deployment \
  --template-file /root/arm-templates/vnet-deployment-template.json
```

### 5. Verify the Virtual Network

```bash
az network vnet show \
  --resource-group kml_rg_main-acfe89d084184832 \
  --name arm-vnet-xfusion \
  --query '{name:name,addressPrefixes:addressSpace.addressPrefixes,tags:tags}' \
  --output json
```

## ✅ Expected Configuration

- **Virtual Network:** `arm-vnet-xfusion`
- **Address Space:** `192.168.0.0/16`
- **Display Name Tag:** `arm-vnet-xfusion`
- **Environment Tag:** `KKE-xfusion`

## 📚 Key Learnings
- Understanding the structure of Azure ARM templates.
- Deploying Azure infrastructure using declarative JSON templates.
- Managing Azure resource tags and address spaces.
- Using Azure CLI to deploy and verify cloud resources.

## 🏁 Conclusion
This challenge demonstrates how to provision and configure Azure networking resources using ARM templates and Azure CLI, helping make infrastructure deployments repeatable and consistent.

#Azure #ARMTemplates #AzureCLI #CloudComputing #AzureNetworking #100DaysOfCloud

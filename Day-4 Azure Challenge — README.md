# Azure VNet Creation — UI Guide

## Nautilus DevOps Migration Task

## Objective

Create an Azure Virtual Network (VNet) named `nautilus-vnet` in the `eastus` region using the Azure Portal UI and any valid IPv4 CIDR block.

---

## Required Configuration

| Setting | Value |
|---|---|
| Virtual Network Name | `nautilus-vnet` |
| Region | `East US` |
| IPv4 Address Space | Any valid IPv4 CIDR block |
| Example CIDR | `10.0.0.0/16` |
| Example Subnet | `default` — `10.0.0.0/24` |

---

## Step 1 — Open Azure Portal

From the `azure-client` host:

1. Open the Azure Portal in the browser.
2. Sign in using the Azure credentials provided for the lab.

> **Note:** If you need to retrieve the lab credentials from the Azure client, use:
>
> ```bash
> showcreds
> ```

---

## Step 2 — Open Virtual Networks

1. Use the Azure Portal search bar.
2. Search for **Virtual networks**.
3. Select **Virtual networks**.
4. Click **Create**.

---

## Step 3 — Configure Basics

On the **Basics** tab, enter:

| Field | Value |
|---|---|
| Subscription | Select the provided Azure subscription |
| Resource group | Select the existing resource group required by the lab |
| Virtual network name | `nautilus-vnet` |
| Region | `East US` |

---

## Step 4 — Configure IP Addresses

Open the **IP Addresses** tab.

Under **IPv4 address space**, configure any valid IPv4 CIDR block.

Example:

```text
10.0.0.0/16
```

If a subnet is required, an example configuration is:

| Subnet Setting | Example |
|---|---|
| Subnet name | `default` |
| Subnet address range | `10.0.0.0/24` |

---

## Step 5 — Review and Create

1. Click **Review + create**.
2. Wait for Azure validation to complete.
3. Confirm that **Validation passed**.
4. Click **Create**.
5. Wait for the deployment to finish.

---

## Step 6 — Verify the VNet

After deployment:

1. Click **Go to resource**.
2. Verify the following:

```text
Name:              nautilus-vnet
Region:            East US
IPv4 Address Space: 10.0.0.0/16
```

The exact IPv4 address space may differ if you selected another valid CIDR block.

---

## Final Checklist

- [ ] Logged in to Azure Portal.
- [ ] Opened **Virtual networks → Create**.
- [ ] Set VNet name to `nautilus-vnet`.
- [ ] Set region to **East US**.
- [ ] Configured a valid IPv4 CIDR block.
- [ ] Validation passed.
- [ ] Created the VNet successfully.
- [ ] Verified the VNet after deployment.

---

## Notes

- Use the existing resource group specified by the lab, if applicable.
- Do not modify unrelated Azure resources.
- The task only requires a VNet in the `eastus` region with an IPv4 CIDR block.
- Advanced networking configuration is not required unless specifically requested by the lab.

---

## Expected Result

The Azure Portal should contain a VNet with:

```text
VNet Name : nautilus-vnet
Region    : East US
IPv4 CIDR : 10.0.0.0/16
```

The task is complete once the VNet is successfully deployed and verified.

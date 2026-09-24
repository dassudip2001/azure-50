# Azure Lab: Allocate a Public IP Address

## Task

The Nautilus DevOps team is migrating infrastructure to Azure incrementally.

For this task, create a Public IP address with the following name:

```text
nautilus-pip
```

The resource should be created using the Azure Portal UI.

---

## Prerequisites

- Azure Portal access
- Azure credentials provided by the lab
- Existing lab subscription
- Existing resource group, if specified by the lab

Azure credentials can be retrieved from the `azure-client` host using:

```bash
showcreds
```

---

## Steps Using Azure Portal UI

### 1. Open Azure Portal

Open:

https://portal.azure.com/

Sign in using the credentials provided by the lab.

---

### 2. Open Public IP Addresses

In the Azure Portal search bar:

1. Search for **Public IP addresses**
2. Select **Public IP addresses**
3. Click **+ Create**

---

### 3. Configure the Public IP

Configure the resource as follows:

| Setting | Value |
|---|---|
| Subscription | Select the lab subscription |
| Resource group | Select the lab resource group |
| Name | `nautilus-pip` |
| Region | Use the region specified by the lab |
| IP version | IPv4 |
| SKU | Standard |
| Tier | Regional |
| IP address assignment | Static |

> **Important:** If the lab specifies a particular resource group or region, use those exact values.

---

### 4. Review and Create

After entering the configuration:

1. Click **Review + create**
2. Wait for Azure validation to complete
3. Click **Create**

Azure will deploy the Public IP resource.

---

## Verification

After deployment:

1. Go to **Azure Portal**
2. Search for **Public IP addresses**
3. Open `nautilus-pip`

Verify that:

```text
Name:        nautilus-pip
IP version:  IPv4
SKU:         Standard
Assignment:  Static
```

An Azure public IP address should also be displayed.

---

## Expected Result

The Azure resource should appear as:

```text
Public IP Address
└── nautilus-pip
```

The task is complete once `nautilus-pip` has been successfully created and is visible under **Public IP addresses**.

---

## Quick Checklist

- [ ] Logged into Azure Portal
- [ ] Opened **Public IP addresses**
- [ ] Clicked **+ Create**
- [ ] Selected correct subscription
- [ ] Selected correct resource group
- [ ] Set name to `nautilus-pip`
- [ ] Selected required region
- [ ] Selected IPv4
- [ ] Selected Standard SKU
- [ ] Selected Static assignment
- [ ] Reviewed configuration
- [ ] Created the resource
- [ ] Verified `nautilus-pip`

---

## Useful Azure Portal Link

https://portal.azure.com/

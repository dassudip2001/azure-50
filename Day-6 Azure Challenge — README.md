# Azure VNet and Subnet Creation Using Azure Portal

## Task

Create an Azure Virtual Network (VNet) named `datacenter-vnet` with one subnet named `datacenter-subnet` in the **West US** region.

## Requirements

| Resource | Value |
|---|---|
| VNet Name | `datacenter-vnet` |
| Region | `West US` |
| IPv4 Address Space | `10.0.0.0/16` |
| Subnet Name | `datacenter-subnet` |
| Subnet Address Range | `10.0.0.0/24` |

> The task specifies the VNet address range but does not specify a subnet CIDR. `10.0.0.0/24` is a valid subnet range inside `10.0.0.0/16`.

---

## 1. What is an Azure VNet?

An **Azure Virtual Network (VNet)** is a private network in Azure. It allows Azure resources such as virtual machines, application services, databases, and containers to communicate with each other.

Example:

```text
datacenter-vnet
10.0.0.0/16
```

The VNet address space can be divided into smaller networks called **subnets**.

---

## 2. What is a Subnet?

A **subnet** is a smaller network inside a VNet.

A simple way to understand it:

```text
VNet    = Building
Subnet  = Room
VM      = Device inside the room
```

For this task:

```text
datacenter-vnet
10.0.0.0/16
│
└── datacenter-subnet
    10.0.0.0/24
```

Subnets help organize resources and allow different network security and routing rules to be applied to different groups of resources.

---

# Step-by-Step: Create the VNet Using Azure Portal

## Step 1: Open Azure Portal

Open:

https://portal.azure.com/

Sign in with your Azure account.

---

## Step 2: Open Virtual Networks

1. In the Azure Portal search bar, search for **Virtual networks**.
2. Select **Virtual networks**.
3. Click **+ Create**.

---

## Step 3: Configure Basics

Enter the following:

| Setting | Value |
|---|---|
| Subscription | Your available subscription |
| Resource group | Use the existing resource group |
| Virtual network name | `datacenter-vnet` |
| Region | `West US` |

Example:

```text
Virtual network name: datacenter-vnet
Region: West US
```

Click:

**Next: IP addresses**

---

## Step 4: Configure IPv4 Address Space

Under **IPv4 address space**, enter:

```text
10.0.0.0/16
```

If Azure automatically adds another address range and the task requires only the specified range, remove the additional range.

The final VNet address space should be:

```text
10.0.0.0/16
```

---

## Step 5: Create the Subnet

Under **Subnets**, click:

**+ Add subnet**

Enter:

| Setting | Value |
|---|---|
| Subnet name | `datacenter-subnet` |
| Subnet address range | `10.0.0.0/24` |

Click **Add**.

The network structure is now:

```text
VNet: 10.0.0.0/16
        │
        └── Subnet: 10.0.0.0/24
```

---

## Step 6: Review and Create

1. Click **Review + create**.
2. Wait for **Validation passed**.
3. Review the configuration.
4. Click **Create**.
5. Wait for the deployment to complete.

---

# Step 7: Verify the VNet

After deployment:

1. Go to **Azure Portal → Virtual networks**.
2. Open:

```text
datacenter-vnet
```

On the **Overview** page, verify:

```text
Name:   datacenter-vnet
Region: West US
```

---

## Verify Address Space

Go to:

**Settings → Address space**

Verify:

```text
10.0.0.0/16
```

---

## Verify Subnet

Go to:

**Settings → Subnets**

Verify:

```text
Subnet name:    datacenter-subnet
Address range:  10.0.0.0/24
```

---

# Final Architecture

```text
Azure
│
└── West US
    │
    └── datacenter-vnet
        │
        ├── Address Space
        │   └── 10.0.0.0/16
        │
        └── datacenter-subnet
            └── 10.0.0.0/24
```

---

# CIDR Explanation

## VNet: `10.0.0.0/16`

The `/16` defines the VNet's address space.

It covers:

```text
10.0.0.0 - 10.0.255.255
```

## Subnet: `10.0.0.0/24`

The `/24` creates a smaller network inside the VNet.

It covers:

```text
10.0.0.0 - 10.0.0.255
```

The subnet must be contained within the VNet address space.

---

# Why Use Subnets?

Subnets allow resources to be separated logically.

For example:

```text
datacenter-vnet
10.0.0.0/16
│
├── web-subnet
│   10.0.0.0/24
│
├── app-subnet
│   10.0.1.0/24
│
└── database-subnet
    10.0.2.0/24
```

Different Network Security Groups (NSGs) and routing rules can be applied to different subnets.

---

# Final Checklist

- [ ] VNet created
- [ ] VNet name is `datacenter-vnet`
- [ ] Region is `West US`
- [ ] IPv4 address space is `10.0.0.0/16`
- [ ] Subnet created
- [ ] Subnet name is `datacenter-subnet`
- [ ] Subnet range is `10.0.0.0/24`
- [ ] Deployment completed successfully
- [ ] VNet and subnet are visible in Azure Portal

---

# Quick Summary

```text
VNet
└── datacenter-vnet
    └── 10.0.0.0/16
        │
        └── Subnet
            └── datacenter-subnet
                └── 10.0.0.0/24
```

## Azure Portal

https://portal.azure.com/

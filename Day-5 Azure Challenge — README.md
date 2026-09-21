# Azure Lab: Create a Virtual Network (VNet) Using Azure Portal

## Objective

Create an Azure Virtual Network (VNet) with the following configuration:

  Setting              Value
  -------------------- ------------------
  VNet Name            `xfusion-vnet`
  Region               `East US`
  IPv4 Address Space   `192.168.0.0/24`
  Method               Azure Portal UI

------------------------------------------------------------------------

## Prerequisites

-   Azure Portal access
-   Azure lab credentials
-   Existing Azure resource group provided by the lab

If you are using the `azure-client` host, retrieve the credentials with:

``` bash
showcreds
```

Open the Azure Portal:

**https://portal.azure.com/**

------------------------------------------------------------------------

# Step 1: Sign in to Azure Portal

1.  Open the Azure Portal.
2.  Sign in using the credentials provided by the lab.
3.  Wait for the Azure Portal dashboard to load.

------------------------------------------------------------------------

# Step 2: Open Virtual Networks

1.  In the Azure Portal, use the top search bar.
2.  Search for:

``` text
Virtual networks
```

3.  Select **Virtual networks** from the search results.
4.  Click **+ Create**.

------------------------------------------------------------------------

# Step 3: Configure the Basics Tab

On the **Basics** tab, configure the following:

### Subscription

Select the subscription provided by the lab.

### Resource group

Select the **existing resource group** provided by the lab.

Do not create a new resource group unless the task specifically requires
it.

### Virtual network name

Enter:

``` text
xfusion-vnet
```

### Region

Select:

``` text
East US
```

The Basics configuration should look similar to:

``` text
Subscription:       <Lab Subscription>
Resource group:     <Existing Resource Group>
Virtual network:    xfusion-vnet
Region:             East US
```

Click:

**Next: IP addresses**

------------------------------------------------------------------------

# Step 4: Configure the IP Address Space

In the **IP addresses** tab, configure the IPv4 address space.

The required address space is:

``` text
192.168.0.0/24
```

If Azure has automatically created a default address space such as:

``` text
10.0.0.0/16
```

remove it and add the required address space:

``` text
192.168.0.0/24
```

The final address space should be:

``` text
IPv4 address space:
192.168.0.0/24
```

------------------------------------------------------------------------

# Step 5: Configure Subnets

For this task, the requirement is only to create the VNet.

If the portal shows a default subnet, you can leave it as configured
unless the lab specifically requires a particular subnet configuration.

If you need to add a subnet, use the subnet configuration required by
the lab.

Otherwise, continue to the next step.

Click:

**Next: Security**

------------------------------------------------------------------------

# Step 6: Configure Security

For this task, no special security configuration is required.

Leave the security settings at their default values unless the lab
specifies otherwise.

Click:

**Next: Tags**

------------------------------------------------------------------------

# Step 7: Tags

Tags are not required for this task.

Leave the tags empty unless the lab provides specific tags.

Click:

**Next: Review + create**

------------------------------------------------------------------------

# Step 8: Review the Configuration

Before creating the VNet, verify the important settings.

Expected configuration:

``` text
Name:
xfusion-vnet

Region:
East US

IPv4 Address Space:
192.168.0.0/24
```

Make sure the configuration is correct.

You should see:

``` text
Validation passed
```

Click:

**Create**

------------------------------------------------------------------------

# Step 9: Wait for Deployment

Azure will start deploying the Virtual Network.

Wait until the deployment completes successfully.

You should see a message similar to:

``` text
Your deployment is complete
```

Click:

**Go to resource**

or open **Virtual networks** and locate `xfusion-vnet`.

------------------------------------------------------------------------

# Step 10: Verify the VNet

Open:

``` text
Azure Portal
    ↓
Virtual networks
    ↓
xfusion-vnet
```

On the **Overview** page, verify:

``` text
Name:
xfusion-vnet

Location:
East US
```

Then go to:

``` text
Settings
    ↓
Address space
```

Verify that the IPv4 address space is:

``` text
192.168.0.0/24
```

------------------------------------------------------------------------

# Final Configuration

The completed VNet should have the following configuration:

  Property             Required Value
  -------------------- ------------------
  Resource Type        Virtual Network
  Name                 `xfusion-vnet`
  Region               `East US`
  IPv4 Address Space   `192.168.0.0/24`

------------------------------------------------------------------------

# Verification Checklist

Use this checklist before submitting the lab:

-   [ ] Logged into Azure Portal
-   [ ] Selected the lab subscription
-   [ ] Used the existing resource group
-   [ ] Created a Virtual Network
-   [ ] VNet name is `xfusion-vnet`
-   [ ] Region is `East US`
-   [ ] IPv4 address space is `192.168.0.0/24`
-   [ ] Deployment completed successfully
-   [ ] Verified the VNet after deployment

------------------------------------------------------------------------

# Quick Summary

``` text
VNet Name     : xfusion-vnet
Region        : East US
IPv4 CIDR     : 192.168.0.0/24
Deployment    : Azure Portal UI
```

The Azure VNet creation task is complete.

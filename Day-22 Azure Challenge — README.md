# Day 22: Configuring Instances with User Data

## Objective

Create an Azure Ubuntu virtual machine named `devops-vm` and use a startup script (user data) to install and start Nginx automatically. Allow inbound HTTP traffic on port 80 so the web server can be accessed from the internet.

## Prerequisites

- Access to the Azure Portal.
- Azure credentials available from the lab environment (run `showcreds` on `azure-client` if needed).
- An SSH public key if the VM is configured to use SSH authentication.

## Steps

### 1. Open Azure Portal

1. Run `showcreds` on the `azure-client` terminal to retrieve the portal URL and credentials.
2. Sign in to the Azure Portal.
3. Search for **Virtual machines**.
4. Select **Create → Azure virtual machine**.

### 2. Configure the VM

On the **Basics** tab, configure:

| Setting | Value |
|---|---|
| Virtual machine name | `devops-vm` |
| Image | Any available Ubuntu Server image |
| Size | An available small size suitable for the lab |
| Authentication | SSH public key or the method permitted by the lab |
| Administrator username | For example, `azureuser` |
| Resource group and region | Use the values required by the lab |

### 3. Configure HTTP Networking

On the **Networking** tab:

1. Select or create a public IP address.
2. Configure the network security group (NSG).
3. Allow inbound HTTP traffic on **TCP port 80** from the internet.

Example NSG rule:

| Setting | Value |
|---|---|
| Source | Any (or the lab-required source) |
| Source port ranges | `*` |
| Destination | Any |
| Service / destination port | HTTP / `80` |
| Protocol | TCP |
| Action | Allow |
| Name | `allow-http` |

If you need SSH access, allow TCP port 22 as well, preferably restricted to your own IP address.

### 4. Configure User Data

Open the **Advanced** tab and locate the **User data** field. Add this startup script:

```bash
#!/bin/bash
apt-get update -y
apt-get install -y nginx
systemctl enable nginx
systemctl start nginx
```

The script updates the package index, installs Nginx, enables it to start on boot, and starts the service.

> If the lab specifically asks for a Custom Script Extension rather than User Data, create the VM first, then open **Extensions + applications → Add → Custom Script** and provide the script using the extension's supported configuration.

### 5. Review and Create

1. Select **Review + create**.
2. Wait for validation to pass.
3. Select **Create**.
4. Wait until the deployment completes.

### 6. Verify the Web Server

1. Open **Virtual machines → devops-vm → Overview**.
2. Copy the VM's public IP address.
3. Visit `http://<PUBLIC_IP>` in a browser.

The default **Welcome to nginx!** page should appear.

If the page does not load, verify that:

- The VM is running.
- The user-data script completed successfully.
- The Nginx service is active.
- The NSG and any subnet-level rules permit inbound TCP port 80.
- The VM has a public IP address and an appropriate route to the internet.

## Expected Result

- An Ubuntu VM named `devops-vm` is deployed.
- Nginx is installed and running automatically through the startup script.
- HTTP traffic on TCP port 80 is allowed by the NSG.
- The Nginx welcome page is reachable through the VM's public IP address.

## Key Learning

**User data** lets you pass a startup script or configuration to a virtual machine so that common setup tasks can run automatically during initialization, reducing manual configuration.

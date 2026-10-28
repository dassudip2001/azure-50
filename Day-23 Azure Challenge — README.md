# Day 23: Automating User Data Configuration Using the CLI

## 📌 Objective
Automate the configuration of an Azure Virtual Machine using the Azure CLI and cloud-init user data. In this lab, we create an Ubuntu VM, install Nginx automatically during startup, and allow HTTP traffic through port 80.

## 🛠️ Technologies Used
- Microsoft Azure
- Azure CLI
- Ubuntu Linux
- Cloud-init
- Nginx
- Network Security Groups (NSG)

## 🚀 Implementation Steps

### Step 1: Set the Resource Group

```bash
RG="kml_rg_main-65f656da3b1145cd"
```

### Step 2: Create the Cloud-init Configuration

Create a file named `cloud-init.txt`:

```bash
cat > cloud-init.txt <<'EOF'
#cloud-config
package_update: true
packages:
  - nginx
runcmd:
  - systemctl enable nginx
  - systemctl start nginx
EOF
```

This configuration updates the package list, installs Nginx, enables the service, and starts it automatically.

### Step 3: Create the Azure Virtual Machine

```bash
az vm create \
  --resource-group "$RG" \
  --name xfusion-vm \
  --location eastus \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --custom-data cloud-init.txt \
  --os-disk-size-gb 64 \
  --storage-sku Standard_LRS \
  --public-ip-sku Standard
```

The `--custom-data` option passes the cloud-init configuration to the VM during provisioning.

### Step 4: Allow HTTP Traffic

```bash
az vm open-port \
  --resource-group "$RG" \
  --name xfusion-vm \
  --port 80 \
  --priority 1010
```

**Note:** Priority `1010` is used to avoid a conflict with the existing SSH security rule at priority `1000`.

### Step 5: Verify the VM

```bash
az vm show \
  --resource-group "$RG" \
  --name xfusion-vm \
  --show-details \
  --query "{PowerState:powerState,PublicIP:publicIps}" \
  --output table
```

### Step 6: Test Nginx

Open the VM's public IP address in your browser:

```text
http://<VM-PUBLIC-IP>
```

You should see the Nginx welcome page if cloud-init completed successfully and HTTP traffic is allowed.

To verify Nginx from the VM:

```bash
sudo systemctl status nginx
```

## ✅ Expected Results

- Azure VM `xfusion-vm` is created and running.
- Cloud-init automatically installs and starts Nginx.
- Inbound HTTP traffic is allowed on port 80.
- The Nginx welcome page is accessible through the VM's public IP address.

## 📚 Key Learnings

- Creating Azure VMs using Azure CLI.
- Automating initial Linux configuration with cloud-init.
- Installing and starting services automatically during provisioning.
- Configuring NSG rules to allow HTTP traffic.
- Verifying VM status and web server availability.

## 🏁 Conclusion

Successfully practiced automating Azure VM configuration using Azure CLI and cloud-init. This approach reduces manual setup and makes server provisioning more consistent and repeatable.

#Azure #AzureCLI #CloudInit #DevOps #Linux #Nginx #100DaysOfCloud

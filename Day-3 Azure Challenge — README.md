# Azure CLI Lab — Create `datacenter-vm`

## Objective

Create an Azure Virtual Machine using Azure CLI from the `azure-client` host.

## Requirements

| Setting | Required value |
|---|---|
| VM name | `datacenter-vm` |
| Resource group | `kml_rg_main-969f3a6e65d2421d` |
| Region | `eastus` |
| Image | `Ubuntu2204` |
| VM size | `Standard_B2s` |
| Admin username | `azureuser` |
| SSH authentication | Generated SSH keys |
| Storage SKU | `Standard_LRS` |
| OS disk size | `30 GB` |
| Final state | `running` |

---

## 1. Connect to the Azure Client

From the lab jump/landing host:

```bash
ssh azure-client
```

Verify the host:

```bash
hostname
```

---

## 2. Verify Azure Login

Check the active Azure account:

```bash
az account show
```

The lab account should show an enabled subscription.

If authentication is required:

```bash
az login
```

---

## 3. Find and Verify the Resource Group

List resource groups:

```bash
az group list --output table
```

For this lab, the existing resource group is:

```text
kml_rg_main-969f3a6e65d2421d
```

Its location is `eastus`.

Verify:

```bash
az group show   --name kml_rg_main-969f3a6e65d2421d   --query "{name:name,location:location}"   -o table
```

Expected:

```text
Name                          Location
----------------------------  ----------
kml_rg_main-969f3a6e65d2421d  eastus
```

---

## 4. Set Variables

Set variables to simplify the remaining commands:

```bash
RG=kml_rg_main-969f3a6e65d2421d
LOCATION=eastus
```

Verify:

```bash
echo "$RG"
echo "$LOCATION"
```

---

## 5. Create the Virtual Machine

Run:

```bash
az vm create   --resource-group "$RG"   --name datacenter-vm   --location "$LOCATION"   --image Ubuntu2204   --size Standard_B2s   --admin-username azureuser   --generate-ssh-keys   --storage-sku Standard_LRS   --os-disk-size-gb 30
```

### Command explanation

- `--resource-group` — uses the existing resource group.
- `--name datacenter-vm` — sets the VM name.
- `--location eastus` — creates the VM in East US.
- `--image Ubuntu2204` — selects the Ubuntu 22.04 image.
- `--size Standard_B2s` — sets the VM size.
- `--admin-username azureuser` — creates the required administrator.
- `--generate-ssh-keys` — generates/configures SSH keys for secure access.
- `--storage-sku Standard_LRS` — uses Standard LRS managed disk storage.
- `--os-disk-size-gb 30` — sets the OS disk size to 30 GB.

The VM is normally started as part of creation.

---

## 6. Verify the VM

Check that the VM exists:

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   -o table
```

---

## 7. Verify VM Power State

Run:

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --show-details   --query "{Name:name,Size:hardwareProfile.vmSize,PowerState:powerState}"   -o table
```

Expected:

```text
Name           Size          PowerState
-------------  ------------  -----------
datacenter-vm  Standard_B2s  VM running
```

You can also use:

```bash
az vm get-instance-view   --resource-group "$RG"   --name datacenter-vm   --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus"   -o tsv
```

Expected:

```text
VM running
```

---

## 8. Verify Disk Size and Storage SKU

Run:

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --query "{DiskSizeGB:storageProfile.osDisk.diskSizeGb,StorageSKU:storageProfile.osDisk.managedDisk.storageAccountType}"   -o table
```

Expected:

```text
DiskSizeGB    StorageSKU
------------  -----------
30            Standard_LRS
```

---

## 9. Verify VM Size

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --query "hardwareProfile.vmSize"   -o tsv
```

Expected:

```text
Standard_B2s
```

---

## 10. Verify Admin Username

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --query "osProfile.adminUsername"   -o tsv
```

Expected:

```text
azureuser
```

---

## 11. Verify the Image

Run:

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --query "storageProfile.imageReference"   -o json
```

The result should identify the Ubuntu 22.04 image.

---

## 12. Verify SSH Keys

Because the VM was created with:

```bash
--generate-ssh-keys
```

Azure CLI generates or uses an SSH key pair in the local user's SSH directory.

Check:

```bash
ls -la ~/.ssh
```

Depending on the environment, you may see files such as:

```text
id_rsa
id_rsa.pub
```

or:

```text
id_ed25519
id_ed25519.pub
```

**Never share your private SSH key.**

---

## 13. Final Verification Command

Use this command to verify the main requirements together:

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --show-details   --query "{Name:name,Size:hardwareProfile.vmSize,PowerState:powerState,DiskSizeGB:storageProfile.osDisk.diskSizeGb,StorageSKU:storageProfile.osDisk.managedDisk.storageAccountType,AdminUser:osProfile.adminUsername}"   -o table
```

Expected result:

```text
Name           Size          PowerState    DiskSizeGB    StorageSKU    AdminUser
-------------  ------------  ------------  ------------  ------------  ----------
datacenter-vm  Standard_B2s  VM running    30            Standard_LRS  azureuser
```

---

## 14. If the VM Is Stopped

Start it with:

```bash
az vm start   --resource-group "$RG"   --name datacenter-vm
```

Then verify:

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --show-details   --query powerState   -o tsv
```

Expected:

```text
VM running
```

---

## 15. Troubleshooting

### VM already exists

Check:

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm
```

If it exists, inspect its configuration rather than creating a duplicate.

### Check the VM completely

```bash
az vm show   --resource-group "$RG"   --name datacenter-vm   --show-details   -o json
```

### Check available VM sizes in the region

```bash
az vm list-sizes   --location "$LOCATION"   --query "[?name=='Standard_B2s']"   -o table
```

### Check available Ubuntu images

```bash
az vm image list   --location "$LOCATION"   --publisher Canonical   --offer UbuntuServer   --all   -o table
```

---

# Complete Command Sequence

If the Azure CLI session is already authenticated, the complete solution is:

```bash
az account show

az group list --output table

az group show   --name kml_rg_main-969f3a6e65d2421d   --query "{name:name,location:location}"   -o table

RG=kml_rg_main-969f3a6e65d2421d
LOCATION=eastus

az vm create   --resource-group "$RG"   --name datacenter-vm   --location "$LOCATION"   --image Ubuntu2204   --size Standard_B2s   --admin-username azureuser   --generate-ssh-keys   --storage-sku Standard_LRS   --os-disk-size-gb 30

az vm show   --resource-group "$RG"   --name datacenter-vm   --show-details   --query "{Name:name,Size:hardwareProfile.vmSize,PowerState:powerState,DiskSizeGB:storageProfile.osDisk.diskSizeGb,StorageSKU:storageProfile.osDisk.managedDisk.storageAccountType,AdminUser:osProfile.adminUsername}"   -o table
```

---
## Final Expected Configuration

```text
VM Name:       datacenter-vm
Resource Group: kml_rg_main-969f3a6e65d2421d
Region:        eastus
Image:         Ubuntu2204
VM Size:       Standard_B2s
Admin User:    azureuser
SSH:           Generated SSH keys
Storage SKU:   Standard_LRS
OS Disk:       30 GB
Power State:   VM running
```

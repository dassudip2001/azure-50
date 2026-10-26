# Day 21: Assigning a Static Public IP to an Azure Virtual Machine

## Objective

The Nautilus DevOps Team needs an Azure VM that hosts a new application and has a stable public IP address.

| Item | Value |
|------|-------|
| VM name | `nautilus-vm` |
| VM size | `Standard_B1s` |
| Image | Ubuntu (Ubuntu 22.04 LTS, `Ubuntu2204`) |
| Static Public IP name | `nautilus-pip` |
| SSH key | `~/.ssh/id_rsa` / `~/.ssh/id_rsa.pub` |
| Admin user | `azureuser` |
| Resource group | `kml_rg_main-f82a3ad30fc547da` |
| Location | `eastus` |

## Prerequisites

- Access to the `azure-client` host with Azure CLI installed
- Azure credentials (run `showcreds` on the host to retrieve them)

## Step 1: Log in to Azure

```bash
showcreds

az login --service-principal -u <CLIENT_ID> -p <CLIENT_SECRET> --tenant <TENANT_ID>
az account set --subscription <SUBSCRIPTION_ID>
```

## Step 2: Identify the resource group and set variables

```bash
az group list -o table

RG=kml_rg_main-f82a3ad30fc547da
LOC=eastus
```

## Step 3: Generate the SSH key pair

```bash
ls ~/.ssh/id_rsa* 2>/dev/null || ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
```

This creates `~/.ssh/id_rsa` (private) and `~/.ssh/id_rsa.pub` (public).

## Step 4: Create the Static Public IP

```bash
az network public-ip create \
  --resource-group $RG \
  --name nautilus-pip \
  --location $LOC \
  --allocation-method Static \
  --sku Standard
```

## Step 5: Create the VM

The subscription has an Azure Policy that blocks Premium disks and disks larger than 128 GB.
`az vm create` defaults to a Premium SSD, so the Standard SKU and a small disk size must be set explicitly.

```bash
az vm create \
  --resource-group $RG \
  --name nautilus-vm \
  --location $LOC \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/id_rsa.pub \
  --public-ip-address nautilus-pip \
  --authentication-type ssh \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30
```

## Step 6: Verify

```bash
# Static IP is allocated and attached
az network public-ip show -g $RG -n nautilus-pip \
  --query "{ip:ipAddress, method:publicIPAllocationMethod}" -o table

# VM size, state and IP
az vm show -g $RG -n nautilus-vm -d \
  --query "{name:name, size:hardwareProfile.vmSize, state:powerState, ip:publicIps}" -o table

# SSH using the generated key
ssh -o StrictHostKeyChecking=no -i ~/.ssh/id_rsa \
  azureuser@$(az network public-ip show -g $RG -n nautilus-pip --query ipAddress -o tsv)
```

Expected results:

- `method` is `Static`
- `size` is `Standard_B1s`
- `state` is `VM running`
- The SSH login succeeds without a password

## Troubleshooting

### `RequestDisallowedByPolicy` on the OS disk

**Error:** `This storage configuration is not allowed. Ensure the disk size is 128 GB or less and the SKU is not Premium...`

**Cause:** The default OS disk is Premium SSD, which the lab policy forbids.

**Fix:** Add `--storage-sku Standard_LRS --os-disk-size-gb 30` to `az vm create`.

### `PropertyChangeNotAllowed: osDisk.managedDisk.storageAccountType`

**Cause:** A failed VM from a previous attempt still exists with a Premium disk setting. A new deployment with the same name tries to modify it in place, which Azure does not allow.

**Fix:** Delete the failed VM, then its NIC, and run `az vm create` again.

```bash
az resource list -g $RG -o table

az resource delete -g $RG -n nautilus-vm --resource-type Microsoft.Compute/virtualMachines
# or
az vm delete -g $RG -n nautilus-vm --yes --force-deletion true

az network nic delete -g $RG -n nautilus-vmVMNic
```

Keep `nautilus-pip`, the NSG (`nautilus-vmNSG`) and the VNet (`nautilus-vmVNET`), since the new VM reuses them.

### Failed VM cannot be deleted (policy error on delete)

If `az vm delete` is also rejected by policy, try these in order:

1. `az resource delete -g $RG -n nautilus-vm --resource-type Microsoft.Compute/virtualMachines`
2. `az vm delete -g $RG -n nautilus-vm --yes --force-deletion true`
3. Raw REST call:
   ```bash
   SUB=$(az account show --query id -o tsv)
   az rest --method delete \
     --url "https://management.azure.com/subscriptions/$SUB/resourceGroups/$RG/providers/Microsoft.Compute/virtualMachines/nautilus-vm?forceDeletion=true&api-version=2024-03-01"
   ```

If none work, inspect the existing VM's disk settings with `az vm show -g $RG -n nautilus-vm --query storageProfile.osDisk` and build a command that matches them.

### Other issues

| Problem | Fix |
|---------|-----|
| `SkuNotAvailable` | Check `az vm list-skus --location $LOC --size Standard_B1s -o table` or try another region |
| Image not recognized | `az vm image list --offer Ubuntu --all -o table`, or try `Ubuntu2404` |
| SSH permission denied | `chmod 600 ~/.ssh/id_rsa` and use user `azureuser` |
| Port 22 blocked | `az vm open-port -g $RG -n nautilus-vm --port 22` |

## Clean-run summary

If starting from a clean resource group, the whole task is:

```bash
RG=kml_rg_main-f82a3ad30fc547da
LOC=eastus

ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""

az network public-ip create -g $RG -n nautilus-pip -l $LOC \
  --allocation-method Static --sku Standard

az vm create -g $RG -n nautilus-vm -l $LOC \
  --image Ubuntu2204 --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/id_rsa.pub \
  --public-ip-address nautilus-pip \
  --authentication-type ssh \
  --storage-sku Standard_LRS --os-disk-size-gb 30
```

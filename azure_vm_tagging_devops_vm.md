# Azure VM Tagging — `devops-vm`

## Task

Add the following tag to the Azure virtual machine:

| Tag Name | Tag Value |
|---|---|
| `Environment` | `dev` |

## Method 1: Azure Portal

1. Open the **Azure Portal**.
2. Go to **Virtual machines**.
3. Open the VM **`devops-vm`**.
4. Select **Tags** from the left-side menu.
5. Add:

   ```text
   Name:  Environment
   Value: dev
   ```

6. Click **Save**.
7. Reopen **Tags** and verify that the tag is present.

## Method 2: Azure CLI

### 1. Log in to Azure

```bash
az login
```

### 2. Find the Resource Group

```bash
az vm list --query "[].{Name:name, ResourceGroup:resourceGroup}" -o table
```

Find `devops-vm` in the output and note its resource group.

### 3. Add the Tag

Replace `<RESOURCE_GROUP>` with the actual resource group:

```bash
az vm update \
  --name devops-vm \
  --resource-group <RESOURCE_GROUP> \
  --set tags.Environment=dev
```

### 4. Verify

```bash
az vm show \
  --name devops-vm \
  --resource-group <RESOURCE_GROUP> \
  --query tags
```

Expected output:

```json
{
  "Environment": "dev"
}
```

## Completion Checklist

- [x] VM identified: `devops-vm`
- [x] Tag name: `Environment`
- [x] Tag value: `dev`
- [x] Tag verified

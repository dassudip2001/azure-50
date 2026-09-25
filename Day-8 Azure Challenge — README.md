# Azure Lab: Attach Existing Managed Disk to VM

## Objective

Attach the existing managed disk `datacenter-disk` to the existing virtual machine `datacenter-vm` as a **data disk** using the Azure Portal UI.

## Existing Resources

| Resource | Name | Region |
|---|---|---|
| Virtual Machine | `datacenter-vm` | `southcentralus` |
| Managed Disk | `datacenter-disk` | `southcentralus` |

## Prerequisites

- Azure Portal access
- `datacenter-vm` already exists
- `datacenter-disk` already exists
- Both resources are in `southcentralus`
- VM initialization has completed before submitting the task

## Steps Using Azure Portal

### 1. Open the VM

1. Sign in to the **Azure Portal**.
2. Search for **Virtual machines**.
3. Open **Virtual machines**.
4. Select **`datacenter-vm`**.

### 2. Open Disks

From the VM page:

1. In the left menu, under **Settings**, select **Disks**.
2. Locate the **Data disks** section.

### 3. Attach the Existing Disk

Depending on the current Azure Portal interface, select the option for attaching an existing disk, such as:

```text
Attach existing disks
```

Select:

```text
datacenter-disk
```

For **LUN**:

- Choose an available LUN.
- If Azure automatically selects an available LUN, keep the default.

For **Host caching**:

- Leave the default value unless the task specifies otherwise.

### 4. Save

Click:

```text
Save
```

Azure will update the VM configuration and attach `datacenter-disk` as a data disk.

## Verification

Go back to:

```text
datacenter-vm → Disks
```

Under **Data disks**, verify that:

```text
datacenter-disk
```

is listed.

Example:

```text
Data disks

Name              LUN
----------------  ---
datacenter-disk   0
```

The exact LUN may be different if Azure selected another available LUN.

## Final Checklist

- [ ] `datacenter-vm` exists
- [ ] `datacenter-disk` exists
- [ ] VM and disk are in `southcentralus`
- [ ] `datacenter-disk` is attached as a data disk
- [ ] `datacenter-disk` appears under **VM → Disks → Data disks**
- [ ] VM initialization has completed
- [ ] Changes have been saved

## Important

Do **not** create a new managed disk. The task requires attaching the **existing** `datacenter-disk`.

Once `datacenter-disk` is visible under the VM's **Data disks** section and VM initialization is complete, the task is ready for submission.

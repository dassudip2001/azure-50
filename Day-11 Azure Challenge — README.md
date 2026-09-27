# Azure VM Resize – `datacenter-vm`

## Task

Change the Azure VM `datacenter-vm` from:

```text
Standard_B1s
```

to:

```text
Standard_B2s
```

After resizing, make sure the VM is in the **Running** state.

## Steps – Azure Portal UI

### 1. Open Virtual Machines

1. Log in to the **Azure Portal**.
2. Search for **Virtual machines**.
3. Open **Virtual machines**.
4. Select **`datacenter-vm`**.

### 2. Stop the VM

1. On the VM **Overview** page, click **Stop**.
2. Confirm the operation.
3. Wait until the VM shows:

```text
Stopped (deallocated)
```

### 3. Resize the VM

1. In the left-side menu, go to:

   **Availability + scale → Size**

2. Search for:

```text
Standard_B2s
```

3. Select **Standard_B2s**.
4. Click **Resize**.
5. Wait for the resize operation to complete.

### 4. Start the VM

1. Return to the VM **Overview** page.
2. Click **Start**.
3. Wait until the VM status becomes:

```text
Running
```

## Verification

Confirm the VM has:

| Property      | Expected Value  |
| ------------- | --------------- |
| VM Name       | `datacenter-vm` |
| Previous Size | `Standard_B1s`  |
| New Size      | `Standard_B2s`  |
| Final State   | `Running`       |

## Result

The `datacenter-vm` VM has been resized from **Standard_B1s** to **Standard_B2s** and returned to the **Running** state.

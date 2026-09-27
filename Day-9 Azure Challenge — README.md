# Attach Public IP to Azure VM

## Task

Attach the existing public IP address `xfusion-pip` to the network interface of the Azure VM `xfusion-vm-pip`.

## Resources

* **VM:** `xfusion-vm-pip`
* **Public IP:** `xfusion-pip`

## Steps — Azure Portal

1. Open the **Azure Portal**.
2. Go to **Virtual Machines**.
3. Open **`xfusion-vm-pip`**.
4. Select **Networking**.
5. Click the **Network Interface (NIC)** attached to the VM.
6. Under **Settings**, select **IP configurations**.
7. Open the existing configuration, usually `ipconfig1`.
8. Under **Public IP address**, select **Associate**.
9. Select **`xfusion-pip`**.
10. Click **Save**.

## Verification

Go back to:

```text
Virtual Machines
→ xfusion-vm-pip
→ Overview
```

Check the **Public IP address**. It should display the IP assigned to `xfusion-pip`.

You can also verify from:

```text
VM
→ Networking
→ Network Interface
→ IP configurations
→ ipconfig1
```

The public IP should show:

```text
xfusion-pip
```

## Architecture

```text
xfusion-vm-pip
      │
      ▼
Network Interface (NIC)
      │
      ▼
IP Configuration (ipconfig1)
      │
      ▼
xfusion-pip
```

## Result

The `xfusion-pip` public IP is successfully associated with the NIC of `xfusion-vm-pip`.

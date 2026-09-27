# Attach Existing NIC to Azure VM

## Task

Attach the existing network interface `datacenter-nic` to the virtual machine `datacenter-vm`.

### Resources

| Resource | Name             | Region      |
| -------- | ---------------- | ----------- |
| VM       | `datacenter-vm`  | `centralus` |
| NIC      | `datacenter-nic` | `centralus` |

## Azure Portal Steps

1. Log in to the **Azure Portal** using the credentials available from the `azure-client` host.

2. Retrieve credentials if required:

```bash
showcreds
```

3. Open **Virtual Machines**.

4. Select:

```text
datacenter-vm
```

5. Make sure the VM initialization/provisioning has completed.

6. Go to:

```text
Networking → Network settings
```

7. Select **Attach network interface**.

8. Choose:

```text
datacenter-nic
```

9. Click **Save**.

10. Wait for the operation to complete.

## Verification

Open:

```text
Network interfaces → datacenter-nic → Overview
```

Verify that the associated VM is:

```text
datacenter-vm
```

Also verify from:

```text
datacenter-vm → Networking → Network settings
```

that `datacenter-nic` is attached.

## Final Checklist

* [ ] `datacenter-vm` initialization is complete
* [ ] `datacenter-vm` is ready/running
* [ ] `datacenter-nic` is attached to the VM
* [ ] NIC status is **Attached**
* [ ] Resources are in `centralus`

**Task completed successfully when `datacenter-nic` shows as attached to `datacenter-vm`.**

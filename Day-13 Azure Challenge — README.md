# Azure VM Root SSH Key Configuration

## Overview

This task configures passwordless SSH access from the Azure client/landing host to the `nautilus-vm` Azure VM as the `root` user.

## VM Details

| Property          | Value                          |
| ----------------- | ------------------------------ |
| VM Name           | `nautilus-vm`                  |
| Resource Group    | `KML_RG_MAIN-7F7F6E622A2B444F` |
| Region            | `westus`                       |
| Public IP         | `20.237.158.155`               |
| Default SSH User  | `azureuser`                    |
| Client Public Key | `/root/.ssh/id_rsa.pub`        |

---

## 1. Find the Resource Group

```bash
az vm list --query "[?name=='nautilus-vm'].{VM:name,ResourceGroup:resourceGroup,Location:location}" -o table
```

Expected:

```text
VM           ResourceGroup                 Location
-----------  ----------------------------  ----------
nautilus-vm  KML_RG_MAIN-7F7F6E622A2B444F  westus
```

---

## 2. Get the VM Public IP

```bash
az vm show \
  -g KML_RG_MAIN-7F7F6E622A2B444F \
  -n nautilus-vm \
  -d \
  --query publicIps \
  -o tsv
```

Expected:

```text
20.237.158.155
```

---

## 3. Connect to the VM

```bash
ssh azureuser@20.237.158.155
```

Verify the user:

```bash
whoami
```

Expected:

```text
azureuser
```

---

## 4. Create Root SSH Directory

On the VM:

```bash
sudo mkdir -p /root/.ssh
sudo chmod 700 /root/.ssh
```

Exit:

```bash
exit
```

---

## 5. Copy Root Public Key

From the `azure-client` host:

```bash
cat /root/.ssh/id_rsa.pub | ssh azureuser@20.237.158.155 "sudo tee -a /root/.ssh/authorized_keys > /dev/null"
```

The command normally produces no output when successful.

---

## 6. Configure Permissions

Connect again:

```bash
ssh azureuser@20.237.158.155
```

Run:

```bash
sudo chmod 700 /root/.ssh
sudo chmod 600 /root/.ssh/authorized_keys
sudo chown -R root:root /root/.ssh
```

---

## 7. Enable Root SSH Login

Configure SSH:

```bash
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
```

Enable public-key authentication:

```bash
sudo sed -i 's/^#\?PubkeyAuthentication.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config
```

Validate the configuration:

```bash
sudo sshd -t
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

---

## 8. Remove Azure Root Login Restriction

Azure Ubuntu images can contain a restricted key in `/root/.ssh/authorized_keys` that forces the following message:

```text
Please login as the user "azureuser" rather than the user "root".
```

Check the file:

```bash
sudo cat /root/.ssh/authorized_keys
```

If you see an entry containing:

```text
command="echo 'Please login as the user \"azureuser\" rather than the user \"root\".'..."
```

replace it with the clean public key from the Azure client.

From `azure-client`:

```bash
cat /root/.ssh/id_rsa.pub | ssh azureuser@20.237.158.155 'sudo tee /root/.ssh/authorized_keys > /dev/null && sudo chmod 600 /root/.ssh/authorized_keys && sudo chmod 700 /root/.ssh && sudo chown -R root:root /root/.ssh'
```

Then verify:

```bash
ssh azureuser@20.237.158.155
```

```bash
sudo cat /root/.ssh/authorized_keys
```

The key should be a normal public-key entry such as:

```text
ssh-rsa AAAA...
```

It should **not** contain the Azure `command="echo ... azureuser ..."` restriction.

---

## 9. Verify SSH Configuration

Run:

```bash
sudo sshd -T | grep -E 'permitrootlogin|pubkeyauthentication|passwordauthentication'
```

Expected:

```text
permitrootlogin yes
pubkeyauthentication yes
passwordauthentication no
```

`passwordauthentication no` is acceptable because SSH public-key authentication is being used.

---

## 10. Final Passwordless Root SSH Test

Exit the VM:

```bash
exit
```

From `azure-client`, run:

```bash
ssh -i /root/.ssh/id_rsa root@20.237.158.155
```

Verify:

```bash
whoami
```

Expected:

```text
root
```

No password should be requested.

---

## Troubleshooting

### `Please login as the user "azureuser" rather than the user "root".`

Check:

```bash
sudo cat /root/.ssh/authorized_keys
```

Look for:

```text
command="echo 'Please login as the user \"azureuser\" rather than the user \"root\".'..."
```

Replace the restricted key with the clean client public key:

```bash
cat /root/.ssh/id_rsa.pub | ssh azureuser@20.237.158.155 'sudo tee /root/.ssh/authorized_keys > /dev/null && sudo chmod 600 /root/.ssh/authorized_keys && sudo chmod 700 /root/.ssh && sudo chown -R root:root /root/.ssh'
```

### `Permission denied (publickey)`

Check permissions:

```bash
sudo chmod 700 /root/.ssh
sudo chmod 600 /root/.ssh/authorized_keys
sudo chown -R root:root /root/.ssh
```

Validate SSH:

```bash
sudo sshd -t
```

Restart:

```bash
sudo systemctl restart ssh
```

### Verify the client public key

On `azure-client`:

```bash
cat /root/.ssh/id_rsa.pub
```

### Verify the VM public key

On `nautilus-vm`:

```bash
sudo cat /root/.ssh/authorized_keys
```

The clean key should match the client's public key.

---

## Final Checklist

* [x] `nautilus-vm` identified
* [x] Resource group identified
* [x] VM public IP identified
* [x] SSH access as `azureuser` verified
* [x] `/root/.ssh` created
* [x] Client root public key copied
* [x] `.ssh` permissions set to `700`
* [x] `authorized_keys` permissions set to `600`
* [x] Ownership set to `root:root`
* [x] `PermitRootLogin yes` configured
* [x] `PubkeyAuthentication yes` configured
* [x] Azure forced-command restriction removed
* [x] Passwordless root SSH verified

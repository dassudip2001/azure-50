# Azure VM Creation --- `xfusion-vm`

## Objective

Create an Azure Virtual Machine using the Azure Portal UI for the
Nautilus DevOps migration task.

## Required configuration

  Setting                  Required value
  ------------------------ -------------------------
  Resource group           Existing resource group
  VM name                  `xfusion-vm`
  Region                   `South Central US`
  Region code              `southcentralus`
  Image                    Ubuntu 24.04 LTS
  VM size                  `Standard_B1s`
  Network Security Group   Default/new NSG
  Inbound access           SSH / TCP 22
  Additional disk          30 GB
  Disk type                Standard HDD
  Other settings           Default
  Final test               SSH into the VM

------------------------------------------------------------------------

# 1. Get Azure credentials

On the `azure-client` host, open a terminal and run:

``` bash
showcreds
```

Use the credentials supplied by the lab to sign in.

Open the Azure Portal:

https://portal.azure.com/

Do not share passwords or private SSH keys.

------------------------------------------------------------------------

# 2. Open Virtual Machines

In Azure Portal:

1.  Search for **Virtual machines**.
2.  Open **Virtual machines**.
3.  Click **Create**.
4.  Select **Azure virtual machine**.

------------------------------------------------------------------------

# 3. Configure Basics

## Project details

### Subscription

Select the subscription provided by the lab.

### Resource group

Select the **existing resource group**.

Do not create another resource group.

------------------------------------------------------------------------

## Instance details

### Virtual machine name

Enter:

``` text
xfusion-vm
```

### Region

Select:

``` text
South Central US
```

The region identifier is:

``` text
southcentralus
```

### Availability options

Leave the default configuration.

### Security type

Leave the default configuration.

------------------------------------------------------------------------

# 4. Select Ubuntu 24.04 LTS

Under **Image**, select:

``` text
Ubuntu 24.04 LTS
```

Make sure the selected image is Ubuntu **24.04 LTS**, not Ubuntu 22.04.

------------------------------------------------------------------------

# 5. Select VM size

Click **See all sizes** if necessary.

Search for:

``` text
Standard_B1s
```

Select:

``` text
Standard_B1s
```

Click **Select**.

Verify that the VM size displays:

``` text
Standard_B1s
```

------------------------------------------------------------------------

# 6. Configure Administrator account

Use the administrator username required by the lab or the username you
selected during VM creation.

For authentication, use an SSH public key.

A recommended approach is to create the SSH key on `azure-client`
yourself.

------------------------------------------------------------------------

# 7. Generate your own SSH key

This is especially useful if Azure shows:

> Error downloading private key

On `azure-client`, run:

``` bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/xfusion-vm
```

When asked for a passphrase, press **Enter**.

Press **Enter** again to confirm.

The files will be:

``` text
~/.ssh/xfusion-vm
~/.ssh/xfusion-vm.pub
```

The private key is:

``` text
~/.ssh/xfusion-vm
```

The public key is:

``` text
~/.ssh/xfusion-vm.pub
```

Never share the private key.

------------------------------------------------------------------------

# 8. Copy the public key

Run:

``` bash
cat ~/.ssh/xfusion-vm.pub
```

Copy the complete single-line output.

It will normally start with something similar to:

``` text
ssh-rsa
```

Only copy the `.pub` key.

Do **not** copy:

``` bash
cat ~/.ssh/xfusion-vm
```

------------------------------------------------------------------------

# 9. Configure the public key in Azure

Return to the VM creation page.

If the portal displays a **Download Private Key** popup:

1.  Click **Return to create a virtual machine**.
2.  Go back to the **Basics** tab.
3.  Find **SSH public key source**.
4.  Select **Use existing public key**.
5.  Paste the output of:

``` bash
cat ~/.ssh/xfusion-vm.pub
```

Keep the administrator username unchanged.

------------------------------------------------------------------------

# 10. Allow SSH port 22

In the inbound port section:

Set:

``` text
Public inbound ports: Allow selected ports
```

Select:

``` text
SSH (22)
```

This creates/configures the network security rule needed for SSH access.

The NSG should allow:

``` text
Protocol: TCP
Destination port: 22
Action: Allow
```

------------------------------------------------------------------------

# 11. Configure Disks

Open the **Disks** tab.

## OS disk

Set:

``` text
OS disk type: Standard HDD
```

Leave other settings at their defaults.

------------------------------------------------------------------------

# 12. Add the 30 GB disk

Under **Data disks**:

1.  Click **Create and attach a new disk**.
2.  Set the size to:

``` text
30 GiB
```

3.  Set storage type to:

``` text
Standard HDD
```

4.  Leave other options at their defaults.
5.  Save the disk configuration.

The VM should now have an additional 30 GB Standard HDD.

> The portal may display disk capacity as GiB. Enter `30` for the
> requested 30 GB-class disk.

Do not format or mount the disk unless the task specifically asks for
it.

------------------------------------------------------------------------

# 13. Networking

Open the **Networking** tab.

Leave the default network configuration.

Verify:

``` text
Public IP: Enabled
Network Security Group: Default/new NSG
Inbound port: SSH (22)
```

Do not add unnecessary inbound ports.

------------------------------------------------------------------------

# 14. Management, Monitoring, Advanced and Tags

The task says the remaining configuration should remain default.

Therefore leave:

-   Management → Default
-   Monitoring → Default
-   Advanced → Default
-   Tags → Default/empty

------------------------------------------------------------------------

# 15. Review the configuration

Go to:

**Review + create**

Verify:

``` text
VM name       : xfusion-vm
Region        : South Central US
Image         : Ubuntu 24.04 LTS
VM size       : Standard_B1s
SSH           : Port 22
Data disk     : 30 GB
Disk type     : Standard HDD
```

Also confirm the existing resource group is selected.

------------------------------------------------------------------------

# 16. Create the VM

Click:

**Create**

If you are using your own SSH public key, Azure should not require you
to download a portal-generated private key.

Wait until deployment finishes successfully.

------------------------------------------------------------------------

# 17. Get the public IP

After deployment:

1.  Open the VM.
2.  Go to **Overview**.
3.  Find **Public IP address**.
4.  Copy the actual IP shown by Azure.

Example only:

``` text
20.x.x.x
```

Use the actual address from your VM.

------------------------------------------------------------------------

# 18. Prepare the SSH private key

On `azure-client`:

``` bash
chmod 600 ~/.ssh/xfusion-vm
```

Verify that it exists:

``` bash
ls -l ~/.ssh/xfusion-vm
```

Do not display or share the private key contents.

------------------------------------------------------------------------

# 19. SSH into the VM

Use the administrator username selected during creation.

Command:

``` bash
ssh -i ~/.ssh/xfusion-vm <USERNAME>@<PUBLIC-IP>
```

Example:

``` bash
ssh -i ~/.ssh/xfusion-vm azureuser@20.x.x.x
```

Replace:

``` text
azureuser
```

with your actual username.

Replace:

``` text
20.x.x.x
```

with the actual public IP.

------------------------------------------------------------------------

# 20. Accept the host key

On the first connection, SSH may display:

``` text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Enter:

``` text
yes
```

A successful login should give you a shell similar to:

``` text
azureuser@xfusion-vm:~$
```

------------------------------------------------------------------------

# 21. Verify hostname

Run:

``` bash
hostname
```

Expected:

``` text
xfusion-vm
```

------------------------------------------------------------------------

# 22. Verify Ubuntu version

Run:

``` bash
cat /etc/os-release
```

Look for Ubuntu 24.04.

You can also run:

``` bash
lsb_release -a
```

The release should show:

``` text
24.04
```

------------------------------------------------------------------------

# 23. Verify SSH service

Run:

``` bash
sudo systemctl is-active ssh
```

Expected:

``` text
active
```

For more details:

``` bash
sudo systemctl status ssh
```

------------------------------------------------------------------------

# 24. Verify the attached disk

Run:

``` bash
lsblk
```

You should see the OS disk and an additional disk.

For more information:

``` bash
sudo fdisk -l
```

The additional disk should have approximately the requested 30 GB
capacity.

A new data disk does not need to be formatted or mounted for this task.

------------------------------------------------------------------------

# 25. Verify Azure configuration

In Azure Portal, open:

**Virtual machines → xfusion-vm → Overview**

Confirm:

``` text
Name     : xfusion-vm
Location : South Central US
Size     : Standard_B1s
```

Then check **Networking**:

``` text
Public IP: Present
SSH/22: Allowed
```

Then check **Disks**:

``` text
OS disk: Standard HDD
Data disk: 30 GB
Data disk type: Standard HDD
```

------------------------------------------------------------------------

# 26. Troubleshooting

## A. "Error downloading private key"

If Azure shows:

``` text
Error downloading private key

An error occurred while trying to download the private key.
Please try again later.
```

Use your own SSH key instead.

On `azure-client`:

``` bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/xfusion-vm
```

Then:

``` bash
cat ~/.ssh/xfusion-vm.pub
```

Copy the public key.

In Azure:

1.  Click **Return to create a virtual machine**.
2.  Open **Basics**.
3.  Select **Use existing public key**.
4.  Paste the public key.
5.  Continue with VM creation.

After creation:

``` bash
chmod 600 ~/.ssh/xfusion-vm
```

Then:

``` bash
ssh -i ~/.ssh/xfusion-vm <USERNAME>@<PUBLIC-IP>
```

------------------------------------------------------------------------

## B. "Permission denied (publickey)"

Check the key permission:

``` bash
chmod 600 ~/.ssh/xfusion-vm
```

Retry:

``` bash
ssh -i ~/.ssh/xfusion-vm <USERNAME>@<PUBLIC-IP>
```

Verify that the Azure public key was copied from:

``` bash
cat ~/.ssh/xfusion-vm.pub
```

Also verify the administrator username.

------------------------------------------------------------------------

## C. SSH connection timed out

In Azure Portal:

**VM → Networking → Network settings → Inbound port rules**

Verify that TCP port `22` is allowed.

Also verify that the VM has a public IP.

------------------------------------------------------------------------

## D. SSH connects but login fails

Make sure you are using the same administrator username configured
during VM creation:

``` bash
ssh -i ~/.ssh/xfusion-vm <USERNAME>@<PUBLIC-IP>
```

Do not use the Azure Portal account email as the Linux username unless
you explicitly configured it that way.

------------------------------------------------------------------------

# 27. Final checklist

Use this checklist before completing the lab:

-   [ ] Existing resource group used
-   [ ] VM name is `xfusion-vm`
-   [ ] Region is `South Central US`
-   [ ] Image is Ubuntu 24.04 LTS
-   [ ] VM size is `Standard_B1s`
-   [ ] NSG is attached
-   [ ] Inbound SSH/TCP 22 is allowed
-   [ ] OS disk is Standard HDD
-   [ ] 30 GB data disk is attached
-   [ ] Data disk is Standard HDD
-   [ ] Remaining settings are default
-   [ ] VM has a public IP
-   [ ] SSH connection works
-   [ ] `hostname` returns `xfusion-vm`
-   [ ] Ubuntu version is 24.04

------------------------------------------------------------------------

# 28. Command reference

## Generate SSH key

``` bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/xfusion-vm
```

## Display public key

``` bash
cat ~/.ssh/xfusion-vm.pub
```

## Secure private key

``` bash
chmod 600 ~/.ssh/xfusion-vm
```

## SSH into VM

``` bash
ssh -i ~/.ssh/xfusion-vm <USERNAME>@<PUBLIC-IP>
```

## Check hostname

``` bash
hostname
```

## Check Ubuntu

``` bash
cat /etc/os-release
```

## Check SSH

``` bash
sudo systemctl is-active ssh
```

## Check disks

``` bash
lsblk
```

------------------------------------------------------------------------

# 29. Final expected state

The completed Azure VM should look like:

``` text
xfusion-vm
│
├── Resource Group
│   └── Existing lab resource group
│
├── Region
│   └── South Central US
│
├── Image
│   └── Ubuntu 24.04 LTS
│
├── Size
│   └── Standard_B1s
│
├── Network
│   ├── Public IP
│   └── NSG
│       └── Allow SSH TCP/22
│
├── Storage
│   ├── OS Disk
│   │   └── Standard HDD
│   └── Data Disk
│       ├── 30 GB
│       └── Standard HDD
│
└── SSH
    └── Working
```

## Completion test

The final test should succeed:

``` bash
ssh -i ~/.ssh/xfusion-vm <USERNAME>@<PUBLIC-IP>
```

Then:

``` bash
hostname
```

Expected:

``` text
xfusion-vm
```

This confirms that the VM was created and that SSH access is working.

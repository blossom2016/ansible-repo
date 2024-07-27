# Ansible-Repo

This is an Ansible repository for my personal use. I used multipass VMs for my testing. I created an `inventory.txt` file to store my hostnames. I have some controller VMs and some VMs that I use for testing.
If you follow these steps, you should be able to use Ansible to configure your VMs. To know how to setup multipass VMs, please refer to [this](https://multipass.run/install) document.

## Setting Hostnames

I set the hostnames first for the controller and then for the testing VMs. In my case, I had 2 VMs. To set the hostnames, I did the following:

Edit the `/etc/hosts` file:

```bash
sudo vi /etc/hosts
```

Add these lines:
```bash
127.0.1.1 primary
127.0.0.1 localhost
```

Edit the /etc/hostname file:
```bash
sudo vi /etc/hostname
```

Set the hostname:
```bash
primary
```

## Setting up SSH

Next, I set up SSH on the host and copied the public key to the other VMs. I SSH'd into each VM from the control VM to ensure it worked. I generated SSH key pairs using this command:
```bash
ssh-keygen -t rsa -b 4096 -C "<EMAIL>"
```
In my case:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

To avoid being prompted for the passphrase repeatedly, I loaded my SSH key into the SSH agent on my control machine:


Start the SSH agent:
```bash
eval "$(ssh-agent -s)"
```
Add the SSH key:
```bash
ssh-add ~/.ssh/id_ed25519
```
### Manually Copying Public Key to VMs

To manually copy the public key to the other VMs:

## Step 1: Generate an SSH Key Pair with a Passphrase
If you haven't already generated an SSH key pair with a passphrase, use the following command:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
This creates a key pair typically located at ~/.ssh/id_ed25519 (private key) and ~/.ssh/id_ed25519.pub (public key). You'll be prompted to enter a passphrase during this process.

## Step 2: Display the Public Key
To display the public key:
```bash
cat ~/.ssh/id_ed25519.pub
```

## Step 3: Access Each VM via Multipass Shell
Access the shell of each VM:
```bash
multipass shell vm1
```
## Step 4: Create the .ssh Directory and authorized_keys File
On each VM, create the .ssh directory and the authorized_keys file if they don't already exist, and set the appropriate permissions:
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```
## Step 5: Paste the Public Key
Paste the public key you copied earlier into the authorized_keys file. Save and exit the editor.

## Step 6: Set the Appropriate Permissions
Ensure the authorized_keys file has the correct permissions:
```bash
chmod 600 ~/.ssh/authorized_keys
```
## Step 7: Repeat for All VMs
Repeat Steps 3-6 for each VM (e.g., vm2 and vm3).

## Step 8: Ensure SSH Configuration on VMs
Ensure the SSH configuration on each VM allows key-based authentication. Edit the SSH daemon configuration file if necessary:
```bash
sudo nano /etc/ssh/sshd_config
```
Ensure the following lines are present and correctly configured:

```bash
PermitRootLogin prohibit-password
PasswordAuthentication no
PubkeyAuthentication yes
```
Restart the SSH service to apply any changes:

```bash
sudo systemctl restart ssh
```
## Step 9: Use SSH Agent for Passphrase Management
Load your SSH key into the SSH agent on your control machine to avoid being prompted for the passphrase repeatedly:

Start the SSH Agent:
```bash
eval "$(ssh-agent -s)"
```
Add SSH Key to the Agent:

```bash
ssh-add ~/.ssh/id_ed25519
```
You will be prompted to enter the passphrase once. After this, the SSH agent will handle the passphrase.

## Step 11: Test SSH Connectivity
Before running Ansible, test SSH connectivity to ensure everything is set up correctly:

```bash
ssh ubuntu@<vm1_ip>
```
You should be able to connect without entering the passphrase, as the SSH agent handles it.

## Step 12: Install Ansible on Control Machine
Install Ansible on the control machine using the following command:
```bash
sudo apt install ansible
```


## Step 13: Create Ansible Inventory File
The Inventory file is among the files here. Modify using the Readme as a guide. 

## Step 14: Create Ansible Playbook
The ansible playbook is also included in this repository. Modify using the Readme as a guide.
## Step 15: Run Ansible Playbook( to install nginx on target VMs)
Run the playbook using the following command:

```bash
ansible-playbook -i inventory.txt install_nginx.yml

```
### Verifying the Playbook Execution
To ensure that the tasks in the playbook were executed successfully on the target machines, you can verify the following:

1. Check nginx Installation
On the Target Machine:

Run:
```bash
dpkg -l | grep nginx
```

Expected Output:
```bash
ii  nginx                             1.18.0-0ubuntu1                              amd64        small, powerful, scalable web/proxy server
```
# This confirms that the nginx package is installed.
### OR
2. Check nginx Service Status
On the Target Machine:
```bash
sudo systemctl status nginx
```
# Expected Output:

```bash
Copy code
● nginx.service - A high-performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2024-07-27 12:00:00 UTC; 1min 30s ago
 ...
```
# This confirms that the nginx service is running and enabled.



# Ansible-Repo

This is an Ansible repository for my personal use. I used multiple VMs for my testing. I created an `inventory.txt` file to store my hostnames. I have some controller VMs and some VMs that I use for testing.

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
# Ansible-Repo 
This an ansible Repo for my personal use. I used multiple vm's for my testing.
I created an inventory.txt file to store my hostnames.
I has some controller vm and some vm that I use for testing.
I set the hostnames first for the controller and then the testing vm in my situation were 2 VMs by doing the following 
sudo vi /etc/hosts
then  
127.0.1.1 primary
127.0.0.1 localhost
sudo vi /etc/hostname
then 
primary


Then set up ssh on host and copied the public key to the other vm 
I ssh into each vm from control to see if it works
I generated ssh keypairs using this commmand
ssh-keygen -t rsa -b 4096 -C "<EMAIL>" in my case ssh-keygen -t ed25519 -C "your_email@example.com"

Then to avoid  to avoid being prompted for the passphrase repeatedly Load your SSH key into the SSH agent on my control machine

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

Then I copy the public key to the other vm. You can copy auttomatically using ssh-copy-id but in my case I had to copy it manually doing the following
Step 1: Generate an SSH Key Pair with a Passphrase
If you haven't already generated an SSH key pair with a passphrase, you can do so using the following command:

ssh-keygen -t ed25519 -C "your_email@example.com"
This will create a key pair typically located at ~/.ssh/id_ed25519 (private key) and ~/.ssh/id_ed25519.pub (public key). You'll be prompted to enter a passphrase during this process.

Step 2: Display the Public Key
You need to copy the contents of your public key. Display it using:

cat ~/.ssh/id_ed25519.pub

Step 3: Access Each VM via Multipass Shell
You will need to access the shell of each VM to manually copy the public key.

multipass shell vm1

Step 4: Create the .ssh Directory and authorized_keys File
On each VM, create the .ssh directory and the authorized_keys file if they don't already exist, and set the appropriate permissions:


mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
Step 5: Paste the Public Key
Paste the public key you copied earlier into the authorized_keys file. Save and exit the editor.

Step 6: Set the Appropriate Permissions
Ensure the authorized_keys file has the correct permissions:

chmod 600 ~/.ssh/authorized_keys
Step 7: Repeat for All VMs
Repeat Steps 3-6 for each VM (e.g., vm2 and vm3).

Step 8: Ensure SSH Configuration on VMs
Ensure the SSH configuration on each VM allows key-based authentication. Edit the SSH daemon configuration file if necessary:

sudo nano /etc/ssh/sshd_config
Ensure the following lines are present and correctly configured:

PermitRootLogin prohibit-password
PasswordAuthentication no
PubkeyAuthentication yes
Restart the SSH service to apply any changes:

sudo systemctl restart ssh
Step 9: Use SSH Agent for Passphrase Management
Load your SSH key into the SSH agent on your control machine to avoid being prompted for the passphrase repeatedly:

Start the SSH Agent:

eval "$(ssh-agent -s)"
Add SSH Key to the Agent:

ssh-add ~/.ssh/id_ed25519
You will be prompted to enter the passphrase once. After this, the SSH agent will handle the passphrase.

Step 11: Test SSH Connectivity
Before running Ansible, test SSH connectivity to ensure everything is set up correctly:

ssh ubuntu@<vm1_ip>
You should be able to connect without entering the passphrase, as the SSH agent handles it.

Step 12: Install Ansible on Control Machine
Install Ansible on the control machine using the following command:
sudo apt install ansible


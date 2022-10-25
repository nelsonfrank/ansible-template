# Ansible

## Contents

- Prerequisite
  - OpenSSH Overview and Setup
  - Setting up the Git repository

- Installation

## Prerequisite

### OpenSSH Overview and Setup

- Make sure OpenSSH is installed on the workstation and servers

- Connect to each server from the workstation, answer "yes" to initial connection prompt

```bash
ssh server_user@server-ip-address

# Ctrl + d - logout of server
```

- Create an SSH key pair (with a passphrase) for your normal user account

```bash
ssh-keygen -t ed25519 -C "new ssh key"

# -t = ssh key type
#i.e
# ed25519 (OpenSSH 6.5+) - most secure and short
# ECDSA (OpenSSH 5.7+)
# RSA
# DSA (No longer allowed by default in OpenSSH 7.0+)
#
# -c = comment, it can be your email or anything to
# distingush your key from other key
```

- Copy that key to each server

```bash
ssh-copy-id -i path/to/your/public/key.pub remote-user@ip-address-of-server

# -i = input file

# explaination
# ssh-copy-id copy your local public key and paste it on
# authorized_keys file in .ssh directory in your server
```

- Create and SSH key that is specific to Ansible

```bash
# generate dedicated ssh key for ansible
ssh-keygen -t ed25519 -C "ansible"
```

- Copy ansible key to each server

```bash
# copy ansible key to all server
ssh-copy-id -i path/to/your/public/ansible.pub user@ip-address
```

- Specify which key you want to use when ssh to a server

```bash
ssh -i ~/.ssh/ansible remote-user@server-ip

# -i = input file
```

- Cache the passphrase

```bash
# Start the ssh-agent in the background.
eval $(ssh-agent)

# Add your SSH private key to the ssh-agent.
ssh-add  ~/.ssh/private_key

# This will prevent you from add passphrase only in lifetime of that specific terminal instance. So if you close that termial you will have to run that commands again.

# Quick fix
# Create and add alias that will run that command for you to your .zshrc or .bashrc file

alias ssha='eval $(ssh-agent) && ssh-add'

# then you can use that alias next time you before you ssh to your server

ssha path/to/private/ssh-key
```

### Setting up the Git repository

- Add your default server private key to your github.
    - Create new ssh to github account under ```Account settings``` > ```SSH and GPG keys``` > ```New SSH key```

- This will help your server to pull and push changes from your github account.


## Installation

```bash

# Ubuntu installation
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

# Or Using pip install
# https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

# Other OS Platform
# Check it here:
# https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html
```
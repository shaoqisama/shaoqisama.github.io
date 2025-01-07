+++
date = '2025-01-07T10:43:51+08:00'
draft = true
title = 'SSH Config for Github Authentication'
tags = ['coding', 'linux']
ShowToc = true
+++
To generate a new SSH key and add it to the SSH agent on a Linux system for connecting to GitHub, follow these summarized steps:

## **1. Check for Existing SSH Keys**
Before generating a new SSH key, check if you already have one by looking in the `~/.ssh` directory.

## **2. Generate a New SSH Key**
Use the following command to create a new SSH key:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
If your system does not support Ed25519, use RSA instead:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Press Enter to accept the default file location and set a secure passphrase when prompted.

## **3. Start the SSH Agent**
Run this command to start the SSH agent in the background:
```bash
eval "$(ssh-agent -s)"
```

## **4. Add Your SSH Key to the SSH Agent**
Add your newly created SSH private key to the agent:
```bash
ssh-add ~/.ssh/id_ed25519
```
(Replace `id_ed25519` with your custom key name if different.)

## **5. Add Your Public Key to GitHub**
Copy your public key to the clipboard:
```bash
cat ~/.ssh/id_ed25519.pub
```
Then, log into GitHub, go to **Settings** > **SSH and GPG keys** > **New SSH key**, and paste your public key there.

## **6. Test Your Connection**
Finally, test if your SSH connection is working by running:
```bash
ssh -T git@github.com
```
You should receive a message confirming successful authentication.

This process allows you to securely connect to GitHub using SSH keys, enhancing both security and convenience in managing repositories.

Citations:
[1] https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux
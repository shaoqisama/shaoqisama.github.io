+++
date = '2025-01-07T10:43:51+08:00'
draft = false
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

PS: and remeber to change your remote repo to ssh-base one as following:
To change your existing Git repository from an HTTPS-based remote URL to an SSH-based one, follow these steps:

### 1. **Open Your Terminal**
   - Navigate to your local repository directory using the command line.

### 2. **Check Current Remote URL**
   - Verify the current remote URL by running:
     ```bash
     git remote -v
     ```
   - This will display the current fetch and push URLs for your remote repository.

### 3. **Change the Remote URL**
   - Use the following command to change the remote URL from HTTPS to SSH:
     ```bash
     git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
     ```
   - Replace `USERNAME` with your GitHub username and `REPOSITORY` with the name of your repository.

### 4. **Verify the Change**
   - Confirm that the remote URL has been updated by running:
     ```bash
     git remote -v
     ```
   - You should now see the SSH URL listed for both fetch and push operations.

### 5. **Push Changes (if necessary)**
   - If you have any changes to push, you can do so using:
     ```bash
     git push origin main
     ```
   - Replace `main` with your branch name if it's different.

### Additional Notes
- If you haven't set up SSH keys yet, you will need to create an SSH key pair and add it to your GitHub account before you can authenticate via SSH. Instructions for setting up SSH keys can be found in GitHub's documentation or other resources[2][5].
- This method is applicable for other Git hosting services as well, just adjust the SSH URL accordingly[1][3].

Citations:
[1] https://www.jvt.me/posts/2019/03/20/git-rewrite-url-https-ssh/
[2] https://www.howtogeek.com/devops/how-to-switch-a-github-repository-to-ssh-authentication/
[3] https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories
[4] https://phoenixnap.com/kb/git-clone-ssh
[5] https://stackoverflow.com/questions/57230972/how-to-migrate-from-https-to-ssh-github
[6] https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux
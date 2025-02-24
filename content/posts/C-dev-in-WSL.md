+++
date = '2024-12-02T14:28:52+08:00'
draft = false
title = 'C dev env in WSL'
tags = ['coding']
+++
# Setting Up a C Development Environment on VS Code with WSL (Ubuntu)

Learn how to configure a complete C development environment on **Visual Studio Code (VS Code)** using **Windows Subsystem for Linux (WSL)** with Ubuntu.

---
<!--more-->

## Step 1: Install Required Software

### Install WSL
1. Open PowerShell as Administrator.
2. Run:
   ```bash
   wsl --install
   ```
3. If WSL is already installed, ensure it’s updated:
   ```bash
   wsl --update
   ```

### Install Ubuntu
1. Install **Ubuntu** from the Microsoft Store.
2. Open Ubuntu and set up a username and password.

### Install VS Code
1. Download and install VS Code from the [official website](https://code.visualstudio.com/).

### Install VS Code WSL Extension
1. Open VS Code.
2. Go to Extensions (`Ctrl+Shift+X`).
3. Search for `Remote - WSL` and install it.

---

## Step 2: Set Up GCC Compiler on WSL
1. **Update Package List:**
   ```bash
   sudo apt update
   ```
2. **Install Build Essentials:**
   ```bash
   sudo apt install build-essential
   ```
   - This includes `gcc`, `g++`, `make`, and other essential tools.
3. **Verify GCC Installation:**
   ```bash
   gcc --version
   ```

---

## Step 3: Install Debugging Tools
1. **Install GDB:**
   ```bash
   sudo apt install gdb
   ```
2. **Verify GDB Installation:**
   ```bash
   gdb --version
   ```

---

## Step 4: Configure VS Code for C Development

### Open WSL in VS Code
1. Press `Ctrl+Shift+P` and type `Remote-WSL: New Window`.
2. Select this option to open VS Code in WSL mode.

### Install Extensions for C Development
1. Open the Extensions view (`Ctrl+Shift+X`).
2. Install:
   - **C/C++** by Microsoft.
   - **Code Runner** (optional, for quick code execution).

### Set Up Your Project
1. Create a project folder in WSL:
   ```bash
   mkdir ~/projects/my_c_project
   cd ~/projects/my_c_project
   ```
2. Open the folder in VS Code:
   ```bash
   code .
   ```

### Configure Build Tasks
1. Press `Ctrl+Shift+P` → type `Tasks: Configure Default Build Task` → select **Create tasks.json file from template** → choose **Others**.
2. Edit `tasks.json`:
   ```json
   {
     "version": "2.0.0",
     "tasks": [
       {
         "label": "Build C Program",
         "type": "shell",
         "command": "gcc",
         "args": [
           "-g",
           "${file}",
           "-o",
           "${fileDirname}/${fileBasenameNoExtension}"
         ],
         "group": {
           "kind": "build",
           "isDefault": true
         },
         "problemMatcher": ["$gcc"],
         "detail": "Generated by the task system."
       }
     ]
   }
   ```

### Configure Debugging
1. Go to the Run and Debug view (`Ctrl+Shift+D`).
2. Click **create a launch.json file** → select **C++ (GDB/LLDB)**.
3. Edit `launch.json`:
   ```json
   {
     "version": "0.2.0",
     "configurations": [
       {
         "name": "GCC Debug",
         "type": "cppdbg",
         "request": "launch",
         "program": "${fileDirname}/${fileBasenameNoExtension}",
         "args": [],
         "stopAtEntry": false,
         "cwd": "${workspaceFolder}",
         "environment": [],
         "externalConsole": false,
         "MIMode": "gdb",
         "setupCommands": [
           {
             "description": "Enable pretty-printing for gdb",
             "text": "-enable-pretty-printing",
             "ignoreFailures": true
           }
         ],
         "preLaunchTask": "Build C Program"
       }
     ]
   }
   ```

---

## Step 5: Write and Run a C Program

### Create a C File
1. Inside your project folder, create a new file `main.c`:
   ```c
   #include <stdio.h>

   int main() {
       printf("Hello, WSL and VS Code!n");
       return 0;
   }
   ```

### Build the Program
1. Press `Ctrl+Shift+B` to build the program.

### Run the Program
1. Execute the compiled program in the terminal:
   ```bash
   ./main
   ```

### Debug the Program
1. Press `F5` to start debugging.

---

## Step 6: Optional Enhancements

### Linting and Formatting
1. Install `clang-format` and `clang-tidy`:
   ```bash
   sudo apt install clang-format clang-tidy
   ```
2. Configure these tools in VS Code settings.

### Git Integration
1. Initialize a Git repository:
   ```bash
   git init
   ```
2. Use VS Code’s Git tools for version control.

### Terminal Customization
1. Install `zsh` and `oh-my-zsh` for a better terminal experience:
   ```bash
   sudo apt install zsh
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```

---

You’re now ready to start C programming on VS Code with WSL Ubuntu. Happy coding!

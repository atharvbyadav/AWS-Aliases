# AWS CLI Aliases Guide

This README.md file serves as a guide to using AWS CLI aliases for EC2 instance management. It includes explanations, commands, and setup instructions for both **Linux (Bash/Zsh/Fish)** and **Windows (PowerShell)** environments.

---

## Table of Contents
1. [Setup](#setup)
2. [Aliases for Linux (Bash/Zsh/Fish)](#aliases-for-linux-bashzshfish)
    1. [Describe All Instances](#1-describe-all-instances)
    2. [Describe Instances with Specific Information (Formatted Output)](#2-describe-instances-with-specific-information-formatted-output)
    3. [Start an EC2 Instance](#3-start-an-ec2-instance)
    4. [Stop an EC2 Instance](#4-stop-an-ec2-instance)
    5. [Reboot an EC2 Instance](#5-reboot-an-ec2-instance)
    6. [Terminate an EC2 Instance (Caution!)](#6-terminate-an-ec2-instance-caution)
3. [Functions for Windows (PowerShell)](#functions-for-windows-powershell)
    1. [Describe All Instances](#1-describe-all-instances-1)
    2. [Describe Instances with Specific Information (Formatted Output)](#2-describe-instances-with-specific-information-formatted-output-1)
    3. [Start an EC2 Instance](#3-start-an-ec2-instance-1)
    4. [Stop an EC2 Instance](#4-stop-an-ec2-instance-1)
    5. [Reboot an EC2 Instance](#5-reboot-an-ec2-instance-1)
    6. [Terminate an EC2 Instance (Caution!)](#6-terminate-an-ec2-instance-caution-1)
4. [How to Add These Aliases and Functions](#how-to-add-these-aliases-and-functions)
5. [License](#license)

---

## Setup
The following sections provide AWS CLI aliases for **Linux** and **Windows** environments. Choose the appropriate section based on your operating system.

---

## Aliases for Linux (Bash/Zsh/Fish)

To use these aliases, add them to your shell configuration file (`~/.bashrc`, `~/.zshrc`, or `~/.config/fish/config.fish`) and reload your shell:

```bash
source ~/.bashrc   # For Bash users  
source ~/.zshrc    # For Zsh users  
```

### **1. Describe All Instances**  
```bash
alias aws.describe='aws ec2 describe-instances'
```
- **Usage:**  
  ```bash
  aws.describe
  ```
- **Description:** Retrieves all EC2 instances' details in raw JSON format.  

---

### **2. Describe Instances with Specific Information (Formatted Output)**  
```bash
alias aws.des='aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId, Tags[?Key==`Name`].Value | [0], State.Name, PublicIpAddress]" --output table --no-cli-pager'
```
- **Usage:**  
  ```bash
  aws.des
  ```
- **Description:** Retrieves instance details (ID, Name, State, and Public IP) in a table format.

---

### **3. Start an EC2 Instance**  
```bash
alias aws.start='aws ec2 start-instances --instance-ids'
```
- **Usage:**  
  ```bash
  aws.start i-1234567890abcdef0
  ```
- **Description:** Starts the specified EC2 instance.

---

### **4. Stop an EC2 Instance**  
```bash
alias aws.stop='aws ec2 stop-instances --instance-ids'
```
- **Usage:**  
  ```bash
  aws.stop i-1234567890abcdef0
  ```
- **Description:** Stops the specified EC2 instance.

---

### **5. Reboot an EC2 Instance**  
```bash
alias aws.reboot='aws ec2 reboot-instances --instance-ids'
```
- **Usage:**  
  ```bash
  aws.reboot i-1234567890abcdef0
  ```
- **Description:** Reboots the specified EC2 instance.

---

### **6. Terminate an EC2 Instance (Caution!)**  
```bash
aws_terminate() {
    echo "Are you sure you want to terminate instance $1? (yes/no)"
    read confirm
    [[ "$confirm" == "yes" ]] && aws ec2 terminate-instances --instance-ids "$1" || echo "Termination aborted."
}
```
- **Usage:**  
  ```bash
  aws_terminate i-1234567890abcdef0
  ```
- **Description:** Asks for confirmation before terminating an instance to prevent accidental deletions.

---

## Functions for Windows (PowerShell)

To use these functions, add them to your **PowerShell profile (`$PROFILE`)** and reload your session.

### **1. Describe All Instances**  
```powershell
function aws.describe {
    aws ec2 describe-instances
}
```
- **Usage:**  
  ```powershell
  aws.describe
  ```
- **Description:** Retrieves all EC2 instances' details in raw JSON format.

---

### **2. Describe Instances with Specific Information (Formatted Output)**  
```powershell
function aws.des {
    aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId, Tags[?Key=='Name'].Value | [0], State.Name, PublicIpAddress]" --output table
}
```
- **Usage:**  
  ```powershell
  aws.des
  ```
- **Description:** Retrieves instance details (ID, Name, State, and Public IP) in a table format.

---

### **3. Start an EC2 Instance**  
```powershell
function aws.start {
    param (
        [string[]]$InstanceIds
    )
    aws ec2 start-instances --instance-ids $InstanceIds
}
```
- **Usage:**  
  ```powershell
  aws.start -InstanceIds i-1234567890abcdef0
  ```
- **Description:** Starts the specified EC2 instance.

---

### **4. Stop an EC2 Instance**  
```powershell
function aws.stop {
    param (
        [string[]]$InstanceIds
    )
    aws ec2 stop-instances --instance-ids $InstanceIds
}
```
- **Usage:**  
  ```powershell
  aws.stop -InstanceIds i-1234567890abcdef0
  ```
- **Description:** Stops the specified EC2 instance.

---

### **5. Reboot an EC2 Instance**  
```powershell
function aws.reboot {
    param (
        [string[]]$InstanceIds
    )
    aws ec2 reboot-instances --instance-ids $InstanceIds
}
```
- **Usage:**  
  ```powershell
  aws.reboot -InstanceIds i-1234567890abcdef0
  ```
- **Description:** Reboots the specified EC2 instance.

---

### **6. Terminate an EC2 Instance (Caution!)**  
```powershell
function aws.terminate {
    param (
        [string[]]$InstanceIds
    )
    aws ec2 terminate-instances --instance-ids $InstanceIds
}
```
- **Usage:**  
  ```powershell
  aws.terminate -InstanceIds i-1234567890abcdef0
  ```
- **Description:** Terminates the specified EC2 instance.

---

## How to Add These Aliases and Functions

### **Linux (Bash/Zsh/Fish)**
1. Add the aliases to `~/.bashrc` or `~/.zshrc`.
2. Run `source ~/.bashrc` or `source ~/.zshrc` to apply changes.

### **Windows (PowerShell)**
1. Open PowerShell and run:
   ```powershell
   notepad $PROFILE
   ```
2. Copy and paste the functions into the file.
3. Save and reload PowerShell:
   ```powershell
   . $PROFILE
   ```

---

## License

This project is completely **free to use, modify, and share**. You can use these aliases and functions for personal, educational, or commercial purposes without any restrictions.  

Feel free to improve or customize them to fit your workflow. Contributions and suggestions are always welcome!  

If you find this useful, consider sharing it with others or giving it a ⭐ star on GitHub. Happy coding! 🚀

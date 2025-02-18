
# AWS CLI Aliases Guide

This README.md file serves as a guide to using AWS CLI aliases for EC2 instance management. It includes explanations, commands, and tips for simplifying your workflow with AWS EC2 using aliases for frequent tasks such as describing, starting, stopping, rebooting, and terminating instances.

---

## Table of Contents
1. [Setup](#setup)
2. [Aliases & Usage](#aliases--usage)
    1. [Describe All Instances](#1-describe-all-instances)
    2. [Describe Instances with Specific Information (Formatted Output)](#2-describe-instances-with-specific-information-formatted-output)
    3. [Start an EC2 Instance](#3-start-an-ec2-instance)
    4. [Stop an EC2 Instance](#4-stop-an-ec2-instance)
    5. [Reboot an EC2 Instance](#5-reboot-an-ec2-instance)
    6. [Terminate an EC2 Instance (Caution!)](#6-terminate-an-ec2-instance-caution)
3. [How to Add These Aliases to Your Shell](#how-to-add-these-aliases-to-your-shell)
4. [License](#license)

---

## Setup
To use these aliases, add them to your shell configuration file (`~/.bashrc`, `~/.zshrc`, or `~/.config/fish/config.fish`) and reload your shell:

```bash
source ~/.bashrc   # For Bash users  
source ~/.zshrc    # For Zsh users  
```

---

## Aliases & Usage

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
Instead of an alias, use a function to prevent accidental termination:

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

## How to Add These Aliases to Your Shell

1. Open your shell configuration file:  
   - **Bash:** `nano ~/.bashrc`  
   - **Zsh:** `nano ~/.zshrc`  
   - **Fish:** `nano ~/.config/fish/config.fish`  

2. Copy and paste the aliases/functions into the file.

3. Save the file and reload the shell:  
   ```bash
   source ~/.bashrc   # or ~/.zshrc
   ```

---

## License  
This repository is open-source. Feel free to modify and use these aliases as needed.

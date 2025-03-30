# Active Directory Homelab

## Disclaimer
This project is for educational and ethical testing purposes only. Unauthorized use of Active Directory configurations for malicious purposes is illegal. Ensure you have permission before conducting any security or administrative assessments.

## Brief Objective
The objective of this project was to set up a Windows Server 2019 Active Directory Domain Controller in a virtualized environment using Oracle VirtualBox and configure a Windows 10 client to join the domain. The process involved:
- Promoting the server to a domain controller
- Installing Active Directory Domain Services (AD DS) and Remote Access
- Configuring network settings
- Adding users via a PowerShell script
- Successfully connecting the client machine (CLIENT1) to the domain

This setup enabled CLIENT1 to automatically receive an IP address and authenticate users within the domain.

## Skills Learned
- Active Directory Configuration
- Virtualization Setup
- Network Configuration
- System Administration
- DHCP and IP Management

## Tools Used
- Oracle VirtualBox
- Windows Server Manager

## Steps

### 1. Downloading and Installing VirtualBox
- Download the latest version of Oracle VirtualBox from the [official website](https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html#vbox).
- Follow the setup wizard to install VirtualBox on your system.

### 2. Setting Up the Virtual Machine and Installing Windows Server 2019
- Download Windows Server 2019 from the [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019).
- Install the OS with the "Desktop Experience" option.
- Configure network adapters: 
  - Adapter 1: NAT for external connectivity
  - Adapter 2: Internal network

### 3. Configuring Network Settings on Windows Server 2019
- Set static IP for the internal network: 
  ```
  IP Address: 172.16.0.1
  Subnet Mask: 255.255.255.0
  Preferred DNS: 127.0.0.1
  ```

### 4. Promoting Windows Server 2019 to a Domain Controller
- Open **Server Manager** and add the **Active Directory Domain Services** role.
- Promote the server to a domain controller.
- Create a new forest with the domain name `mydomain.com`.

### 5. Creating a Dedicated Administrator Account
- Open **Active Directory Users and Computers**.
- Create an **Organizational Unit (OU)** named `_ADMIN`.
- Add a new user (e.g., `a-jsmith`) with administrator privileges.

### 6. Configuring Remote Access and NAT
- Install the **Remote Access** and **Routing** roles.
- Enable **Network Address Translation (NAT)** for internet access.

### 7. Configuring DHCP for Automatic IP Assignment
- Add the **DHCP role** in Server Manager.
- Create a new DHCP scope: 
  - IP Range: `172.16.0.100 - 172.16.0.200`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `172.16.0.1`

### 8. Populating Users via PowerShell Script
- Download the [PowerShell script](https://github.com/joshmadakor1/AD_PS/archive/master.zip).
- Run the script in PowerShell with administrative privileges:
  ```powershell
  Set-ExecutionPolicy Unrestricted
  .\AD_User_Creation.ps1
  ```
- Verify user creation in **Active Directory Users and Computers**.

### 9. Setting Up Windows 10 Client (CLIENT1)
- Download Windows 10 Enterprise from [Microsoft](https://www.microsoft.com/en-gb/software-download/windows10).
- Configure network settings in VirtualBox:
  - Adapter 1: Internal network
- Verify the IP configuration using:
  ```cmd
  ipconfig
  ```

### 10. Joining CLIENT1 to the Domain
- Go to **System Settings > Rename this PC (Advanced) > Change**.
- Enter `mydomain.com` as the domain.
- Authenticate with the admin account `a-jsmith`.
- Restart the client and log in with domain credentials.

### 11. Verifying CLIENT1's Domain Connection
- Check **DHCP leases** in the Domain Controller.
- Confirm CLIENT1 is listed under **Active Directory Computers**.
- Run the following command in Command Prompt:
  ```cmd
  whoami
  ```


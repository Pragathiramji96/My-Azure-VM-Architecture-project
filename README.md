# Azure VM High Availability Architecture Project Documentation

This project demonstrates how to deploy a high-availability setup on Azure using:
- Azure Virtual Machines (Linux and Windows)
- Load Balancer with health probes
- Network Security Groups (NSG)
- Azure Recovery Services Vault for backup
- Log Analytics for health monitoring and simulation

## 📌 Prerequisites
- Azure subscription with Contributor permissions
- Azure CLI installed
- PowerShell (for Windows setup)
- Visual Studio Code with Bicep extension (optional)

## 🧱 Architecture Overview

- 1 Virtual Network (VNet) with a subnet
- 2 Virtual Machines (1 Linux, 1 Windows) in different Availability Zones
- 1 Azure Load Balancer with backend pool and health probes
- Network Security Groups with rule to allow HTTP
- Recovery Services Vault for VM backup
- Log Analytics workspace for monitoring

---

## 🚀 Step-by-Step Deployment

### Step 1: Clone the Repository
```bash
git clone https://github.com/your-username/azure-vm-ha-architecture.git
cd azure-vm-ha-architecture
```

### Step 2: Create the Resource Group
```powershell
cd scripts
./create-resource-group.ps1
```
> This creates a resource group named `AzureHAProject` in `East US`.

### Step 3: Deploy Infrastructure using Bicep
```powershell
./deploy-infra.ps1
```
> This deploys the base architecture using the `main.bicep` template and `parameters.json` file.

### Step 4: Log in to Azure and Retrieve Public IPs
Use Azure Portal or CLI to find the public IP addresses of your VMs.

### Step 5: Install Web Servers
#### Linux VM (Apache)
```bash
ssh azureuser@<linux-vm-public-ip>
sudo apt update && sudo apt install -y apache2
```

#### Windows VM (IIS)
```powershell
Enter-PSSession -ComputerName <windows-vm-public-ip> -Credential (Get-Credential)
Install-WindowsFeature -Name Web-Server
```

---

## 💡 Configure Backup with Recovery Services Vault
1. Navigate to Recovery Services Vault in the Azure Portal
2. Select "Backup"
3. Choose the deployed VM(s)
4. Create a Backup Policy and apply
5. Run an initial backup
6. Perform a test restore to validate configuration

---

## 📊 Enable Monitoring with Log Analytics
1. In the Azure Portal, go to "Log Analytics Workspaces"
2. Create a workspace (or use existing)
3. Link it to both VMs under "Monitoring -> Diagnostics settings"
4. Use `Azure Monitor` > `Logs` to query for health degradation:
```kusto
Heartbeat | summarize LastSeen=max(TimeGenerated) by Computer
```

---

## 🖼️ Architecture Diagram
![HA Architecture]
![deepseek_mermaid_20250602_1ff13c](https://github.com/user-attachments/assets/7ed82b52-3f57-4747-a60d-4248fa3af38c)




---

## ✅ Validation Checklist
- [x] Linux and Windows VMs deployed across different AZs
- [x] NSGs and Load Balancer properly configured
- [x] HTTP access tested via public IP
- [x] Backup and test restore completed
- [x] Log Analytics monitoring connected

---

## 🔄 Cleanup Resources
To delete all resources:
```powershell
Remove-AzResourceGroup -Name AzureHAProject -Force
```

---



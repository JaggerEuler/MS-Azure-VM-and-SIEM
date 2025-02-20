
# Building a Miniature SOC Environment in Azure with Sentinel Integration

## Introduction
In this project, we will create a miniature Security Operations Center environment using Microsoft Azure. We'll deploy and connect a virtual machine to Azure Sentinel, Microsoft's cloud-native Security Information and Event Management system. This setup provides a hands-on experience in cloud security monitoring and incident response.

## Prerequisites
- **Azure Account**: Ensure you have an active Azure subscription. If not, you can sign up for a free account.
- **Basic Azure Knowledge**: Familiarity with Azure portal navigation and resource management.

## Step 1: Create a Resource Group
1. **Navigate to Azure Portal**: Log in to the [Azure Portal](https://portal.azure.com/).
2. **Create Resource Group**:
   - Go to **Resource Groups** > **Create**.
   - Enter a **Resource Group Name** (e.g., `SOC-ResourceGroup`).
   - Select your **Region**.
   - Click **Review + Create**, then **Create**.

## Step 2: Deploy a Virtual Machine
1. **Create Virtual Machine**:
   - Navigate to **Virtual Machines** > **Create** > **Azure Virtual Machine**.
   - Select the previously created **Resource Group**.
   - Enter the **Virtual Machine Name** (e.g., `SOC-VM`).
   - Choose the **Region** matching your resource group.
   - Select an appropriate **Image** (e.g., Windows Server 2019 Datacenter).
   - Choose a **Size** based on your requirements.
   - Configure **Administrator Account** with a username and password.
   - Set **Inbound Port Rules** to allow necessary ports (e.g., RDP - 3389).
   - Click **Review + Create**, then **Create**.

2. **Configure Networking**:
   - In the **Networking** tab, ensure a new **Virtual Network (VNet)** and **Subnet** are created.
   - Assign a **Public IP** for remote access.
   - Configure **Network Security Group** rules as needed.

## Step 3: Install and Configure Azure Sentinel
1. **Access Azure Sentinel**:
   - In the Azure Portal, navigate to **Azure Sentinel**.
   - Click **Add** to create a new Sentinel workspace.

2. **Create Log Analytics Workspace**:
   - Click **Create New Workspace**.
   - Assign it to the existing **Resource Group**.
   - Provide a **Workspace Name** (e.g., `SOC-Workspace`).
   - Select the same **Region** as your resources.
   - Click **OK**, then **Add**.

3. **Connect Data Sources**:
   - Within the Sentinel workspace, go to **Data Connectors**.
   - Select **Windows Security Events** and click **Open Connector Page**.
   - Follow the instructions to connect your VM:
     - Install the **Log Analytics Agent** on the VM; do not install the legacy analytics.
     - Configure the agent to report to your Log Analytics Workspace.

## Step 4: Create an RDP Login Rule
1. **Navigate to Analytics Rules** in Azure Sentinel.
2. Click **Create** > **Scheduled Query Rule**.
3. Name the rule (e.g., `Successful RDP Logins - Non-System Accounts`).
4. Create the specific query that will show us the successful RDP connection of non-system accounts:

SecurityEvent
| where activity contains "success" and Account !contains "system

5. Configure alert settings, defining severity and response actions; set this query to run every 5 minutes for this project.
6. Save and enable the rule.

## Step 5: Verify Data Ingestion
1. **Generate Test Logs**:
   - RDP into the VM using its public IP and administrator credentials.
   - Perform actions that generate security logs (e.g., failed login attempts).

2. **Monitor Logs in Sentinel**:
   - In Azure Sentinel, navigate to **Logs**.
   - Run queries to verify that logs from your VM are being ingested (e.g., search for Event ID 4625 for failed logins).
 

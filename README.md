# SIEM Lab

## Description
This repository contains a step by step walkthrough of how I created a home SIEM lab in the cloud using Microsoft Azure. In this lab, I utilized Microsoft Azure to set up a virtual machine and purposefully disabled the firewall, transforming the virtual machine into a honeypot designed to attract and analyze potential attackers. I also configured log forwarding to direct the failed attack attempts to a central repository and connected this repository to a Security Information and Event Management (SIEM) system for further analysis and monitoring. Lastly, i created an attack map that allowed me to see where the attacks are coming from. Running this lab has allowed me to gain hands-on experience with Microsoft Azure, honeypot setup, log management, and SIEM integration.

<h2>Languages and Utilities Used</h2>

- <b>KQL</b> 
- <b>Microsoft Azure Sentinel</b>
- <b>Windows Event Viewer</b>

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Microsoft Azure</b>

## Diagram
![image](https://github.com/user-attachments/assets/8be59972-ebba-4796-a089-79fd849880e6)

# Walkthrough:
## Created Free Azure Account
[Microsoft Azure](https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account)

## Created a resource group inside Microsoft Azure
A Resource Group organizes all project assets (VM, Virtual Network, Sentinel, Log Analytics).

![Image](https://github.com/user-attachments/assets/805ee78c-3dee-4d77-a36e-0eccc86c4567)

## Create a Virtual Network (VNet)
The VNet represents our cloud network. Any machine in Azure must exist within a network to communicate with the internet or other services.

![Image](https://github.com/user-attachments/assets/8b8967f0-806c-452d-8657-7ba8249363e4)

## Deployed the Honeypot VM
 <img width="1707" height="555" alt="image" src="https://github.com/user-attachments/assets/b60c5f05-de6c-4ff7-b0bb-3744ce5a596d" />


## Created a rule that allows all traffic inbound into the virtual machine
In the Network Security Group (NSG), set Inbound Rule = Allow Any/Any. This exposes RDP to the world so bots/attackers can attempt login.
In a real company, this would be a critical misconfiguration, commonly flagged by SOC teams.

<img width="429" height="643" alt="image" src="https://github.com/user-attachments/assets/f680fb8e-11f2-4956-a5fc-335af0392495" />

![Image](https://github.com/user-attachments/assets/05e9ba55-f3cc-4f03-acad-35317f36ee13)

## Signed into the Virtual Machine through RDC
RDP = Main attack surface we are monitoring.

![image](https://github.com/user-attachments/assets/b4e74c83-3d04-4c26-8944-ff67327f6203)

## Used Windows Defender to Disable the Firewall within the Virtual Machine
This intentionally removes host-level protection so attacks are visible and unblocked.

![Image](https://github.com/user-attachments/assets/6b804838-0c75-455f-9ca0-75e175342bb5)

## Pinged Virtual Machine from local computer
This ensures that our honeypot can be reached over the internet.

![image](https://github.com/user-attachments/assets/98b7c9b1-8b77-4824-b77f-a6a85d38c6ba)

## Created a Log Analytics workspace
This is our central log repository, where Windows logs are aggregated.

![image](https://github.com/user-attachments/assets/43731aec-6e3e-4815-aa5a-fc298482ccaf)

## Enable Microsoft Sentinel for the Workspace
Sentinel lets us query, correlate, visualize, and alert on logs from the honeypot. 

![image](https://github.com/user-attachments/assets/032b8d64-4cdb-4d15-aa5a-be5938772882)

## Configured the "Windows Security Events via AMA" within Sentinel.
This agent forwards security logs like:

- Failed login attempts (Event ID 4625)

- Successful logons (Event ID 4624)

- Authentication failures

- RDP attacks

![Image](https://github.com/user-attachments/assets/20fab5e0-7613-4a28-9d27-9f9b1660ee55)

## Created a Data Collection Rule (DCR)
The DCR defines WHAT logs to forward and WHERE they go.
Without it, the VM produces logs, but Sentinel will never receive them.

![image](https://github.com/user-attachments/assets/649a073a-63eb-4b4b-88b0-94cb0d395480)

Azure Monitor Windows Agent can be seen inside the Virtual Machine

![image](https://github.com/user-attachments/assets/db1e7bab-2432-474c-87e8-c33467b2591b)

## Confirm Logs in Log Analytics
This validates communication between: VM → AMA Agent → Log Analytics Workspace → Sentinel

![image](https://github.com/user-attachments/assets/7b8cc0e9-36e7-4653-a26d-8f2dbc0b7a3b)

## Query Failed Logins With KQL
```kusto
SecurityEvent
| where EventID == 4625
| where AccountType == "User"
| summarize Count = count() by IPAddress = RemoteHost, TimeGenerated
| sort by Count desc
```
![image](https://github.com/user-attachments/assets/1ad4081a-596f-4908-98af-54b1e1f416cb)

## Imported a [Spreadsheet](https://drive.google.com/file/d/13EfjM_4BohrmaxqXZLB5VUBIz2sv9Siz/view?usp=sharing) from Josh Madakor into Sentinel Watchlist
This spreadsheet contains geographic information for each block of IP addresses which we can view in Log Analytics. A watchlist converts raw IP addresses into real-world city/country data.
This reveals WHERE attacks originate (e.g., Russia, China, Brazil).

![image](https://github.com/user-attachments/assets/bef686f1-f483-45f6-b897-53b6bfe97713)

## Logs now have geographic information
![image](https://github.com/user-attachments/assets/51bc6b76-bf8e-4af8-9692-43515a5d4cc2)

## Created an Attack Map in Sentinel
This was done by pasting a [query](https://drive.google.com/file/d/1ErlVEK5cQjpGyOcu4T02xYy7F31dWuir/view?usp=drive_link) from Josh Madakor into a Sentinel workbook. Visualizing attacks helps SOC analysts identify:

- Multiple attacks from the same country
- Global botnet behavior
- Nation-state targeting trends

![image](https://github.com/user-attachments/assets/b09c798f-f40b-43c6-8790-1d951c2aae4e)

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
![Image](https://github.com/user-attachments/assets/805ee78c-3dee-4d77-a36e-0eccc86c4567)

## Created a virtual network within the resource group 
![Image](https://github.com/user-attachments/assets/8b8967f0-806c-452d-8657-7ba8249363e4)

## Created the Honey Pot/Virtual Machine
 ![Image](https://github.com/user-attachments/assets/d78d378b-0026-4674-81d8-a61af9002d7f)
![Image](https://github.com/user-attachments/assets/68b73fe4-0e47-4e1f-a390-20806671882a)
![image](https://github.com/user-attachments/assets/1d3bccc7-6e8a-4414-819c-1cf36c9465ed)

## Created a rule that allows all traffic inbound into the virtual machine
Configured this through the network security group. As a result, anyone from the internet can now access our honeypot/virtual machine.

![Image](https://github.com/user-attachments/assets/c5ab1d46-c39f-4c8a-a70e-d7ae51676178)
![Image](https://github.com/user-attachments/assets/05e9ba55-f3cc-4f03-acad-35317f36ee13)

## Signed into the Virtual Machine through RDC
![Image](https://github.com/user-attachments/assets/993e1e2a-09fa-4e6b-ad63-d3a4a59b322d)
![image](https://github.com/user-attachments/assets/b4e74c83-3d04-4c26-8944-ff67327f6203)

## Disabled WIndows Firewall within the Virtual Machine
![Image](https://github.com/user-attachments/assets/6b804838-0c75-455f-9ca0-75e175342bb5)

## Pinged Virtual Machine from local computer
This ensures that our honeypot can be reached over the internet.

![image](https://github.com/user-attachments/assets/98b7c9b1-8b77-4824-b77f-a6a85d38c6ba)

## Created a Log Analytics workspace
This will act as my log repository.

![image](https://github.com/user-attachments/assets/43731aec-6e3e-4815-aa5a-fc298482ccaf)

## Created a Sentinel and connected it to the Log Analytics workspace.
This allows me to access the logs in my log repository from my SIEM. 

![image](https://github.com/user-attachments/assets/032b8d64-4cdb-4d15-aa5a-be5938772882)

## Configured the "Windows Security Events via AMA" within Sentinel.
This  allows me to receive and observe the logs from the VIrtual Machine in Sentinel.

![image](https://github.com/user-attachments/assets/b49e8f77-9c88-4858-a387-a204cdcf5985)
![Image](https://github.com/user-attachments/assets/20fab5e0-7613-4a28-9d27-9f9b1660ee55)

## Create Data Collection Rule
This rule is used by the virtual machine to foward logs to our Log Analytics workspace which allows me to acess them from my SIEM

![image](https://github.com/user-attachments/assets/649a073a-63eb-4b4b-88b0-94cb0d395480)

Azure Monitor Windows Agent can be seen inside the Virtual Machine

![image](https://github.com/user-attachments/assets/db1e7bab-2432-474c-87e8-c33467b2591b)

## Viewed the Logs in the Log Analytics Workspace
Azure Monitor Windows Agent fowards logs to Log Analytics Workspace

![image](https://github.com/user-attachments/assets/7b8cc0e9-36e7-4653-a26d-8f2dbc0b7a3b)

## Queried the Log Analytics workspace With KQL to show failed attacks against our Honey Pot
![image](https://github.com/user-attachments/assets/1ad4081a-596f-4908-98af-54b1e1f416cb)

## Imported a [Spreadsheet](https://drive.google.com/file/d/13EfjM_4BohrmaxqXZLB5VUBIz2sv9Siz/view?usp=sharing) from Josh Madakor into Sentinel Watchlist
This spreadsheet contains geographic information for each block of IP addresses which we can view in Log Analytics. 

![image](https://github.com/user-attachments/assets/bef686f1-f483-45f6-b897-53b6bfe97713)

## Logs now have geographic information
![image](https://github.com/user-attachments/assets/51bc6b76-bf8e-4af8-9692-43515a5d4cc2)

## Created an Attack Map in Sentinel
This was done by pasting a [query](https://drive.google.com/file/d/1ErlVEK5cQjpGyOcu4T02xYy7F31dWuir/view?usp=drive_link) from Josh Madakor into a Sentinel workbook

![image](https://github.com/user-attachments/assets/891299fe-e843-4992-8991-f899822b3fd7)
![image](https://github.com/user-attachments/assets/b09c798f-f40b-43c6-8790-1d951c2aae4e)

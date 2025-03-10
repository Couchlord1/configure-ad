<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this project I demonstrate understanding of Window's Active Directory using Azure Virtual Machines to set up an active directory domain as shown in the following concept:

<br><img src="https://github.com/user-attachments/assets/88acad7c-617b-49d0-a990-088f1fd6385d"/><br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Setup 2 Virtual Machines in Azure
- Step 2: Set Up the Acive Directory on the Domain Controller
- Step 3: Join Client-1 to the domain
- Step 4: Edit Group Policy

<h2>Deployment and Configuration Steps</h2>
<h3>Step 1:</h3>
A minimum of 2 VMs will be required; one machine will be a domain controller (DC-1) running Windows Server 2022  and the other will be a client (Client-1) running Windows 10 Pro. Both VMs should be running on the same virtual network so for best practice, all reources should be created within the same resource group.

<br><img src="https://github.com/user-attachments/assets/1f411299-e051-4364-b3ad-b850b39b114e"/><br />

<br>DC-1 will act as the Domain Name System sever. Therefore, **DC-1's network interface card Private IP address must be set to _static_** by going to:
Home >> Virtual machines>> DC-1 >> Network settings >> IP configuration <br />

<br>Then manually configure Client-1 to use the private IP address of DC-1 for its DNS sever by going to:
Home >> Virtual machines >> Client-1 >> Network settings >> IP configuration >> DNS servers >> custom<br />

You can check to see if everything was configured correctly by logging into Client-1 and running **ipconfig /all** in command prompt. The listed DNS sever should be identical to DC-1's private IP.

<br><img src="https://github.com/user-attachments/assets/7f2f28c7-33fb-4057-9e77-d29968fd32bc"/><br />

<h3>Step 2:</h3>

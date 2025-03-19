<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this project I demonstrate an understanding of Window's Active Directory by using Azure Virtual Machines to set up an active directory domain as shown in the following concept:

<br><img src="https://github.com/user-attachments/assets/88acad7c-617b-49d0-a990-088f1fd6385d"/><br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- PowerShell/Cmd

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Setup 2 Virtual Machines in Azure
- Step 2: Set Up the Acive Directory on the Domain Controller
- Step 3: Join Client-1 to the domain
- Optional Step 4: Edit Group Policy

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

Install Active Directory Domain Services:

![Capture](https://github.com/user-attachments/assets/86738712-1dba-49b9-8e23-444124055812)

Then promote DC-1 to a domain controller for a new forest that, for the sake of this project, will be called "mydomain.com".

![Capture1](https://github.com/user-attachments/assets/4b96cc9d-88a1-4714-8734-c5ca372e4a26)

Go through the installation wizard, after installation the virtual machine will need to restart for changes to take effect. This means the remote desktop session will end and you will need to reconnect with new credentials (mydomain.com/"yourusername"). So long as all the steps were followed you should now be able to log in to DC-1 as a main domain admin account. To double check everything is working find you should go to the windows search bar >> *Active Directory Users and Computers* >> *mydomain.com* >> *Users* then find your account in the list:

![Capture2](https://github.com/user-attachments/assets/6ba788df-b9bd-4ee3-9064-0af2ea73fec0)

<h3>Step 3:</h3>

Now from Client-1, joing the computer to the newly created domain. *settings* >> *system* >> *about* >> *Rename this PC(advanced)* In the *computer Name* tab click *Change* then select "member of: Domain":
![Capture3](https://github.com/user-attachments/assets/a0fdca13-87e2-4962-aaf7-6cb362f629a1)

Using the same credentials as before "mydomain.com\labuser" you should be able to join the computer to the domain, at which point the computer will need to restart and end the remote desktop session. You can esnure the computer was added to the domain by logging in to DC-1 >> *Active Directory Users and Computers* >> *Computers* and checking if Client-1 has been added.:
![Capture4](https://github.com/user-attachments/assets/d9ba6263-62ab-4a7e-af01-0c1c41119c48)

The domain is now set up.

<h3>Step 4:</h3>

To explore further concepts and additional features, lets edit the group policy for this domain. From DC-1: *Group Policy Management* >> *Forest:mydomain.com* >> *Default Domain Policy* >> *Right click: Edit* >> From *Group Policy Management Policy Editor* >> *Computer Configuration* >> * Policies* >> *Windows Settings* >> *Security Settings* >> *Account Lockout Policy* >> Edit *Account lockout duration* for 30 minutes. Also edit *Allow Administrator account lockout*. This should set up the domain with a lockout feature after multple failed login attempts.
![Capture5](https://github.com/user-attachments/assets/5d081619-0f9f-4ca5-88f4-62a16d24c509)

Test this new feature by creating a new user from DC-1: *Active Directory Users and Computers* >> *Users* >> *Right Click* >> *New* >> *Users*r. Lock out this user from logging in by attempting to log in to Cleint-1 and incorrectly entering the password 5 times. Try to use the real password to log in on the 6th attempt to receive a warning window. Go back to DC-1, you should be able to see the created user's locked account in the *Active directory Users and Computers*.
![image](https://github.com/user-attachments/assets/917f10a0-f2b6-40d9-8f0e-07d90b24cb49)

You have now set up an Active Directory and set up a security feature using group policy, just one of the many features of Active Directory.


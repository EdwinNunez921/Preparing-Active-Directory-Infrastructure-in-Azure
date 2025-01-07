<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Preparing active directory Infrastructure in Azure (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<h2>Step 1: Create a Resource Group on Microsoft Azure</h2>

<p>
<img src="https://github.com/user-attachments/assets/4032ea3b-95bf-4473-8456-76184ca23aed" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Navigate to the Microsoft Azure portal and sign in with your credentials.

In the Azure menu, select Resource groups or search for it in the search bar.

Click + Create to start creating a new resource group.

Name the resource group Active-Directory-Lab. Ensure that the name is typed exactly as shown for consistency.

Select the appropriate Subscription and Region for your lab setup.


Click Review + Create to review the settings.
Once the validation passes, click Create to finalize the resource group.
</p>
<br />

<h2>Step 2: Create a Virtual Network in Microsoft Azure</h2>

<p>
<img src="https://github.com/user-attachments/assets/87fde712-9ee4-470f-afcb-37f0b400db35" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the Microsoft Azure portal, locate the Search bar at the top of the page and click on it.

Type Virtual Networks into the search bar and select Virtual Networks from the search results.

Click + Create to begin creating a new virtual network.

Under the Basics tab:

For the Resource Group, select Active-Directory-Lab (the resource group created in Step 1).

Name the virtual network Active-Directory-Vnet. Ensure the name is typed exactly as shown for consistency.

Click Review + Create to review the configuration settings.
Once the validation passes, click Create to finalize the virtual network.
</p>
<br />

<h2>Step 3: Create a Virtual Machine in Microsoft Azure</h2>
<p>
<img src="https://github.com/user-attachments/assets/f1e665e9-829b-4cc0-80be-b97dbf01a497" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the Microsoft Azure portal, navigate to the Search bar and type Virtual Machines. Select Virtual Machines from the search results.

Click + Create to start creating a new virtual machine.

Under the Basics tab:
For the Resource Group, select Active-Directory-Lab (the resource group created in Step 1).

Set the Virtual Machine Name to dc-1.

For the Image, select Windows Server 2022.

For the Size, choose any configuration with at least 2 vCPUs or higher based on your requirements.

Set your Username and Password for accessing the virtual machine. Ensure you remember these credentials.

Check the box to confirm the licensing agreement.

Click Next through the subsequent configuration tabs until you reach the Networking tab.

In the Networking tab:
Ensure the virtual machine is connected to the virtual network Active-Directory-Vnet created in Step 2.

Click Review + Create to validate the configuration.
Once the validation passes, click Create to deploy the virtual machine.
</p>
<br />

<h2>Step 4: Create a Client Virtual Machine in Microsoft Azure</h2>

<p>
<img src="https://github.com/user-attachments/assets/63ef13e8-3908-47c0-bfa8-42cfe91020ce" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Navigate back to Virtual Machines in the Microsoft Azure portal.

Click + Create to start creating a new virtual machine.

Under the Basics tab:
For the Resource Group, select Active-Directory-Lab (the same resource group used for dc-1).

Set the Virtual Machine Name to client-1.

For the Image, select Windows 10 Pro.

For the Size, choose any configuration with at least 2 vCPUs or higher.

Set your Username and Password for accessing the virtual machine. Ensure you remember these credentials.

Check the box to confirm the licensing agreement.

Click Next through the subsequent configuration tabs until you reach the Networking tab.

In the Networking tab:

Ensure the virtual machine is connected to the virtual network Active-Directory-Vnet.

Click Review + Create to validate the configuration.
Once the validation passes, click Create to deploy the virtual machine.
</p>
<br />

<h2>Step 5: Change DC-1 Private IP Address to Static in Microsoft Azure</h2>

<p>
<img src="https://github.com/user-attachments/assets/9d71e3da-ab3f-424b-80eb-79850f976da5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Navigate back to Virtual Machines in the Microsoft Azure portal.

Locate and click on DC-1 to access its configuration.

In the left-hand menu, select Networking.

Under Network Interface, click on the green network interface link (e.g., dc-1-nic).

On the Network Interface page, find and click IP Configurations from the left-hand menu.

Click on ipconfig1 to edit its settings.

Change the IP Allocation from Dynamic to Static.

Click Save to apply the changes.

By setting the private IP address to static, we ensure DC-1, acting as a server, maintains a consistent IP address. This configuration is essential because we will set Client-1 to use DC-1 as its DNS server. A static IP prevents disruptions that could occur if DC-1's IP address changes dynamically.
</p>
<br />

<h2>Step 6: Disable the Firewall on DC-1 for Lab Purposes</h2>

<p>
<img src="https://github.com/user-attachments/assets/7a4dd8e3-3abb-4fc1-b519-2c64c7e5bd4d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Retrieve Public IP Address:

Navigate to Virtual Machines in Azure.
Click on DC-1 to view its details.
Copy the Public IP Address of DC-1.
Connect via RDP:

Open Remote Desktop Protocol (RDP) on your local machine.
Enter the Public IP Address of DC-1.
Log in using the credentials you set during the VM creation process.
Verify DC-1 Access:

Ensure you're connected to DC-1 (Server Manager will open by default).
Access Windows Firewall Settings:

Right-click the Windows icon on the lower-left corner.
Select Run.
Type wf.msc into the Run dialog and press Enter.
Disable Windows Defender Firewall:

In the Windows Defender Firewall with Advanced Security window, click on Windows Defender Firewall Properties.
For each profile (Domain Profile, Private Profile, and Public Profile):
Select Off under the first option.
Click Apply to save changes.
Close and Confirm:

Close the firewall settings window and ensure all profiles are turned off.
Note: Disabling the firewall is not a best practice for production environments. This is done strictly for lab purposes to simplify communication between VMs.
</p>
<br />

<h2>Step 7: Configure Client-1 to Use DC-1 as the DNS Server</h2>

<p>
<img src="https://github.com/user-attachments/assets/d146cecc-3a70-4684-b34f-2a3344682ff9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Retrieve DC-1's Private IP Address:

In Azure, navigate to Virtual Machines and click on DC-1.
Scroll down to find the Private IP Address under Networking.
Copy the Private IP Address.
Navigate to Client-1 Settings:

Go back to Virtual Machines in Azure and click on Client-1.
Select Networking from the left-hand menu.
Under Network settings, click on the associated Network Interface.
Modify DNS Server Settings:

In the left-hand menu, click on DNS Servers.
Select Custom.
Paste DC-1's Private IP Address into the custom DNS field.
Click Save to apply the changes.
Verify Configuration:

Confirm that the DNS settings for Client-1 now point to DC-1's Private IP Address.
Purpose: Configuring Client-1 to use DC-1 as its DNS server allows it to resolve domain names within the Active Directory environment. This setup ensures a stable and reliable DNS configuration for the lab.
</p>
<br />

<h2>Step 8: Restart Client-1 and Verify Connection to DC-1</h2>

<p>
<img src="https://github.com/user-attachments/assets/3c4638b2-745a-44f7-85dc-561c90be6d03" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Navigate back to the virtual machine dashboard.
  
Select the checkbox next to Client-1.

Click the Restart button to reboot Client-1.

Once the machine has restarted, log in to Client-1 via Remote Desktop Protocol (RDP).

In Client-1, retrieve the private IP address of DC-1.

In the search bar of Client-1, type PowerShell and open the application.

Within the PowerShell window, type ping followed by DC-1’s private IP address and press Enter.

The ping request should successfully go through, confirming connectivity between Client-1 and DC-1.
</p>
<br />

<h2>Step 9: Verify DC-1’s Private IP Address in Client-1</h2>

<p>
<img src="https://github.com/user-attachments/assets/511ab250-4d2f-4acd-8463-1d148226c681" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the PowerShell window on Client-1, type the following command:

ipconfig /all

Press Enter to execute the command.

Review the output displayed in the PowerShell window.

Locate the second-to-last entry in the output.

The second-to-last entry should display the private IP address of DC-1.
</p>
<br />

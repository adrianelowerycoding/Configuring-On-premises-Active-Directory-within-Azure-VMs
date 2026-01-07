<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2025
- Windows 11 Pro (24H2)

<h2>High-Level Deployment and Configuration Steps</h2>

  1. Create Domain Controller Virtual Machine
  2. Set Domain Controller VM's NIC Private Address to Static
  3. Disable Domain Controller's Windows Firewall
  4. Create Client Virtual Machine
  5. Set Client's DNS settings to Domain Controller's Private IP Address
  6. Test Client and Domain Controller's Connection

<h2>Deployment and Configuration Steps</h2>

<h4>Creating Domain Controller Virtual Machine:</h4>
<p>
Azure -> Resource Group -> New -> Name resource group "ADLab" -> Set to certain region
</p>
<p>
<img width="729" height="328" alt="Step 1" src="https://github.com/user-attachments/assets/64981b98-8f26-451b-9ece-a3bef78c4b7e" />
</p>
<br />

<p>
Virtual Network -> New -> Set to ADLab resource group -> Name it "AD-Vnet" -> Set to same region as ADLab 
</p>
<p>
<img width="783" height="651" alt="Step 2" src="https://github.com/user-attachments/assets/cebafae2-1dd5-46d3-9d12-d2c5129de010" />
</p>
<br />

<p>
Virtual Machines -> New ->  Set to ADLab resouce group -> Name it "DC-1" -> Set to same region as resource group -> Set image to Windows Server 2025 Datacenter
</p>
<p>
<img width="828" height="901" alt="Step 3" src="https://github.com/user-attachments/assets/d96aef35-9e50-4c8e-9228-07e22d92ce31" />
</p>
<p>
Set username and password -> Check both licensing boxes 
</p>
<p>
<img width="795" height="640" alt="Step 4" src="https://github.com/user-attachments/assets/4230c5d2-32bd-4b74-9cb6-6e716fbb58e6" />
</p>
<p>
Set virtual network to ADVnet -> Set subnet as default 
</p>
<p>
<img width="776" height="180" alt="Step 5" src="https://github.com/user-attachments/assets/25221ee7-8f03-4480-83f9-aade9cb6ae34" />
</p>
<br />

<h4>Set Domain Controoller's NIC Private Address to Static: </h4>

<p>
Virtual Machines -> DC-1 -> Network -> Network Settings -> Click Network Interface / IP Configuration
</p>
<p>
<img width="922" height="544" alt="Step 6" src="https://github.com/user-attachments/assets/3fa78d5a-1cee-4b30-a594-b9d77d9558c3" />
</p>
<p>
-> ipconfig1 -> Click 'Static' in Allocation -> Save 
</p>
<p>
<img width="1261" height="626" alt="Step 7" src="https://github.com/user-attachments/assets/4bdb9007-f51a-43cb-b540-fc4ce26df41d" />
</p>
<p>
*Purpose: DC-1's private IP address should now never change no matter how many times it's restarted.* 
</p>

<h4>Disable DC-1's Windows Firewall:</h4>
<p>
Log into DC-1 -> Start Menu -> Run -> Open: wf.msc -> Ok -> Under Public Profile click "Windows Defender Firewall Properties" -> Turn Firewall State to Off -> Apply -> Ok 
</p>
<p>
<img width="399" height="197" alt="Step 8" src="https://github.com/user-attachments/assets/f9d6b3c7-92ff-4559-89a8-d1e474aeff6f" />
<img width="1044" height="782" alt="Step 9" src="https://github.com/user-attachments/assets/12e0d8da-58a6-4d3c-8a08-0559f6f8c9cb" />
</p>
<br />

<h4>Create Client Virtual Machine: </h4>

<p>
Create a VM called "Client-1" -> Place in ADLab resource group -> Place in same region as DC-1 -> Set image as Windows 11 Pro -> Set size as Standard_D2s_v3 
</p>
<p>
<img width="789" height="887" alt="Step 10" src="https://github.com/user-attachments/assets/0ef2087a-4669-45c3-bcd5-da4f2b8b3d50" />
</p>
<p>
Set username and password -> Check licensing box 
</p>
<p>
<img width="857" height="578" alt="Step 11" src="https://github.com/user-attachments/assets/84efda7e-06f1-46a8-b768-f65dd5a87e74" />
</p>

<h4>Set Client-1's DNS Settings to DC-1's Private IP Address: </h4>

<p>
Virtual Machines -> DC-1 -> Copy private IP address 
</p>
<p>
<img width="930" height="317" alt="Step 12" src="https://github.com/user-attachments/assets/09457533-bdc6-42c9-9383-34c57341f9aa" />
</p>
<p>
Virtual Machines -> Client-1 -> Network -> Network Settings -> Click Network Interface / IP Configuration -> Settings -> DNS Servers -> Set DNS server to Custom -> Paste DC-1's private IP address -> Save 
</p>
<p>
<img width="683" height="416" alt="Step 13" src="https://github.com/user-attachments/assets/bc8fb2a6-2302-45b1-a235-fdf115a2c4ae" />
</p>
<p>
Virtual Machines -> Client-1 -> Restart Client-1 
</p>

<h4>Test Client-1 and DC-1 Connection: </h4>
<p>
Log into Client-1 -> Open Powershell -> Type "ping (insert DC-1's private IP address)" to ping DC-1 -> If you get data back in your reply you're connected 
</p>
<p>
<img width="538" height="326" alt="Step 15" src="https://github.com/user-attachments/assets/5db9318c-9a55-467d-b9a1-ad4d2e3f1a3c" />
</p>
<p>
Run "ipconfig /all" in Powershell -> Output for the DNS settings should show DC-1's Private IP address if everything was done correctly. 
</p>
<p>
<img width="742" height="63" alt="Step 16" src="https://github.com/user-attachments/assets/84fc9ecc-497c-4767-9648-e1adbecde17e" />
</p>













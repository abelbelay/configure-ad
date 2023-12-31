<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>
<ul>
<li>Setup Resources in Azure
<li>Ensure Connectivity between the client and Domain Controller
<li>Install Active Directory
<li>Create an Admin and Normal User Account in AD
<li>Join Client-1 to your domain (mydomain.com)
<li>Setup Remote Desktop for non-administrative users on Client-1
<li>Create a bunch of additional users and attempt to log into client-1 with one of the users
</ul>

<h2>Deployment and Configuration Steps</h2>
<h4>Create a Virtual Machine for domain controller</h4>
<p>
<img src="https://user-images.githubusercontent.com/142059616/261184714-7833ec95-4dc9-48fa-aa71-b7f4cb8e0e4c.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
<ul>
  <li>choose a Resource group
  <li>Name your virtual machine(in this case "DC-1" for Domain controller1)
    <li>Select a Region
      <li>Choose Wndows Server for the image
        <li>Select a cpu with at least 2 vcpus and one with ample RAM
          <li>Enter your user name and password you will use to log in
  </li>
</ul>
<img src="https://user-images.githubusercontent.com/142059616/264208397-6f20905f-07d1-46b4-97d8-48a621326acd.png" height="60%" width="60%">
<img src="https://user-images.githubusercontent.com/142059616/264208527-881544ae-47b3-457e-9d1d-90b7b1848b0b.png" height="60%" width="60%">
</p>
<p>With aforementioned steps create another Virtual Machine, this one will be called client-1 as you will use this VM to act as a user.</p>
<br />

<h3>Change Domain controller ip address from dynamic to static</h3>
<p>select "networking" in the DC-1 main tab and then click on the network interface "dc-1996</p>
<img src="https://user-images.githubusercontent.com/142059616/264229168-df5e6a32-6a88-4b4f-8b0e-937017d11936.png">
<p>click on "IP configurations"</p>
<img src="https://user-images.githubusercontent.com/142059616/264230233-758e276b-f367-492c-aa76-696c6a63039a.png">
<p>change the IP address allocation from "Dynamic"--> "Static"</p>
<img src="https://user-images.githubusercontent.com/142059616/264232363-b62d0a59-6f1a-4126-9b01-77554dd71ad0.png">
<h4>Changing the Ip adress to static will allow it to remain constant over time, this means that the ip adrdresss is always reachable athe same address makinfg it easier to manage and access them. It is commonly used in Servers such as this one </h4>
<br>
</p>
<h3>Login to the domain controller and enable ICMPv4 in on the local windows firewall to ensure connectivity between Client and Domain Controller</h3>
<p>
Log into both the Domain controller and Client VM's using Remote Desktop Connection
<img src="https://user-images.githubusercontent.com/142059616/264488760-9adfebcd-292a-4eb3-81fa-a591791160f6.png" height="60%" width="60%">
</p>
<p>Click the start/windows key and select "windows defender firewall with advanced security"</p>
<img src="https://user-images.githubusercontent.com/142059616/264489496-04ab4f1b-61b8-4e8a-9dc4-11e4587a2dc2.png">
<p>Go to "Inbound rules" tab on the left, sort by "protocol" then right-click both "file printer sharing (echo request) icmpv4" and "virtual machine monitoring (echo request) icmpv4" and select "enable rule"</p>
<img src="https://user-images.githubusercontent.com/142059616/264491122-6f7d7e75-e05b-46a7-b824-fa8d1b978e5a.png">
Now when you ping the Domain controller the fireall isnt blocking the connection. You are now able to ping your client vm to the domain controller
<img src="https://user-images.githubusercontent.com/142059616/264491676-6cfe43e5-9179-4a87-8b0a-0742c98468cc.png">
<br />

<h3>Install Active Directory</h3>
<p>Go to manage-->"add roles and features"
<img src="https://user-images.githubusercontent.com/142059616/264793219-3dc7af07-f20d-420f-a335-69dd4361525e.png">
  Once the "add Role and feature wizard is open, select "next" twice until the "Server Roles" tab is open.Then you want to select "Active directory Domain Services" and then click "add features.
<img src="https://user-images.githubusercontent.com/142059616/264794408-b62c352f-5079-4855-ae27-3ceba8281732.png">
  Select "next" 3 more times until the "install" button appears and click that</p>
  

<p> go to the "Manage" tab on the right and select "promote server to a domain controller"
<img src="https://user-images.githubusercontent.com/142059616/264796118-fc18033c-3ec0-4b00-990e-30114a8d858e.png">
  Select "Add a new forest" and enter a root domain name. For this example we will call it mydomain.com
  <img src="https://user-images.githubusercontent.com/142059616/264796711-87e2eec8-d01f-4419-b3dd-207414aa12c7.png">
 <li> Next you will be prompted to enter a password for the domain.
 <li>After that it will allow you to install your domain services
 <li>You will then be automatically signed out
 <li>Log back into DC-1 as mydomain.com\labuser
  </p>
<br />
<h3>Create an Admin and Normal User Account in Active Directory</h3>
<p>
  Press the ⊞ Win key and type in "Active Directory Users & Computers"
  <img src="https://user-images.githubusercontent.com/142059616/264802035-5d100dbb-26ed-42c1-8449-c27e9bbaf295.png">
  <i>In this example we will create something called an Organizational unit(OU). An organizational unit (OU) in Active Directory is like a digital folder that helps you organize and manage your resources, such as users, computers, and other network objects. It's a way to group similar items together for easier management.</i>
  </p>
  <p>Right click "mydomain"-->New-->Organizational Units
  <img src="https://user-images.githubusercontent.com/142059616/264803273-933dcaca-f628-4d7e-963e-418f5e5591e4.png">
  </p>
  <p>We are going to name one "_EMPLOYEES" & "_ADMINS"
  <img src="https://user-images.githubusercontent.com/142059616/264805279-a6b5d81e-9faa-4724-ab06-96eaf30488b8.png">
  </p>
<p>We are going to create an admin.</p>
<img src="https://user-images.githubusercontent.com/142059616/264808964-e7da80dc-4229-4c49-9d1a-76bacd40be8e.png" height="50%" width="50%">
  <img src="https://user-images.githubusercontent.com/142059616/264809437-259462e5-08df-4ead-8234-dc21ceea7382.png" height="50%" width="50%">
  <img src="https://user-images.githubusercontent.com/142059616/264810862-727fd098-bd47-4257-87b1-40976fc51f7d.png" height="50%" width="50%">
</p>
<p>Now we have to make the user a domain admin.
  <li>
    Right click the user and go to "Properties"
    <li> CLick the "Member Of" tab 
      <li>Select "Add"
  </li>
  <img src="https://user-images.githubusercontent.com/142059616/264812861-294eb764-4f64-45eb-a171-dfe02ff7a7a8.png">
</p>
<p>Type in "Domain Admins", then select "ok"
<img src="https://user-images.githubusercontent.com/142059616/264813130-a4d44d5d-cce3-4d0b-b25c-6c0e883bd955.png">
<i>We have now created an admin user. Now we will log out of DC-1 and log in as the admin, which in this case is "abel_admin"</i>
</p>
<br>
<h3>Join Client-1 to the Domain</h3>
<h4>Set Client-1's DNS settings to the Domain Controllers Private IP address</h4>
<p>Go to Client-1's Azure portal @portal.azure.com, select the "Networking tab and click on the network interface-"client-1492_z1"</p>
<img src="https://user-images.githubusercontent.com/142059616/264890026-c6154d9d-10fc-4d6b-a3c9-f11ce5e59412.png">
<p>Go to "DNS servers", type in the DC-1 private ip address, which is 10.0.0.4
<img src="https://user-images.githubusercontent.com/142059616/264909532-63189f1b-45de-459f-bfe5-ea97479005b5.png">
</p>
<p><li>Right-click the ⊞ Win key and select "systems"
  <li>Then select "Rename this PC"</li>
  <li>The finally select "Change"</li>
<li>
  <img src="https://user-images.githubusercontent.com/142059616/264929013-991a813a-8362-4051-9bd7-f5de6ea86711.png">
</p>
<p>
  Click the Domain checkbox  and enter the root domain name of the domain controller, which is "mydomain.com"
  <img src="https://user-images.githubusercontent.com/142059616/264930188-9664a18b-2d72-482c-83bf-3193d131e05e.png">
</p>
<p>You will then be prommpted to enter the name and password of the admin account we created earlier, which is "abel_admin"</p>
<p><i>You caan  now log into your admin account from Client-1 and are connected to Active directory</i></p>
<h4>Allow "Domain Users" access to remote desktop</h4>
<p><li>Log into Client-1 as admin(ex.abel_admin)
<li>Right-click the ⊞ Win key and select "Systems"
<li>Select "Remote Desktop"
<li>Click "Select Users That Can Remotely Access Desktop Remotely"
<li>Click "add"
<li>Type in "Domain Users"
<li>Click "OK" Twice
<img src="https://user-images.githubusercontent.com/142059616/264937012-73d37857-20d9-459d-bcbf-186bfddfad7c.png">
<p><img src="https://user-images.githubusercontent.com/142059616/264937708-ad3a2592-64bf-47f4-a38b-edd7abd6bdc1.png">
<p><i>You can now log into Client-1 as a normal, non-administrative user now, and are now a domain user.</i></p>
  </p>
<h4>Create a bunch of additional users and attempt to log into client-1 with one of the users</h4>
<p>
  <li>Go to DC-1 Virtual Machine
  <li>'
  Open Powershell and run as <i>administrator</i>
    <li>
      paste the contents of the <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1"> script</a> into it
      <img src="https://user-images.githubusercontent.com/142059616/264946294-e53f5478-2b75-4bb0-927e-d9f4547e96ff.png">
   <p> Run the script</p>
  <img src="https://user-images.githubusercontent.com/142059616/264949498-60f9b756-ef7d-421e-8ca8-c330c64a2ed4.png">
</p>
<p>
  The users have been created!
  <img src="https://user-images.githubusercontent.com/142059616/264950444-07661d03-1c34-4f60-abc9-70651c58498f.png">
</p>
<h5>Now we will attempt to log in with one of the other accounts in the Client-1 VM</h5>
<img src="https://user-images.githubusercontent.com/142059616/264951121-2974209e-3d15-4209-ac54-2a894e056552.png">
<h5>SUCCESSFUL!</h5>
<img src="https://user-images.githubusercontent.com/142059616/264951442-d01ddf4a-add0-48c5-a88f-c75cbab1e949.png">
<br>
<h4>Resetting Passwords and Unlocking Accounts</h4>
<p>Go to the "_EMPLOYEES" tab in "Active Directory Users & Computers
<li>right click a user</li>
  <li>go to "Reset Password"</li>
  <img src="https://user-images.githubusercontent.com/142059616/264955764-1f5ea162-8782-41cf-8f85-76e780a0898a.png">
  Enter new password
  <img src="https://user-images.githubusercontent.com/142059616/264956039-46035929-1619-45e2-84fe-693a99a6399f.png">
  <h5>To unlock account, right-click, select properties-->"Account-->check "unlock account"  </h5>
  <img src="https://user-images.githubusercontent.com/142059616/264956736-dc5ca3e2-6af5-4402-a91a-281015b4d409.png">
</p>
<br>
<hr>
<h2>ALL DONE! GREAT WORK!!😊</h2>
</h2>


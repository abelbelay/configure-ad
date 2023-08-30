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
<h3>Login to the domain controller and enable ICMPv4 in on th local windows firewall to ensure connectivity between Client and Domain Controller</h3>
<p>
Log into both the Domain controller and Client VM's using Remote Desktop Connection

  
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

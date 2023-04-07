# Azure
<h1> SIEM - Azure Sentinel Setup Tutorial</h1>
<p align="center">
<img src="https://i.imgur.com/NRfvbrz.png" alt="Azure Sentinel Logo"/>
</p>

<h1>How to Setup a Basic Home Lab and use Azure Sentinel (SIEM), connect it to a live virtual machine acting as a honey pot. We will observe live attacks from around the world. We will also use a custom PowerShell script to look up the attackers geolocation information and plot it on the Sentinel Map.</h1>

<h2>Environments and Technologies Used</h2>

- Oracle VirtualBox (Virtual Machines/Compute)
- Remote Desktop
- Microsoft Azure
- Azure Sentinel (SIEM)
- Log Analytics Workspace
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2019
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<img src="https://i.imgur.com/wk2mBsL.jpg" alt="Azure Setup"/>
<p>
In this lab we will create a virtual machine in Azure, and create a vulnerable environment by turning the external and Windows firewall off to make it enticing to attackers. Next we are going to create a Log Repository in Azure called a Log Analytics Workspace, which will be used to ingest our logs from the virtual machine. And then we're going to setup Azure Sentinel (SIEM) to create a map that maps all the different attacks from all over the world. To help us locate locate each attack by country and coordniates, we will use a custom PowerShell script to extract the IP Address from the Windows Log and send it to a third party API. And the API will derive the information and send it back to our virtual machine, which will then be used to create a custom log with the attackers location data.
</p>
<br />
  Our first step is creating our Azure account and setting up our virtual machine for our vulnerable environment. We will name our resource group "Honeypot" and virtual machine "Honeypot-vm" respectfully.
  <img src="https://i.imgur.com/7Dtl9jQ.png" alt="Lab Plan"/>
  
  Next we will create our Network Security Group, name it "Honeypot-vm-nsg" and set custom settings to allow our computer to be very discoverable on the internet. Now we can create our virtual machine, this process will take a few minutes to deploy. Next we can create our Log Analytics Workspace, which will be keeping logs of each login attempt for our honeypot.
  
  <img src="https://i.imgur.com/s3cOZM0.png" alt="Network Security Group"/>
  
  Search "log analytics workspace" in the Azure search bar, select our resource group and name it "law-honeypot1," Review and Create. Next we're going to go to the security center to enable the ability to gather logs from the virtual machine to the log analytics workspace. From Security Center select "settings," turn Azure Defender "on" select Data Collection and toggle "all events". From the log analytcs workspace we will connect it to our virtual machine.
  Next we can setup and add Microsoft Sentinel to our new workspace, and this is the SIEM we will be using to visually track and map our attacks by country, longitude, latitude, date & time, and number of attacks. To do so search "Sentinel" and select our "law-honeypot1" resource group that we just created.
  
 <img src="https://i.imgur.com/azR6cxG.png" alt="Adding Sentinel"/>
 
 With Sentinel added to our workspace, we can now open our virtual machine environment. From Azure we can find the public IP address for our VM, from here we will copy it and open Oracle VirtualBox. With our Windows 10 VM open search "Remote Desktop Connection" to create access our Azure VM using the IP address provided.
 
 <img src="https://i.imgur.com/4qCnbuf.png" alt="Remote Desktop Connection"/>
 
 Once we are logged in to our Azure VM, the first thing we want to do is turn off the firewalls. Search "wf.msc" in the search bar and turn off firewalls for both our "domain profile" and "public profile."

<img src="https://i.imgur.com/10msmyU.png" alt="Firewall settings"/>
  

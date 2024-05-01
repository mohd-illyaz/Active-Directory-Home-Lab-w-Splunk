# Active Directory Home Lab w/ Splunk
 
   <p align="center">
Configuration Reference Diagram: <br/>
<img src="https://imgur.com/S9h8OiQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
     
## Objectives
- On-prem lab setting up with virtualization and Implementing various security and event management processes.
- The goal is to have home lab setup configured correctly and then to have our Target-PC to join our newly created domain
- After, using our Kali Linux machine to launch a brute force attack onto our vulnerable machine to generate telemetry in Splunk.
- Finally, setup and install Atomic Red Team and use it to run tests to also, generate telemetry via Splunk.


 
### Tools Used
- Virtualbox
- Ubuntu
- Splunk
- Windows
- Kali-linux
- Atomic Redteam
- MITRE Att&ck Framework


## Walkthrough

First, we will install virtual box.

After, will start by downloading the VM .ISO files for:

- Windows Server 2022- 4gb Ram/ with 2 cores / 50gb
- Windows 10 Pro- 4gb ram/ with 1 core/50gb
- Ubuntu Desktop Server- 8gb Ram/ with 2 cores/100gb
- Kali Linux- 4gb Ram/with 1 core/25gb

   <div>
    We will begin to install them with these minimum requirements and have them set them up by default. We will be documenting our credentials in a text document for demo purposes. Once you finished setting up Ubuntu Desktop we shall begin setting up our Splunk Server.
  </div>
# Configuring our Splunk Sever
  <p align="center">
Let’s run this command to update and upgrade our repositories: <br/>
<img src="https://imgur.com/qQFHpm2.png" height="80%" width="80%" alt=""/>
<br />
<br />
Let’s create a NAT network for all of our VMs to be interconnected with one another. Apply the NAT network we just created to all VMs:  <br/>
<img src="https://imgur.com/2B87FqZ.png" height="80%" width="80%" alt=""/>
<br />
<br />
 <br/>
<img src="https://imgur.com/IsEGDR3.png" height="80%" width="80%" alt=""/>
<br />
<br />
Our IP isn’t the same as our lab diagram. We shall proceed by creating a static IP address of 192.168.10.10/24. Lets run the following command to begin editing our machine’s IP using nano and editing the .YAML file  <br/>
<img src="https://imgur.com/bPFKauc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
With these edits and indentations we have set the static IP. We than proceed to apply the changes using the following command
sudo netplan apply
It will be followed with some warnings but that’s fine. Next,  
<br/>
<img src="https://imgur.com/eHpMMrJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
We just checked that the Ip address truly changed and set itself to 192.168.10.10/24 Success!!  
<br/>

<div>
 - Now we must go on over to our preferred web browser and go to Splunk sign up with an account because we are going to install Splunk Enterprise onto our Splunk Server. Make sure it’s the .deb version of it as shown.
</div>

# Splunk Server Installation and Configuration


1. **Install Splunk Enterprise:** 
   - Download the .deb version of Splunk Enterprise and install it using dpkg.
     <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

2. **VirtualBox Guest Additions:** 
   - Install guest additions for VirtualBox to enhance VM performance.
 <br/>
<img src="https://imgur.com/ntzM8WV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
3. **Create Share Folder:** 
   - Create a 'share' folder for file sharing between the host and VMs.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
4. **Install Splunk:** 
    - Add the Splunk .deb file to the 'share' folder and install it using dpkg.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
5. **User and Group Configuration:** 
    - Add the user 'splunk' to the vboxuser group for proper permissions.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
6. **Splunk Server Startup:** 
    - Start the Splunk server and access it via a web browser. Document the login credentials.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
7. **Rename PC:** 
    - Rename the PC to 'target-PC' as per the lab diagram.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
8. **IP Configuration:** 
    - Change the IP address of 'target-PC' to match the lab diagram.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />
9. **Forwarder Installation:** 
    - Install Splunk Universal Forwarder and Sysmon on 'target-PC'.
  <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />

10. **Receiving Indexer Deployment:** 
    - Deploy the receiving indexer with the specified IP and port number.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />
11. **Sysmon Download:** 
    - Download Sysmon and extract it on 'target-PC'.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />
12. **Sysmon Setup:** 
    - Launch Sysmon using PowerShell and configure inputs.conf for log forwarding.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />
13. **Forwarder Service Restart:** 
    - Restart Splunk's Universal Forwarder service for the changes to take effect.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />
14. **Verification:** 
    - Verify that logs are being ingested from 'target-PC' on the Splunk server.
 <br/>
<img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />
15. **Rename Server:** 
    - Rename the Active Directory Server PC to 'ADDCC01'.

16. **IP Configuration:** 
    - Set the static IP address of 'ADDCC01' as specified in the lab diagram.

17. **Forwarder Installation:** 
    - Download and install Splunk Universal Forwarder on 'ADDCC01'.

18. **Sysmon Installation:** 
    - Install Sysmon on 'ADDCC01' and configure inputs.conf for log forwarding.

19. **Service Restart:** 
    - Restart Splunk's Universal Forwarder service after configuring inputs.conf.

20. **Verification:** 
    - Verify that logs are being ingested from both hosts on the Splunk server.

21. **Conclusion:** 
    - This concludes the configuration of the home lab project. The lab environment has been set up according to the provided diagram, and all necessary configurations have been completed successfully.





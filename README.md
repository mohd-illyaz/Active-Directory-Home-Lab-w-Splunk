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
<img src="https://imgur.com/bPFKauc.png" height="80%" width="80%" alt=""/>
<br />
<br />
With these edits and indentations we have set the static IP. We than proceed to apply the changes using the following command
sudo netplan apply
It will be followed with some warnings but that’s fine. Next,  
<br/>
<img src="https://imgur.com/eHpMMrJ.png" height="80%" width="80%" alt=""/>
<br />
<br />
We just checked that the Ip address truly changed and set itself to 192.168.10.10/24 Success!!  
<br/>

<div>
 - Now we must go on over to our preferred web browser and go to Splunk sign up with an account because we are going to install Splunk Enterprise onto our Splunk Server. Make sure it’s the .deb version of it as shown.
</div>



 **Install Splunk Enterprise:** 
   - Download the .deb version of Splunk Enterprise and install it using dpkg.
 <br/>
   <img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />

<div>
  - We are going to go back to our CLI and add our guest add ons for virtual box.
  - Say ‘Yes’ to installing it. After it’s finished installing click enter
  - As we attempted to add a user to the vboxuser group. We were hit with an error. We were missing an additional feature

use the command-sudo apt-get install virtualbox-guest-utils

If we attempt it again we shall be able to add a user ‘splunk’ to the vboxuser group.
</div>
     <br/>
<img src="https://i.imgur.com/xFcEJ18.png" height="80%" width="80%" alt=""/>
<br />
<br />
     <br/>
<img src="https://i.imgur.com/LuV2NWt.png" height="80%" width="80%" alt=""/>
<br />
<br />

<div>
Next, we are going to proceed into installing Splunk Enterprise onto our Splunk Server by adding a share folder onto our VM.
I created a folder where I kept the .deb file of Splunk Enterprise and all files related to the project called ‘AD Project’. Next, let’s reboot our machine by,

sudo reboot
</div>
<br/>
<img src="https://i.imgur.com/fxxW3yR.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Now We will want to create a directory called ‘share’.
</div>
<br/>
<img src="https://i.imgur.com/FOC1RDu.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
We will want to run the following command as shown by adding our ‘AD_Project’ folder in our host system onto the share folder that we just created. We will follow up with that by changing directory into our newly created share folder. Next, let’s run the following command 
sudo dpkg -i splunk-9.2.0.1-d8ae995bf219-linux-2.6-amd64.deb
</div>
<br/>
<img src="https://i.imgur.com/PP93a8O.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Now, we opted out into the cd/opt/splunk directory. We proceed to look into the directory and see the permissions are given to the user splunk. We will now want to act as the user splunk by the command, sudo -u splunk bash
</div>
<br/>
<img src="https://i.imgur.com/6FfTtFF.png" height="80%" width="80%" alt=""/>
<br />
<br />

<div>
Now we will want to change into the bin directory as that folder contains all the binaries that Splunk can use.

./splunk start

We will now want to launch our Splunk server! We will want to then put in our credentials and document them into our text file for future reference.
</div>
<br/>
<img src="https://i.imgur.com/wHkis1L.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Let’s exit out of being the user ‘splunk’. Change into the bin directory again. We then shall run the following command shown above to make sure every time we boot up Ubuntu server it automatically boots up the Splunk server.
</div>









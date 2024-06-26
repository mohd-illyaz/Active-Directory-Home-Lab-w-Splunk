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



## Install Splunk Enterprise:
   Download the .deb version of Splunk Enterprise and install it using dpkg.
 <br/>
   <img src="https://imgur.com/ivfXLaw.png" height="80%" width="80%" alt=""/>
<br />
<br />

<div>
  We are going to go back to our CLI and add our guest add ons for virtual box.
  Say ‘Yes’ to installing it. After it’s finished installing click enter
  As we attempted to add a user to the vboxuser group. We were hit with an error. We were missing an additional feature

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

We will want to run the following command as shown by adding our ‘AD_Project’ folder in our host system onto the share folder that we just created. We will follow up with that by changing directory into our newly created share folder. Next, let’s run the following command 
  ```bash
sudo dpkg -i splunk-9.2.0.1-d8ae995bf219-linux-2.6-amd64.deb
```
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

## Configuring Target-PC w/Sysmon & Splunk Universal Forwarder
<div>
We will go ahead by starting off by renaming our PC to ‘target-PC’.
Search ‘PC’ in search bar>properties>rename this PC>restart PC
Expectedly should have changed to our desired name target-PC.
We see our current IP is 192.168.10.8 we want to change it to what our lab diagram mentions.
Open Network & Internet Settings>Change Adapter Options>Right click on properties>double click on ipv4
Change the following IP address as shown in fig. We have /24 subnet with the DNS server being pointed towards Google’s.
</div>
<br/>
<img src="https://i.imgur.com/OHUlEKv.png" height="80%" width="80%" alt=""/>
<br />
<br />
<br/>
<img src="https://i.imgur.com/LgDRvKD.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Let’s go onto our browser and start a private window and let’s pull up our Splunk server and head over to splunk.com to download & install Splunk Universal Forwarder.
</div>
<br/>
<img src="https://i.imgur.com/wqplaa7.png" height="80%" width="80%" alt=""/>
<br />
<br />
<br/>
<img src="https://i.imgur.com/G9xKDbs.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Once it finishes downloading, we will want to open the file and check the box to agree to terms and click next.
We then next create our credentials using the default username ‘admin’ & by checking the random password box and click next
And Let’s deploy our receiving indexer with the following IP and the default port number 9997. Afterwards click install and wait for it to install.
</div>
<br/>
<img src=".mhttps://i.imgur.com/CwCK8ES.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
We will then proceed to downloading Sysmon.Save the .xml file on the VM.
Next, we will want to extract the whole file onto our downloads directory.
</div>
<br/>
<img src="https://i.imgur.com/fkguLya.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Now we will want to copy the path of the extracted Sysmon file. This will bring us to our next step by opening up PowerShell by running it as an admin.
We will used the path we previously copied that leads to Sysmon files. Use the following command to launch Sysmon.
<br/>
<img src="https://i.imgur.com/BrXrFDX.png" height="80%" width="80%" alt=""/>
<br />
<br />
<br/>
<img src="https://i.imgur.com/SA7JitY.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
 Next, follow the following path above to locate the ‘inputs.conf’ directory to then proceed with creating a copy in the local directory instead. Our default directory is just a backup we shouldn’t mess with.
 Search up notepad in the search bar and run it as an administrator. We will copy and paste the contents of inputs.conf file into a .txt file.
 We will be sending over the Windows Event Logs pertaining Application, Security, System, and Sysmon operations over to our Splunk Server.
 Next, we will save our notepad .txt file over to our PC via the same path from earlier as shown below.
 Rename the file to inputs.conf and change the file type to all types and save. NOTE: Anytime we update our inputs.conf file we must restart Splunk’s Universal Forwarder service.
 Search in search bar “Services”>Run as admin>SplunkFowarder>Double Click It> Log On>Click Local System Account>Apply>OK>Restart Service
 We will get an error pop up but that’s okay. Just start the service again.
 Look for the steps in the following images:
</div>
<br/>
<img src="https://i.imgur.com/CFWymSe.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/V8qYsvY.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/s9LTO79.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Now we will want to head over to our Splunk instance again and do the last configurations to our Splunk server by hovering over to settings>Indexes.
Here we can see all of the indexes that Splunk has to offer. Scroll to the top right corner and click on “New Index”.
Name our new index “Endpoint” and click ‘Save’.
Next, we need to enable our Splunk server to receive the data.
then click on Settings>Forwarding and receiving
Click on configure receiving & on the top right “New Receiving Port”. Next, the default port will be 9997 like earlier when setting up our forwarder.
After clicking Search & Reporting. We will want to search ‘index=”endpoint”. We should be getting thousands of events for that filter.
</div>
<br/>
<img src="https://i.imgur.com/YcykC3w.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/bTbdYBC.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/KbbNiY8.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/a9uwDda.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
We have successfully configured Splunk. Remember the inputs.conf file from earlier which had the four windows event log types we wanted to ingest well here they are!
</div>

## Configure Active Directory Server w/Splunk Universal Forwarder & Sysmon
<div>
First, we will want to rename our Active Directory Server PC to ‘ADDCC01’.
Search bar search “PC”>Click on Properties>Rename this PC>’ADDCC01′ We shall proceed to restart the PC to apply the changes now we may proceed to installing Splunk & Sysmon onto our AD Server.
It will be the same process like our Target machine the only difference is that the Server Static IP will be different. We will want to change our static IP to 192.168.10.7
Our current IP is 192.168.10.9
Again Open Network & Internet Settings>Change Adapter Options>Right click on properties>double click on ipv4
</div>
<br/>
<img src="https://i.imgur.com/1Rr235k.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Now that we have set our proper network connectivity. It is time to install Splunk Universal Forwarder and Sysmon onto our Active Directory Server.
We will want to go over to splunk.com and login in with our credentials. Go over to free trials & downloads and download Splunk Universal Forwarder for Windows 64 bit.
Once the download is complete let’s go over to the file and open the Splunk file to install the Universal forward onto our server.
Check the box to agree to terms with the default on premises Splunk Enterprise instance checked off as well. You may go ahead and click next.
Let’s create a ‘admin’ username. While, also checking for “Generate Random Password.”
We may also skip the deployment server by clicking next.
We will want to send all of our logs over to Splunk via the Receiving Indexer specifying our Splunk server IP and default port number 9997 shown, click next and Install
</div>
<br/>
<img src="https://i.imgur.com/3DUIJMi.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Now we will go and install Sysmon. Sysmon will be installed
Next we will save the inputs.conf file from the shown github repo. It’s already in raw file all we got to do is right click and save as ‘inputs.conf.’ Let’s put it into our Downloads folder and extract all of it there as well.
<br/>
<img src="https://i.imgur.com/sYaDeeX.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/wcXSNtV.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Let’s copy the file path and head on over to Powershell and run it as an administrator.
</div>
<br/>
<img src="https://i.imgur.com/6i2TchS.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Now we will change directories into the file path where our Sysmon folder was extracted. Followed with the following command as shown to run Sysmon in conjunction with sysmonconfig.xml file.
Next, click agree to begin running Sysmon on our Active Directory server.
Now, here’s a very important step we must instruct our Splunk forwarder on what to send over to our Splunk server.
</div>
<br/>
<img src="https://i.imgur.com/QbSQgKX.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
We will want to follow the file path shown above to locate the inputs.conf file in the default folder. We wouldn’t to edit in there because the default folder is considered a backup in case anything goes wrong.
We will want to put the inputs.conf file into the local directory instead. We will need administrative privileges to do so.
Open up Notepad and run it as an administrator. Copy the below Github repo’s .xml file code and paste it into notepad. It should look like this down below
And save it into the local directory.
</div>
<br/>
<img src="https://i.imgur.com/hi32ZGL.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/daKo0Bh.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Again this inputs.conf file is instructing our Splunk forwarder to push events corresponding with Application, Security, System, and Sysmon operational to our Splunk server. The index is also being pointed towards “Endpoint” it will not receive the events under the Index “Endpoint” since we had already done so earlier we don’t need to again.
NOTE: Due to updating our inputs.conf file we need to restart Splunk’s Universal Forwarder Service in the “Services” settings.
Search in search bar “Services”>Run as Administrator> Splunk Forwarder (Type S)>Click on Restart Service
And When you double click Splunk Forwarder and you see a NTSERVICE account it might inhibit log ingestions because of some of the permissions so instead we will want to log on via the local account instead by checking it instead.
Next, Apply the changes and click OK. It will prompt you to restart the service to apply the changes.
</div>
<br/>
<img src="https://i.imgur.com/MlrQpmc.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Let’s head over to our Splunk server to see if we began ingesting logs from both hosts. Remember, we had configured our Server earlier so we do not need to do it again.
</div>
<br/>
<img src="https://i.imgur.com/b9s8K3T.png" height="80%" width="80%" alt=""/>
<br />
<br />
<div>
Success! We have added both our hosts to our Splunk server and should be ready now to begin receiving logs from both hosts as we please. This concludes our configuration portion of the project.
</div>

# Configuring AD Server to Join Target Machine to Domain Controller

Now we focus on our Target-PC to join our newly created domain!
then, we shall start by setting a static IP of 192.168.10.7 on our AD server. We reference this by looking our diagram we created.
Click Change Adapter Options> Right click on ethernet>Properties>IPv4
It should be changed to these configurations listed belowto create our static IP and go ahead and click OK

<br/>
<img src="https://i.imgur.com/LcMz9NT.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/u3Rmp6w.png" height="80%" width="80%" alt=""/>
<br />
<br />

Let’s hover over to our Server Manager Dashboard and click on manage for options to show up and click on “Add Roles and Features”. Keep clicking Next until you get to Server Roles.
Click on “Active Directory Domain Services”. Then click on Add Features. Keep clicking “Next” till we get to the Results tab and click “Install”. It will prompt itself complete when done.

<br/>
<img src="https://i.imgur.com/d6cNx54.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/r7ySs0J.png" height="80%" width="80%" alt=""/>
<br />
<br />

Back to our Server Manager. We shall hover over this flag icon with a caution right over it. Click on Promote this Server to Domain Controller.
Next, let’s click on create new forest.
In my case I will be calling my Root domain name: eddy.local
Click Next
Leave Domain Options default but set a Domain Controller password and document it with the rest of our credentials. Click Next.
Click Next when you get to the DNS options tab as well.

<br/>
<img src="https://i.imgur.com/EGkmKXp.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/T1dJlyy.png" height="80%" width="80%" alt=""/>
<br />
<br />

When we get to the Additional Options we are able to configure our NetBIOS domain name. Leave as is.
Keep clicking next till we get to the ‘Prerequisites Check’ tab let it do it’s check and click next afterwards. 
Proceed, to installing ADDS. It will then prompt us to sign out and automatically restart our AD server. Let’s log back in and begin to add users using our Domain Controller credentials.

<br/>
<img src="https://i.imgur.com/hUbzfAE.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/MqPA0hm.png" height="80%" width="80%" alt=""/>
<br />
<br />

As soon as we’re given access let’s hover over to Tools as shown above.
Let’s recreate a Enterprise environment and add users into a real world department by heading over to the local domain and creating it as shown above. Next, let’s create a department called “IT”. Right afterwards another called “HR”.

<br/>
<img src="https://i.imgur.com/GdH1HlZ.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/IiUyXx1.png" height="80%" width="80%" alt=""/>
<br />
<br />

Next, we want to create new users for these groups.
Jenny Smith at IT hypothetically speaking and Terry Smith at HR will be created for demo purposes.
The configurations needed are shown below. Make sure to document how they are being named for later when we need to login

<br/>
<img src="https://i.imgur.com/LhcpifC.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/YLk0hOB.png" height="80%" width="80%" alt=""/>
<br />
<br />

Let’s head over to our Target machine and join it to our newly created domain eddy.local and join it.
Next, Lets search up in search ‘PC’>Click on Properties>Advanced System Settings>Computer Name>Change>Domain>EDDY.LOCAL

<br/>
<img src="https://i.imgur.com/rEQ8cam.png" height="80%" width="80%" alt=""/>
<br />
<br />

If you get this error it might be a problem with our DNS servers. Let’s go back to the way we configured our Static IP and click and configure our IPv4 settings
We shall point our DNS server to our AD server instead to join our domains together by adding 192.168.10.7 to our Target-PC IPV4 properties. Let’s check everything is running smooth by opening up the command prompt.

<br/>
<img src="https://i.imgur.com/lkg3UKq.png" height="80%" width="80%" alt=""/>
<img src="https://i.imgur.com/snmUbAk.png" height="80%" width="80%" alt=""/>
<br />
<br />

Now, we should have the DNS server properly configured upon checking.
Now that is done let’s hop on over to joining our Target-PC to the domain controller. We will be using our administrator account. We will be using administrator as the username. Input the password we had saved in our credentials document.

<br/>
<img src="https://i.imgur.com/2DZ8oEw.png" height="80%" width="80%" alt=""/>
<br />
<br />

If everything was done correctly it should look like this! Restart the computer to apply changes. Once we are back in we will want to login as our newly created user Jenny Smith. We should have their credentials saved.
Once we are in, we have successfully configured our AD server and created two users by also joining our machine to our newly created domain. We now have the environment to play around with Active Directory.

# Generating Attack Telemetry to Splunk Server w/ Atomic Red Team tests based off Mitre Att&ck Framework

Now we use our Kali Linux machine to launch a brute force attack onto our vulnerable machine to generate telemetry in Splunk. After, setup and install Atomic Red Team and use it to run tests to also, generate telemetry via Splunk.
Note: Now would be a good time to shut down both Target-Pc and ACCDC01 downgrade machines to 2gb ram if we are running on 16gb ram to be able to run all the necessary VMs at once.
We will want to turn on our Kali machine and set a static IP of 192.168.10.250 per our diagram in the first part of our project.
Right click on the top right where the connections icon is at and click on ‘Edit Connections’.
We will want to select the first profile which is ‘Wired Connection 1’.
We will want to click on the section of of the settings like before on our Windows machine on IPv4 settings.

<br/>
<img src="https://imgur.com/KMTDoMa.png" height="80%" width="80%" alt=""/>
<img src="https://imgur.com/9MBtNOU.png" height="80%" width="80%" alt=""/>
<br />
<br />

We will run the following commands to simultaneously update and upgrade our machines repositories and saying yes to all prompts.

sudo apt-get update && sudo apt-get upgrade -y

It shall take a while dependent on connection speed. Once, it’s done we can begin launching our attack by creating a new directory.

mkdir ad-project

All the files we create and use will be put under this directory for organizational purposes. The tool we will be using next is crowbar.

sudo apt-get install -y crowbar

Now that we have it installed we will want to install a popular word list called ‘rockyou.’ If we change our directory:

cd /usr/share/wordlists/

Afterwards lets see what files are in the wordlists directory by running;

ls

Afterwards we will want to unzip the file ‘rockyou.txt.gz’ via:

sudo gunzip rockyou.txt.gz

If we run the ls command again in the ‘wordlists’ directory we shall see that the ‘rockyou.txt.gz’ file was unzipped to ‘rockyou.txt’.

Next, we will want to copy the rockyou.txt file onto our ad-project directory

cp rockyou.txt ~/Desktop/ad-project/

Click enter and should have copied it we will want to change into our ‘ad-project directory.

cd ~/Desktop/ad-project

We shall then proceed to see what’s inside the directory by:

ls -lh

<br/>
<img src="https://imgur.com/1LsXJ98.png" height="80%" width="80%" alt=""/>
<br />
<br />

We don’t want to use all the passwords in our rockyou.txt file as it would take too long and for demo purposes we will want to create our own password list for crowbar to use instead. Instead, we will use the first 20 lines by running the following command:

head -n 20 rockyou.txt

If we hit enter we shall see the first 20 lines

<br/>
<img src= "https://imgur.com/zKaZUd5.png" height="80%" width="80%" alt=""/>
<br />
<br />

As we can see these are the first 20 most command end users use to sign in.
Next, we are going to output the first 20 commands into a file called passwords.txt via the following command:

head -n 20 rockyou.txt > passwords.txt

We can open it up by concatenate the file by running the following command:

cat passwords.txt

<br/>
<img src= "https://imgur.com/sXfxJ8r.png" height="80%" width="80%" alt=""/>
<br />
<br />

Now we will have it target a specific password by using the text editor nano using the following command:

nano password.txt

We shall scroll down and adding our TARGET-PC password and save it.
If we cat passwords.txt again we should be able to see the newly added password in the file and we should be good to go!

<br/>
<img src= "https://imgur.com/iAH6yOX.png" height="80%" width="80%" alt=""/>
<br />
<br />

Next, we will want to enable RDP in our TARGET-PC machine.

On our windows TARGET machine we will begin to enable RDP by:

Search in search bar “PC”> Click Properties>Advanced system settings> Login via administrator credentials
Click on top right on Remote and click on “Allow remote connections to this computer”. Afterwards, click on “Select Users”

<br/>
<img src= "https://imgur.com/MM22XwT.png" height="80%" width="80%" alt=""/>
<br />
<br />

Note: Have our Windows Server 22 running. So our users and domain may appear.

We will want to add our users Jenny and Terry smith by typing ‘jsmith’ & ‘tsmith’ and clicking check names it should automatically add the rest of their info if it was indeed setup correctly. It should look like how it’s shown below. Next, click OK. Click on ‘Apply’. Then click OK to close it out and we have now enabled RDP.

<br/>
<img src= "https://imgur.com/oJK5Mxf.png" height="80%" width="80%" alt=""/>
<br />
<br />

Lets go back to our Linux machine to begin launching our brute force attack using the crowbar tool. We may use crowbar -h to help explain the command abbreviations. Next, run the following command:

crowbar -b rdp -u tsmith -C passwords.txt -s 192.168.10.100/32

-b ; target service to attack

-u; static username to target

-C; the specific password file wordlist to use

-s; specific server to target

/32 to only target that specific IP only since it’s in the /32 subnet.

It should have successfully been able to brute force as shown below

<br/>
<img src= "https://imgur.com/FzsIAmj.png" height="80%" width="80%" alt=""/>
<br />
<br />

We now have the necessary telemetry to go over to Splunk and inspect the data generated.
We will be using our Splunk server IP followed by the default port 8000 to login in via our credentials we should have saved. After, we have logged in successfully let’s click on ‘Search & Reporting’. In the search bar type:

index=endpoint tsmith

If hover over to ‘EventCode’ and see the EventIDs generated we shall see 4 created. If we were to search up what EventID 4625 means; An account failed to logon. Which means there were 20 attempts to logon. If you remember it was from our crowbar tool attempting the 20 passwords before nailing our actual password at the end! We can specify even further;

index=endpoint tsmith EventCode=4625

Which will specify the search result to only EventID 4625 which will only show the 20 events.

<br/>
<img src="https://imgur.com/cfHM0Pd.png" height="80%" width="80%" alt=""/>
<br />
<br />

If we look at the time we can see that it’s happening in a matter of seconds. A clear indicator of a brute force attack.

Now on to the next peocess;
Let’s go on now to installing Atomic Red Team to begin testing out different telemetry for our Splunk server. Open powershell using admin privileges.

Set-ExecutionPolicy Bypass CurrentUser

Type ‘y’ right after.

<br/>
<img src="https://imgur.com/Anc7fCN.png" height="80%" width="80%" alt=""/>
<br />
<br />

We shall click on the up arrow and click on Windows Security.
Click on ‘Virus & threat protection’> Manage Settings> Scroll Down & Click ‘Add or remove exclusion’> Yes> Add an exclusion> Folder> C:\ Drive

<br/>
<img src="https://imgur.com/yL5xYXE.png" height="80%" width="80%" alt=""/>
<br />
<br />

It should look as shown above. Now we will be allowed to install Atomic Red Team! The following github link will allow you to copy and paste the appropriate Powershell command. Scroll down until you see ‘Install Execution Framework and Atomics Folder’ copy the following command:

IEX (IWR ‘https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1’ -UseBasicParsing);
Install-AtomicRedTeam -getAtomics

Type ‘y’ when prompted to continue installation.

Once, finished installing let’s head over to our C:\ drive and head over to our AtomicsRedTeam folder>atomics
We should see a ton of TechniqueIDs as shown below. Which reference the MITRE Att&ck Framework.
<br/>
<img src="https://imgur.com/OzL67of.png" height="80%" width="80%" alt=""/>
<br />
<br />

We can hover right under Persistence and hover right under “Create Account”. It’s labelled as T1136. If we go back to our Atomics directory and scroll looking for it we shall find it!
<br/>
<img src="https://imgur.com/1DPGCWp.png" height="80%" width="80%" alt=""/>
<br />
<br />

We highlighted there’s three different types of techniques that are eligible to be tested. If we click on our Mitre Att&ck Framework technique we shall one is a local account attack, second being a domain attack, and last being a cloud attack technique. We shall test out the local account attack technique first by running the following command in Powershell:

Invoke-AtomicTest T1136.001

<br/>
<img src="https://imgur.com/kYFsVm0.png" height="80%" width="80%" alt=""/>
<br />
<br />

We shall see that our test technique created telemetry with the user ‘NewLocalUser’ being used. 
We can now go into Splunk and search specifically for the user ‘NewLocalUser’ even generated.
<br/>
<img src="https://imgur.com/5ycBmuB.png" height="80%" width="80%" alt=""/>
<img src="https://imgur.com/kAtwCmH.png" height="80%" width="80%" alt=""/>
<br />
<br />

Now one more;
We will hover now right under the ‘Execution’ section and look for the attack techniqueID T1059.
We will hover now right under the ‘Execution’ section and look for “Command and Scripting Interpreter” the attack techniqueID T1059.
If we go into our Atomics directory and search for it we should we have 9 techniques to choose from.
<br/>
<img src="https://imgur.com/lCMC8G0.png" height="80%" width="80%" alt=""/>
<img src="https://imgur.com/1R9Xt5X.png" height="80%" width="80%" alt=""/>
<br />
<br />

If we expand it we can see that there is quite of few script languages we can choose from to test.
We are going to choose Powershell since we’re already there. Let’s open it back up and run the following command:

Invoke-AtomicTest T1059.001

<br/>
<img src="https://imgur.com/86dmI3W.png" height="80%" width="80%" alt=""/>
<img src="https://imgur.com/4Y8lxjy.png" height="80%" width="80%" alt=""/>
<br />
<br />

Here is something specific we may search in Splunk by searching up ‘powershell’ and see what kind of telemetry it generates and filtering the time to the ‘Last 15 minutes’. 
In my case I let it do it’s thing for a while and had only 1 event within the last 60 minutes.
It was the -exec bypass I was looking for so, I queried for a more specific search

<br/>
<img src="https://imgur.com/SgYBh82.png" height="80%" width="80%" alt=""/>
<img src="https://imgur.com/8yUMrqm.png" height="80%" width="80%" alt=""/>
<br />
<br />

Now I am able to see the bypass no profile script being implemented now using the query above.
We can create an alert to detect for this activity in the future since we were having issues detecting it at first.

## This is one reason why using the Mitre Att&ck Framework and as many resource as possible to test your environment’s security posture to increase the defense. Being able to detect as many positive alerts in your environment is key to threat prevention! Real learning is done based off of breaking things!!!














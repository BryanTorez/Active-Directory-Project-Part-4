<h1>SOC Automation (Home Lab) - Part 4</h1>

<p align="center">
<img src="https://snipboard.io/VLbBIk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to part four of five for the series on the active directory project. If you haven't seen the previous parts where we go over on how to build out a diagram for this lab, install our virtual machines, and configure both Sysmon and Splunk. I highly recommend you go and watch that first.
<br />
<br />
<img src="https://snipboard.io/hLEopa.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Today's objective is to install and configure active directory onto our server. We will then promote it to a domain controller and finally configure the target machines to join our newly created domain. Let's get started.
<br />
<br />
<img src="https://snipboard.io/9BWA3v.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
On our server, we want to set a static IP address of '192.168.10.7' and we can do this by right clicking the network icon on the bottom right. Then select "Open Network and Internet Settings". Click on "Change adapter options". Right-click the interface and click on "Properties".
<br />
<br />
<img src="https://snipboard.io/s3PXtA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/c2QvNb.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ld5One.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/6fjcim.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Double-click "Internet Protocol version 4". Select "Use the following IP address:" and here we will type in '192.168.10.7' subnet mask of '255.255.255.0'. The default gateway is '192.168.10.1' and the DNS is going to be '8.8.8.8', which is again Google's DNS.
<br />
<br />
<img src="https://snipboard.io/6UpNMJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/rHZkSD.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now let's open up command prompt by searching for "cmd" and click on command prompt. We'll type in "ipconfig" and our IP address should be '192.168.10.7'. So let's ping google.com and get some connectivity.
<br />
<br />
<img src="https://snipboard.io/BlG1nP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Qeg6IN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Xqg7da.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/wPKd3L.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, let's ping our Splunk server on '10.10' so we can get connectivity. Now, just as an FYI, you can't ping between Windows computers because by default it's blocking ICMP, which are pings. If you want to enable that you need to create an inbound rule to allow ICMP traffic, but in this case since we can ping our Splunk server, we can assume that there is connectivity between the computers.
<br />
<br />
<img src="https://snipboard.io/Do80Zp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/WAeRM2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now that we verified connectivity, let's head over to Server Manager, which is this icon right here. On the top right corner, we want to click "Manage". Now, select "Add Roles and Features". Make sure it is selected as "Role-based or feature-based installation".
<br />
<br />
<img src="https://snipboard.io/8FeGcE.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/bKkr2q.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/aBDgVv.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/csqnyA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If you have multiple servers, this is where it'll be listed, but because we only have one, this is it. So we'll just select "Next" and we want to select "Active Directory Domain Services". Then, click on "Add Features". I'll keep on clicking "Next" until we start installing.
<br />
<br />
<img src="https://snipboard.io/2EatdX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/IZb7Ek.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/uBcXmt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/p9n8bV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once we install it, it will take some time for it to install "Active Directory Domain Services", but after a couple minutes you want to take a look at the progress bar and if it says "installation succeeded", that is when you know that it installed our active directory.
<br />
<br />
<img src="https://snipboard.io/YkBCFV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So after a couple minutes, we get the "configuration required. Installation succeeded on ADDC01." So we can go ahead and close this out. See the flag icon beside "Manage", you want to click on that.
<br />
<br />
<img src="https://snipboard.io/UTN3MG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Xvy1Ao.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
You'll see "Promote This Server to a domain controller". Go ahead and click on that. Now, for this option we want to select "Add a new forest" because we are creating a brand new domain.
<br />
<br />
<img src="https://snipboard.io/ZYOEcu.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/T6Bqrv.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
As for the domain name, I'll call it "mydfir.local". The domain name must have a top-level domain, so in other words my domain name cannot simply be "mydfir". It must be "mydfir.something".
<br />
<br />
<img src="https://snipboard.io/dKcwEz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll hit "Next". I'll leave everything default. Put in a password and click "Next" until you see "Paths". These will be the paths used to store our database file named "NTDS.dit".
<br />
<br />
<img src="https://snipboard.io/gFyrKL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/eOjtC9.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
As an FYI, attackers love to target domain controllers. Not only because it has access to everything, but also because of this file. As it contains everything related to active directory including passwords and hashes. If you notice any unauthorized activity towards this file, you can assume that your entire domain is unfortunately compromised. Click on "Next". Once the setup finishes verifying prerequisites, we can go ahead and click on "Install"
<br />
<br />
<img src="https://snipboard.io/0Sw3As.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/9KRDFj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once the setup is completed, the server will automatically restart. Your gonna see that you're about to be signed out and that is absolutely okay. We can go ahead and click on "Close" and our server should automatically restart.
<br />
<br />
<img src="https://snipboard.io/7ITuC6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/zGqCVD.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
All right, let's log back into our server and you will see our domain followed by a backslash which indicates that we have successfully installed "ADDS", and promoted our server to a domain controller.
<br />
<br />
<img src="https://snipboard.io/N6dHCL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
The next step is to start creating some users, so let's log in and do just that. On our server manager, we want to click on "Tools" at the top right corner and we want to select "Active Directory Users and Computers".
<br />
<br />
<img src="https://snipboard.io/F1amHx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
This is where we can create objects such as users, computers, groups, and many more. Let's go ahead and expand our domain and we see a couple of folders called "Builtin", "Computers", "Domain Controllers", and many more.
<br />
<br />
<img src="https://snipboard.io/g7BkTr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/3KuQ1c.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if we click into "Builtin", we see all of the groups listed on the right hand side. These groups have been automatically created by the active directory and we can double-click any of these and read the description.
<br />
<br />
<img src="https://snipboard.io/1ueoVN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
For example, I'll double click "Administrators" and the description here is "Administrators have complete and unrestricted access". Under the tab "Members", we will see who is assigned to this group. Under the tab "Member Of", we will see what other groups this group is in.
<br />
<br />
<img src="https://snipboard.io/li0b6p.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/aXPf91.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/vaqIkV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
As an FYI, you cannot add additional groups within a built-in group but you can create a custom group and add that built-in group to the custom group. So for example, I can't add anything here but if I right-click "New".
<br />
<br />
<img src="https://snipboard.io/gOB10S.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/EXMxhr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Then click "Group" and type in "test". Hit "OK". Double-click the test and click on "Member Of". I can type in any group I want. For example, type "Administrators" and then click "Check Names" and hit "OK".
<br />
<br />
<img src="https://snipboard.io/Nycrjh.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/kPzahZ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/uPh3e6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So this is the "Builtin" group. If we click on the "User" folder, we will see a list of users along with additional groups.
<br />
<br />
<img src="https://snipboard.io/EIVB2F.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/2XgnAt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if we want. We can create a user within this folder similar to how I did it with the group, but in a real world environment it is likely broken up to different departments, AKA organizational units. For example, HR Finance, IT, Sales, etc.  
<br />
<br />
<img src="https://snipboard.io/d6enao.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
To mimic that, we can right-click the domain go under "New" and select "Organizational unit". We'll name this "IT". I'll click on "OK" and now we have a new organizational unit created.
<br />
<br />
<img src="https://snipboard.io/Oxf7bN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/gWuHVX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/EwSyxC.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Zi6H1U.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, within this unit I'll right-click "New" and I'll create a new user. The first name for my new user will be "Jenny" and the last name will be "Smith". So Jenny's username will be "J Smith". I'll hit "Next".
<br />
<br />
<img src="https://snipboard.io/goqaIA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/UtZK1w.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
As for the password, we can put whatever we want. Now, because we are in a lab environment, we can go ahead and uncheck "User must change password". I'll click on "Next" and "Finish".
<br />
<br />
<img src="https://snipboard.io/c43S8h.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/6W2n09.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/gnlLPe.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, that we have a new user, let's create another organizational unit and this time we'll call it "HR". So I'll right-click the domain. Click "new" and then click "Organizational unit". Type in "HR". Click on "OK"
<br />
<br />
<img src="https://snipboard.io/vhR5OP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/LriZMt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, let's create another user and for this user I will name "Terry Smith" and the username is "T Smith". Click on "Next". For the password, put in a super secure password and I'll uncheck the "user must change password". Click on "Next" and "Finish".
<br />
<br />
<img src="https://snipboard.io/uzT1kX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/gJGmuc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/cfhyS2.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/CyHAMD.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So now we have two users that we just created. There are many scripts out there that can help you auto-create users, groups, and computers but for this project we'll only create two users.
<br />
<br />
<br />
<br />
Now that we have our active directory and our server is a domain controller, we will head over to our Windows target machine. We'll join it to our newly created domain called "mydfir.local" and authenticate using "Jenny Smith" account.
<br />
<br />
<br />
<br />
On our Windows 10 machine we want to search up "PC" and then click on "Properties". Scroll down to select "Advanced system settings". Then, for the tab we want to click on "Computer Name". Select "Change" and make sure you select "Domain".
<br />
<br />
<img src="https://snipboard.io/Abpx4n.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/xs0ZAm.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/YOAb5Q.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/jTQGRc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, we can type in our domain of "mydfir.local". Now, you might get an error saying "An Active Directory Domain Controller for the domain "mydfir.local" could not be contacted". This is because our target machine does not know how to resolve "mydfir.local" and this all goes back to how DNS works.
<br />
<br />
<img src="https://snipboard.io/yxB0SU.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/OT7WrN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now to fix this, we can head over to our network adapter and right-click. Click on "Open Network and Internet settings". Click on "Change adapter options". We want to right-click our adapter and select "Properties". Double-click "Internet Protocol version 4".
<br />
<br />
<img src="https://snipboard.io/LJa5Ww.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/WugQPi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/IuDl4f.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/XZsFEA.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if you take a look at our DNS server, it is pointing to Google's DNS. We want to change this to point to our domain controller. Which is hosted on '192.168.10.7'. Then, I'll hit "OK". Now, just to make sure everything is good to go. We'll type in "CMD" to open up our Command Prompt.
<br />
<br />
<img src="https://snipboard.io/FGcSdp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/SpkCRh.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/VSYnuH.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Then, I'll type in "ipconfig /all". This will show our DNS server. In this case, we can see that it is pointing to our domain controller. So in theory, we should be good to join our domain. So let's go ahead and do that.
<br />
<br />
<img src="https://snipboard.io/y9pfNZ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/HsmBSM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/wzcvdm.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll select "OK" again. Now, we're prompted to enter some credentials. We will use the administrator account of the server to log in, as this account will have the proper permissions. 
<br />
<br />
<img src="https://snipboard.io/qp2nsy.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/3KNFuo.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So I'll type in "administrator" and then the password. In a real-world environment, you would create users and put them into a custom group that are authorized to allow computers to join the domain. Now we see a window popup saying "Welcome to the mydfir.local domain."
<br />
<br />
<img src="https://snipboard.io/voaO0E.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/O8vrm4.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
It'll say you must restart your computer to apply these changes. So let's go ahead and close it out. You'll be prompted to restart now or later, let's do it now. Once we're on the log on screen, we want to log in with our newly created user called "Jenny Smith" and Jenny's username is "J Smith".
<br />
<br />
<img src="https://snipboard.io/UdSPYx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ZkbPwM.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So in order to do that, we want to select "Other User" and we want to make sure that our "Sign in to:" is pointing to our domain. We see it as "mydfir" so that's perfect. Well, the username is "J Smith" and we'll enter in our password. So that is how we create a new user, join our computer to a new domain, and log in as a domain user.
<br />
<br />
<img src="https://snipboard.io/VDyR2E.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/oULDIP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Rm57kZ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/7w6fFe.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Congratulations on configuring your own active directory server and creating two users, as well as joining a machine to your newly created domain. The sky is the limit from here on out. If you want to learn more about IT Administration, you now have a fully functional active directory server to play around with. The last thing to do here is to make sure that you snapshot all of your virtual machines. That way, in case you do, break something, which you should never be afraid of, you can always restore to a known good state. In the next part of our series, which will be the last episode, we will use Kali Linux to perform a brute force attack and also set up Atomic Red Team on our Windows target machine to generate some telemetry to view on Splunk. That is it for this part and I hope you are enjoying this project series.

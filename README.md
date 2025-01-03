<h1>SOC Automation (Home Lab) - Part 1</h1>

<p align="center">
<img src="https://snipboard.io/6rb2ue.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to part four of five for the series on the active directory project. If you haven't seen the previous parts where we go over on how to build out a diagram for this lab, install our virtual machines, and configure both Sysmon and Splunk. I highly recommend you go and watch that first.

Today's objective is to install and configure active directory onto our server. We will then promote it to a domain controller and finally configure the target machines to join our newly created domain. Let's get started.

On our server, we want to set a static IP address of '192.168.10.7' and we can do this by right clicking the network icon on the bottom right. Then select "Open Network and Internet Settings". Click on "Change adapter options". Right-click the interface and click on "Properties".

Double-click "Internet Protocol version 4". Select "Use the following IP address:" and here we will type in '192.168.10.7' subnet mask of '255.255.255.0'. The default gateway is '192.168.10.1' and the DNS is going to be '8.8.8.8', which is again Google's DNS.

Click on "OK". Click "OK" to save and now let's open up command prompt by searching for "cmd" and click on command prompt. We'll type in "ipconfig" and our IP address should be '192.168.10.7'. So let's ping google.com and get some connectivity.

Now, let's ping our Splunk server on 10.10 so we can get connectivity. Now, just as an FYI, you can't ping between Windows computers because by default it's blocking ICMP, which is pings. By default, if you want to enable that you need to create an inbound rule to allow ICMP traffic, but in this case since we can ping our Splunk server, we can assume that there is connectivity between the computers.

Now that we verified connectivity, let's head over to Server Manager, which is this icon right here. On the top right corner, we want to click "Manage". Now, select "Add Roles and Features". We'll select "Next" and make sure it is selected as "Role-based or feature-based installation".

If you have multiple servers, this is where it'll be listed, but because we only have one, this is it. So we'll just select "Next" and we want to select "Active Directory Domain Services". Then, click on "Add Features". I'll keep on clicking "Next" until we start installing.

Once we install it, it will take some time for it to install "Active Directory Domain Services", but after a couple minutes you want to take a look at the progress bar and if it says "installation succeeded", that is when you know that it installed our active directory.


So after a couple minutes, we get the "configuration required. Installation succeeded on ADDC01." So we can go ahead and close this out. I'll minimize the command prompt. See the flag icon beside "Manage", you want to click on that.

You'll see "Promote This Server to a domain controller". Go ahead and click on that. Now, for this option we want to select "Add a new forest" because we are creating a brand new domain.

As for the domain name, I'll call it "mydfir.local". The domain name must have a top-level domain, so in other words my domain name cannot simply be "mydfir". It must be "mydfir.something".

I'll hit "Next". I'll leave everything default. Put in a password and click "Next" until you see "Paths". These will be the paths used to store our database file named "NTDS.dit".

As an FYI, attackers love to target domain controllers. Not only because it has access to everything, but also because of this file as it contains everything related to active directory including passwords and hashes.

If you notice any unauthorized activity towards this file, you can assume that your entire domain is unfortunately compromised. Click on "Next". Once the setup finishes verifying prerequisites, we can go ahead and click on "Install"

Once the setup is completed, the server will automatically restart. Your gonna see that you're about to be signed out and that is absolutely okay. We can go ahead and click on "Close" and our server should automatically restart.

All right, let's log back into our server and you will see our domain followed by a backslash which indicates that we have successfully installed ADDS, and promoted our server to a domain controller.

The next step is to start creating some users, so let's log in and do just that. On our server manager, we want to click on "Tools" at the top right corner and we want to select "Active Directory Users and Computers".

This is where we can create objects such as users, computers, groups, and many more. Let's go ahead and expand our domain and we see a couple of folders called "Builtin", "Computers", "Domain Controllers", and many more.

Now, if we click into "Builtin", we see all of the groups listed on the right hand side. These groups have been automatically created by the active directory and we can double-click any of these and read the description.

For example, I'll double click "Administrators" and the description here is "Administrators have complete and unrestricted access". Under the tab "Members", we will see who is assigned to this group. Under the tab "Member Of", we will see what other groups this group is in.

As an FYI, you cannot add additional groups within a built-in group but you can create a custom group and add that built-in group to the custom group. So for example, I can't add anything here but if I right-click "New".

Then click "Group" and type in "test". Hit "OK". Double-click the test and click on "Member Of". I can type in any group I want. For example, type "Administrators" and then click "Check Names" and hit okay so this is the built group whereas a built-in group you cannot do that because if you click on member of these are all grayed out if we click on the user folder here we will see a list of users along with additional groups now if we want we can create a user within this folder similar to how I did it with the group but in a real world environment it is likely broken up to different departments AKA organizational units for example HR Finance it sales Etc to mimic that we can rightclick the domain go under new and select organizational unit and we'll name this as it I'll click on okay and now we have a new organizational unit created now within this unit I'll rightclick new and I'll create a new user the first name for my new user will be Jenny and the last name will be Smith so Jenny's username will be J Smith I'll hit next and as for the password we can put whatever we want now because we are in a lab environment we can go ahead and uncheck user must change password and I'll click on next and finish great now that we have a new user let's create another organizational unit and this time we'll call it HR so I'll right click the domain new organizational unit HR click on okay now let's create another user and for this user I will name Terry Smith and the username is T Smith click on next password put in a super secure password and I'll uncheck the user must change password click on next and finish so now we have two users that we just created there are many scripts out there that can help you autocreate users groups computers but for this project will only create two users now that we have our active directory set up and our server is now a domain controller we will now head over to our Windows Target machine and join it to our newly created domain called my dfir dolo and authenticate using Jenny Smith account on our Windows 10 machine we want to search up PC and then click on properties scroll down and then we want to select Advanced system settings and then for the tab we want to click on computer name select change and make sure you select domain and now we can type in our domain of my dfir do local now you might get an error saying an active directory domain controller for the domain my df.loc could not be contacted this is because our Target machine does not know how to resolve my dfir local and this all goes back to how DNS works now to fix this we can head over to our network adapter and right click open network and internet settings click on change adapter options we want to right click our adapter and select properties and double click Internet Protocol version 4 now if you take a look at our DNS server it is pointing to Google's DNS we want to change this pointing to our domain controller which is hosted on 192.168.10.0 and then I'll hit okay select okay now just to make sure everything is good to go we'll type in CMD to open up our Command Prompt and then I'll type in ip config /all this will show our DNS server in this case we can see that it is pointing to our domain controller So in theory we we should be good to join our domain so let's go ahead and do that I'll select okay again and now we're prompted to enter in some credentials we will use the administrator account of the server to log in as this account will have the proper permissions so I'll type in administrator the password in a real world environment you would create users and put them into a custom group that are authorized to allow computers to join the domain and we do see a window popup saying welcome to the my df.loc domain we'll go ahead and hit okay and then click on okay again it'll say Hey you must restart your computer to apply these changes so let's go ahead and close it out and you'll be prompted to restart now or later let's do now once we're on the log on screen we want to log in with our newly created user called Jenny Smith and Jenny's username is J Smith so in order to do that we want to select other user and we want to make sure that our sign in to is pointing to our domain and we see it is my dfir so that's perfect well the username is J Smith and we'll enter in our password and that is how we create a new user join our computer to a new domain and log in as a domain user congratulations on configuring your own active directory server and creating two users and joining a machine to your newly created domain the sky is the limit from here on out if you want to learn more about it Administration you now have a fully functional active directory server play around with the last thing to do here is to make sure that you snapshot all of your virtual machines that way in case you do break something which you should never be afraid of you can always restore to a known good state in the next part of our series which will be the last episode we will use Cali Linux to perform a Brute Force attack and also set up Atomic red team on our Windows Target machine to generate some Telemetry to view on Splunk that is it for the video and I hope you are enjoying this project series if you are let me know by hitting that like button and subscribe if you want to

<h1>osTicket: Installation & Virtual Machine Creation</h1>
This tutorial will show how to create the virtual machine through Microsoft Azure which will house the help desk ticketing software, osTicket

<h2>Video Demonstration</h2>
<iframe width="560" height="315" src="https://www.youtube.com/embed/FpdC_98Ekgs?controls=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

YouTube: osTicket Backdrop and Installation
Environments and Technologies Used
•	Microsoft Azure
•	Remote Desktop Connection
•	Internet Information Services (IIS)
•	Windows 10 (Operating System)


Installation Steps
Chapter 1: Creating the Microsoft Azure Virtual Machine
1.	Go to https://portal.azure.com/ and search/click on Resource Groups, then click on Create resource group.
2.	Give your resource group a name, for this we will call it ‘osTicket.’ Click on Review + create to create the resource group. https://i.imgur.com/RS42CI5.png
3.	Check your resource group to see that it is there. https://i.imgur.com/prG54jp.png
4.	Search for Virtual Machines in the search bar and click on it, then click on Create and in the dropdown menu, click on Azure virtual machine to setup your virtual machine.
5.	Give the virtual machine a name, here it is vm-osticket. Give it a region closest to where you are and in the Image dropdown, click on Windows 10 Pro, version 21H2 – x64 Gen2 (free services eligible) https://i.imgur.com/VUsmK9w.png
6.	Scroll down to Size and in the drop down, select Standard D4s_v3 – 4vcpus, 16 GiB memory ($140.16/month). This will give your virtual machine enough cores and RAM to be able to run it responsibly. Of course, be sure to delete the virtual machine and resource group afterwards to prevent accruing cost of your free credits. https://i.imgur.com/y4F5EuX.png
7.	Under Administrator account, enter a Username and Password (make sure you remember them) and under Licensing, click the checkmark, and click Next : Disks >  and then Next : Networking >. https://i.imgur.com/ucY0LFK.png
8.	Check out the list under Networking then click on Review + create, and click on Create on the ensuing page. The virtual machine will take about a minute to deploy. https://i.imgur.com/Qdu0Lyv.png

Chapter 2: Configuring the VM for osTicket
1.	Once the virtual machine has deployed, click on the Go to resource button to see all of the virtual machine’s information https://i.imgur.com/ceogmYP.png
2.	Now we’re going to remote desktop into the VM. Copy the Public IP address, which in this case is shown as 74.235.122.69. It will be unique to you once you create yours. https://i.imgur.com/6r8J3pm.png
3.	Go to the Start menu and open Remote Desktop Connection. Paste the IP and click on Connect. Upon connecting, enter the Username and Password that you created when creating the VM and press OK. Then press Yes to bypass security authentication and the VM will launch. https://i.imgur.com/2KmcenT.png https://i.imgur.com/Xs5HkQM.png https://i.imgur.com/XY9nBAk.png 
4.	Upon logging into the VM, enable network discoverability by pressing Yes. After that, open Microsoft Edge.
5.	While doing step 4, open Control Panel, click on Programs, then click on Turn Windows features on or Off under Programs and Features. Enable the block check beside Internet Information Services, expand it, and make sure block checks are enabled beside Web Management Tools and World Wide Web Services. Expand World Wide Web Services and expand Application Development Features, and enable the checkmark next to CGI.
All of this is critical to make sure osTicket will work. To make sure this step worked, type 127.0.0.1 in Microsoft Edge’s address bar and it should show Internet Information Services of Windows. If the page cannot load, this step was not done correctly.
https://i.imgur.com/cWs3XoE.png https://i.imgur.com/WgH8z7U.png 

Chapter 3: Installing osTicket
1.	Go to this Google Drive link https://drive.google.com/drive/u/1/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6 and download all the files individually or as a zip file.
2.	Install PHPManagerForIIS_V1.5.0 and rewrite_amd64_en-US, then go to C:\ and create a folder called PHP. Extract the php-7.3.8-nts-Win32-VC15-x86 zip file into C:\PHP.
3.	Install VC_redist.x86 and mysql-5.5.62-win32. With the latter, select the Typical installation. After the install is finished, click Finish while Launch the MySQL Instance Configuration Wizard is checked. When that popups up, advance with it and select the Standard Configuration. https://i.imgur.com/vL0wv4R.png
4.	When it asks to set the security options, enter a root password and type it again to confirm. ‘root’ will be the Username we’ll use later on, and the password selected here will be used to create the database. Finish the installation, then Execute and Finish. https://i.imgur.com/WB0vc15.png
5.	Open Internet Information Services (IIS) Manager and open PHP Manager within. Click on Register new PHP version and copy-paste the path of where php-cgi.exe is, which in this case should be C:\PHP\php-cgi.exe. https://i.imgur.com/pybLKhh.png
6.	Go back to the installed files and open the .zip file of osTicket v.1.15.8. Copy the upload folder to C:\inetpub\wwwroot. After it is copied, rename the folder to osTicket. https://i.imgur.com/YBVY6Qi.png
7.	Return to IIS and press Restart under Actions at the right of the window. At the left under Connection, expand Sites and expand Default Web Site to view and click the osTicket folder. On the right side of the window under Actions, click on Browse *.80 (http) to open the localhost osTicket webpage. https://i.imgur.com/2tHXfz8.png https://i.imgur.com/DVUpSoP.png 
8.	Return to IIS and open the PHP Manager. A few extensions need to be activated in order for osTicket to work properly. Under PHP Extensions, click on Enable or disable an extension. In the next window, enable php_imap.dll, php_inet.dll, and php_opcache.dll. Upon refreshing the internet browser page, a few more checkmarks appear.
9.	Return to C:\inetpub\wwwroot\osTicket and scroll down for a file called ost-sampleconfig.php. Right click on it and rename it to ost-config.php. After that, right click on the file and click on Properties. Go to the Security tab click on Advanced. Click on Disable inheritance and then Remove all inherited permissions from this object.
10.	Now we need to add custom permissions. Click on Add and type ‘everyone’ in the ‘Enter the object name’ area and click on Check Names and press OK. Check the Full control and Modify checkboxes and press OK. Then Apply and OK.
11.	Return back to the web browser with osTicket on it and click Continue. You can then name your own Helpdesk, give it a custom email, and fill out information as you would want in the Admin User section with an email address and password that you will need to log into the software. https://i.imgur.com/M2F2pRV.png
12.	Go back to the installation files and click on the HeidiSQL_12.3.0.6589_Setup.exe file which will show a link which will need to be visited in order to download the program. Upon downloading, install the HeidiSQL_12.3.0.6589_Setup file, and launch HeidiSQL.
13.	Upon launching HeidiSQL, click on New and enter the same password from the previous MySQL installation and click Open. Return to the web browser and enter the same MySQL Username and Password, which again is ‘root’ and your entered password. https://i.imgur.com/LWZipba.png https://i.imgur.com/1WnetbU.png
14.	Return to HeidiSQL and right-click Unnamed on the left side, hover over Create new and click on Database. https://i.imgur.com/PczAPr0.png Type in ‘osTicket’ under Name and click OK. https://i.imgur.com/UgfOm2k.png Go back to the web browser and type ‘osTicket’ under MySQL Database.
15.	You did it! Click on Install Now. To clean everything up, return to C:\inetpub\wwwroot\osTicket and delete the setup folder. Return to the ost-config.php file and edit the permissions to remove the checkmarks from Full control and Modify. https://i.imgur.com/UypVMRq.png
16.	Upon going back to Microsoft Edge, the osTicket main page should look something like the picture below, congrats, you’ve set up osTicket! https://i.imgur.com/AF6VwUs.png
Click for Part 2: osTicket Configuration – Creating Agents and Clients

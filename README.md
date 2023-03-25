<h1>Microsoft Sentinel (Azure SIEM) Project</h1>

<h2>Description</h2>
This project consists of setting up Microsoft Sentinel (cloud-based SIEM), as well as a virtual machine acting as a honeypot, to monitor and log IP addresses and locations of the attacks (failed RDP logs) happening worldwide on the virtual machine, and then display the data on a map. 
<br />

<h2>Summary </h2>

- <b>- Extracted and forwarded metadata from Windows Event Viewer to a third party API using a custom PowerShell script, to derive geolocation data.

- <b>- Configured Log Analytics Workspace in Azure to ingest custom logs containing geographic data (latitude, longitude, state/province, and country.

- <b>- Configured custom fields in Log Analytics Workspace to map geo data in Microsoft Sentinel.

- <b>- Configured Microsoft Sentinel workbook to display the RDP brute force attacks data on a world map according to the physical location and the number of attacks.


<h2>Project walk-through:</h2>

<p>
 <h3> Create a Virtual Machine that will be attacked </h3>
1. In Azure, search and go to the Virtual Machines page <br/>
 <img src="https://imgur.com/z2ns25d.png" height="80%" width="80%" alt="1"/>
<br />
<br />
2. Click on Create > Azure virtual machine <br/>
  <img src="https://imgur.com/ajDeLHM.png" height="80%" width="80%" alt="2"/>
<br />
<br />
3. Create a new resource group <br/>
  <img src="https://imgur.com/0i8agnY.png" height="80%" width="80%" alt="3"/>
<br />
<br />
4. Name the virtual machine, choose a region, and choose an image <br/>
  <img src="https://imgur.com/RW1zOTB.png" height="80%" width="80%" alt="4"/>
<br />
<br />
5. Create an administrator account <br/>
  <img src="https://imgur.com/tqPEXgc.png" height="80%" width="80%" alt="5"/>
<br />
<br />
6. Check the Licensing box, and click Next <br/>
  <img src="https://imgur.com/Q5r9w03.png" height="80%" width="80%" alt="6"/>
<br />
<br />
7. Click Next to go to Networking <br/>
  <img src="https://imgur.com/43KGhVy.png" height="80%" width="80%" alt="7"/>
<br />
<br />
8. Pick Advanced in NIC network security group, and click Create new <br/>
  <img src="https://imgur.com/l9tGsV3.png" height="80%" width="80%" alt="8"/>
<br />
<br />
9. Remove the default inbound rule by clicking the 3 dots, then Remove <br/>
  <img src="https://imgur.com/qwnq1Vt.png" height="80%" width="80%" alt="9"/>
<br />
<br />
10. Click Add an inbound rule to allow everything into the VM, for our testing purposes <br/>
11. In the Destination port ranges delete the port and add a star (*) the start signifies all ports, change the priority to 100 and click Add, then click Ok <br/>
  <img src="https://imgur.com/YmKtco9.png" height="80%" width="80%" alt="10"/>
<br />
<br />
12. Click Review + Create, then create <br/>

<br/>
<h3> Create a log analytics workspace </h3>
<h4> To ingest logs from the virtual machine, the SIEM will connect to the workspace to display geo data of attacks on the map. </h4>
<br/>
1. Search for and go to Log Analytics workspaces <br/>
  <img src="https://imgur.com/BkNx2He.png" height="80%" width="80%" alt="11"/>
<br />
<br />
2. Click create log analytics workspace <br/>
  <img src="https://imgur.com/zOcxldV.png" height="80%" width="80%" alt="12"/>
<br />
<br />
3. Select the resource group created <br/>
  <img src="https://imgur.com/Ex7Ds0F.png" height="80%" width="80%" alt="13"/>
<br />
<br />
4. Choose a name and a region, then click Review + Create > Create <br/>
  <img src="https://imgur.com/YUzUuOF.png" height="80%" width="80%" alt="14"/>
<br />
<br />

<br/> 
<h3> Enable gathering VM Logs in the Security center </h3>
1. Search for and go to Microsoft defender for cloud <br/>
  <img src="https://imgur.com/rsc4rrp.png" height="80%" width="80%" alt="15"/>
<br />
<br />
2. Scroll down to management and click Environment settings <br/>
  <img src="https://imgur.com/wK0Ys0Y.png" height="80%" width="80%" alt="16"/>
<br />
<br />
3. Unfold the Azure subscription and click on the workspace we have created <br/>
  <img src="https://imgur.com/YvQzibV.png" height="80%" width="80%" alt="17"/>
<br />
<br />
4. Turn on Foundational CSPM and Servers, then click Save <br/>
  <img src="https://imgur.com/WdFvvlB.png" height="80%" width="80%" alt="18"/>
<br />
<br />
5. Click on the Data collection tab, and select All Events, and click Save <br/>
  <img src="https://imgur.com/I5kE2w5.png" height="80%" width="80%" alt="19"/>
<br />
<br />
 <img src="https://imgur.com/wInM5H7.png" height="80%" width="80%" alt="20"/>
<br />
<br />

<br/>
<h3> Connect log analytics workspace to the VM </h3>
1. Go to the Log analytics workspaces <br/>
2. Click on the workspace created <br/>
3. Scroll down to Virtual Machines <br/>
  <img src="https://imgur.com/E6JtuuX.png" height="80%" width="80%" alt="21"/>
<br />
<br />
4. Click on the VM <br/>
  <img src="https://imgur.com/eZN61jG.png" height="80%" width="80%" alt="22"/>
<br />
<br />
5. Click Connect <br/>
  <img src="https://imgur.com/SH2LWSk.png" height="80%" width="80%" alt="23"/>
<br />
<br />

<br/>
<h3> Setup Microsoft Sentinel </h3>
1. Search for and go to Azure sentinel <br/>
  <img src="https://imgur.com/zzc7RyF.png" height="80%" width="80%" alt="24"/>
<br />
<br />
2. Click Create Microsoft Sentinel <br/>
  <img src="https://imgur.com/DXY3K3w.png" height="80%" width="80%" alt="25"/>
<br />
<br />
3. Pick the analytics workspace that we want to connect to, and click Add <br/>
  <img src="https://imgur.com/FMCuDn5.png" height="80%" width="80%" alt="26"/>
<br />
<br />

<br/>
<h3> Log into VM with remote desktop </h3>
1. Search for and go to Virtual machines <br/>
2. Click on our virtual machine and copy the public address <br/>
  <img src="https://imgur.com/ULgfjvq.png" height="80%" width="80%" alt="27"/>
<br />
<br />
3. Open remote desktop connection on your PC <br/>
  <img src="https://imgur.com/uD0qMqB.png" height="80%" width="80%" alt="28"/>
<br />
<br />
4. Paste the public IP copied and click connect <br/>
  <img src="https://imgur.com/OKPuuje.png" height="80%" width="80%" alt="29"/>
<br />
<br />
5. Type in the username and password that where set when creating the VM <br/>
6. Choose the privacy settings for your VM <br/>

<br/>
<h3> Observe Event Viewer logs in VM </h3>
1. Inside the VM, search for and open Event viewer <br/>
  <img src="https://imgur.com/S8nsQHb.png" height="80%" width="80%" alt="30"/>
<br />
<br />
2. Open windows logs, then click on Security, to see security events on the VM <br/>
  <img src="https://imgur.com/ZsCY4YZ.png" height="80%" width="80%" alt="31"/>
<br />
<br />
3. Double click an event to see the details, such as username, IP and other information of the person trying to access the VM in case of audit failure <br/>

<br/>
<h3> Turn off windows firewall on the VM </h3>
1. Inside the VM, search for wf.msc <br/>
  <img src="https://imgur.com/1YOAbVi.png" height="80%" width="80%" alt="32"/>
<br />
<br />
2. Click on windows defender firewall properties <br/>
  <img src="https://imgur.com/bjWcBi6.png" height="80%" width="80%" alt="33"/>
<br />
<br />
3. Turn off firewall state on all profiles, click Apply > Ok <br/>
4. Open CMD on your PC and ping the IP of the VM to test <br/>


<br/>
<h3> Run the PowerShell script on the VM </h3>
1. Copy PowerShell script <br/>
2. Open PowerShell ISE on VM <br/>
3. Click New Script <br/>
  <img src="https://imgur.com/3KJjg0Z.png" height="80%" width="80%" alt="34"/>
<br />
<br />
4. Paste the script inside PowerShell ISE <br/>
5. Click Save, and save it on the desktop <br/>
  <img src="https://imgur.com/F1blBYE.png" height="80%" width="80%" alt="35"/>
<br />
<br />
6. Go to the website: ipgeolocation.io, sign up, and type in the given API key inside the code on PowerShell ISE, as seen below <br/>
  <img src="https://imgur.com/4K4QUn0.png" height="80%" width="80%" alt="36"/>
<br />
<br />
7. Run the script <br/>

<h4> The script runs as a loop, and looks through the event viewer, and grabs all the events of people that failed to login, and gets their geo data using their IP address, and creates a new log file. To test it, we can fail a login and check the log file. </h4>


<h3> Create a custom log in Log Analytics Workspace to bring in our customer log </h3>
1. Go to Log analytics workspaces in Azure <br/>
2. Go to the workspace created and scroll down to legacy custom logs, then add a custom log <br/>
  <img src="https://imgur.com/dGbeY9C.png" height="80%" width="80%" alt="37"/>
<br />
<br />
3. Copy the logs inside the log file from our VM, then paste it into a notepad, and add it in the sample log on Azure (used to train log analytics on what to look for in the log file) <br/>
  <img src="https://imgur.com/yubl0pX.png" height="80%" width="80%" alt="38"/>
<br />
<br />
  <img src="https://imgur.com/X46iipa.png" height="80%" width="80%" alt="39"/>
<br />
<br />
4. Click Next > Next > In the collection path, type in the path where the log file is present on the VM (So it knows where to collect logs from) <br/>
  <img src="https://imgur.com/KQR6VMt.png" height="80%" width="80%" alt="40"/>
<br />
<br />
5. Click Next <br/>
6. In the Details section, name the custom log > Next <br/>
  <img src="https://imgur.com/p4EfsyO.png" height="80%" width="80%" alt="41"/>
<br />
<br />
7. Click Create <br/>

<h4> To run the log > Go to log analytics workspaces > click on the workspace > logs > Type in the name of the custom log > Click Run </h4>


<h3> Create custom fields </h3>
1. Run the custom log (To run the log > Go to log analytics workspaces > click on the workspace > logs > Type in the name of the custom log > Click Run) <br/>
2. Right click a column in the results tab > Extract fields <br/>
  <img src="https://imgur.com/m533PcP.png" height="80%" width="80%" alt="42"/>
<br />
<br />
3. To extract the fields from the raw custom data highlight the fields and add a field title, and change the type, then click extract > verify the accuracy > Save extraction, example: <br/>
  <img src="https://imgur.com/d3j37Yf.png" height="80%" width="80%" alt="43"/>
<br />
<br />
  <img src="https://imgur.com/PCHLd3r.png" height="80%" width="80%" alt="44"/>
<br />
<br />
4. Do the same for all fields <br/>

<h4> In case the highlighted search results are wrong, click the pen (edit button) on the search result > Modify this highlight > highlight the correct number and click Extract > fix all search results > Save extraction </h4>
5. To show the new fields in the results tab after running the query Click Columns and toggle the new fields <br/>
  <img src="https://imgur.com/U0Jb5FQ.png" height="80%" width="80%" alt="45"/>
<br />
<br />
6. If the fields are breaking after logging data, go to Legacy custom logs > Custom fields > delete the custom field > redo the custom fields the same way they where done the first time with more records <br/>

<br/>
<h3> Setup map in sentinel with latitude and longitude </h3>
1. Search for and go to Microsoft Sentinel <br/>
2. Click on the sentinel we setup earlier <br/>
3. Click on Workbooks and add a workbook <br/>
  <img src="https://imgur.com/I8u5JXx.png" height="80%" width="80%" alt="46"/>
<br />
<br />
4. Click on Edit and remove the default widgets <br/>
  <img src="https://imgur.com/xZvD3R7.png" height="80%" width="80%" alt="47"/>
<br />
<br />
5. Add a query <br/>
  <img src="https://imgur.com/ikYRPem.png" height="80%" width="80%" alt="48"/>
<br />
<br />
6. Add a query for the map and run <br/>
  <img src="https://imgur.com/MRQoFdG.png" height="80%" width="80%" alt="49"/>
<br />
<br />
7. Set Visualization to Map <br/>
  <img src="https://imgur.com/MsrZfOB.png" height="80%" width="80%" alt="50"/>
<br />
<br />
8. Set the metric label to Label_CF, and the Metric Value to event_count <br/>
9. We can plot the map by country, or by latitude and longitude in the Layout settings <br/>
10. Click Apply > Save and close <br/>
11. Save (and name) the new workbook <br/>
  <img src="https://imgur.com/1v2J305.png" height="80%" width="80%" alt="51"/>
<br />
<br />
12. Turn on auto refresh and set for 5 min > save<br/>


<h4> Now we have access to a map that will show us any failed login attempt to the VM, by pulling data from the script that is running on the VM to Azure. Keep in mind the script in the VM has to be always running, this is a proof of concept implementation. See the result below: </h4>
 <img src="https://imgur.com/4yLH8SH.png" height="80%" width="80%" alt="52"/>
<br />
<br />



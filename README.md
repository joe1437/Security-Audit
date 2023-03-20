<h1>Azure Sentinel (SIEM) Project</h1>

<h2>Description</h2>
This project consists of setting up Microsoft Sentinel (cloud-based SIEM), as well as a virtual machine acting as a honeypot, to monitor and log IP addresses and locations of the attacks (failed RDP logs) happening worldwide on the virtual machine, and then display the data on a map. 
<br />

<h2>Summary </h2>

- <b>- Extracted and forwarded metdata from Windows Event Viewer to third party API using custom PowerShell script, to derive geolocation data.

- <b>- Configured Log Analytics Workspace in Azure to ingest custom logs containing geographic data (latitude, longitude, state/province, and country.

- <b>- Configured custom fields in Log Analytics Workspace to map geo data in Microsoft Sentinel.

- <b>- Configured Microsoft Sentinel workbook to display the RDP brute force attacks data on world map according to physical location and number of attacks.


<h2>Project walk-through:</h2>

<p>
 <h3> Create a Virtual Machine that will be attacked </h3>
1. In Azure, search and go to the Virtual Machines page <br/>
2. Click on Create > Azure virtual machine <br/>
3. Create a new resource group <br/>
4. Name the virtual machine, choose a region, and choose an image <br/>
5. Create an administrator account <br/>
6. Check the Licensing box, and click Next <br/>
7. Click Next to go to Networking <br/>
8. Pick Advanced in NIC network security group, and click Create new <br/>
9. Remove the default inbound rule by clicking the 3 dots, then Remove <br/>
10. Click Add an inbound rule to allow everything into the VM, for our testing purposes <br/>
11. In the Destination port ranges delete the port and add a star (*) the start signifies all ports, change the priority to 100 and click Add, then click Ok <br/>
12. Click Review + Create, then create <br/>

<br/>
<h3> Create a log analytics workspace </h3>
<h4> To ingest logs from the virtual machine, the SIEM will connect to the workspace to display geo data of attacks on the map. </h4>
<br/>
1. Search for and go to Log Analytics workspaces <br/>
2. Click create log analytics workspace <br/>
3. Select the resource group created <br/>
4. Choose a name and a region, then click Review + Create > Create <br/>

<br/> 
<h3> Enable gathering VM Logs in the Security center </h3>
1. Search for and go to Microsoft defender for cloud <br/>
2. Scroll down to management and click Environment settings <br/>
3. Unfold the Azure subscription and click on the workspace we have created <br/>
4. Turn on Foundational CSPM and Servers, then click Save <br/>
5. Click on the Data collection tab, and select All Events, and click Save <br/>


<br/>
<h3> Connect log analytics workspace to the VM </h3>
1. Go to the Log analytics workspaces <br/>
2. Click on the workspace created <br/>
3. Scroll down to Virtual Machines <br/>
4. Click on the VM <br/>
5. Click Connect <br/>

<br/>
<h3> Setup Microsoft Sentinel </h3>
1. Search for and go to Azure sentinel <br/>
2. Click Create Microsoft Sentinel <br/>
3. Pick the analytics workspace that we want to connect to, and click Add <br/>

<br/>
<h3> Log into VM with remote desktop </h3>
1. Search for and go to Virtual machines <br/>
2. Click on our virtual machine and copy the public address <br/>
3. Open remote desktop connection on your PC <br/>
4. Paste the public IP copied and click connect <br/>
5. Type in the username and password that where set when creating the VM <br/>
6. Choose the privacy settings for your VM <br/>

<br/>
<h3> Observe Event Viewer logs in VM </h3>
1. Inside the VM, search for and open Event viewer <br/>
2. Open windows logs, then click on Security, to see security events on the VM <br/>
3. Double click an event to see the details, such as username, IP and other information of the person trying to access the VM in case of audit failure <br/>

<br/>
<h3> Turn off windows firewall on the VM </h3>
1. Inside the VM, search for wf.msc <br/>
2. Click on windows defender firewall properties <br/>
3. Turn off firewall state on all profiles, click Apply > Ok <br/>
4. Open CMD on your PC and ping the IP of the VM to test <br/>


<br/>
<h3> Run the PowerShell script on the VM </h3>
1. Copy PowerShell script <br/>
2. Open PowerShell ISE on VM <br/>
3. Click New Script <br/>
4. Paste the script inside PowerShell ISE <br/>
5. Click Save, and save it on the desktop <br/>
6. Go to the website: ipgeolocation.io, sign up, and type in the given API key inside the code on PowerShell ISE, as seen below <br/>
7. Run the script <br/>

<h4> The script runs as a loop, and looks through the event viewer, and grabs all the events of people that failed to login, and gets their geo data using their IP address, and creates a new log file. To test it, we can fail a login and check the log file. </h4>


<h3> Create a custom log in Log Analytics Workspace to bring in our customer log </h3>
1. Go to Log analytics workspaces in Azure <br/>
2. Go to the workspace created and scroll down to legacy custom logs, then add a custom log <br/>
3. Copy the logs inside the log file from our VM, then paste it into a notepad, and add it in the sample log on Azure (used to train log analytics on what to look for in the log file) <br/>
4. Click Next > Next > In the collection path, type in the path where the log file is present on the VM (So it knows where to collect logs from) <br/>
5. Click Next <br/>
6. In the Details section, name the custom log > Next <br/>
7. Click Create <br/>

<h4> To run the log > Go to log analytics workspaces > click on the workspace > logs > Type in the name of the custom log > Click Run </h4>


<h3> Create custom fields </h3>
1. Run the custom log (To run the log > Go to log analytics workspaces > click on the workspace > logs > Type in the name of the custom log > Click Run) <br/>
2. Right click a column in the results tab > Extract fields <br/>
3. To extract the fields from the raw custom data highlight the fields and add a field title, and change the type, then click extract > verify the accuracy > Save extraction, example: <br/>
4. Do the same for all fields <br/>

<h4> In case the highlighted search results are wrong, click the pen (edit button) on the search result > Modify this highlight > highlight the correct number and click Extract > fix all search results > Save extraction </h4>
5. To show the new fields in the results tab after running the query Click Columns and toggle the new fields <br/>
6. If the fields are breaking after logging data, go to Legacy custom logs > Custom fields > delete the custom field > redo the custom fields the same way they where done the first time with more records <br/>

<br/>
<h3> Setup map in sentinel with latitude and longitude </h3>
1. Search for and go to Microsoft Sentinel <br/>
2. Click on the sentinel we setup earlier <br/>
3. Click on Workbooks and add a workbook <br/>
4. Click on Edit and remove the default widgets <br/>
5. Add a query <br/>
6. Add a query for the map and run <br/>
7. Set Visualization to Map <br/>
8. Set the metric label to Label_CF, and the Metric Value to event_count <br/>
9. We can plot the map by country, or by latitude and longitude in the Layout settings <br/>
10. Click Apply > Save and close <br/>
11. Save (and name) the new workbook <br/>
12. Turn on auto refresh and set for 5 min > save <br/>

<h4> Now we have access to a map that will show us any failed login attempt to the VM, by pulling data from the script that is running on the VM to Azure. Keep in mind the script in the VM has to be always running, this is a proof of concept implementation. To test the map, I failed to login, see the result below: </h4>








 
 
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

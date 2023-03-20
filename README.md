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

<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!># AzureSIEM

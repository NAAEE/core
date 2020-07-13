<div style='float:right; margin-left:40px; margin-right:40px'>
<b>Site Setup</b><br>
<a href="#setup">Server Setup</a><br>
<a href="#database">Database Setup</a><br>
<a href="#email">Email Setup</a><br>
<a href="about/">About Web API</a><br>
</div>


#### Environmental Education Website Tools

# Resources and Event Calendars


Developed in partnership with [NAAEE](https://naaee.org) for [Georgia](http://eeingeorgia.org/core/news/list.aspx), [Wisconsin](http://EEinWisconsin.org), [North Carolina](http://web.eenorthcarolina.org/core/event/calendar.aspx) and [Hawaii](http://heea.org/core/news/list.aspx).  
With new US EPA tools for Input-Output modeling developed at [Model.Earth](https://model.earth).  



## History of Updates

<a href="https://thegeep.org/"><img src="img/logo/geep.png" style="max-width:300px; float:right"></a>

### Updates prepared for Integration with NAAEE Fork  

#### Deployment Updates

2020 - Streamlining Server Migration.  
2019 - Updates for deployment from Github. 

#### Free HTTPS Certs

2018 - Use of CloudFlare to provide free https security.

#### Email Summary Updates

2017 - Email summaries of exceptions are now sent to admins once every 30 minutes containing the remote IPs and the counts per IP address.  This was added to adjust for bursts of email during high traffic.  

For PageName, we've added the highlighted line. If the script_name ends with a trailing slash, as in “/events/”, we return an empty string rather than “events”.  

For IContains, we now check for an empty string as well as null. If the string had been empty, IContains would have returned 0 as the index, and therefore true, rather than false as would be expected.  


#### Custom Page URLs

2016 - Added support for custom page URLs for improved search indexing and short URLs.  

#### Static Page Generation Updates

2015 - Pages generated from multiple queries are now stored in static folders for fast loading.  

<!--
	Fork the Core repo, copy in recent changes from NAAEE version. Changes are primarily removal of remaining IsSite settings. These can be replaced with database settings in the "site" table.
-->
<br>




<a name="setup"></a>
<br>

# Server Setup Steps

Build a dev site locally or host on a server.  The following uses .NET 4.8.



<!--
Instruction below are for: Vista - Windows 7/10 / Windows Server 2008/2016 and forward.  
Commented out: XP - Windows XP / Windows Server 2005  
-->

## Install and Configure IIS

Launch Internet Information Services(IIS)
You can create one IIS entry for multiple sites that use the same IP addreess.  

<!--
Make sure a primary website is viewable in a browser while on the machine itself.  Some networks may require changes to the firewall settings.  PDF generation requires that the machine can load content via the domains it hosts.<br>
-->




<h2>Application Pool Setup</h2>
In IIS, edit or create an Application Pool for each website or group of websites.<br>

<ul>
<li>Set the .NET Framework Version to ASP.NET 4.0.</li>

<li>Assign the App Pool in IIS under Basic Settings</li>
</ul>

Didn't do the following yet on new server:
<ul>
<li>For 64-Bit operating systems - may need to set "Enable 32-Bit Applications" to True  
-- For COM dlls used by the asp pages.</li>
<li>Set up each Application Pool to run under the Network Service Identity rather than the default ApplicationPoolIdentity Identity. This allows write permissions to be set for common folders that are used by all websites, such as for mail or upload folders, to be set up using one identity. (Else the mail pickup folder will display error: Access to the path is denied.)</li>
</ul>


<h2>Install and Configure .NET, Run Windows Update</h2>

Get the latest updates related to the .NET Framework - 
<a href="https://dotnet.microsoft.com/download/dotnet-framework/">Download .NET Framework</a> using the Web Installer.<!--
OLD NOTE (We're switching to .NET 4.8, use link above)  
If not already installed, install the <a target="_blank" href="http://www.microsoft.com/en-us/download/details.aspx?id=17718">.NET Framework 4.0 (Standalone Installer)</a> or the <a target="_blank" href="http://www.microsoft.com/en-us/download/details.aspx?id=17851">.NET Framework 4.0 (Web Installer)</a>.  
-->  
The Web Installer is ease to use and ensures that any prerequisites are installed before installing the framework.

<!-- XP
   
Install the <a target="_blank" href="http://www.microsoft.com/downloads/details.aspx?familyid=0856eacb-4362-4b0d-8edd-aab15c5e04f5&displaylang=en">
    .NET Framework 2.0 Redistributable Package:</a>

For laptops viewing localhost with ASP.NET 2.0, IIS also requires running:<br>
C:\WINDOWS\Microsoft.NET\Framework\<version>\aspnet_regiis -i<br><br>

<strong>Time:</strong> 2-4 hrs depending on updates needed<br>
-->


<a name="database"></a>

## Install and Setup Database

Details in <a href='about/setup/Database.aspx'>Database Setup</a><br>


**Install Sql Server using the followign steps**  


Install standalone server<br>
Use the default instance and use a different drive if possible for user databases and backups.<br>
<ul>
    <li>Data and Transaction Log files: E:\Data</li>
    <li>Backup Files: E:\Bkup</li>
</ul>
Install the following:<br>
<ul>
    <li>Full Text</li>
    <li>Client tools connectivity</li>
    <li>Integration Services</li>
    <li>Client Tools Backwards Compatibility</li>
    <li>Documentation Components</li>
</ul>
From the installation program, install Sql Server Management Studio (SSMS). For Sql Server 2016+, SSMS is a separate install  

Restore Databases, including Lookup<br>
Create Backup Maintenance Plans and Schedules<br>
Create logins<br>
Setup Network Connectivity and update Firewall settings. If the website and server will reside on the same server,
this step won't be needed.<br>

<strong>Time:</strong>&nbsp;20-40 hrs<br>


## Add Web.Config file

<a href='about/setup/File.aspx'>Generate Web.config File</a> and place in root folder.<br>

<strong>Time:</strong>&nbsp;4-8 hrs Setup/Testing/Troubleshooting<br>
   


<h2>Configure IIS and Folders - MIME types</h2>
<strong>Set the IIS root directory to D:\Web</strong> (or other site root folder)<br>
Create a Bin folder in this new site root.<br>

IMPORTANT: <a href="http://www.iis.net/configreference/system.webserver/staticcontent/clientcache#004">Set browser caching</a><br>
Set the time to cache in the browser to one day.<br>


The following bindings are installed in the default IIS site on Windows Server 2008.<br><br>
net.tcp, net.pipe, net.msmq, msmq.formatname  

**Add MIME types for trail vector maps:**  

	Extension: .kml  
	MIME type: application/vnd.google-earth.kml+xml  

	Extension: .kmz  
	MIME type: application/vnd.google-earth.kmz  

<strong>Time:</strong>&nbsp;2-4 hrs





<a name="email"></a>

<h2>Setup Email</h2>
Setup email to allow website to send emails.<br>
Turn on the SMTP Service and set to Automatic Startup.<br>
In C:\inetpub\maillroot, give the Network Service account full permissions.  
This should be the same user account that the IIS Application Pools runs under.

<strong>Time:</strong>&nbsp; 1-2 hrs  

    
<h2>Directory Permission Setting</strong></h2>
Apply the following permission change to the following folders:<br>
<ul>
    <li>Files</li>
    <li>Source - Larger original versions</li>
    <li>Removed - Source images stored for resizing, but expendable if storage reaches max.</li>
    <li>Page</li>
</ul>

<!--
Permission change probably not needed here:
<ul>
    <li>Content - 2004 to 2013. Prior to "go" folder and other repos. Included FTP uploads.</li>
</ul>
-->

Right click on the Files directory and select Properties. Click on the Security tab. 

<!--
	XP
Right click on the Files directory and select Sharing and Security. 
-->
Click Add to enter the Network Service username if not already present.  

	[Machine Name]\NETWORK SERVICE  

Give it Full Control. Click Advanced. Keep the first box checked and also check
the second "Replace permission entries on all child objects..."  
    
<strong>Time:</strong>&nbsp;2-3 hrs<br>
<br>



<br>


# pishing-api-master

4lph4 pishing api master

This API has three main features. One allows you to easily deploy cloned landing pages for credential stealing, another is weaponized Word doc creation, and the third is saved email campaign templates. Both attack methods are integrated into Slack for real-time alerting. <b>Unfortunately, I'm no longer running this code as a free service @ https://phishapi.com due to cost, sorry!</b>

## Update

This latest version no longer redirects users of the landing pages to the API directly by default, but instead sends an AJAX request to the API server prior to posting the form data to the legitimate target site. This provides for a more seamless experience for the "victim" and will actually log them into the target site when they submit their credentials, instead of performing what appears to be a refresh on the login page. CSRF protection is bypassed by the API grabbing the token beforehand! However, I haven't yet gotten around to updating all of the cloned portal pages to use this new method so many will still perform the redirect. FYI!

<img src="https://github.com/sobhan-azimzadeh/pishing-api-master/blob/master/asset/screencapture-localhost-Phishing-API-master-2024-10-20-19_35_26.png" >

<img src="https://github.com/sobhan-azimzadeh/pishing-api-master/blob/master/asset/Screenshot%202024-10-20%20193608.png" >
<img src="https://github.com/sobhan-azimzadeh/pishing-api-master/blob/master/asset/screencapture-localhost-Phishing-API-master-templates-templatecreation-php-2024-10-20-19_36_25.png" >
<img src="https://github.com/sobhan-azimzadeh/pishing-api-master/blob/master/asset/screencapture-localhost-Phishing-API-master-templates-templatecreation-php-2024-10-20-19_36_41.png" >

# To Setup :

1. Import the DB SQL Dump Schema to a new MySQL Instance `mysql -u root -h localhost < DatabaseSQLDump.sql;`. You may have to create a new user that's not "root" and grant all privileges to all databases for your config if you have issues.

2. Host the PHP (PHP7 is supported!) from a web service (Tested with Apache)

3. Configure `/var/www/html/config.php` with your variables

4. Install `apt-get install zip`

5. Chmod 777 all `/var/www/html/phishingdocs` and `/var/www/html/templates/` subdirectories (or Docs and Templates will not work!)

6. Limit Access to the "Results" Directories `/var/www/html/results` and `/var/www/html/phishingdocs/results` (Apache's Basic Auth is Recommended)

7. Use HTTPS (Let's Encrypt!) and a Domain for the Hosted API

8. Optionally run Responder and BeEF in a screen session and import the crontab file

9. Enable browscap in your php.ini config and point to it in your web directory `/var/www/html/browscap.ini` (included in this repo)

10. Enjoy! :) Message me if you have any issues. This does not work on Windows!

# 2) To Use the API for Generating Word Doc Payloads :

1. Create `/var/www/uploads` Path and `sudo chmod 777 /var/www/uploads -R` the path

2. Browse out to your hosted API (YOUR_URL.com) and select "Weaponized Documents" to generate your DOCX

3. Optionally set up [Responder](https://github.com/SpiderLabs/Responder "Responder") in a background process and run `phishinghashes.sh` every minute or so with cron

4. Set up your php.ini to allow uploads of at least 15MB and enable browscap.ini for parsing UserAgent strings, otherwise some functionality may be limited.

5. Email your doc and wait for the Slack alerts!

<p align="center"><b>Bonus points if you use your docs as honeypot bait! :)</b></p>

<br /><br/>

<p align="center">
<img src="https://i.imgur.com/LW4BUjN.png"><br />
<b>Figure 1: Web Based Payload Generation - Create New Doc or Upload Existing w/ Payload Options</b>
</p>
                  
            
<br /><br/>
<p align="center">
<img src="https://i.imgur.com/onsPyFp.png"><br />
<b>Figure 2: Opening Document Generated (New) by Service</b>
</p>

<br /><br/>

<p align="center">
<img src="https://i.imgur.com/sw8JWQE.png" width="40%"><br />
<b>Figure 3: If "Auth Prompt" is Selected in Payload Options, Display Basic Auth Prompt to User for Credential Capturing (like Phishery)</b>
</p>


<br /><br/>

<p align="center">
<img src="https://i.imgur.com/HlY3T4G.png" width="80%"><br />
<b>Figure 4: HTTP Beacon is Selected by Default and Alerts When the Target Opens the Document</b>
</p>

<br /><br/>

<p align="center">
<img src="https://i.imgur.com/ku6UTNI.png" width="75%"><br />
<b>Figure 5: If Credentials are Entered from Figure 3 Above, Notify via Slack When Captured</b>
</p>

<br /><br/>

<p align="center">
<img src="https://i.imgur.com/OO0sjDR.png"><br />
<b>Figure 6: Clicking on the Slack Alert Displays Captured Details (Hashes, Credentials, Client Details)</b>
</p>

<br /><br/>

<p align="center">
<img src="https://i.imgur.com/qZFGmXA.png"><br />
<b>Figure 7: Slack Alert when UNC/SMB Hashes are Received from Word Document</b>
</p>

<br /><br/>

<p align="center">
	<b>Currently, I'm running <a href="https://github.com/SpiderLabs/Responder">Responder</a> in a Screen session with <i>phishinghashes.sh</i> scheduled via Cron to run every minute to pick up hashes, correlate phished users, and alert via Slack.  You can also relay those hashes with another tool if you'd like to take things even further.  Enjoy! :)</b></p>

# 3) To Use the API to Store and Generate Email Campaign Templates :

Leverage a template by creating or choosing an existing template from the local repository, or, you can compose a blank email and embed the invisible HTML beacon to be notified when the recipient opens their email.

<br />
<p align="center">
<img src="https://i.imgur.com/AmwZbbF.png"><br />
<b>Figure 1: Existing, New, or No Campaign Choices</b>
</p>

If a new campaign is chosen, you can create variables for dynamic re-use in the future and store them as HTML templates in a database. The WYSIWYG editor makes things simple, but you can also copy and paste from a text editor or another source if you'd like!

<br />
<p align="center">
<img src="https://i.imgur.com/COHaq6q.png"><br />
<b>Figure 2: New Campaign w/ Variables & Images</b>
</p>

Next time, choosing the existing template will dynamically provide input fields for the stored variables. They can be applied in real time using JavaScript to update the email body. Checking the "Embed Notification for Opened Email" box will automatically append invisible code to your template that will alert you when your recipient opens their email. (Images must be allowed to render for this to work)

<br />
<p align="center">
<img src="https://i.imgur.com/SsBAqKv.png" width="75%"><br />
<b>Figure 3: Existing Campaign</b>
</p>

Sit back and watch as your target opens their email and cross your fingers you later recieve another alert for BeEF, Maldocs, or your captured credentials!

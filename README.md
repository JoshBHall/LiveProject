# Live Project

## Introduction
During my two week sprint internship with Prosper IT Consulting, I was tasked with solving many security challenges.  I completed this work while being part of a team operating from the scrum framework to maintain contact and teamwork.  Some examples of the work I did include penetration testing using techniques such as SQL injection, Burp Suite intruder/repeater attacks to find admin pages and/or escalate account privileges to admin, & JavaScript injection via XSS.  I also performed defensive maneuvers, analyzing malicious PowerShell scripts, diagnosing & researching ransomware, as well as unencrypting files following attack, analyzing pcap files with Wireshark to determine infection information and determine plans for malware removal & prevention of future infection reoccurence.  Almost all of the work I completed was performed on virtual machines that I set up and have worked with extensively.  Following the completion of each story, I wrote an incident report to detail my findings, including screenshots.

Below I've included descriptions of the stories I completed, as well as code snippets and navigation links.  

## Penetration Testing Experience
For these stories, I was able to gain more great experience working with basic attacks such as SQL injection & XSS as well as using Burp Suite and developer tools to find vulnerabilities.


### Admin Login

The TTA Bookstore contracted our team to test their new web app for vulnerability in the login page, logging in to the admin account.  I decided to start by trying a SQL injection attack due to it's prevalence and high success rate, as well as it's ease of use.  I was successful in gaining access using SQL injection on the login page.  I suggested that TTA Bookstore implement escaping and input sanitization techniques on their SQL database.
![JH-10479-Admin-Login-attack](https://user-images.githubusercontent.com/84826626/142253754-7c8bdee3-14f6-4cb2-88fa-a521f0d04c56.png)


### User Login

Working again for the TTA Bookstore, our team was tasked with testing vulnerability of the login page to accessing another user's account.  Since they gave me the name of the user, and the knowledge that all users of the site have the same email domain, I once again used SQL injection and was able to gain access.  I recommended the same solution as before to prevent this attack.  They need to apply input sanitization and escaping of dangerous characters on all user input received from the website.
![JH-10480-User-Login-Attack](https://user-images.githubusercontent.com/84826626/142254246-60fdfd80-1921-4536-a383-75762939fb3f.png)


### Offensive XSS

Continuing with our work penetration testing for the TTA Bookstore, this time my task was to see if the site is vulnerable to XSS.  Again, I found that it was.  I first tested the escaping of the search bar with a simple html tag for italics, <i></i>.  The website displayed the searched for term in italics, telling me there would be vulnerability to XSS due to improper escaping.  I then tried some more common XSS payloads such as scripts for displaying an alert bar.  Most did fail, but I eventually found some tags that got past the defense of the site.  When I injected JavaScript via these tags, I was able to render its effects onscreen, without proper input sanitization.
![JH-10528-Offensive-XSS-1](https://user-images.githubusercontent.com/84826626/142255000-7edc3f1d-afc4-4438-91a7-c176133660cb.png)


### Offensive Admin

For this story, the TTA Bookstore now asked us to find their administration page, to see how vulnerable it is.  I first tried some simple file paths to access it and then tried a more robust brute force attack using a common list in the program Dirp.  When this didn't work either, I viewed the developer tools for the site and searched for the word "admin" in the JavaScript files and was able to find the correct file path to access the administration page.  I was also able to access another user's cart by loading the admin page cart while intercepting the traffic with Burp Suite.  As I saw the GET request for the cart page go through, I sent it to the repeater tab in Burp Suite and changed the user number from 1 to 2.  Then I sent the request and viewed the cart page for the 2nd user.  In order to fix the vulnerability, I recommended moving the admin page to a less conspicuous file path as well as removing the references to it from the JavaScript.
![JH-10529-Offensive-Admin](https://user-images.githubusercontent.com/84826626/142257144-63b7a460-d54f-4012-9be6-296723c6a43e.png)


### Admin Privilege Escalation

For the last contract for the TTA Bookstore, we were asked to test their app for vulnerability to privilege escalation.  Specifically, they wanted to know if we could create a new user and escalate their privileges to administrator.  In order to test this vulnerability I used Burp Suite to intercept the traffic as I created a user account to see what the requests looked like.  I saw that it created a POST request with the user info and the corresponding response showed a line that said: "role":"customer".  So I sent this request to the repeater and edited it to create a new email address and added the line: "role":"admin".  I sent this request and received a successful response. I was able to create the new user account and give it admin privileges.  To test this, I tried to login to the admin page while still logged in as this user and was successful.  Following completion of the story, I recommend that they make sure to encrypt traffic such as their POST requests/responses and separate role application (at least admin) from the general account creation table on the server.
![JH-10544-Admin-Access-2](https://user-images.githubusercontent.com/84826626/142258653-d36e7349-e6f3-43a8-a215-13d952d23771.png)


## Defensive Analysis Training

For the completion of these stories, I used tools such as Wireshark and HashMyFiles, as well as websites like Virustotal.com and Hybrid-analysis.com.  All of these stories were completed in either a Kali Linux or Windows VM.


### Malware Traffic Analysis

This story involved helping a friend who contacted me about their infected PC that has been running slowly and is opening apps on its own.  They sent me the pcap file to analyze and find out what's going on.  I opened up the pcap in Wireshark on my Kali Linux VM.  I first filtered by dhcp and found our friend's IP address, then I filtered by his IP and looked at GET requests.  It looked like our friend was viewing some unsecure websites over HTTP and clicked on the wrong link.  The traffic is fairly normal until a GET request is sent to a certain IP, then we saw POST requests for images at a couple of specific websites.  After that, there are many GET requests sent to a malicious website for image files, including one for cryptocurrency.   I recommended to my friend to always be careful when clicking on links, especially untrusted ones from sketchy websites.
![JH-10481-Malware-Traffic-4](https://user-images.githubusercontent.com/84826626/142260151-5ea49c3d-c2b4-432e-ba96-e3e9fd41a742.png)


### Malicious PowerShell Scrip Analysis

We found a malicious PowerShell script that we were concerned might be dangerous.  I was tasked with investigating and reporting on my findings.  I used HashMyFiles to analyze the hash values and viewed the script in a Windows VM.  I was able to determine that the script was a keylogger which sent out the information to a specific email.  My recommendation to avoid infection by this type of malicious script was to be very careful when clicking on links from untrustworthy sites or in emails.
![JH-10545-PowerShell-Analysis-1](https://user-images.githubusercontent.com/84826626/142260779-bf641fba-dc97-4cd1-9e16-52e3d1a5b4bd.png)


### Malware Deep Investivation

In this story, my friend's aunt opened a sketchy email and her bank account was hacked.  We were tasked with reviewing the pcap file to determine information about her computer as well as the hack itself.  I was asked to find data such as her IP address, MAC address, Host Name, and Windows Account Username.  I found all that using Wireshark in a Kali Linux box.  I also discovered the type of malware, a W32/Ursnif Trojan, and the source of the infection as well as the site visited following infection.  She opened a link in a phishing email and the malware took her to a banking website immediately following infection.  To find this information, I filtered the traffic with bootp to find her host information and then I found her Windows account by sniffing kerberos traffic.  I viewed the alerts file to find the type of malware and viewed the traffic before and after it.  I google searched for information on the malware host name and found out details about the particular malware.  I recommend to her to not click on links from people she doesn't know or isn't 100% certain it's the correct email.  I also suggested looking at the url of the link before clicking on it.
![JH-10574-Malware-Deep-Investigation-2](https://user-images.githubusercontent.com/84826626/142262181-30bdfc0b-d205-4e4d-b052-2606ba66d00a.png)


### Ransomware Analysis

Here, the company I work for was contacted by a dentist's office that has had their computers infected with a scary looking program that they didn't install.  It encrypted their files.  They called us to help.  When I loaded up their machine I was greeted with a scary looking face on a program telling me that it had encrypted their files and that if they wanted to have them unencrypted that they would need to pay a ransom of .4 bitcoin.  I checked, and their files were, indeed, encrypted.  I did some research to figure out that I was dealing with the ransomware popularly known as Jigsaw.  To fix the problem, I opened the task manager and ended the firefox process.  I also stopped it from running on startup in the task manager.  I then downloaded a tool to unencrypt the files: Emsisoft Decryptor for Jigsaw.  I then updated their antivirus, ran a scan, and removed the Jigsaw Ransomware.  I told them that a good way to prevent this type of ransomware in the future would be to educate your employees about the risks of downloading attachments on emails or from unknown sources.  For a larger company, you might also want to have a endpoint detection and response suite (EDR) package to protect all of your endpoints.
![JH-10589-Ransomware-Analysis-5](https://user-images.githubusercontent.com/84826626/142263175-ad49504c-03aa-45b7-8051-e99b5208b786.png)
![JH-10589-Ransomware-Analysis-8](https://user-images.githubusercontent.com/84826626/142263240-678c5729-0367-4727-940a-f853ef99bfba.png)


###  Find the Culprit

The last story I completed was another pcap analysis story.  I received a pcap from an infected machine and was asked to investigate and find out how the infection happened.  I again was asked to find information on the victim, including IP address, MAC address, & Host Name, which I did using Wireshark to view the pcap and filtering the traffic by DHCP.  I was also asked to find the operating system which I did by filtering the traffic by http.request and following the tcp stream of a GET request on the infected IP.  I then noticed some strange traffic and searched the names of the hosts associated on Virustotal.com to find more information about them.  When I discovered the name of a malicious .exe file that they relate with, I searched about that, leading me to information about the infection that we were looking for.  I found out that this attack connects the infected host to a malicious payload on a Russian email server, which then loads a .jpg file containing a malicious .exe file trojan.  I think this is a variation of the infamous Zeus Trojan that is involved in banking credential theft.  It has similar files and file names such as cfg.bin, as well as similar executables.  I suggested the way to prevent this type of infection in the future would be to, again, not click on untrustworthy links in emails, especially on foreign email servers.  Continuous education of employees is required to make sure they are aware of this type of vulnerability.
![JH-10605-Find-Culprit-3](https://user-images.githubusercontent.com/84826626/142264743-74e58706-36fc-4b60-853d-61bde2210250.png)
![JH-10605-Find-Culprit-4](https://user-images.githubusercontent.com/84826626/142264839-1b192d9a-88b0-4c48-bc1f-f8265a292b6b.png)
![JH-10605-Find-Culprit-5](https://user-images.githubusercontent.com/84826626/142264926-a1131f6c-b56a-42b1-96e3-5d0e857ab6df.png)



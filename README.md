Red Vs Blue Project

## Day 1 Activity File: Red Team

### Monitoring Setup Instructions

- As the you attack a web server today, it will send all of the attack info to an ELK server.

- The following setup commands need to be run on the **Capstone** machine before the attack takes place in order to make sure the server is collecting logs.

- Be sure to complete these steps before starting the attack instructions.

**Instructions**

- Double click on the 'HyperV Manager' Icon on the Desktop to open the HyperV Manager.

- Choose the `Capstone` machine from the list of Virtual Machines and double-click it to get a terminal window.

- Login to the machine using the credentials: `vagrant:tnargav`

- Switch to the root user with `sudo su`

#### Setup Filebeat

Run the following commands:
- `filebeat modules enable apache`
- `filebeat setup`

The output should look like this:

![](../../../Images/ELk-Setup/filebeat.png)

#### Setup Metricbeat

Run the following commands:
- `metricbeat modules enable apache`
- `metricbeat setup`

The output should look like this:

![](../../../Images/ELk-Setup/Metricbeat.png)

#### Setup Packetbeat

Run the following command:
- `packetbeat setup`

The output should look like this:

![](../../../Images/ELk-Setup/Packetbeat.png)

Restart all 3 services. Run the following commands:
- `systemctl restart filebeat`
- `systemctl restart metricbeat`
- `systemctl restart packetbeat`

These restart commands should not give any output:

![](../../../Images/ELk-Setup/enable.png)

Once all three of these have been enabled, close the terminal window for this machine and proceed with your attack.

---

### Attack!

Today, you will act as an offensive security Red Team to exploit a vulnerable Capstone VM.

You will need to use the following tools, in no particular order:
- Firefox
- Hydra
- Nmap
- John the Ripper
- Metasploit
- curl
- MSVenom

### Setup

Your entire attack will take place using the `Kali Linux` Machine.

- Inside the HyperV Manager, double-click on the `Kali` machine to bring up the VM login window.

- Login with the credentials: `root:toor`

### Instructions

Complete the following to find the flag:

- Discover the IP address of the Linux web server.
- Locate the hidden directory on the web server.
    - **Hint**: Use a browser to see which web pages will load, and/or use a tool like `dirb` to find URLs on the target site.
- Brute force the password for the hidden directory using the hydra command:
    - **Hint**: You may need to use `gunzip` to unzip `rockyou.txt.gz` before running Hydra.
    - **Hint**: `hydra -l <username> -P <wordlist> -s <port> -f -vV <victim.server.ip.address> http-get <path/to/secret/directory>`
- Break the hashed password with the Crack Station website or John the Ripper.
- Connect to the server via WebDav.
    - **Hint**: Look for WebDAV connection instructions in the file located in the secret directory. Note that these instructions may have an old IP Address in them, so you will need to use the IP address you have discovered.
- Upload a PHP reverse shell payload.
    - **Hint**: Try using your scripting skills! MSVenom may also be helpful.
- Execute payload that you uploaded to the site to open up a meterpreter session.
- Find and capture the flag.

After you have captured the flag, show it to your instructor.

Be sure to save important files (e.g., scan results) and take screenshots as you work through the assessment. You'll use them again when creating your presentation.


## Day 2 Activity File: Incident Analysis with Kibana


Today, you will use Kibana to analyze logs taken during the Red Team attack. As you analyze, you will use the data to develop ideas for new alerts that can improve your monitoring.

**Important**: Any time you use data in a dashboard to justify an answer, take a screenshot. You'll need these screenshots when you develop your presentation on Day 3 of this project.

:warning: **Heads Up**: To complete today's part of the project, you must complete steps 1-6 from the last class. Finding the flag isn't critical, but you want to get past the point of uploading the reverse shell script.

### Instructions

Even though you already know what you did to exploit the target, analyzing the logs is still valuable. It will teach you:
- What your attack looks like from a defender's perspective.

- How stealthy or detectable your tactics are.

- Which kinds of alarms and alerts SOC and IR professionals can set to spot attacks like yours while they occur, rather than after.



#### Adding Kibana Log Data

To start viewing logs in Kibana, we will need to import our filebeat, metricbeat and packetbeat data.

Double-click the Google Chrome icon on the Windows host's desktop to launch Kibana. If it doesn't load as the default page, navigate to http://192.168.1.105:5601.

This will open 4 tabs automatically, but for now, we only want to use the first tab.

Click on the `Explore My Own` link to get started.

##### Adding Appache logs
Click on `Add Log Data`
![](../../../Images/Elk-setup-2/Add_logs.png)

Click on `Apache logs` 

![](../../../Images/Elk-setup-2/apache_logs.png)

Scroll to the bottom of the page. 
Click on `Check Data`
You should see a message highlighted in green: `Data successfully received from this module`

![](../../../Images/Elk-setup-2/apache_data.png)

Return to the Home screen by moving back 2 pages.

##### Adding System Logs
Click on `Add Log Data`


![](../../../Images/Elk-setup-2/Add_logs.png))


Click on `System logs` 

![](../../../Images/Elk-setup-2/system_logs.png)

Scroll to the bottom of the page. 
Click on `Check Data`
You should see a message highlighted in green: `Data successfully received from this module`

![](../../../Images/Elk-setup-2/system_data.png)

Return to the Home screen by moving back 2 pages.

#### Adding Apache Metrics
Click on `Add Metric Data`

![](../../../Images/Elk-setup-2/add_metrics.png)

Click on `Apache Metrics` 

![](../../../Images/Elk-setup-2/apache_metrics.png)

Scroll to the bottom of the page. 
Click on `Check Data`
You should see a message highlighted in green: `Data successfully received from this module`

![](../../../Images/Elk-setup-2/apache_metrics_data.png)

Return to the Home screen by moving back 2 pages.

#### Adding System Metrics
Click on `Add Metric Data`

![](../../../Images/Elk-setup-2/add_metrics.png)

Click on `System Metrics` 

![](../../../Images/Elk-setup-2/system_metrics.png)

Scroll to the bottom of the page. 
Click on `Check Data`
You should see a message highlighted in green: `Data successfully received from this module`

![](../../../Images/Elk-setup-2/system_metrics_data.png)

Close Google Chrome and all of it's tabs. Double click on Chrome to re-open it.

#### Dashboard Creation

Create a Kibana dashboard using the pre-built visualizations. On the left navigation panel, click on **Dashboards**.

Click on **Create dashboard** in the upper right hand side. 

![](../../../Images/Elk-setup-2/create_dashboard.png)

On the new page click on **Add an existing** to add the following existing reports:

- `HTTP status codes for the top queries [Packetbeat] ECS`
- `Top 10 HTTP requests [Packetbeat] ECS`
- `Network Traffic Between Hosts [Packetbeat Flows] ECS`
- `Top Hosts Creating Traffic [Packetbeat Flows] ECS`
- `Connections over time [Packetbeat Flows] ECS`
- `HTTP error codes [Packetbeat] ECS`
- `Errors vs successful transactions [Packetbeat] ECS`
- `HTTP Transactions [Packetbeat] ECS`

**Example for adding the first report:**

![](../../../Images/Elk-setup-2/add_existing.png)

![](../../../Images/Elk-setup-2/search.png)

The remaining steps will be a process of self-discovery to be completed without screen shot examples.

Get familiar with running search queries in the `Discover` screen with Packetbeat. This will be located on your fourth tab in Chrome. 

- On the Discover page, locate the search field.
- Start typing `source` and notice the suggestions that come up.
- Search for the `source.ip` of your attacking machine.
- Use `AND` and `NOT` to further filter you search and look for communications between your attacking machine and the victim machine.
- Other things to look for: 
	- `url`
	- `status_code`
	- `error_code`

After creating your dashboard and becoming familiar with the search syntax, use these tools to answer the questions below:


1. Identify the offensive traffic.
   - Identify the traffic between your machine and the web machine:
     - When did the interaction occur?
     - What responses did the victim send back?
     - What data is concerning from the Blue Team perspective?

2. Find the request for the hidden directory.
   - In your attack, you found a secret folder. Let's look at that interaction between these two machines.
     - How many requests were made to this directory? At what time and from which IP address(es)?
     - Which files were requested? What information did they contain?
     - What kind of alarm would you set to detect this behavior in the future?
     - Identify at least one way to harden the vulnerable machine that would mitigate this attack.

3. Identify the brute force attack.
   - After identifying the hidden directory, you used Hydra to brute-force the target server. Answer the following questions:
     - Can you identify packets specifically from Hydra?
     - How many requests were made in the brute-force attack?
     - How many requests had the attacker made before discovering the correct password in this one?
     - What kind of alarm would you set to detect this behavior in the future and at what threshold(s)?
     - Identify at least one way to harden the vulnerable machine that would mitigate this attack.

4. Find the WebDav connection.
   - Use your dashboard to answer the following questions:
     - How many requests were made to this directory? 
     - Which file(s) were requested?
     - What kind of alarm would you set to detect such access in the future?
     - Identify at least one way to harden the vulnerable machine that would mitigate this attack.

5. Identify the reverse shell and meterpreter traffic.
   - To finish off the attack, you uploaded a PHP reverse shell and started a meterpreter shell session. Answer the following questions:
     - Can you identify traffic from the meterpreter session?
     - What kinds of alarms would you set to detect this behavior in the future?
     - Identify at least one way to harden the vulnerable machine that would mitigate this attack.

Day 3 Activity File: Reporting
Congratulations, you've made it! You've worn two hats this week, playing the roles of both attacker and defender. Don't underestimate the magnitude of this achievement: Learning enough to infiltrate a machine and analyze data collected during the attack is a milestone that takes many professionals a long time to achieve.
Today, you'll take a break from flexing your technical skills and focus on communicating what you've learned in the past two days. In a real engagement, your client pays you not to break into their network, but to teach them how to protect it. This is why communication skills are vital in the cybersecurity field.
To that end, you will summarize your work in a presentation containing the following sections:


Network Topology

What are the addresses and relationships of the machines involved?
All of the VMs in the attack should be described. Optionally, you can also include the hypervisor machine itself.



Red Team

What were the three most critical vulnerabilities you discovered?

Choose the three vulnerabilities that you consider to be most critical.





Blue Team

What evidence did you find in the logs of the attack?
What data should you be monitoring to detect these attacks next time?



Mitigation

What alarms should you set to detect this behavior next time?
What controls should you  put in place on the target to prevent the attack from happening?




Instructions
Open the template on Google Slides: Project 2 Report Template


Make a copy by clicking File > Make a Copy.


Fill out the prompts on the slides as indicated. Make sure to remove all instructional text and prompts.


Some examples of vulnerabilities to look for are:

Sensitive Data Exposure
Unauthorized File Upload
Remote Code Execution
Brute Force Vulnerability
Local File Inclusion
Cross Site Scripting
Code Injection
SQL Injection
Security Misconfiguration

# Simple-Elastic-SIEM
Using a Kali Linux VM and Elastic cloud to a setup a basic SIEM. Inspired by Abdullahi Ali

Prerequisites: VirtualBoc and knowledge kali Linux

Step1: Setting up Elastic account
1. set up an Elastic Clous account at https://cloud.elastic.co/registration
2. CLick on "Create Deployment" and select "Elasticsearch" as deployment type
3. Choose a region and deployment size that fits your needs and click on “Create Deployment.”
4. Wait for the configuration to complete.
5. Once the deployment is ready, click “continue.”

Step2: Setting up Linux VM
1. Download the Kali Linux VM from the official Kali website at https://www.kali.org/get-kali/#kali-virtual-machines.
2. Create a new VM with the Kali VM file in your preferred virtualization platform, such as VirtualBox or VMware.
3. Start the VM and follow the on-screen prompts to install Kali.
4. Once the installation is complete, log in to the Kali VM using the credentials “kali” for both the username and password.

Step3: Setting up the Agent to collect logs
1. Log in to your Elastic SIEM instance and navigate to the Integrations page by: clicking on the Kibana main menu bar at the top left, then selecting “Integrations” at the bottom.
2. Search for “Elastic Defend” and click on it to open the integration page.
3. Click on “Install Elastic Defend” and follow the instructions provided on the integration page to install the agent on your Kali VM.
Make sure “Linux” is selected and then copy that command to your clipboard.
4. Paste that command into the Kali terminal (command line).
5. Once the agent is installed, which can take a few minutes, you’ll see a message that says “Elastic Agent has been successfully installed.” It will automatically start collecting and forwarding logs to your Elastic SIEM instance, although it might take a few minutes for the logs to appear in the SIEM.
You can verify that the agent has been installed correctly by running this command: sudo systemctl status elastic-agent.service
If you get an error installing the agent, make sure that your Kali is connected to the internet before proceeding by pinging google.com.

Step 4: Generating Security Events on the Kali VM
To verify that the agent is working correctly, you can generate some security-related events on your Kali VM. To do this, we can use a tool like Nmap. Nmap (Network Mapper) is a free and open-source utility used for network exploration, management, and security auditing. It is designed to discover hosts and services on a computer network, thus creating a “map” of the network. Nmap can be used to scan hosts for open ports, determine the operating system and software running on the target system, and gather other information about the network.
To run an Nmap scan, follow these steps:
Install Nmap on the Linux VM if you’re not using Kali, Nmap already comes preinstalled in Kali. Open a new Terminal and run this command to install it: sudo apt-get install nmap.
Run a scan on Kali machine by running the command: sudo nmap <vm-ip>. You can also run a scan of your host machine if you place your Kali VM on a “bridged” network.
An Nmap scan of my host machine.
3. This scan generates several security events, such as the detection of open ports and the identification of services running on those ports. Run a few more Nmap scans (“nmap -sS <ip address>”, “nmap -sT <ip address>”, “nmap -p- <ip address>”etc..”

Step 5: Querying for Security Events in the Elastic SIEM
1. Now that we have forwarded data from the Kali VM to the SIEM, we can start querying and analyzing the logs in the SIEM.
To do this, follow these steps:
Inside your Elastic Deployment, click on the menu icon at the top-left with the three horizontal lines and then click on the “Logs” tab under “Observability” to view the logs from the Kali VM.
2. In the search bar, enter a search query to filter the logs. For example, to search for all logs related to Nmap scans, enter the query: event.action:
“nmap_scan” or process.args: “sudo”.
3. Click on the “Search” button to execute the search query.
But please note that it can sometimes take a while for the events to populate and show up on the SIEM, so this query might not work right away.
4. The results of the search query will be displayed in the table below. You can click on the three dots next to each event to view more details.



By generating and analyzing different types of security events in Elastic SIEM like the one above, or generating authentication failures by typing in the wrong password for a user or attempting SSH logins an incorrect password, you can gain a better understanding of how security incidents are detected, investigated, and responded to in real-world environments.

Step 6: Create a Dashboard to Visualize the Events
You can also use the visualizations and dashboards in the SIEM app to analyze the logs and identify patterns or anomalies in the data. For example, you can create a simple dashboard that shows a count of security events over time.
Here’s how you can do that:

1.Navigate to the Elastic web portal at https://cloud.elastic.co/.
2.Click on the menu icon on the top-left, then under “Analytics,” click on “Dashboards.”
3. Click on the “Create dashboard” button on the top right to create a new dashboard.
4. Click on the “Create Visualization” button to add a new visualization to the dashboard.
5.. Select “Area” or “Line” as the visualization type, depending on your preference. This will create a chart that shows the count of events over time.
6. In the “Metrics” section of the visualization editor on the right, select “Count” as the vertical field type and “Timestamp” for the horizontal field. This will show the count of events over time.
Click “close” once you’re done.
7. Click on the “Save” button to save the visualization and then complete the rest of the settings.


Step 7: Create an Alert
In a SIEM, alerts are a crucial feature for detecting security incidents and responding to them in a timely manner. Alerts are created based on predefined rules or custom queries, and can be configured to trigger specific actions when certain conditions are met. In this task, we will walk through the steps of creating an alert in the Elastic SIEM instance to detect Nmap scans. By following these steps, you can create an alert that will monitor your logs for Nmap scan events and then notify you when they are detected.

Here are the steps:
1. Click on the menu icon on the top-left, then under “Security,” click on “Alerts.”
2. Click on “Manage rules” at the top right.
3. Click on the “Create new rule” button at the top right.
4. Under the “Define rule” section, select the “Custom query” option from the dropdown menu.
5. Under “Custom query,” set the conditions for the rule. You can use the following query to detect Nmap scan events.
This query will match all events with the action “nmap_scan.” Then click “Continue.”
6. Under the “About rule” section, give your rule a name and a description (Nmap Scan Detection).
7. Set the severity level for the alert, which can help you prioritize alerts based on their importance. Keep all the other default settings under “Schedule rule” and click “Continue.”
8. In the “Actions” section, select the action you want to take when the rule is triggered. You can choose to send an email notification, create a Slack message, or trigger a custom webhook.
9.Finally, click the “Create and enable rule” button to create the alert.


Once you’ve created the alert, it will monitor your logs for Nmap scan events. If an Nmap scan event is detected, the alert will be triggered and the selected action will be taken. You can view and manage your alerts on the “Alerts” section under “Security.”

The full tutorial can be found at: https://medium.com/@aali23/a-simple-elastic-siem-lab-6765159ee2b2


# Hands-On SOC Experience: Deploying a SIEM System Using Azure and Microsoft Sentinel

This project involves creating a Security Operations Center (SOC) using Microsoft Azure and Sentinel, with a focus on deploying a Security Information and Event Management (SIEM) system. The project includes setting up a virtual machine with a Windows Server, configuring Microsoft Sentinel to monitor devices and generate alerts, and establishing a threat intelligence feed that sends commonly seen and newly found indicators of compromise to the SIEM. By monitoring RDP events, users can better understand and mitigate potential security risks.

---






### Creating VM

I deployed an Azure Virtual Machine using a preset configuration and created a resource group to serve as the container for all project components. The virtual machine is running the selected operating system (Windows 11 Pro), and to facilitate immediate traffic, I enabled Remote Desktop Protocol (RDP) by opening Port 3389 in the VM's network settings. While I acknowledge that exposing this port to the internet increases the system's vulnerability to external threats, RDP access is essential for this project. Securing the RDP connection is crucial to mitigating potential risks while maintaining the required functionality.


![Screenshot](https://s11.gifyu.com/images/S1X2F.png)




---
### Creating SIEM

I deployed Microsoft Sentinel, a cloud-based Security Information and Event Management (SIEM) platform. Sentinel was added to the Azure Log Analytics workspace, which serves as a centralized container for all resources, configuration management systems (CMS), and SIEM tools. I ensured the workspace was located in the same resource group and region as the virtual machine for seamless integration and optimal performance.

![Screenshot](https://s1.gifyu.com/images/S1Xby.png)
###
### Linking SIEM To Endpoint

Although our Windows VM is logging events like a typical Windows OS, they are not immediately visible. To capture these events, we need to pull them from the endpoint (our VM) and send them to Sentinel. This involves setting up a data connector, a data connector is a tool that integrates data from different sources into Microsoft Sentinel for monitoring and analysis. In this case, we use the Azure Monitor Agent as the data connector to link the VM’s event logs to Sentinel. Afterward, we'll configure a data collection rule. A data collection rule defines what data to collect from the endpoint (our VM) and how it should be processed. It specifies which logs or metrics to pull from the VM and ensures they are sent to the Log Analytics workspace for further analysis in Sentinel.

![Screenshot](https://s1.gifyu.com/images/S1Xzx.png)
##
### Creating Sentinel Rule

I created a rule in Microsoft Sentinel to track successful RDP sign-ins, excluding system account logins. The rule, named "Successful Local Sign-Ins," is set with medium severity and aligned with the MITRE ATT&CK framework's "Initial Access" tactic. The rule logic is defined as:

> | where Activity contains "success" and Account !contains "system"

This rule is configured to trigger every 5 minutes automatically. I set the alert threshold to 0 and grouped all related events. You can review this rule in the Analytics tab of the Microsoft Sentinel page.

![Screenshot](https://s11.gifyu.com/images/S1Xzw.png)
### 
### Test Run 
We have successfully configured event logs to be forwarded to the log analysis workspace and alerts are now set to be sent to Microsoft Sentinel for monitoring and analysis. Every five minutes, an alert will be triggered for successful login sign-ins.

![Screenshot](https://s1.gifyu.com/images/S1Xz6.png)

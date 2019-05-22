# Rsyslog walkthrough for Azure Sentinel

Microsoft Azure Sentinel is a scalable, cloud-native, security information event management (SIEM) and security orchestration automated response (SOAR) solution. Azure Sentinel delivers intelligent security analytics and threat intelligence across the enterprise, providing a single solution for alert detection, threat visibility, proactive hunting, and threat response.

With that being said, in order for Azure Sentinel to provide all of these funtionalies we need to ingest data from our applications and services into Azure Sentinel's Log Analytics workspace.

Azure Sentinel provides the following data connectors: https://docs.microsoft.com/en-us/azure/sentinel/connect-data-sources#data-connection-methods

This walkthrough is in the specific sernario of getting your appliance (Like a Firewall), sending the logs over Syslog Deamon (Rsyslog), using the OMS agent to phone home to the ODS Service endpoint.

To connect your external appliance to Azure Sentinel, the agent must be deployed on a dedicated machine (VM or on premises) to support the communication between the appliance and Azure Sentinel. You can deploy the agent automatically or manually. Automatic deployment is only available if your dedicated machine is a new VM you are creating in Azure.

Requirements before proceeding forwward:

-   You've onboarded Azure Sentinel to a workspace within your orgnization
https://docs.microsoft.com/en-us/azure/sentinel/quickstart-onboard
-   You'll need access to deploy a Virtual Machine that can communicate to the workspace


Good knowlage to know before starting:

- Understanding Syslog: https://stackify.com/syslog-101/ 
- Ubuntu Server: https://www.ubuntu.com/download/server
- Rsyslog : https://www.rsyslog.com/
- OMS Agent for Linux: https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-linux-computer
- Data Sources for Azure Log Analytics Workspaces: https://docs.microsoft.com/en-us/azure/azure-monitor/platform/data-sources-syslog 
- SSH Enabled for Ubuntu: https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-18-04/ 




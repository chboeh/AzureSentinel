# Investigating an issue with Kusto Query Language

## Getting prepared for the exercise

[ ] Open up the [Azure Log Analytics](http://aka.ms/LAdemo) demo site

[ ] Open up the [Kusto Query Refence Guide](https://docs.microsoft.com/en-us/azure/kusto/query/)


Operators  we're going to be using

Basic Operators     | Advance Operators    
--------- | ---------  
 search   | ago
 where    | sort
 take     | translate
 count    | iif
 summarize | parse_json
 extend | todatetime
 project | tostring
 distinct | split

## Goal of this training:

In this tutorial you will learn how to use Azure Monitor Log Analytics in the Azure portal to write Azure Monitor log queries. 
It will teach you how to:
- Write simple queries
- Understand the schema of your data
- Filter, sort, and group results
- Apply a time range
- Create charts
- Parse Json queries
- Create your own reports
- Renaming with "project"
- Creating custom data sets with "extend"
- Pulling data  "extract"
- Save and load queries
- Export and share queries

### BackStory
You’re an Security Analyst that’s been tasked to investigate into a Web Service called “ContosoAppSrv1”, you've been given a "Case" to investigate into a large up-take in volume on this server. Nothing is being alerted based on the security tools your team uses on a daily basis, but your manager feels like it's worth investigating into.

It’s been some time since you’ve used Microsoft’s Azure Log Analytics, you’re thinking you should refresh yourself before investigating.

#### Refresher Course

1. Start by pulling everything from "ProtectionStatus" table from the AntiMalware Catagory 

[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLRefresher_1.md)

2. The results are a little over whelming, take it down to a sample set of data. We're going to take it down to 10 

[Hint](https://docs.microsoft.com/en-us/azure/kusto/query/takeoperator)  
[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLRefresher_2.md)  


3. You're wanting to make sure you're getting the latest data, add a operator to confirm you're getting the latest generated events.

[Hint](https://docs.microsoft.com/en-us/azure/kusto/query/sortoperator)  
[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLRefresher_3.md)  


4. Remove your limiters; Within the "ProtectionStatus" table, search for "Contoso" then "Cotoso*"
> TIP: this can be done in more than one way

[Hint](https://docs.microsoft.com/en-us/azure/kusto/query/searchoperator)    
[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLRefresher_4.md)

5. You've noticed the run button is not working, how else can you run a query with just your keyboard?

[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLRefresher_5.md)


6. Understanding the schema

> INFO: Examine the scheme. Specifically look into the "Security" category that has been provided, as you'll notice there are mulitlpe tables ( e.g. CommonSecurityLog, SecurityEvent, SecurityAlert ). In each table, data is organized in columns with different data types as indicated by icons next to the column name. For example, the Event table shown in the screenshot contains columns such as Computer which is text, EventCategory which is a number, and TimeGenerated which is date/time.

[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLRefresher_6.md)

#### Starting the Investigation

Based on the "case" you’ve been provided, you know the following facts
-	You’re looking for “ContosoAppSrv1” within your Log Analytics workspace
-	Your boss is concerned about the increase of events from one of our web servers within the company
-	You’ve been informed that we’ve been getting a large volume of event data from this server, although it’s not clear exactly how much is a large volume.
    >	*Personal Note* - Will need to collect a report to show the cloud infrastructure team
-	If we do discover something, we need to report it directly to management in a clean fashion. They’re only wanting to see the following data, ServerName, Users impacted, Services Impact, Event window(Start and End), Total time of the event, Attacker information and if there we were infiltration. You were scolded the last time you presented raw data.


1. Knowing from experience, we're going to start pulling everything from "SecurityEvent", then limit it to 1000. Pull this query to see the current data before we filter further.

[Hint](https://docs.microsoft.com/en-us/azure/kusto/query/limitoperator)  
[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_1.md)

2. You've been asked to look into the computer name "ContosoAppSrv1" within the SecurityEvent table; keep the limit for a faster result. 

[Hint](https://docs.microsoft.com/en-us/azure/kusto/query/whereoperator)  
[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_2.md)

3. Remove the limit; Find the amount of events "ContosoAppServ1" has generated within the past 24 hours; within the SecurityEvent table.

> Number of results: Results are limited to maximum of 10,000 records.

[Hint 1](https://docs.microsoft.com/en-us/azure/kusto/query/agofunction)    
[Hint 2](https://docs.microsoft.com/en-us/azure/kusto/query/countoperator)  
[Anwser](Training\Answers\KQLInvestigation_2.md)

4. While this amount does seem high, it doesn't appear abnormal for you. You're wanting to see how many tables have "ContosoAppSrv1" within them. Serach within all tables for "ConotosAppServ1" to create a list. When searching use within 30 mins.

[Hint](https://docs.microsoft.com/en-us/azure/kusto/query/distinctoperator)   
[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_4.md)

5. After seeing the activity across so many tables, you're wanting to present a chart to the infrastructure team. Create a pie chart showing the traffic over the past month with all event tables, goal is to summarize them by count per table.

[Hint](https://docs.microsoft.com/en-us/azure/kusto/query/summarizeoperator)  
[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_5.md)

Questions: 
 1. Which tables are on the top 3 count list?

[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_5_A.md)

 2. Which tables are on the bottom 3 count list?

 [Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_5_B.md)

 3. Can you export the data to PowerBI? Find out how

 [Answer](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/powerbi#export-query)

>Need to know answer *two* to proceed forward

6. From your discovery of creating the chart, you noticed a small amount of detections. Investigate into the detections of the "ContosoAppSrv1"
>Note:You'll need to keep the 1 month timewindow

[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_6.md)

7. After careful investigaiton you've discovered a user login attempt has occoured. Thankfully it failed.  You're needing to report this to your manager, but the data is a little over overkill. Create the following fields and clean up the data for easier understanding:

- Computer Name
- Impacted account
- Start of the attack time
- End of the attack time
- Total time duration of the attack
- Was the attack sucessful?
- Attacker source IP

[Hint1](https://github.com/chboeh/MsftEntropy/blob/master/Training/Hints/CTF_Hint1.md)  
[Hint2](https://github.com/chboeh/MsftEntropy/blob/master/Training/Hints/CTF_Hint2.md)  
[Hint3](https://github.com/chboeh/MsftEntropy/blob/master/Training/Hints/CTF_Hint3.md) - Hint 2 Answer  
[Hint4](https://github.com/chboeh/MsftEntropy/blob/master/Training/Hints/CTF_Hint4.md) - Hint 3 Answer  
[Hint5](https://github.com/chboeh/MsftEntropy/blob/master/Training/Hints/CTF_Hint5.md) - Hint 4 Answer  

[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLInvestigation_7.md)

8. Woo! Hard work paid off, don't forget to save your query for future use. Don't want to have to build that again. Export the results into CSV format and save for future use.
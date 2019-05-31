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
You’re as Security Analysts that’s been tasked to investigate into a Web Service called “ContosoAppSrv1”, the tasked informed you there has been a large up-take in volume on this server and you’ve been tasked to confirm if everything is working as expected, as well to look over the log volume to make sure everything has been configured correctly.

It’s been some time since you’ve use Microsoft’s Azure Log Analytics, you’re thinking you should refresh yourself before investigating.

#### Refresher Course

1. Start by pulling everything from "ProtectionStatus" table from the AntiMalware Catagory 

[Anwser](https://github.com/chboeh/MsftEntropy/blob/master/Training/Answers/KQLRefresher_1.md)

~~~
ProtectionStatus
~~~

2. The results are a little over whelming, take it down to a sample set of data. We're going to take it down to 10

~~~
ProtectionStatus
| take 10
~~~

3. You're wanting to make sure you're getting the latest data, add a line to confirm you're getting the latest generated events.

~~~~
ProtectionStatus
| sort by TimeGenerated desc
| take 10
~~~~


2. Remove your limiters; Within the "ProtectionStatus" table, search for "Contoso" then "Cotoso*"
> TIP: this can be done in more than one way

3. You've noticed the run button is not working, how else can you run a query with just your keyboard?


4. Understanding the schema

> INFO: Examine the scheme. Specifically look into the "Security" category that has been provided, as you'll notice there are mulitlpe tables ( e.g. CommonSecurityLog, SecurityEvent, SecurityAlert ). In each table, data is organized in columns with different data types as indicated by icons next to the column name. For example, the Event table shown in the screenshot contains columns such as Computer which is text, EventCategory which is a number, and TimeGenerated which is date/time.

#### Starting the Investigation

Based on the "case" you’ve been provided, you know the following facts
-	You’re looking for “ContosoAppSrv1” within your Log Analytics workspace
-	Your boss is concerned something could have been missed within the data, this is an important web service the company uses and it's worth our time to confirm everything is ok.
-	You’ve been informed that we’ve been getting a large volume of event data from this server based on a reporting tool, although it’s not clear exactly how much.
    >	*Personal Note* - Might be best to capture some data to give the appropriate team information on why we’re getting so many events.
-	-	If we do discover something, we need to report it directly to management in a clean fashion. They’re only wanting to see the following data, ServerName, Users impacted, Services Impact, Event window(Start and End), Total time of the event, Attacker information.


1. Start pulling everything from "SecurityEvent", then limit it to 1000.

[Hint1](https://docs.microsoft.com/en-us/azure/kusto/query/limitoperator)


~~~
SecurityEvent
| limit 1000
~~~

2. You've been asked to look into the computer name "ContosoAppSrv1" within the SecurityEvent table; keep the limit for a faster result.

[Hint1](https://docs.microsoft.com/en-us/azure/kusto/query/whereoperator)

~~~
 SecurityEvent
| limit 1000
| where Computer == "ContosoAppSrv1" 
~~~
3. Remove the limit; Find the amount of events "ContosoAppServ1" has generated within the past 24 hours; within the SecurityEvent table.

> Number of results: Results are limited to maximum of 10,000 records.

[Hint1](https://docs.microsoft.com/en-us/azure/kusto/query/agofunction)

[Hint2](https://docs.microsoft.com/en-us/azure/kusto/query/countoperator)

~~~
SecurityEvent 
| where Computer == "ContosoAppSrv1"
| where TimeGenerated > ago(24h) 
| count 
~~~
> Should be around 27,000+

4. While investigating you're wanting to see how many tables have "ContosoAppSrv1" within them. Serach within all tables for "ConotosAppServ1" to create a list. When searching use within 30 mins.

[Hint1](https://docs.microsoft.com/en-us/azure/kusto/query/distinctoperator)
~~~
search "ContosoAppSrv1"
| where TimeGenerated > ago(30m) 
| distinct $table
~~~
5. After seeing the activity across so many tables, you're wanting to present a chart to the infrastructure team. Create a pie chart showing the traffic over the past month with all event tables, goal is to summarize them by count per table.

[Hint1](https://docs.microsoft.com/en-us/azure/kusto/query/summarizeoperator)

~~~
search "ContosoAppSrv1" 
| where TimeGenerated > ago(31d) 
| summarize count() by $table 
~~~

Questions: 
 1. Which tables are on the top 3 count list?
> Perf, WindowsFirewall, WireData
 2. Which tables are on the bottom 3 count list?
> SecurityDetection,UpdateSummary,SecurityAlert

6. From your discovery of creating the chart, you noticed a small amount of detections. Investigate into the detections of the "ContosoAppSrv1"
>Note:You'll need to keep the 1 month timewindow

~~~~
SecurityDetection
| where TimeGenerated > ago(31d) 
| where Computer == "ContosoAppSrv1"
~~~~

7. After careful investigaiton you've discovered a user login attempt has occoured. Thankfully it failed.  You're needing to report this to your manager, but the data is a little over overkill. Create the following fields and clean up the data for easier understanding:

- Computer Name
- Impacted account
- Start of the attack time
- End of the attack time
- Total time duration
- Attack Status, yes or no
- Attacker source IP

[Hint1](https://docs.microsoft.com/en-us/azure/kusto/query/projectoperator)  
[Hint2](https://docs.microsoft.com/en-us/azure/kusto/query/parsejsonfunction)   
[Hint3](https://docs.microsoft.com/en-us/azure/kusto/query/translatefunction)   
[Hint4](https://docs.microsoft.com/en-us/azure/kusto/query/todatetimefunction)  
[Hint5](https://docs.microsoft.com/en-us/azure/kusto/query/tostringfunction)  

~~~~
SecurityDetection
| where TimeGenerated > ago(31d) 
| where Computer == "ContosoAppSrv1"
| extend end_time_key = todatetime(translate("/", "-", tostring(parse_json(ExtendedProperties).["Activity end time (UTC)"])))
| extend start_time_key = todatetime(translate("/", "-", tostring(parse_json(ExtendedProperties).["Activity start time (UTC)"])))
| project Computer,
          AttackStatus = iff(parse_json(ExtendedProperties).["Was RDP session initiated"] == "Yes","Infiltrated" ,"Safe" ) ,
          AttackedUserAccount=parse_json(ExtendedProperties).["Existing accounts used but failed to sign in to host"],
          Attack_StartTime=start_time_key ,
          Attack_EndTime=end_time_key ,
          Attackers_IPAddress=tostring(split(parse_json(ExtendedProperties).["Attacker source IP"],":").[1])
| extend Attack_Duration = Attack_EndTime - Attack_StartTime      
~~~~

## Answers

~~~
SecurityEvent 
| where Computer == "ContosoAppSrv1"
| where TimeGenerated > ago(24h) 
| count 
~~~
> Should be around 27,000+
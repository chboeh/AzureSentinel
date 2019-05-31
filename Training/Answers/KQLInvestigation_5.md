## Answers

~~~
search "ContosoAppSrv1" 
| where TimeGenerated > ago(31d) 
| summarize count() by $table 
~~~
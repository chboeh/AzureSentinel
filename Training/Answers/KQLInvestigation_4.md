## Answers

~~~
search "ContosoAppSrv1"
| where TimeGenerated > ago(30m) 
| distinct $table
~~~
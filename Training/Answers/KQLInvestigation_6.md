## Answers

[Quick Link](https://portal.loganalytics.io/Demo?q=H4sIAAAAAAAAAwtOTS4tyiypdEktSU0uyczP4%2BWqUSjPSC1KVQjJzE11T81LLUosSU1RsFNITM%2FXMDZM0VRAKHHOzy0oLUktUrC1VVByzs8ryS%2FOdywoCC4qM1TiAgDxEaTSWwAAAA%3D%3D)
~~~~
SecurityDetection
| where TimeGenerated > ago(31d) 
| where Computer == "ContosoAppSrv1"
~~~~
![Answer](https://github.com/chboeh/MsftEntropy/blob/master/Training/Pictures/KQLInvestigation_6.png)
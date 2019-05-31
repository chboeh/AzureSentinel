## Answers

[Quick Link](https://portal.loganalytics.io/Demo?q=H4sIAAAAAAAAA1MITk0uLcosqXQtS80r4eWqUcjJzM0sUTA0MDAA8cozUotSFZzzcwtKS1KLFGxtFZSc8%2FNK8ovzHQsKgovKDJUUuAAdSwxNRAAAAA%3D%3D&timespan=P1D)
~~~
 SecurityEvent
| limit 1000
| where Computer == "ContosoAppSrv1" 
~~~
![Answer](https://github.com/chboeh/MsftEntropy/blob/master/Training/Pictures/KQLInvestigation_2.png)
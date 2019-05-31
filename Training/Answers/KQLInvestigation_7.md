## Answers


[Quick Link](https://portal.loganalytics.io/Demo?q=H4sIAAAAAAAAA7VSwUrDQBC9F%2FyHYU8JtErxJkQIbZHeiqmISAlrdlJX627YmVQFP97ZKLVqoF5cyDJJZt97%2B94UWLXB8usUGSu23h0N3uD5HgPC0j7hBToMmtHAOei1T07HJoWvlol%2FalrGAFkGauIde%2FJ50xRhO1axC18YnQF5Sha08hFfIQP2RiDjh4SDdrSRt0SdqCGokWwCwsG6ddLoQFg%2BkHfJrANCswi%2BwcAWKT2%2BVbko3or4SAARD5Kr5SRVqzRN9%2BiJdeD%2FFdBR9Elogn8QY3dODY8GsFs5s64eC9bcksiydX2Y8loTXE4XQEgkcYF1lm1MSK26FG6Q1FDNXW033CWnYKgKXaOCFHrY0VwRhryqfOs4O0g%2Fe7HE4g3ojxMELcl03LUMtbYbKdkD2XUUFst78VKtenjLIloWZyz7kU9f88yZrvXbJPXdJlA5X%2BTGBLEn2wVJzcbyH%2BL8xADybagQ5guRrs6U%2FBuv9ifqU9W0FYdjCNkPnTD6dcsPlYN3qqCCknADAAA%3D)
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
![Answer](https://github.com/chboeh/MsftEntropy/blob/master/Training/Pictures/KQLInvestigation_7.png)
## Answers

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

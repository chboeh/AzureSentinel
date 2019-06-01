

Stuck?

 Example:
~~~~
d:dynamic, key:string

 todatetime(translate("/", "-", tostring(parse_json(d)[key])))
~~~~

click on todatetime for the last hint answer

[todatetime](https://portal.loganalytics.io/Demo?q=H4sIAAAAAAAAA7VSy2rDMBC8B%2FIPi042uC2hZxdMEkpvoU4opQSj2OtEeUhGu04T6Md35UIaiiGnCiRWYjUzzGyOZesNnyfIWLJxdjj4gs8NeoS5OeAzWvSasYIn0GsXPY6qGH5bxu7QtIwe0hTU2Fl25LKmyf1xpEIXnhhtBbILFrRih2dIgV0lkOEhYq8t7eUWqQeVgLqTQ0DYG7uOGu0Jiy05G007IKxm3jXo2SDF9x8qE8VHER8IIOBBtJiPY7WM4%2FiKnlh7%2Fl8BHUWfhMa7rRh7cSoZDuCyMmZd7nLW3JLIMnV9m%2FJNE7xOZkBIJHGBsYZNSEgtuxTekVSiXmxt9twlpyBRua5RQQw97FgtCH1Wlq61nN6kn54MsXgD%2BucHQUsyHauWodZmLyU7ILMOwkK5ES%2FVsoe3yINlYcbSP%2Fn0NU9t1bVeT9I3YZfeL7sCAAA%3D)

You're almost done collected the list of data, just missing two requirements

1. Attackers IP address
2. Attack Duration

I'm pretty sure the Attack duation could be solved with an [extend operator](https://docs.microsoft.com/en-us/azure/kusto/query/extendoperator)

The IP Address one is pretty stright forward but i keep getting "IP address:" in the data, i remember reading somewhere [split](https://docs.microsoft.com/en-us/azure/kusto/query/splitfunction) could help me out here.
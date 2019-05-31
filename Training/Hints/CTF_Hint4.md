Stuck? click on iff for the last hint answer

[iif](https://portal.loganalytics.io/Demo?q=H4sIAAAAAAAAA4WQTUvDQBCG74H8h2FPCQSheI4Q2lK8lUYRkSJrdpJOP3aWnVmt4I83qVAvgnN6D8%2FM%2BzAtdimSfi5QsVNin2df8LHDiPBAJ1yhx2gVHdyBHbi4nbkSfpE5n0JSjFDXYObslYWbENr4PjMTFSLvx7NXrsozuE6jartDq1aTQA3U90WwUfB1L%2ByL5VnRO3TryAGjEkp582KerMBmsQZBkVEWyJPS5Ge2F4dnFFOZe9%2FTUS%2FeBirT2h4NlPBHO7pHwdh0HSev9b%2F1yzOJkh%2FA%2FmwIJBl%2F85YUekvHMSqD0DCJTXHHomabZ9%2BlmTB1ZgEAAA%3D%3D)

After using every search engine possible, looking up the queries i need, i still can't figure out how to collect the total time of the event cleanly within the JSON dataset. I reached out to my team and asked for her, thankfully my good bud "Timmy" threw me a bone. 

He explained to me the reason it wasn't working was due to the formating of the date within the JSON table. You'll have to change the format to a supported datetime within Kusto.  [datetime Supported Formats](https://docs.microsoft.com/en-us/azure/kusto/query/scalar-data-types/datetime#supported-formats)

In order to query the data, you'll hvae to use the [todatetime](https://docs.microsoft.com/en-us/azure/kusto/query/todatetimefunction
 ) to convert the time into a scalar, but first you'll have to [translate](https://docs.microsoft.com/en-us/azure/kusto/query/translatefunction) it to the proper format because the formating isn't going to ever work within kusto.

 After testing, i kept getting a string expection, after some tinkering i discovered using the [tostring](https://docs.microsoft.com/en-us/azure/kusto/query/tostringfunction) did the trick when getting the JSON data into a string representation.

 Example:
~~~~
d:dynamic, key:string

 todatetime(translate("/", "-", tostring(parse_json(d)[key])))
~~~~

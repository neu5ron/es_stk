# Security Use Case Elasticsearch Analyzer

This is a custom elasticsearch analyzer built with security logging use cases in mind.

Information about the usefulness/necessity of a custom analyzer **even when running Elastic Common Schema (ECS)** can be found [here](https://github.com/elastic/ecs/issues/1837).

The analyzer will lowercase the values and remove extra whitespaces. More importantly it retains the functionality that makes elasticsearch a useful string searching tool besides just using it as a "contains" query database. For example this still allows you to do fuzzy/levenshtein distance queries.
 
 
Problems this helps alleviate are well noted in this [blog](https://socprime.com/blog/elastic-for-security-analysts-part-1-searching-strings/).
 
 
 ## Example and Testing
 
 You can test the analyzer by the following (currently just showing from Kibana GUI, but you could use cURL or other commandline/scripting you want).
 **NOTE** this is not applying to your index, this is just so you can test the analyzer. You can apply the template however you want, but want to point this out just in case.
 1. Go to Kibana and select the "Dev Tools" tab on the left side. 
 1. Copy the contents of [index template](es_stk_template.json) and paste into "Dev Tools"
 1. Copy and paste `PUT /_template/es_stk_template` above the line from the previous step.
 1. Then run (ctrl+enter) the input from the previous step.
 ![image](https://user-images.githubusercontent.com/6351842/159288886-c5796e06-5d1c-4066-92e9-3c32632020d2.png)
 1. Create index to use this template, copy and paste (and run) the following in "Dev Tools":
 
 `PUT /es_stk_test` 


 1. Now copy and paste the following into "Dev Tools". **NOTE** You can replace the content after "text" with whatever values you want to test.
 ```
POST /es_stk_test/_analyze
{
  "analyzer": "es_stk_analyzer",
  "text": """C:\Users\test\rundll32.exe   -exec       a bypass ^T^e^S^t  """
}
```
Example output:
```text
{
  "tokens" : [
    {
      "token" : """c:\users\test\rundll32.exe -exec a bypass ^t^e^s^t""",
      "start_offset" : 0,
      "end_offset" : 60,
      "type" : "word",
      "position" : 0
    }
  ]
}
```
 
 
 # Housekeeping
 
 ## #TODO:
- Leave feedback if you are using this!
  
 ## Known Issues
 N/A
 
 
 
 
 ## ETC
 Meaningless side note:
 The term `stk` (in the repo name) means `search to know`...

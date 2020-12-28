# Security Use Case Elasticsearch Analyzer

This is a custom elasticsearch analyzer built with security logging use cases in mind.

The analyzer will lowercase the values and then [tokenize](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-tokenizers.html) values based upon
- `whitespace`
- `\ `
- `/`
 
 
 Problems this solves are well noted in this [blog](https://socprime.com/blog/elastic-for-security-analysts-part-1-searching-strings/).
 
 
 ## Example and Testing
 
 You can test the analyzer by the following (currently just showing from Kibana GUI, but you could use cURL or other commandline/scripting you want).
 **NOTE** this is not applying to your index, this is just so you can test the analyzer. You can apply the template however you want, but want to point this out just in case.
 1. Go to Kibana and select the "Dev Tools" tab on the left side. 
 1. Copy the contents of [index template](es_stk_template.json) and paste into "Dev Tools"
 1. Copy and paste the following before the line that you just pasted  
 `PUT /es_stk_test` 
 1. Now copy and paste the following into "Dev Tools". **NOTE** You can replace the content after "text" with whatever values you want to test.
 ```
POST /es_stk_test/_analyze
{
  "analyzer": "es_stk_analyzer",
  "text": """C:\Users\test\example_binary.exe -exec bypass ^t^e^s^t"""
}
```
Example output:
```text
{
  "tokens" : [
    {
      "token" : """c:\users\test\example_binary.exe""",
      "start_offset" : 0,
      "end_offset" : 32,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : """c:\users\test\""",
      "start_offset" : 0,
      "end_offset" : 32,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "example_binary.exe",
      "start_offset" : 0,
      "end_offset" : 32,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "-exec",
      "start_offset" : 33,
      "end_offset" : 38,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "bypass",
      "start_offset" : 39,
      "end_offset" : 45,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "^t^e^s^t",
      "start_offset" : 46,
      "end_offset" : 54,
      "type" : "word",
      "position" : 3
    }
  ]
}
```
 
 
 # Housekeeping
 
 ## #TODO:
 - look into https://www.elastic.co/guide/en/elasticsearch/reference/7.10/index-prefixes.html#index-prefixes
 - tokenize on 2 or more symbols
 - exclude/remove tokenization of single forward slash or backslash (need to just add negate regex to the regex token capture)
  
 ## Known Issues
 N/A
 
 
 
 
 ## ETC
 Meaningless side note:
 The term `stk` (in the repo name) means `search to know`...

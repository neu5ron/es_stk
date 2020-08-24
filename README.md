# Security Use Case Elasticsearch Analyzer

This is a custom elasticsearch analyzer built with security logging use cases in mind.

The analyzer will lowercase the values and then [tokenize](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-tokenizers.html) values based upon
- `whitespace`
- `\ `
- `/`
 
 
 Problems this solves are well noted in this [blog](https://socprime.com/blog/elastic-for-security-analysts-part-1-searching-strings/).
 
 
 
 ## #TODO:
 - tokenize on 2 or more symbols
 - exclude/remove tokenization of single forward slash or backslash (need to just add negate regex to the regex token capture)
  
 ## Known Issues
 N/A
 
 
 
 
 # ETC
 Meaningless side note:
 The term `stk` (in the repo name) means `search to know`...
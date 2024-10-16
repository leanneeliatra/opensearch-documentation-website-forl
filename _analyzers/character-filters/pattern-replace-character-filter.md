---
layout: default
title: Pattern Replace Character Filter
parent: Character Filters
nav_order: 130
---

# Pattern Replace Character Filter
The pattern replace character filter allows you to use regular expressions (regex) to define patterns for matching and replacing characters in the input text. It is a flexible tool for advanced text transformations, especially when dealing with complex string patterns.

This filter replaces all instances of a pattern with a specified replacement string, allowing for easy substitutions, deletions, or complex modifications to the input text. You can use it to normalize the input before tokenization.

## Example of the pattern replacement character filter
The following example standardizes phone numbers by removing spaces, dashes, or parentheses. 

```
GET /_analyze
{
  "tokenizer": "standard",
  "char_filter": [
    {
      "type": "pattern_replace",
      "pattern": "[\\s()-]+",
      "replacement": ""
    }
  ],
  "text": "Call me at (555) 123-4567 or 555 987 6543"
}
```
This filter will produce the following text:
```
Call me at 5551234567 or 5559876543 
```
## Configuring the pattern replace filter
There are two parameters needed to configure the pattern replace filter:

1. Pattern: Provide a regular expression (regex) to match parts of the input text. The filter will identify and match this pattern in the input for replacement. 
2. Replacement: Specify the string that will replace any matches found by the pattern. If you want to remove the matched text, use an empty string as the replacement.

## Example of a custom analyzer 
The pattern replace character filter can be used to remove currency symbols and spaces from a string.
```
GET /_analyze
{
  "tokenizer": "standard",
  "char_filter": [
    {
      "type": "pattern_replace",
      "pattern": "[\\$,€]",
      "replacement": ""
    },
    {
      "type": "pattern_replace",
      "pattern": "[\\s,]+",
      "replacement": " "
    }
  ],
  "text": "Total: $ 1,200.50 and € 1.100,75"
}
```
This request includes 2 pattern filters:
1. `"pattern": "[\\$,€]":`
   This pattern removes the currency symbols ($, €) by replacing them with an empty string.
2. `"pattern": "[\\s,]+":`
This pattern matches any sequence of spaces and commas, replacing them with a single space. This ensures the numbers are formatted correctly and separated from other text. 

Summary of the outputted response text
```
Total
1200.50
and
1100.75
 ```

By applying these filters, the input text is normalized by removing unwanted characters and preserving the correct spacing between numbers and words.

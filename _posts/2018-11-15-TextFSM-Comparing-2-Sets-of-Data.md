---
published: true
---

## TextFSM - Comparing 2 sets of data

This blog post will explain how to use TextFSM in a Python script to parse 2 seperate input files in the same script

### Quick Summary

I was trying to create a script to compare 2 outputs from an Cisco ASA before and after a migration. I wanted to compare the `show route` output but I kept having an issue where only the last file that was parsed...

### Non-Working Code

The initial code was

```
import textfsm

table = textfsm.TextFSM(open('asa_show_route.template))

#
#code to input the command output into a variables (before_raw_text_data and after_raw_text_data)
#

before_data = table.ParseText(before_raw_text_data)

after_data = table.ParseText(after_raw_text_data)

```

but after running the script the before_data variable contained the information in the after_raw_text_data variable. Nothing I seemed to do in the script seemed to make any difference.

### The Fix

The fix was that the template needed to be called twice to create 2 tables, like this

```
before_table = textfsm.TextFSM(open('asa_show_route.template))

after_table = textfsm.TextFSM(open('asa_show_route.template))
```

and then the parse commands reference the seperate tables

```
before_data = before_table.ParseText(before_raw_text_data)

after_data = after_table.ParseText(after_raw_text_data)

```

### Final thoughts

I think this is something to do with objects in Python and may be obvious to anyone who knows more about object oriented programming (and maybe one day I'll understand it too) but it had me stumped for days!!
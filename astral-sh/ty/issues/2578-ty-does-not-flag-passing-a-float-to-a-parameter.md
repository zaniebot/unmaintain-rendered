```yaml
number: 2578
title: ty does not flag passing a float to a parameter expecting Decimal, while Pyright correctly catches this as a type error.
type: issue
state: open
author: tspecht
labels:
  - needs-mre
assignees: []
created_at: 2026-01-21T14:46:10Z
updated_at: 2026-01-21T14:55:08Z
url: https://github.com/astral-sh/ty/issues/2578
synced_at: 2026-01-21T15:05:30Z
```

# ty does not flag passing a float to a parameter expecting Decimal, while Pyright correctly catches this as a type error.

---

_@tspecht_

### Summary

Minimal reproducible example:                                                                                                        

```py                                                                                              
from decimal import Decimal                                                                                                          

def process_balance(value: Decimal) -> Decimal:                                                                                      
    return value * 2                                                                                                                 

# Case 1: Direct float                                                                                                               
balance: float = 123.45                                                                                                              
process_balance(balance)  # No error from ty, Pyright catches this                                                                   

# Case 2: float | Literal[0] (common pattern with defaults)                                                                          
def get_balance(use_default: bool) -> float | int:                                                                                   
    return 0 if use_default else 123.45                                                                                              

opening_balance = get_balance(True)                                                                                                  
process_balance(opening_balance)  # No error from ty, Pyright catches this                                                           
```                                                                                                                   
 
Expected behavior:                                                                                                       

ty should report invalid-argument-type for both cases since float is not assignable to Decimal.                                      

Actual behavior:                                                                                                                     

ty reports no errors. Pyright reports:                                                                                               
Argument of type "float" cannot be assigned to parameter "value" of type "Decimal"                                                   
  "float" is not assignable to "Decimal"                                                                                             

Command:                                                                                                                             
`ty check example.py`                                                                                                               

`ty.toml` (if relevant):    
```                                                                                                           
# No rules disabled that would affect this                                                                                           
[rules]                                                                                                                              
unresolved-import = "ignore"                                                                                                       
```                                                                                     

### Version

0.0.12

---

_Comment by @carljm on 2026-01-21 14:55_

Hi, thanks for the report! I can't reproduce this: https://play.ty.dev/5dea368b-6004-4ca0-a4af-f8d8785127fb

I also can't really think of anything that would cause this to happen. Maybe `from decimal import Decimal` is somehow an unresolved import, since I see you have those silenced? That shouldn't be possible, though, since it's in the standard library.

But ty definitely does emit the two expected diagnostics on this snippet, in normal circumstances.

---

_Label `needs-mre` added by @carljm on 2026-01-21 14:55_

---

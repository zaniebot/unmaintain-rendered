---
number: 2121
title: "type[UnionAlias] for union of classes reveals as @Todo instead of distributing over union"
type: issue
state: closed
author: sunny-zuo
labels:
  - bug
  - type aliases
assignees: []
created_at: 2025-12-19T21:22:26Z
updated_at: 2025-12-21T23:45:31Z
url: https://github.com/astral-sh/ty/issues/2121
synced_at: 2026-01-10T01:52:52Z
---

# type[UnionAlias] for union of classes reveals as @Todo instead of distributing over union

---

_Issue opened by @sunny-zuo on 2025-12-19 21:22_

### Summary

When a function returns `type[UnionAlias]` where the alias is a union of classes, `reveal_type` shows `@Todo` instead of the expected type. ([playground link](https://play.ty.dev/bd72c54e-7402-4508-876f-332366ec7a31))                                                                              
                                                                                                                                
```python                                                                                                                     
from typing import reveal_type                                                                                                
                                                                                                                              
class Class1:                                                                                                                 
    pass                                                                                                                      
                                                                                                                              
class Class2:                                                                                                                 
    pass                                                                                                                      
                                                                                                                              
ClassTypes = Class1 | Class2                                                                                                  
                                                                                                                              
def some_fn() -> type[ClassTypes]:                                                                                            
    return Class1                                                                                                             
                                                                                                                              
class_val = some_fn()                                                                                                         
reveal_type(class_val)  # Shows `@Todo`                                       
```
`mypy` reveals `Union[type[Class1], type[Class2]]`, pyrefly reveals `type[ClassTypes]` and pyright reveals `type[Class1] | type[Class2]` for this example.


### Version

ty 0.0.4 (c1e6188b1 2025-12-18)

---

_Label `bug` added by @AlexWaygood on 2025-12-19 21:34_

---

_Label `type aliases` added by @AlexWaygood on 2025-12-19 21:34_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-19 21:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-20 16:21_

---

_Closed by @charliermarsh on 2025-12-21 23:45_

---

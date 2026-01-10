```yaml
number: 515
title: "Callables called with **dict report error"
type: issue
state: closed
author: heinerlehr
labels: []
assignees: []
created_at: 2025-05-26T12:47:59Z
updated_at: 2025-05-26T12:52:08Z
url: https://github.com/astral-sh/ty/issues/515
synced_at: 2026-01-10T02:34:10Z
```

# Callables called with **dict report error

---

_Issue opened by @heinerlehr on 2025-05-26 12:47_

### Summary

```
class Test:
    
    def __init__(self, value1:str, value2:str):
        pass

def test2(value1:str, value2:str):
    pass
    
if __name__ == "__main__":
    test = Test("value1", "value2")
    print("Test instance created successfully.")
    
    dict = {'value1':'value1', 'value2': 'value2'}
    test = Test(**dict)
    
    test2(**dict)
```

Both Test(**dict) and test2(**dict) report "missing_argument" - which is not correct.

### Version

 2025.11.11402026

---

_Closed by @sharkdp on 2025-05-26 12:52_

---

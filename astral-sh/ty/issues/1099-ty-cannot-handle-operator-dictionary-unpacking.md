```yaml
number: 1099
title: "ty Cannot handle ** operator (dictionary unpacking) when the dictionary comes from the user."
type: issue
state: closed
author: boompig
labels:
  - question
assignees: []
created_at: 2025-08-26T18:43:39Z
updated_at: 2025-08-26T18:45:56Z
url: https://github.com/astral-sh/ty/issues/1099
synced_at: 2026-01-12T15:54:24Z
```

# ty Cannot handle ** operator (dictionary unpacking) when the dictionary comes from the user.

---

_@boompig_

### Question

Example code:

```python
import json
import logging
from dataclasses import dataclass

@dataclass
class MyClass:
      x: int

with open("file.json") as fp:
    d = json.load(fp)
    assert isinstance(d, dict)
    try:
        foo = MyClass(**d)
    except TypeError as err:
        logging.error("The input does not conform to my class. Stop.")
```

Not sure if this is a bug or what the correct way to ignore this error is.

### Output

```
error[missing-argument]: No argument provided for required parameter `x`
  --> /tmp/foo.py:13:15
   |
11 |     assert isinstance(d, dict)
12 |     try:
13 |         foo = MyClass(**d)
   |               ^^^^^^^^^^^^
14 |     except TypeError as err:
15 |         logging.error("The input does not conform to my class. Stop.")
   |
info: rule `missing-argument` is enabled by default

Found 1 diagnostic
```


### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Label `question` added by @boompig on 2025-08-26 18:43_

---

_Comment by @sharkdp on 2025-08-26 18:45_

Thank you for reporting this.

Please see https://github.com/astral-sh/ty/issues/247

---

_Closed by @sharkdp on 2025-08-26 18:45_

---

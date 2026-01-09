---
number: 20087
title: UP035 breaks code
type: issue
state: closed
author: IvanKirpichnikov
labels:
  - question
assignees: []
created_at: 2025-08-25T18:56:28Z
updated_at: 2025-08-26T15:04:48Z
url: https://github.com/astral-sh/ruff/issues/20087
synced_at: 2026-01-07T13:12:16-06:00
---

# UP035 breaks code

---

_Issue opened by @IvanKirpichnikov on 2025-08-25 18:56_

### Summary

# MRE
```py
from typing_extensions import Generic as GenericExtensions
```
Run command `ruff check --fix` and we get a broken code.
And we get
```py
from typing import Generic as GenericExtensions
```

# Why do we get it?

The `Generic` of `typing_extensions` implements the logic of new versions, for example, the defaults of `TypeVar`. If you change the imports to `typing`, then the code based on the defaults of `TypeVar` will break.


### Version

ruff 0.12.10

### Python version

Python 3.10.17

---

_Comment by @ntBre on 2025-08-25 20:22_

Thanks for the report!

I'm probably missing something here, but could you maybe share a larger snippet of code that illustrates the breakage? I'm not aware of the interaction with `TypeVar` you mentioned.

---

_Label `question` added by @ntBre on 2025-08-25 20:22_

---

_Comment by @IvanKirpichnikov on 2025-08-26 15:04_

I thought that default typing.Generic does not support defaults since defaults appeared in 3.13, but no. But it supports defaults, which is amazing to me. 

In general, the code does not break. I apologize

---

_Closed by @IvanKirpichnikov on 2025-08-26 15:04_

---

```yaml
number: 15846
title: Support for black formatting of function arguments
type: issue
state: closed
author: fhg-isi
labels:
  - question
  - formatter
assignees: []
created_at: 2025-01-31T10:17:44Z
updated_at: 2025-02-07T10:11:52Z
url: https://github.com/astral-sh/ruff/issues/15846
synced_at: 2026-01-12T15:54:55Z
```

# Support for black formatting of function arguments

---

_@fhg-isi_

### Description

I would like to switch from pylint and black to ruff but keep the black formatting. 

Currently the wanted behavior regarding function arguments with my (black) setup is as follows:

**a)** If I specify a trailing comma after the function arguments:
```
def foo(a, b, c, d,):
    pass
```

code will be formatted to use separate lines for the arguments:

```
def foo(
    a,
    b,
    c,
    d,
):
    pass
```
**b)** If I do not have a trailing comma after the function arguments:

```
def foo(
    a,
    b
):
    pass
```

the arguments will be merged to a single line:

```
def foo(a, b):
    pass
```

However, the behavior of ruff is different:

**a)** Trailing commas are removed

**b)** Arguments that are spread over several lines without trailing commas won't be merged to a single line. 



=> Can you please support an option for ruff formatter to reproduce the black format for function arguments?

=> Or can you please tell me what ruff options I need to set to get the wanted behavior for the function arguments?

Related: https://github.com/astral-sh/ruff/issues/9992

---

_Label `formatter` added by @AlexWaygood on 2025-01-31 11:03_

---

_Comment by @dhruvmanila on 2025-01-31 11:10_

Ruff should also format in the same way as Black in these cases https://play.ruff.rs/0dca8756-2df9-4fa7-97ca-04678f6685d8, it might be something else like misconfigured editor setup?

---

_Label `question` added by @dhruvmanila on 2025-01-31 11:11_

---

_Closed by @MichaReiser on 2025-02-07 10:11_

---

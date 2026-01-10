```yaml
number: 1990
title: "don't emit diagnostics about \"shadowing\" when assignment target is not a local variable"
type: issue
state: open
author: SerTetora
labels:
  - question
assignees: []
created_at: 2025-12-17T09:02:16Z
updated_at: 2025-12-18T23:18:57Z
url: https://github.com/astral-sh/ty/issues/1990
synced_at: 2026-01-10T01:53:59Z
```

# don't emit diagnostics about "shadowing" when assignment target is not a local variable

---

_Issue opened by @SerTetora on 2025-12-17 09:02_

### Summary

```python
from configparser import ConfigParser

cfgparser = ConfigParser()
cfgparser.optionxform = str  # <--- Should be allowed, per Python docs
``` 
Ref. https://docs.python.org/3/library/configparser.html#configparser.ConfigParser.optionxform

### ty

```
Implicit shadowing of function `optionxform` (invalid-assignment) [Ln 4, Col 1]
``` 

### pyrefly


```
ERROR sandbox.py:4:25-28: `type[str]` is not assignable to attribute `optionxform` with type `BoundMethod[ConfigParser, (self: ConfigParser, optionstr: str) -> str]` [[bad-assignment](https://pyrefly.org/en/docs/error-kinds/#bad-assignment)]
  Positional parameter name mismatch: got `object`, want `optionstr`
``` 

### pyright

```
Cannot assign to attribute "optionxform" for class "ConfigParser"
  No overloaded function matches type "(optionstr: str) -> str"  (reportAttributeAccessIssue)
``` 

### mypy

```
main.py:4: error: Cannot assign to a method  [method-assign]
main.py:4: error: Incompatible types in assignment (expression has type "type[str]", variable has type "Callable[[str], str]")  [assignment]
Found 2 errors in 1 file (checked 1 source file)
``` 

### Version

0.0.2

---

_Label `question` added by @sharkdp on 2025-12-17 09:09_

---

_Comment by @sharkdp on 2025-12-17 09:12_

Thank you for reporting this.

Our error message is different from the other type checkers, but it looks like all other type checkers also issue an error. If this should really be allowed, maybe it's a problem with the [stubs](https://github.com/python/typeshed/blob/8d96801533918957fb194e101cb321bfe1f836f8/stdlib/configparser.pyi#L324) for this attribute?

---

_Comment by @SerTetora on 2025-12-17 09:47_

Thanks for the feedback!

> If this should really be allowed, maybe it's a problem with the [stubs](https://github.com/python/typeshed/blob/8d96801533918957fb194e101cb321bfe1f836f8/stdlib/configparser.pyi#L324) for this attribute?

What would you recommend to do in this case?

---

_Comment by @sharkdp on 2025-12-17 10:17_

I think this would be an appropriate place to use a suppression comment (`# type: ignore` or `# ty: ignore, see https://docs.astral.sh/ty/suppression/).

---

_Comment by @AlexWaygood on 2025-12-17 10:27_

If we believe that there's a bug in the stubs, then a report should be filed over at [typeshed](https://github.com/python/typeshed) (if there isn't already an issue there regarding this)

---

_Comment by @sharkdp on 2025-12-17 10:36_

Looks like this was discussed before:

* https://github.com/python/typeshed/issues/1857
* https://github.com/python/mypy/issues/5062
* https://github.com/python/mypy/issues/708

---

_Comment by @sharkdp on 2025-12-18 11:36_

Closing this for now, let me know if anyone thinks there is something we could do in ty to address this.

---

_Closed by @sharkdp on 2025-12-18 11:36_

---

_Comment by @carljm on 2025-12-18 23:17_

Our diagnostic is bad here and should be improved; the logic we attempt to use for detecting implicit shadowing of a function or class wrongly fires on assignments like this (because it's based solely on the declared type, not the syntactic context.)

---

_Reopened by @carljm on 2025-12-18 23:17_

---

_Renamed from "`ConfigParser.optionxform` assignment flagged as “shadowing” (false positive)" to "don't emit diagnostics about "shadowing" when assignment target is not a local variable" by @carljm on 2025-12-18 23:18_

---

_Added to milestone `Stable` by @carljm on 2025-12-18 23:18_

---

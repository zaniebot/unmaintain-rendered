```yaml
number: 2434
title: Strings are considered as LiteralStrings
type: issue
state: open
author: Salamandar
labels: []
assignees: []
created_at: 2026-01-10T10:02:56Z
updated_at: 2026-01-10T11:25:31Z
url: https://github.com/astral-sh/ty/issues/2434
synced_at: 2026-01-10T12:00:02Z
```

# Strings are considered as LiteralStrings

---

_Issue opened by @Salamandar on 2026-01-10 10:02_

### Summary

There are multiple issues:
* typeshed defines str.split() and str.join() with a return type of LiteralString or list[LiteralString]
  https://github.com/python/typeshed/issues/10887
* `ty` considers str as LiteralString:

```py
a = str()
reveal_type(a)
page_lines = "".join(some_method_returning_str()).split("\n")
reveal_type(page_lines)
```
gives `Literal[""]` and `list[LiteralString]`

while `mypy` gives:
```
note: Revealed type is "builtins.str"
note: Revealed type is "builtins.list[builtins.str]"
```

and as list is invariant, we can't pass `list[LiteralString]` to functions expecting a list[str]…

### Platform

Linux

### Version

0.0.8, 0.0.11

### Python version

3.13.11

---

_Label `bug` added by @Salamandar on 2026-01-10 10:02_

---

_Label `bug` removed by @AlexWaygood on 2026-01-10 11:17_

---

_Comment by @AlexWaygood on 2026-01-10 11:25_

Thanks for the report!

We intend to soon change our behaviour to mypy's to allow you to use an explicit inline annotation to do a "safe upcast". In this case, you want ty to infer a `str` rather than `Literal[""]` for your initial assignment, and once we implement this behaviour change, you'd be able to achieve that with

```py
a: str = str()
```

Apart from that, the other thing ty could do here would be to _implicitly_ infer (even without the annotation) that we need to infer the broader type here at the assignment of `a` to avoid false positives later on in the scope. That _might_ be possible with sufficiently advanced bidirectional inference, but it seems hard in this specific case.

---

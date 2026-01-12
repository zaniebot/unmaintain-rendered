```yaml
number: 19028
title: INT rules have false negatives for other ways to use gettext functions
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-06-29T15:55:34Z
updated_at: 2025-10-20T22:24:56Z
url: https://github.com/astral-sh/ruff/issues/19028
synced_at: 2026-01-12T15:54:56Z
```

# INT rules have false negatives for other ways to use gettext functions

---

_@dscorbett_

### Summary

[`f-string-in-get-text-func-call` (INT001)](https://docs.astral.sh/ruff/rules/f-string-in-get-text-func-call/), [`format-in-get-text-func-call` (INT002)](https://docs.astral.sh/ruff/rules/format-in-get-text-func-call/), and [`printf-in-get-text-func-call` (INT003)](https://docs.astral.sh/ruff/rules/printf-in-get-text-func-call/) do not recognize internalization functions when written as attribute references, e.g. `gettext.ngettext` instead of `ngettext`. This is a false negative because tools like xgettext recognize attribute references too.

These rules do not recognize internalization functions when bound to different names, e.g. `gettext.ngettext` imported as `N_` instead of `ngettext`. This is a false negative because if someone writes such a binding, they probably also specify it as a gettext keyword, e.g. with `xgettext -kN_:1,2`, even if they don’t configure Ruff with [`function-names`](https://docs.astral.sh/ruff/settings/#lint_flake8-gettext_function-names) or [`extend-function-names`](https://docs.astral.sh/ruff/settings/#lint_flake8-gettext_extend-function-names) explicitly.

[False negatives](https://play.ruff.rs/d54ecf1a-f020-4294-823a-d33ebcccaffb):
```python
import builtins
import gettext
from gettext import gettext as _gettext
gettext.install("int_fn")
name = "world"

print(builtins._(f"Hello, {name}!"))
print(gettext.gettext(f"Hello, {name}!"))
print(_gettext(f"Hello, {name}!"))

print(builtins._("Hello, {}!".format(name)))
print(gettext.gettext("Hello, {}!".format(name)))
print(_gettext("Hello, {}!".format(name)))

print(builtins._("Hello, %s!" % name))
print(gettext.gettext("Hello, %s!" % name))
print(_gettext("Hello, %s!" % name))
```

### Version

ruff 0.12.1 (32c54189c 2025-06-26)

---

_Label `bug` added by @ntBre on 2025-06-29 23:35_

---

_Label `rule` added by @ntBre on 2025-06-29 23:35_

---

_Label `help wanted` added by @ntBre on 2025-06-29 23:42_

---

_Comment by @ntBre on 2025-06-29 23:44_

It looks like these rules just check for a `Name` expression and compare the name directly instead of matching the qualified name, for example:

https://github.com/astral-sh/ruff/blob/e7aadfc28b5a1447c45d39017a37f1acd1001f14/crates/ruff_python_semantic/src/analyze/imports.rs#L73-L77

That means we also have [false positives](https://play.ruff.rs/01fe174e-1a25-4c4e-b8cd-1b708af0d548) for any function with the same name.

---

_Comment by @dscorbett on 2025-06-30 00:36_

Checking for unqualified names is necessary to support [deferred translations](https://docs.python.org/3/library/gettext.html#deferred-translations). Though the `gettext` in your example is not the real `gettext.gettext` at runtime, a tool like xgettext will still try to extract strings from its calls, so it is still helpful for Ruff to warn about it.

---

_Comment by @ntBre on 2025-06-30 00:45_

Oh, thank you, that's good to know! We'll have to be a bit more careful here than I thought.

---

_Comment by @robsdedude on 2025-06-30 10:55_

@dscorbett I sympathize with all but the `builtins` examples. Why should
```python
print(builtins._(f"Hello, {name}!"))
```
trigger the lint? The `builtins` module has not attribute `_`. And I don't think many people would be assigning it dynamically.

---

_Comment by @dscorbett on 2025-06-30 12:10_

`builtins._` should trigger the lint for two reasons. In general, anything matching a name in [function-names](https://docs.astral.sh/ruff/settings/#lint_flake8-gettext_function-names) (which includes `_` by default) should be considered a gettext function, either because it really is at runtime, or to support deferred translations. In particular, people _do_ dynamically assign `builtins._` using the standard library’s [`gettext.install`](https://docs.python.org/3/library/gettext.html#gettext.install). People also [set it dynamically to various third-party gettext implementations.](https://github.com/search?q=language%3APython+%2F%5Cbbuiltins%5C..*%5Cb_%5Cb.*+%3D+.*gettext%2F&type=code)

---

_Comment by @robsdedude on 2025-06-30 21:19_

> In general, anything matching a name in [function-names](https://docs.astral.sh/ruff/settings/#lint_flake8-gettext_function-names) (which includes _ by default) should be considered a gettext function, either because it really is at runtime, or to support deferred translations

The deferred translation example in the Python docs, does not actually bind `_` to `builtins`, just as a local/global variable.

> In particular, people do dynamically assign builtins._ using the standard library’s [gettext.install](https://docs.python.org/3/library/gettext.html#gettext.install). People also [set it dynamically to various third-party gettext implementations.](https://github.com/search?q=language%3APython+%2F%5Cbbuiltins%5C..*%5Cb_%5Cb.*+%3D+.*gettext%2F&type=code)

Now that's a very compelling argument 😅 seems quite idiomatic then. Not sure I like the design, but that's not relevant here =)

---

_Closed by @ntBre on 2025-10-20 22:24_

---

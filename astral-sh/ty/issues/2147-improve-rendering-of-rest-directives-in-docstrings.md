```yaml
number: 2147
title: Improve rendering of ReST directives in docstrings
type: issue
state: closed
author: AlexWaygood
labels:
  - good first issue
  - server
assignees: []
created_at: 2025-12-21T16:44:09Z
updated_at: 2026-01-14T12:33:09Z
url: https://github.com/astral-sh/ty/issues/2147
synced_at: 2026-01-14T13:42:04Z
```

# Improve rendering of ReST directives in docstrings

---

_@AlexWaygood_

### Summary

For example, the stdlib method `logging.Filterer.filter` has several ReST directives at the bottom of its docstring that tell you when the API was updated: https://github.com/astral-sh/ruff/blob/b4c2825afdd8c1010c3a5859521629d2d1e0a6df/crates/ty_vendored/vendor/typeshed/stdlib/logging/__init__.pyi#L136-L142. Pylance renders these as sentences that use English words:

<img width="1450" height="246" alt="Image" src="https://github.com/user-attachments/assets/c5cecc68-adc0-4962-88bd-94cb66460024" />

---

And Sphinx renders them similarly in [CPython's online HTML documentation](https://docs.python.org/3/library/logging.html#logging.Filter):

<img width="1690" height="494" alt="Image" src="https://github.com/user-attachments/assets/158277e7-7b91-4973-9146-64c86687ba42" />

---

But we currently just render these as "**versionchanged**:: 3.2":

<img width="964" height="648" alt="Image" src="https://github.com/user-attachments/assets/f6b92f6a-a5ea-4feb-aeb9-7827f245be17" />

---

_Label `server` added by @AlexWaygood on 2025-12-21 16:44_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-21 20:18_

---

_Label `good first issue` added by @Gankra on 2026-01-09 04:12_

---

_Comment by @Gankra on 2026-01-09 04:14_

This should be a nice and easy first contribution. We already parse out and find these keywords, someone just needs to write a simple mapping from known keywords to pretty phrases here:

https://github.com/astral-sh/ruff/blob/f9f7a6901bc78f7429c6b2393d747350ac704f33/crates/ty_ide/src/docstring.rs#L361



---

_Comment by @AryanBagade on 2026-01-11 23:23_

hey @Gankra can i work on it, been contributing to pyrefly owuld love to contribute here !!!

---

_Closed by @AlexWaygood on 2026-01-14 12:33_

---

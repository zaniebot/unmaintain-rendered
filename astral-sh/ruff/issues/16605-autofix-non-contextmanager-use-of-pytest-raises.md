```yaml
number: 16605
title: "Autofix non-contextmanager use of `pytest.raises` & friends"
type: issue
state: closed
author: jakkdl
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-03-10T15:36:13Z
updated_at: 2025-06-16T17:03:55Z
url: https://github.com/astral-sh/ruff/issues/16605
synced_at: 2026-01-10T11:09:57Z
```

# Autofix non-contextmanager use of `pytest.raises` & friends

---

_Issue opened by @jakkdl on 2025-03-10 15:36_

### Summary

# Rule request

https://github.com/pytest-dev/pytest/pull/13241 wants to deprecate the use of the callable form of `raises`, `warns` and `deprecated_call` - i.e.
```pytest
excinfo = pytest.raises(ValueError, int, "hello")
# should be
with pytest.raises(ValueError) as excinfo:
  int("hello")
```
but in the rare codebase where it's widely used it will be a massive pain to rewrite it manually, so we don't want to deprecate it until there's a good option for autofixing it. I thought ruff would be a good option for supplying this.

It can optionally also handle `match`, which requires a separate assert when using the callable form:
```pytest
excinfo = pytest.raises(ValueError, int, "hello")
assert excinfo.match("^invalid literal")
# to
with pytest.raises(ValueError, match="^invalid literal"):
    int("hello")
```


## Rationale
The context-manager form is more readable, easier to extend, and supports additional kwargs. And is not on the verge of being deprecated

Requested uptream in flake8-pytest: https://github.com/m-burst/flake8-pytest-style/issues/331, but it's mostly the autofix we're interested in

---

_Label `rule` added by @MichaReiser on 2025-03-10 18:23_

---

_Label `wish` added by @MichaReiser on 2025-03-10 18:23_

---

_Comment by @MichaReiser on 2025-03-10 18:24_

I don't see a reason not to add this rule if it is accepted upstream if someone's interested in implementing the rule + fix

---

_Label `wish` removed by @MichaReiser on 2025-03-10 18:24_

---

_Label `help wanted` added by @MichaReiser on 2025-03-10 18:24_

---

_Comment by @jakkdl on 2025-03-11 09:18_

> I don't see a reason not to add this rule if it is accepted upstream if someone's interested in implementing the rule + fix

With upstream, do you mean `pytest` or `flake8-pytest-style`?

---

_Comment by @MichaReiser on 2025-03-11 09:39_

`flake8-pytest-style`. The alternative is to make it a Ruff rule

---

_Closed by @ntBre on 2025-06-16 17:03_

---

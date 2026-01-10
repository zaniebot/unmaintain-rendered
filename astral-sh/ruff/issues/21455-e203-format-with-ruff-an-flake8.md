```yaml
number: 21455
title: "E203: format with ruff an flake8"
type: issue
state: closed
author: misery
labels:
  - question
assignees: []
created_at: 2025-11-14T14:13:51Z
updated_at: 2025-12-10T17:27:27Z
url: https://github.com/astral-sh/ruff/issues/21455
synced_at: 2026-01-10T11:10:00Z
```

# E203: format with ruff an flake8

---

_Issue opened by @misery on 2025-11-14 14:13_

### Summary

I have this snipped that will be formatted by `ruff format` like this.

```
OPTIONS = {}
HG_ENV_PREFIX = 'HG_USERVAR_'
for key, value in six.iteritems(os.environ):
  if key.startswith(HG_ENV_PREFIX):
    OPTIONS[key[len(HG_ENV_PREFIX) :]] = value
```


flake8 will throw a warning for this: ```E203 whitespace before ':'```

ruff check won't give a warning here.
```
ruff check --select E203 --preview
All checks passed!
```

Is flake8 or ruff correct here? I need to use ```# fmt: skip``` here. Maybe the formatter should avoid that whitespace?

### Version

0.14.5

---

_Comment by @dylwil3 on 2025-11-14 17:24_

I believe that `flake8`'s implementation of E203 is not PEP 8 compliant - see, e.g., https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#slices . Ruff's implementation of E203 does not emit a lint on the formatted version of your snippet - [Playground](https://play.ruff.rs/6a39cc04-bb03-410d-973c-c65404974658)

---

_Label `question` added by @dylwil3 on 2025-11-14 17:24_

---

_Comment by @amyreese on 2025-11-14 19:50_

Generally speaking, Ruff/Black style is not compatible with flake8's pycodestyle lints. If you have a formatter enforcing your code style, then it's suggested that you disable style related lint rules. This is what we do with the [default Ruff configuration](https://docs.astral.sh/ruff/configuration/).

---

_Comment by @MichaReiser on 2025-11-14 20:29_

To clarify, the E rules in Ruff are compatible with the formatter. It's just unnecessary to run them when using the formatter too.

---

_Closed by @MichaReiser on 2025-12-10 17:27_

---

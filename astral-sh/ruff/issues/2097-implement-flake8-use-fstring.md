```yaml
number: 2097
title: "Implement `flake8-use-fstring`"
type: issue
state: closed
author: Secrus
labels:
  - question
  - plugin
assignees: []
created_at: 2023-01-22T22:57:36Z
updated_at: 2023-04-24T14:40:24Z
url: https://github.com/astral-sh/ruff/issues/2097
synced_at: 2026-01-12T15:54:42Z
```

# Implement `flake8-use-fstring`

---

_@Secrus_

https://github.com/MichaelKim0407/flake8-use-fstring

* `FS001`: `%` formatting is used.

* `FS002`: `.format` formatting is used.

* `FS003`: f-string missing prefix (ignored by default).

Config options:
`--percent-greedy`, `--format-greedy`
* Level 0 (default): only report error if the value before `%` or `.format` is a string literal.

* Level 1: report error if a string literal appears before `%` or `.format` anywhere in the statement.

* Level 2: report any usage of `%` or `.format`.

---

_Comment by @charliermarsh on 2023-01-23 00:18_

I believe we support FS001 and FS002, at least partially, as UP031 and UP032, which also automatically upgrade `%` strings to `.format` strings, and `.format` strings to f-strings.

(I say "partially", because it doesn't flag _every_ usage of `%` and `.format` strings -- it omits some cases depending on formatting.)

---

_Label `plugin` added by @charliermarsh on 2023-01-23 00:18_

---

_Comment by @charliermarsh on 2023-01-23 01:42_

We could change UP031 and UP032 to _always_ flag those string formatting methods, even if they can't fix them. That'd be one way to close out this issue. But it's a bit of a behavior change for those rules.

---

_Comment by @Secrus on 2023-01-25 20:36_

Let me test how it works right now, looking at the rules description, it might actually be enough.

---

_Label `question` added by @charliermarsh on 2023-01-25 20:37_

---

_Closed by @charliermarsh on 2023-01-30 22:56_

---

_Comment by @martinhoyer on 2023-04-24 14:40_

@charliermarsh Sorry for necro'ing a closed issue - just want to mention that in my recent usage, UP032 did indeed left some `.format()`s, which are a bit more complex then just already assigned variable(s).  
I'm thinking an opt-in option in settings for it to flag them, even though auto-fix is not available could be nice?  
Might include comment, like "consider adding helper variable for complex formatting before using it in f-string" 

---

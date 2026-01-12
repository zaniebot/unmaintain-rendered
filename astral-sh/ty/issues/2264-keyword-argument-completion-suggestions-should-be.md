```yaml
number: 2264
title: "Keyword-argument completion suggestions should be suppressed if the user is in the middle of a non-`Name` expression"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-12-29T20:02:12Z
updated_at: 2025-12-30T13:10:58Z
url: https://github.com/astral-sh/ty/issues/2264
synced_at: 2026-01-12T15:54:26Z
```

# Keyword-argument completion suggestions should be suppressed if the user is in the middle of a non-`Name` expression

---

_@AlexWaygood_

### Summary

Consider:

<img width="1266" height="616" alt="Image" src="https://github.com/user-attachments/assets/7d14dfe8-2187-43de-9b59-c78fdf8da3cd" />

Accepting the `aaaaa` suggestion here (which is rendered, less confusingly, as `aaaaa=` inside VSCode), will introduce invalid syntax into the code:

<img width="800" height="484" alt="Image" src="https://github.com/user-attachments/assets/9bbff3c6-cc2b-4971-a44b-13ecc9e504e9" />

if the user has already typed an attribute expression inside the parentheses for a call expression (and there isn't a comma following that attribute expression), they cannot possibly be trying to type a keyword argument, so the suggestion should be suppressed.

For a less contrived example: I was surprised to get the `name=` autocomplete suggestion herewhen editing docstring-adder in VSCode just now. I would expect _only_ attribute-member completions in this context:

<img width="2436" height="1060" alt="Image" src="https://github.com/user-attachments/assets/1279a101-4064-4f1e-99cd-851a446b8e98" />

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-12-29 20:02_

---

_Label `server` added by @AlexWaygood on 2025-12-29 20:02_

---

_Label `completions` added by @AlexWaygood on 2025-12-29 20:02_

---

_Comment by @RasmusNygren on 2025-12-29 21:34_

Nice catch! This will be fixed by https://github.com/astral-sh/ruff/pull/22110. I added your example as a test directly to that PR for good measure :)

---

_Closed by @AlexWaygood on 2025-12-30 13:10_

---

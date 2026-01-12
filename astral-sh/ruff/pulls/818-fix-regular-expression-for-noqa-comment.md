```yaml
number: 818
title: Fix regular expression for noqa comment
type: pull_request
state: closed
author: harupy
labels: []
assignees: []
base: main
head: fix-noqa-regex
created_at: 2022-11-19T16:33:42Z
updated_at: 2022-11-19T17:19:59Z
url: https://github.com/astral-sh/ruff/pull/818
synced_at: 2026-01-12T05:48:45Z
```

# Fix regular expression for noqa comment

---

_Pull request opened by @harupy on 2022-11-19 16:33_

The current regular expression matches a string that looks like a noqa comment:

```python
x = "# noqa: E401"
```

The fixed regular expression doesn't.

---

_Comment by @harupy on 2022-11-19 16:34_

Tested flake8

```bash
> echo 'a = lambda: 0' | flake8 -
stdin:1:1: E731 do not assign a lambda expression, use a def

# a is a tuple of lambda and str
> echo 'a = lambda: 0, "# noqa: E731"' | flake8 -
```

---

_Comment by @charliermarsh on 2022-11-19 17:09_

I think this is gonna be hard to get right as long as we’re not using the lexer tokens. For example, I think this will now reject: “# noqa: E501 # other comment” — does Flake8 respect that?

---

_Comment by @harupy on 2022-11-19 17:16_

Good point. Yes, Flake8 respects that.

---

_Closed by @harupy on 2022-11-19 17:16_

---

_Comment by @charliermarsh on 2022-11-19 17:19_

I think what we could do is collect all the comments from the lexer… then we can map from line number to comment in here.

---

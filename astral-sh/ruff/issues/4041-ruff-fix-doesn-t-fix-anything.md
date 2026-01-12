```yaml
number: 4041
title: "ruff --fix doesn't fix anything"
type: issue
state: closed
author: anentropic
labels: []
assignees: []
created_at: 2023-04-20T08:44:15Z
updated_at: 2023-04-21T01:25:18Z
url: https://github.com/astral-sh/ruff/issues/4041
synced_at: 2026-01-12T15:54:44Z
```

# ruff --fix doesn't fix anything

---

_@anentropic_

```
$ ruff --version
ruff 0.0.262
```
```
$ ruff check --fix anetz
anetz/mappings.py:92:89: E501 Line too long (98 > 88 characters)
anetz/tools/evaluate.py:44:89: E501 Line too long (95 > 88 characters)
anetz/tools/evaluate.py:91:89: E501 Line too long (92 > 88 characters)
anetz/tools/evaluate.py:96:89: E501 Line too long (93 > 88 characters)
anetz/tools/evaluate.py:143:89: E501 Line too long (100 > 88 characters)
Found 5 errors.

$ ruff check --fix anetz
anetz/mappings.py:92:89: E501 Line too long (98 > 88 characters)
anetz/tools/evaluate.py:44:89: E501 Line too long (95 > 88 characters)
anetz/tools/evaluate.py:91:89: E501 Line too long (92 > 88 characters)
anetz/tools/evaluate.py:96:89: E501 Line too long (93 > 88 characters)
anetz/tools/evaluate.py:143:89: E501 Line too long (100 > 88 characters)
Found 5 errors.
```

I found the same with the VS Code plugin, it doesn't seem to do anything when you ask it to fix.

What am I doing wrong?


---

_Comment by @dhruvmanila on 2023-04-20 09:07_

You aren't doing anything wrong. Auto-fixes aren't available for all rules. As per the analysis output, the file only contains diagnostics which aren't fixable, so nothing happens.

You can refer to the [rules documentation](https://beta.ruff.rs/docs/rules/) and check if a diagnostic code has a fix available or not. In your case there's only `E501` for which `ruff` doesn't provide a fix.

---

_Comment by @anentropic on 2023-04-20 10:54_

ah, shame... "line too long" is the most common error and most useful one to have auto fixed

---

_Comment by @dhruvmanila on 2023-04-20 11:04_

Usually, a formatter like `black` would take care of splitting long lines but I guess, in your case, this might be a long string which it cannot _reliably_ split and thus doesn't.

---

_Comment by @anentropic on 2023-04-20 11:06_

ah I see, so I should still use `black` as well... I had thought this would replace it

---

_Comment by @evanrittenhouse on 2023-04-20 15:50_

I believe we have plans to add a Ruff formatter, but it's a ways off. For now, `black` is still required.

---

_Comment by @charliermarsh on 2023-04-21 01:25_

That's right. For now, we don't autofix line wrapping, and we do recommend using Ruff alongside Black.

---

_Closed by @charliermarsh on 2023-04-21 01:25_

---

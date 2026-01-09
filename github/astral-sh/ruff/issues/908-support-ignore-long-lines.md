---
number: 908
title: "Support `--ignore-long-lines`"
type: issue
state: closed
author: harupy
labels:
  - configuration
assignees: []
created_at: 2022-11-26T09:15:20Z
updated_at: 2022-11-28T03:36:19Z
url: https://github.com/astral-sh/ruff/issues/908
synced_at: 2026-01-07T13:12:14-06:00
---

# Support `--ignore-long-lines`

---

_Issue opened by @harupy on 2022-11-26 09:15_

pylint has `--ignore-long-lines` to specify a regular expression for a line that is allowed to be longer than the limit. 

```
> pylint --help
  ...
  --ignore-long-lines <regexp>
    Regexp for a line that is allowed to be longer than the limit. (default: ^\s*(# )?<?https?://\S+>?$)
```

This option is useful when you insert a comment containing a very long URL.

```
# For more information, see https://very/long/URL/very/long/URL/very/long/URL/very/long/URL
```

---

_Comment by @charliermarsh on 2022-11-26 15:05_

As an alternative, should we just allow long lines that end in a URL?

---

_Label `configuration` added by @charliermarsh on 2022-11-26 15:05_

---

_Comment by @harupy on 2022-11-26 16:29_

Agreed!

---

_Referenced in [astral-sh/ruff#920](../../astral-sh/ruff/pulls/920.md) on 2022-11-27 05:42_

---

_Closed by @charliermarsh on 2022-11-28 03:36_

---

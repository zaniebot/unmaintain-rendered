---
number: 5933
title: "Simplify `key in dct and dct[key]` to `dct.get(key)`"
type: issue
state: closed
author: janosh
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-07-20T22:24:38Z
updated_at: 2023-10-13T01:08:53Z
url: https://github.com/astral-sh/ruff/issues/5933
synced_at: 2026-01-07T13:12:15-06:00
---

# Simplify `key in dct and dct[key]` to `dct.get(key)`

---

_Issue opened by @janosh on 2023-07-20 22:24_

```py
# bad
if "key" in dct and dct["key"]:
    ...

# good
if dct.get("key"):
    ...
```

---

_Comment by @agnes-sharan on 2023-07-21 00:45_

Hi could I work on this?

---

_Label `rule` added by @charliermarsh on 2023-07-21 04:10_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-21 04:10_

---

_Comment by @charliermarsh on 2023-07-21 04:13_

Hi @agnes-sharan -- thanks for asking! We need to make a decision as maintainers on whether to include this (and how it should be categorized) before opening it up for contributors. We'll label it as such and comment here when there's consensus.

---

_Comment by @sbrugman on 2023-07-21 08:17_

Examples in the wild: https://grep.app/search?q=%20%22%28.%2B%29%22%20in%20%5Ba-z0-9%5D%2B%20and%20%5Ba-z0-9%5D%2B%5C%5B%22%28.%2B%29%22%5C%5D%3A&regexp=true

It would make sense to propose this rule in [`flake8-simplify`](https://github.com/MartinThoma/flake8-simplify). They already have a couple of dict-related rules, and then port it to `ruff` (if they are responsive).

---

_Renamed from "Simplify `key in dict` check before access to `dict.get`" to "Simplify `key in dct and dct[key]` to `dct.get(key)`" by @janosh on 2023-07-21 14:19_

---

_Comment by @janosh on 2023-07-21 14:35_

[The regex can be made more generic to increase the number of hits](https://grep.app/search?q=%22%28.%2B%29%22%20in%20.%2B%20and%20.%2B%5C%5B%22%28.%2B%29%22%5C%5D&regexp=true). The site doesn't seem to support backreferences `(.+) in .+ and .+\[\1\]` so might be some false positives in there.

---

_Comment by @sbrugman on 2023-07-23 07:49_

Refurb might be a candidate to include this rule into: https://github.com/dosisod/refurb

In the cases that `dct` is a common dict, this syntax is clearly more readable (just one ref to `dct` and `key`)

Alternatively, this could be a RUF category rule.

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-21 00:55_

---

_Label `accepted` added by @charliermarsh on 2023-09-21 00:55_

---

_Comment by @charliermarsh on 2023-09-21 00:56_

We can add this as a `ruff` rule if there's interest.

---

_Comment by @harupy on 2023-10-10 12:01_

@charliermarsh can I work on this?

---

_Comment by @charliermarsh on 2023-10-10 12:03_

Go for it!

---

_Comment by @harupy on 2023-10-10 12:03_

How would you name this rule?

---

_Comment by @charliermarsh on 2023-10-10 12:03_

We may need to limit this to boolean tests, since it returns `None` rather than `False`.

---

_Comment by @charliermarsh on 2023-10-10 12:12_

I'm not sure, maybe like `UnnecessaryDictIn`? Easy to rename during review though.

---

_Referenced in [astral-sh/ruff#7895](../../astral-sh/ruff/pulls/7895.md) on 2023-10-10 14:02_

---

_Closed by @charliermarsh on 2023-10-13 01:08_

---

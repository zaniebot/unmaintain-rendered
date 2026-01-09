---
number: 7434
title: Method for enabling specific preview rules
type: issue
state: closed
author: btjones0
labels:
  - configuration
  - needs-decision
  - preview
assignees: []
created_at: 2023-09-16T09:48:52Z
updated_at: 2023-09-28T20:00:35Z
url: https://github.com/astral-sh/ruff/issues/7434
synced_at: 2026-01-07T13:12:15-06:00
---

# Method for enabling specific preview rules

---

_Issue opened by @btjones0 on 2023-09-16 09:48_

My ruff configuration is structured as a deny list where I've enabled all rules and then disable the rules that I don't want.

In 0.0.289, this looked something like:
```
preview = true  # added in 0.0.289
select = [
    "ALL",
    # other specific preview codes
]
ignore = [
    "PREVIEW",  # added in 0.0.289
    # other selectors I don't want
]
```

With the removal of the "PREVIEW" selector in 0.0.290, it doesn't look like there's an easy way to disable all the preview checks that I don't want without either:
* changing my config to be an allow list rather than a deny list, or
* explicitly ignoring every preview check that I'm not selecting with it's fully qualified code.

I would love to have some mechanism to select specific preview rules without doing either of these.

---

_Comment by @charliermarsh on 2023-09-16 13:15_

Thanks for this -- we actually discussed this internally last week and have something proposed in https://github.com/astral-sh/ruff/pull/7390 that would enable this kind of workflow (although we haven't yet decided to merge it). I'll \cc @zanieb who is owning preview behavior and thinking about these kinds of configuration issues.

(Zanie is out next week so might be a few days before a reply.)

---

_Assigned to @zanieb by @charliermarsh on 2023-09-16 13:15_

---

_Label `configuration` added by @charliermarsh on 2023-09-16 13:15_

---

_Label `preview` added by @charliermarsh on 2023-09-16 13:15_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-16 13:15_

---

_Comment by @zanieb on 2023-09-16 14:32_

Ah we thought the `PREVIEW` selector was generally useless for ignores because of it's precedence but I missed that it was usable with `ALL`.

I'm happy with the approach in #7390 but wanted to wait for some user feedback regarding the current experience to ensure it resolves your needs. Please let us know if it'd work for you!


---

_Comment by @btjones0 on 2023-09-16 19:29_

The approach in #7390 looks good to me.

---

_Referenced in [astral-sh/ruff#7390](../../astral-sh/ruff/pulls/7390.md) on 2023-09-26 20:27_

---

_Closed by @zanieb on 2023-09-28 20:00_

---

_Referenced in [astral-sh/ruff#12343](../../astral-sh/ruff/issues/12343.md) on 2024-07-16 09:13_

---

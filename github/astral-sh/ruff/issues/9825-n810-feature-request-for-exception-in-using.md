---
number: 9825
title: "[N810] Feature Request for Exception in using CapWords for Class Names"
type: issue
state: closed
author: kunaltyagi
labels:
  - needs-decision
assignees: []
created_at: 2024-02-05T08:24:48Z
updated_at: 2024-02-08T04:19:46Z
url: https://github.com/astral-sh/ruff/issues/9825
synced_at: 2026-01-07T13:12:15-06:00
---

# [N810] Feature Request for Exception in using CapWords for Class Names

---

_Issue opened by @kunaltyagi on 2024-02-05 08:24_

```
custom_ops.py:173:7: N801 Class name `PReLU_Function` should use CapWords convention
```

We can use `PreluFunction` (which hides some semantics) but `PReLUFunction` looks ugly. Can we add an exception such that `_` is allowed only if the previous letter is upper case?

We can ofc add an ignore on this line, but maybe there should be a teeny tiny exception for this?

```
# current
splits = class_name.split("_")
if len(splits) > 1:
    log_error()
```

```
# request
splits = class_name.split("_")

for prev_split, split in zip(splits[:-1], splits[1:]):
    if prev_split[-1].isupper() and split[0].isupper():  # split should have upper case on either end
        continue
    else:
        log_error()
```

---

_Label `needs-decision` added by @charliermarsh on 2024-02-08 04:17_

---

_Comment by @charliermarsh on 2024-02-08 04:19_

Hmm, I think the intention of `CapsWords` is that you write it as `PReLUFunction`. I could understand an argument that it's ugly but it does adhere to the intent and description as in [PEP 8](https://peps.python.org/pep-0008/#descriptive-naming-styles).

My suggestion would probably be to explicitly ignore it via [`ignore-names`](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names), which lets you exempt certain names (and does support glob patterns).

---

_Closed by @charliermarsh on 2024-02-08 04:19_

---

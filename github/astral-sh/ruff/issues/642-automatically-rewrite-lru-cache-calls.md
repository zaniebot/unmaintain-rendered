---
number: 642
title: "Automatically rewrite `lru_cache` calls"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - rule
assignees: []
created_at: 2022-11-07T14:57:11Z
updated_at: 2022-11-16T04:08:21Z
url: https://github.com/astral-sh/ruff/issues/642
synced_at: 2026-01-07T13:12:14-06:00
---

# Automatically rewrite `lru_cache` calls

---

_Issue opened by @charliermarsh on 2022-11-07 14:57_

See: https://github.com/asottile/pyupgrade#remove-parentheses-from-functoolslru_cache and https://github.com/asottile/pyupgrade#replace-functoolslru_cachemaxsizenone-with-shorthand.


---

_Label `good first issue` added by @charliermarsh on 2022-11-07 14:57_

---

_Label `rule` added by @charliermarsh on 2022-11-07 14:57_

---

_Comment by @charliermarsh on 2022-11-07 14:59_

If interested, there are some instructions on adding new rules in the `CONTRIBUTING.md` and [here](https://github.com/charliermarsh/ruff/issues/312#issuecomment-1280008634).


---

_Comment by @chammika-become on 2022-11-09 04:36_

@charliermarsh I have a working version on this. ATM, it can correctly identify the and generate checks. Could you please have a look at this. 
https://github.com/charliermarsh/ruff/compare/main...chammika-become:ruff:rewrite-lru-cache

sp.  `src/pyupgrade/checks.rs#unnecessary_lru_cache_params`

Any improvements are appreciated. FYI, I am learning Rust. If you are satisfied I will make a PR 👍 

---

_Comment by @charliermarsh on 2022-11-09 04:44_

Generally looks good! I have some small comments, but feel free to send a PR and I'll make them inline.

The biggest comment is: rather than using two arms for the `match &func.node` with very similar case bodies, could we instead use `match_name_or_attr` in `src/ast/helpers.rs` to match against `lru_cache`, then move all the check logic into a single code block?


---

_Comment by @charliermarsh on 2022-11-09 05:15_

If we want to be even more rigorous, you could use `match_name_or_attr_from_module` in the same module, and add `functools` to `TRACK_FROM_IMPORTS` in `check_ast.rs`. That way, we would enforce that `lru_cache` has to be imported from `functools`.

---

_Comment by @chammika-become on 2022-11-09 05:45_

@charliermarsh above! Thanks 
I was wondering how to enforce that we are checking only the `functools.lru_cache` and not any user defined `lru_cache`

---

_Referenced in [astral-sh/ruff#664](../../astral-sh/ruff/pulls/664.md) on 2022-11-09 05:51_

---

_Referenced in [astral-sh/ruff#667](../../astral-sh/ruff/pulls/667.md) on 2022-11-10 14:21_

---

_Closed by @charliermarsh on 2022-11-16 04:08_

---

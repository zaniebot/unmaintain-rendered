---
number: 4761
title: "`--fix` doesn't have any effect on nursery rules"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
created_at: 2023-05-31T16:39:51Z
updated_at: 2023-06-05T02:25:02Z
url: https://github.com/astral-sh/ruff/issues/4761
synced_at: 2026-01-07T13:12:14-06:00
---

# `--fix` doesn't have any effect on nursery rules

---

_Issue opened by @charliermarsh on 2023-05-31 16:39_

Running, e.g., `cargo run -p ruff_cli -- check ../scipy --select E201,E202,E203 --fix` doesn't actually cause those rules to be marked as fixable. In `src/settings/mod.rs`, we initialize the set of fixable rules with `let mut fixable_set: RuleSet = RuleSelector::All.into_iter().collect();`. However, nursery rules are omitted from `RuleSelector::All`, to avoid adding them when users do `--select ALL`.

---

_Label `bug` added by @charliermarsh on 2023-05-31 16:39_

---

_Label `cli` added by @charliermarsh on 2023-05-31 16:39_

---

_Comment by @evanrittenhouse on 2023-05-31 17:34_

@charliermarsh feel free to assign this to me, I can take a look. Seems like we'll have to union the `fixable_set` with the command line rules.

---

_Assigned to @evanrittenhouse by @charliermarsh on 2023-05-31 17:45_

---

_Comment by @evanrittenhouse on 2023-06-03 01:47_

@charliermarsh I'm actually curious how we want to do this. I was originally thinking the easiest way would be to find all autofixable rules in the `--select` block, then change them into `RuleSelectors` and add them to `Configuration::fixable`. I don't think that would respect the specificity differences between `--select` and `--extend-select`. I guess we can just merge them and let `Configuration` handle it later, right?

---

_Comment by @charliermarsh on 2023-06-03 03:07_

I'm not sure. I was wondering if we could change the `let mut fixable_set: RuleSet = RuleSelector::All.into_iter().collect();` initialization line in some way, such that it includes _all_ rules rather than using the `ALL` selector.

---

_Comment by @evanrittenhouse on 2023-06-03 13:45_

Ah interesting, okay let me take a look at that. That seems like it'd be easier than what I was thinking anyway (parsing `RuleSelectors` from the CLI into `Rules`, checking if they're autofixable, then translating back into `RuleSelectors`).

I wonder if it's worth having `RuleSelector::All` be *all* rules, and `RuleSelector::Stable` (or some other name) represent all non-nursery rules. We already expose the "nursery" idea to users, so I think it's safe to bring into a `RuleSelector` ([ref](https://beta.ruff.rs/docs/rules/indentation-with-invalid-multiple-comment/)). That'd be a pretty breaking change though.

---

_Comment by @charliermarsh on 2023-06-03 14:52_

What if we did the opposite: added a RuleSelector::NURSERY? And then initialized the fixable set to ALL and NURSERY.

---

_Comment by @evanrittenhouse on 2023-06-03 15:12_

Sounds like a plan to me!

---

_Referenced in [astral-sh/ruff#4852](../../astral-sh/ruff/pulls/4852.md) on 2023-06-04 16:07_

---

_Closed by @charliermarsh on 2023-06-05 02:25_

---

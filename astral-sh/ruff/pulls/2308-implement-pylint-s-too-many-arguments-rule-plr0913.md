```yaml
number: 2308
title: "Implement Pylint's `too-many-arguments` rule (`PLR0913`)"
type: pull_request
state: merged
author: akhildevelops
labels:
  - rule
assignees: []
merged: true
base: main
head: plr0913
created_at: 2023-01-28T19:06:30Z
updated_at: 2023-01-30T13:04:38Z
url: https://github.com/astral-sh/ruff/pull/2308
synced_at: 2026-01-12T04:52:00Z
```

# Implement Pylint's `too-many-arguments` rule (`PLR0913`)

---

_Pull request opened by @akhildevelops on 2023-01-28 19:06_

Pylint Rule: too-many-arguments / R0913 https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/too-many-arguments.html

related to https://github.com/charliermarsh/ruff/issues/970

---

_Renamed from "plr0913" to "Pylint Rule: plr0913" by @akhildevelops on 2023-01-28 19:07_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/too_many_args.rs`:9 on 2023-01-28 19:54_

I suspect that this also needs to include `kwargs`, `kwonly_args`, and the other fields on `Arguments`. Can you check?

---

_@charliermarsh reviewed on 2023-01-28 19:54_

---

_Review comment by @charliermarsh on `src/rules/pylint/settings.rs`:63 on 2023-01-28 19:54_

Looks like this was copied over from `allow_magic_value_types` -- mind writing something relevant for this new option?

---

_@charliermarsh reviewed on 2023-01-28 19:54_

---

_Review comment by @charliermarsh on `src/rules/pylint/settings.rs`:60 on 2023-01-28 19:54_

Nit: should be `max-args = 5`

---

_@charliermarsh reviewed on 2023-01-28 19:54_

---

_@charliermarsh reviewed on 2023-01-28 19:55_

---

_Review comment by @charliermarsh on `src/violations.rs`:886 on 2023-01-28 19:55_

Can you move this into `src/rules/pylint/rules/too_many_args.rs`? We're trying to colocate violations with their rule implementations going forward.

---

_Review comment by @akhildevelops on `src/rules/pylint/rules/too_many_args.rs`:9 on 2023-01-29 04:40_

This rule only checks for args: https://github.com/PyCQA/pylint/blob/af555eddf6ecdb1993345dce728a6b4c38dd4a02/pylint/checkers/design_analysis.py#L519-L524

---

_@akhildevelops reviewed on 2023-01-29 04:40_

---

_Comment by @akhildevelops on 2023-01-29 07:09_

Hi @charliermarsh :headphones: 

I've made changes as suggested and also added an extra configuration param: [`ignored-argument-names`](https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#ignored-argument-names). It's used to [ignore arguments that match the pattern](https://github.com/PyCQA/pylint/blob/af555eddf6ecdb1993345dce728a6b4c38dd4a02/pylint/checkers/design_analysis.py#L511-L518) and then validate the rule.

---

_@charliermarsh reviewed on 2023-01-29 16:35_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/too_many_args.rs`:9 on 2023-01-29 16:35_

Ohh, interesting! Thanks.

---

_@charliermarsh reviewed on 2023-01-29 16:36_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pylint/too_many_args.py`:19 on 2023-01-29 16:36_

Can we add some test cases here for: arguments with default values (`a=1`), keyword-only args (`def f(a, b, *, c, d)`), and positional-only args (`def f(a, b, /, c, d)`)?

---

_@charliermarsh reviewed on 2023-01-29 16:37_

---

_Review comment by @charliermarsh on `src/rules/pylint/settings.rs`:66 on 2023-01-29 16:37_

Definitely appreciate you going through the exercise of adding this, but could we instead use `settings.dummy_variable_rgx`? It's kind of nice to have a centralized definition for "ignorable names", and we already use that across a variety of checks.

---

_Review comment by @akhildevelops on `src/rules/pylint/settings.rs`:66 on 2023-01-29 16:54_

Sure! I will use this. Thanks for pointing out :point_up:

---

_@akhildevelops reviewed on 2023-01-29 16:54_

---

_@charliermarsh reviewed on 2023-01-29 18:20_

---

_Review comment by @charliermarsh on `src/rules/pylint/settings.rs`:66 on 2023-01-29 18:20_

No prob! Sorry that you went through the effort of doing this only to roll it back. But hopefully it'll be a more cohesive experience for users.

---

_Comment by @akhildevelops on 2023-01-30 06:29_

Hi @charliermarsh, Done with the changes. 

---

_Renamed from "Pylint Rule: plr0913" to "Implement Pylint's `too-many-arguments` rule (`PLR0913`)" by @charliermarsh on 2023-01-30 12:34_

---

_Label `rule` added by @charliermarsh on 2023-01-30 12:34_

---

_Merged by @charliermarsh on 2023-01-30 12:34_

---

_Closed by @charliermarsh on 2023-01-30 12:34_

---

_Branch deleted on 2023-01-30 13:04_

---

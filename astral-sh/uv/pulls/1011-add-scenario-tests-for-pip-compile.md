```yaml
number: 1011
title: "Add scenario tests for `pip-compile`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/compile-scenarios
created_at: 2024-01-19T18:46:10Z
updated_at: 2024-01-21T23:47:44Z
url: https://github.com/astral-sh/uv/pull/1011
synced_at: 2026-01-10T15:39:03Z
```

# Add scenario tests for `pip-compile`

---

_Pull request opened by @zanieb on 2024-01-19 18:46_

e.g. for scenarios that test resolution _without_ installation.

This refactors the `update` script to generate scenario test files for `pip compile` _and_ `pip install`. We don't overlap scenarios to save time. We only generate `pip compile` test cases for scenarios we cannot represent with `pip install` e.g. a `--python-version` override.

The _one_ scenario I added happened to reveal a bug in our resolver where we were incorrectly filtering versions by the installed version when wheels were available. Per the comment at https://github.com/astral-sh/puffin/issues/883#issuecomment-1890773112, we should _only_ need to check for a compatible installed Python version when using a different _target_ Python version if we need to build a source distribution. https://github.com/astral-sh/puffin/commit/53bce684007aec7662ad2abb493a5f083e194681 resolves this by removing the excessive constraints — the correct Python version incompatibilities are applied elsewhere.

---

_@zanieb reviewed on 2024-01-19 18:47_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:72 on 2024-01-19 18:47_

So uh this is wrong I feel like this should have resolved successfully since we set 3.11?

---

_@zanieb reviewed on 2024-01-19 18:48_

---

_Review comment by @zanieb on `scripts/scenarios/update.py`:194 on 2024-01-19 18:48_

Here we check if the scenario requires a custom resolver option that wouldn't let it be installed

---

_@zanieb reviewed on 2024-01-19 18:51_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:53 on 2024-01-19 18:51_

I would _also_ like to include satisfiability assertions here like this scenario tells us it should succeed so we should assert the command succeeded and we also know which versions we should resolve but I can't figure out how to do that _and_ have a nice snapshot.

---

_@zanieb reviewed on 2024-01-19 18:52_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:53 on 2024-01-19 18:52_

@konstin ?

---

_@zanieb reviewed on 2024-01-19 22:19_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:72 on 2024-01-19 22:19_

Resolved by https://github.com/astral-sh/puffin/pull/1011/commits/53bce684007aec7662ad2abb493a5f083e194681

This was a bug in our resolver.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-19 22:31_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile_scenarios.rs`:53 on 2024-01-20 12:26_

This is the best I have for it: https://github.com/astral-sh/puffin/blob/53b7e3cb4f44e9ac7178154c0dcdf99ebd3f8cef/crates/puffin/tests/pip_compile.rs#L3326.

---

_@charliermarsh reviewed on 2024-01-20 12:26_

---

_@charliermarsh approved on 2024-01-20 12:26_

---

_@charliermarsh reviewed on 2024-01-20 12:27_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile_scenarios.rs`:53 on 2024-01-20 12:27_

Err, wait, I think I'm misunderstanding the comment. Can you rephrase?

---

_@zanieb reviewed on 2024-01-20 16:36_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile_scenarios.rs`:53 on 2024-01-20 16:36_

Oh I could read the output file! Good idea.

---

_Merged by @zanieb on 2024-01-21 23:47_

---

_Closed by @zanieb on 2024-01-21 23:47_

---

_Branch deleted on 2024-01-21 23:47_

---

```yaml
number: 2559
title: Rename the misnamed pylint messages
type: pull_request
state: merged
author: Pierre-Sassoulas
labels: []
assignees: []
merged: true
base: main
head: rename-misnamed-pylint-messages
created_at: 2023-02-03T22:41:11Z
updated_at: 2023-02-04T08:03:59Z
url: https://github.com/astral-sh/ruff/pull/2559
synced_at: 2026-01-12T04:52:00Z
```

# Rename the misnamed pylint messages

---

_Pull request opened by @Pierre-Sassoulas on 2023-02-03 22:41_

The name was wrong for three messages:

https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/too-many-arguments.html
https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/comparison-of-constants.html
https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/consider-using-sys-exit.html

Follow-up to https://github.com/charliermarsh/ruff/pull/2546#discussion_r1096002355

It might be a good thing to add a check for the link validity in #2546 later on

---

_Review comment by @charliermarsh on `src/rules/pylint/mod.rs`:23 on 2023-02-03 23:00_

I think we need to rename the files as well -- they're in `./resources/test/fixtures/pylint`.

---

_@charliermarsh reviewed on 2023-02-03 23:00_

---

_Review comment by @charliermarsh on `src/rules/pylint/mod.rs`:23 on 2023-02-03 23:23_

Just LMK if you have any trouble with this -- you'll probably need to rename these files, then run `cargo test --lib`, then `cargo insta review`, _then_ `cargo insta test --delete-unreferenced-snapshots` :joy:

---

_@charliermarsh reviewed on 2023-02-03 23:23_

---

_@Pierre-Sassoulas reviewed on 2023-02-03 23:29_

---

_Review comment by @Pierre-Sassoulas on `src/rules/pylint/mod.rs`:23 on 2023-02-03 23:29_

Huhu, thank you, I missed the last one 😄 The commands are incredibly slow on my machine and I need to rebuild on each commit, is there any way to make it faster ? Maybe it's because I don't have enough disk space, only python was supposed to run on this machine originally.

---

_Review requested from @charliermarsh by @Pierre-Sassoulas on 2023-02-03 23:43_

---

_Merged by @charliermarsh on 2023-02-03 23:58_

---

_Closed by @charliermarsh on 2023-02-03 23:58_

---

_@charliermarsh reviewed on 2023-02-04 00:00_

---

_Review comment by @charliermarsh on `src/rules/pylint/mod.rs`:23 on 2023-02-04 00:00_

Unfortunately there's no trick to making the rebuilds faster. Some of it is just Rust, but a lot of it is that we have a bad project structure right now -- we need to split the main crate into multiple crates, which should make compilation a lot faster (#1820).

---

_Branch deleted on 2023-02-04 08:03_

---

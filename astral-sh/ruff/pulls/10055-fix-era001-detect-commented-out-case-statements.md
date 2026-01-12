```yaml
number: 10055
title: "fix(ERA001): detect commented out `case` statements, add more one-line support"
type: pull_request
state: merged
author: ottaviohartman
labels: []
assignees: []
merged: true
base: main
head: thartman/era-add-case-statemnt
created_at: 2024-02-20T00:28:40Z
updated_at: 2024-02-20T03:57:09Z
url: https://github.com/astral-sh/ruff/pull/10055
synced_at: 2026-01-12T15:55:31Z
```

# fix(ERA001): detect commented out `case` statements, add more one-line support

---

_@ottaviohartman_

## Summary

Closes #10031 

- Detect commented out `case` statements. Playground repro: https://play.ruff.rs/5a305aa9-6e5c-4fa4-999a-8fc427ab9a23
- Add more support for one-line commented out code.

## Test Plan

Unit tested and tested with
```sh
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/eradicate/ERA001.py --no-cache --preview --select ERA001
```

TODO:
- [x] `cargo insta test`

---

_@charliermarsh reviewed on 2024-02-20 00:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/rules/commented_out_code.rs`:51 on 2024-02-20 00:29_

Was this change intentional? Just checking.

---

_@charliermarsh reviewed on 2024-02-20 00:29_

Thanks!

---

_@ottaviohartman reviewed on 2024-02-20 00:30_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 00:30_

note: this group ends with `.*` to detect the one-line case:
```python
case "hi": print()
```

---

_@ottaviohartman reviewed on 2024-02-20 00:30_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/eradicate/rules/commented_out_code.rs`:51 on 2024-02-20 00:30_

Yes, but I can revert! Just was a style lint in VSCode

---

_@charliermarsh reviewed on 2024-02-20 00:32_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 00:32_

I feel like the other cases don't catch that -- can you try to add a test case with `try` followed by a one-line body?

---

_@ottaviohartman reviewed on 2024-02-20 00:34_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 00:34_

Yep - I think this will catch more non-code statements though like "to try: use iterators" or something. I'll write some tests to see

---

_@ottaviohartman reviewed on 2024-02-20 00:38_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 00:38_

Ok seems to work well, should I add `.*` for all of the cases, and add tests?

---

_Renamed from "fix(ERA001): detect commented out case statements" to "fix(ERA001): detect commented out `case` statements, add more one-line support" by @ottaviohartman on 2024-02-20 00:51_

---

_Review requested from @charliermarsh by @ottaviohartman on 2024-02-20 00:52_

---

_@charliermarsh reviewed on 2024-02-20 02:34_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 02:34_

Let's remove the trailing `.*` from this PR, and we can open a separate PR to add `.*` to all of the cases. It'll make it easier to approve and merge these as separate changes.

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 03:05_

Cool will do

---

_@ottaviohartman reviewed on 2024-02-20 03:05_

---

_@charliermarsh reviewed on 2024-02-20 03:17_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 03:17_

Ah sorry, I meant the opposite: can we remove the `.*` from here, so that we don't support one-line for _any_ of the patterns (like before this PR), and then open a separate PR that adds the `.*` to all of them at once?

---

_@ottaviohartman reviewed on 2024-02-20 03:21_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 03:21_

Yep, that's what I understood you to mean! I'll do that and open an issue to add `.*` in another PR


---

_@ottaviohartman reviewed on 2024-02-20 03:48_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 03:48_

Done! Ticket opened, I'll open that PR next.

---

_Merged by @charliermarsh on 2024-02-20 03:56_

---

_Closed by @charliermarsh on 2024-02-20 03:56_

---

_Comment by @charliermarsh on 2024-02-20 03:56_

Thank you!

---

_@charliermarsh reviewed on 2024-02-20 03:57_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:29 on 2024-02-20 03:57_

> Yep, that's what I understood you to mean! I'll do that and open an issue to add .* in another PR

(Oops, sorry, I think I was looking at the code, and thought you'd already pushed!)

---

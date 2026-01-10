```yaml
number: 13600
title: "[flake8-bugbear] Consider replacing f\"'{foo}'\" with f\"{foo!r}\" which is both easier to read and will escape quotes inside foo if that would appear (B907)"
type: pull_request
state: closed
author: agpt8
labels: []
assignees: []
draft: true
base: main
head: implement-b903-b907
created_at: 2024-10-02T09:57:16Z
updated_at: 2024-10-14T14:15:41Z
url: https://github.com/astral-sh/ruff/pull/13600
synced_at: 2026-01-10T20:59:36Z
```

# [flake8-bugbear] Consider replacing f"'{foo}'" with f"{foo!r}" which is both easier to read and will escape quotes inside foo if that would appear (B907)

---

_Pull request opened by @agpt8 on 2024-10-02 09:57_

B907: Consider replacing f"'{foo}'" with f"{foo!r}" which is both easier to read and will escape quotes inside foo if that would appear. The check tries to filter out any format specs that are invalid together with !r. If you’re using other conversion flags then e.g. f"'{foo!a}'" can be replaced with f"{ascii(foo)!r}". Not currently implemented for python<3.8 or str.format() calls.

Implement B907 flake8-bugbear rules in Ruff.

* Add `crates/ruff_linter/src/rules/flake8_bugbear/rules/b907.rs` to implement B907 rule to replace f"'{foo}'" with f"{foo!r}".
* Update `crates/ruff_linter/src/rules/flake8_bugbear/mod.rs` to include B907 rules.
* Add test cases for B907 rules in `crates/ruff_linter/src/rules/flake8_bugbear/tests.rs`.



---

_Comment by @dhruvmanila on 2024-10-04 05:46_

FYI, there's another implementation of `B903` at https://github.com/astral-sh/ruff/pull/12259

---

_Comment by @agpt8 on 2024-10-04 05:54_

Thanks @dhruvmanila. I'll remove the B903 implementation and instead create a PR for B907

---

_Renamed from "Implement B903 and B907 flake8-bugbear rules" to "[flake8-bugbear] Consider replacing f"'{foo}'" with f"{foo!r}" which is both easier to read and will escape quotes inside foo if that would appear (B907)" by @agpt8 on 2024-10-14 14:00_

---

_Closed by @agpt8 on 2024-10-14 14:13_

---

_Branch deleted on 2024-10-14 14:13_

---

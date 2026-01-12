```yaml
number: 679
title: Only scan checks once in check_lines
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: quadratic
created_at: 2022-11-11T06:15:11Z
updated_at: 2022-11-11T18:34:23Z
url: https://github.com/astral-sh/ruff/pull/679
synced_at: 2026-01-12T05:48:45Z
```

# Only scan checks once in check_lines

---

_Pull request opened by @andersk on 2022-11-11 06:15_

Avoids a quadratic slowdown when there are many checks. For example, `yes | head -n 100000 | ruff -` goes from 25s to 0.2s. Fixes #483.

---

_Review comment by @Stranger6667 on `src/check_lines.rs`:97 on 2022-11-11 12:13_

Should this condition be merged with the one below? I.e:

```rust
checks_iter.next_if(|(_index, check)| check.location.row() == lineno + 1)
```

---

_@Stranger6667 reviewed on 2022-11-11 12:13_

---

_Review comment by @charliermarsh on `src/check_lines.rs`:97 on 2022-11-11 16:20_

Yeah I think you're right -- do you agree @andersk?

---

_@charliermarsh reviewed on 2022-11-11 16:20_

---

_@andersk reviewed on 2022-11-11 17:51_

---

_Review comment by @andersk on `src/check_lines.rs`:97 on 2022-11-11 17:51_

Is there still a possibility of a check on line 0? If so, it would prevent that merged condition from ever succeeding.

---

_@charliermarsh reviewed on 2022-11-11 17:54_

---

_Review comment by @charliermarsh on `src/check_lines.rs`:97 on 2022-11-11 17:54_

I don't think so, the rows are always 1-indexed.

---

_@andersk reviewed on 2022-11-11 18:08_

---

_Review comment by @andersk on `src/check_lines.rs`:97 on 2022-11-11 18:08_

I’m thinking specifically of #237, before which there were errors like

```
test.py:0:0: E999 SyntaxError: unexpected EOF while parsing at line 3 column 7
```

This happened because `Location::default()` is 0:0. We still use `Location::default()` when reporting `IOError`. Is that definitely going to be the only case?

I guess we can add an assertion for this at the top of the loop.

---

_Merged by @charliermarsh on 2022-11-11 18:34_

---

_Closed by @charliermarsh on 2022-11-11 18:34_

---

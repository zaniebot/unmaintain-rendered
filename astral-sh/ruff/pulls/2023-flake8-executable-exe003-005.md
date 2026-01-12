```yaml
number: 2023
title: "[`flake8-executable`] EXE003-005"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: feat/flake8-executable-e004-e005
created_at: 2023-01-20T11:40:28Z
updated_at: 2023-01-20T23:48:21Z
url: https://github.com/astral-sh/ruff/pull/2023
synced_at: 2026-01-12T04:52:00Z
```

# [`flake8-executable`] EXE003-005

---

_Pull request opened by @sbrugman on 2023-01-20 11:40_

Tracking issue: https://github.com/charliermarsh/ruff/issues/2024

Implementation for EXE003, EXE004 and EXE005 of `flake8-executable` 
(shebang should contain "python", not have whitespace before, and should be on the first line)

Please take in mind that this is my first rust contribution.

The remaining EXE-rules are a combination of shebang (`lines.rs`), file permissions (`fs.rs`) and if-conditions (`ast.rs`). I was not able to find other rules that have interactions/dependencies in them. Any advice on how this can be best implemented would be very welcome.

For autofixing `EXE005`, I had in mind to _move_  the shebang line to the top op the file. This could be achieved by a combination of `Fix::insert` and `Fix::delete` (multiple fixes per diagnostic), or by implementing a dedicated `Fix::move`, or perhaps in other ways. For now I've left it out, but keen on hearing what you think would be most consistent with the package, and pointer where to start (if at all).

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_executable/EXE005_3.py`:6 on 2023-01-20 12:21_

Are these intentionally missing newlines at end-of-file?

---

_Renamed from "[`flake8-executable`] EXE004 and EXE005" to "[`flake8-executable`] EXE003-005" by @sbrugman on 2023-01-20 12:22_

---

_Review comment by @charliermarsh on `src/rules/flake8_executable/rules/shebang_whitespace.rs`:17 on 2023-01-20 12:24_

Sorry for the nit, but we tend to exclude punctuation unless it's a multi-sentence message. Stylistically, this might fit in a little more: `format!("Avoid whitespace before shebang")`

---

_Review comment by @charliermarsh on `src/rules/flake8_executable/helpers.rs`:11 on 2023-01-20 12:24_

Can we add a comment here to document these fields? (Or use a struct with named fields and comments?) Will make it a bit clearer by disambiguating all the `usize` members.

---

_@charliermarsh reviewed on 2023-01-20 12:25_

This looks excellent, thank you for putting it together.

---

_Merged by @charliermarsh on 2023-01-20 23:19_

---

_Closed by @charliermarsh on 2023-01-20 23:19_

---

_Review comment by @charliermarsh on `src/checkers/lines.rs`:71 on 2023-01-20 23:20_

@sbrugman - I did some benchmarking and using a single `extract_shebang` call (+ the weird optimization track I added to the extraction method) brought time down from ~180ms to 100ms.

---

_@charliermarsh reviewed on 2023-01-20 23:20_

---

_Review comment by @charliermarsh on `src/checkers/lines.rs`:71 on 2023-01-20 23:20_

(Just noting here so that you're not confused if you come back to finish off the plugin.)

---

_@charliermarsh reviewed on 2023-01-20 23:20_

---

_Review comment by @sbrugman on `src/checkers/lines.rs`:71 on 2023-01-20 23:42_

Excellent, thanks. I considered it but was not sure if having the logic in `lines.rs` was ok, or that is should have been moved to the `flake8-executable` rules.

---

_@sbrugman reviewed on 2023-01-20 23:42_

---

_@sbrugman reviewed on 2023-01-20 23:42_

---

_Review comment by @sbrugman on `src/rules/flake8_executable/helpers.rs`:17 on 2023-01-20 23:42_

Good one

---

_Branch deleted on 2023-01-20 23:43_

---

_Review comment by @charliermarsh on `src/rules/flake8_executable/helpers.rs`:17 on 2023-01-20 23:48_

I feel like my brain is too small to understand why this is so effective (it reduced time from ~120ms to 100ms). I guess we're exploiting the fact that we know `!` is very uncommon, which `regex` itself can't know.

---

_@charliermarsh reviewed on 2023-01-20 23:48_

---

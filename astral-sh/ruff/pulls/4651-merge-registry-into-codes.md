```yaml
number: 4651
title: Merge registry into codes
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: macro_simplifications
created_at: 2023-05-25T08:57:33Z
updated_at: 2023-06-02T10:54:58Z
url: https://github.com/astral-sh/ruff/pull/4651
synced_at: 2026-01-12T03:50:03Z
```

# Merge registry into codes

---

_Pull request opened by @konstin on 2023-05-25 08:57_

## Summary

Currently, we register each rule twice: Once in registry.rs and once in codes.rs. This PR changes this to defining every rule only once in codes.rs. In the process i've refactored and documented `map_codes` a bit.

I've currently only driven this to the point where we have only one macro for registering all the rule, i could push this further by merging codes.rs and registry.rs into a single file (or two, one for registry and one for the helpers) and trimming down the unused `code_to_rule` to just a macro call.

You may find looking at the commits individually helpful commits helpful to see the refactor step by step.

## Test Plan

`cargo test` (and possible CI complaints)


---

_@MichaReiser approved on 2023-05-25 09:29_

---

_Comment by @github-actions[bot] on 2023-05-25 09:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.08ms     2.7 MB/sec    1.00     15.0±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    376.1±1.21µs     7.8 MB/sec    1.00    376.2±1.01µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.4 MB/sec    1.00      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1603.6±1.95µs    10.4 MB/sec    1.00   1602.5±3.70µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.0±0.35µs    16.6 MB/sec    1.00    178.4±0.38µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.02ms     7.1 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1123.6±3.06µs    14.8 MB/sec    1.00   1124.0±3.80µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    116.0±0.24µs    25.4 MB/sec    1.00    116.1±0.31µs    25.4 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     18.5±0.40ms     2.2 MB/sec    1.00     18.2±0.45ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.6±0.13ms     3.6 MB/sec    1.00      4.4±0.08ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   506.3±10.71µs     5.8 MB/sec    1.00   504.4±10.84µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.6±0.24ms     3.4 MB/sec    1.00      7.4±0.10ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.20ms     4.6 MB/sec    1.00      8.7±0.09ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1838.4±62.30µs     9.1 MB/sec    1.00  1819.2±23.28µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.2±6.94µs    14.5 MB/sec    1.01    205.4±8.95µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.06ms     6.6 MB/sec    1.03      4.0±0.15ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.5±0.06ms     6.2 MB/sec    1.00      6.6±0.07ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1223.6±18.56µs    13.6 MB/sec    1.00  1221.2±15.89µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    125.3±2.98µs    23.5 MB/sec    1.00    125.2±2.15µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.2 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @JonathanPlasse on `scripts/add_rule.py`:120 on 2023-05-25 11:50_

To fix the CI.
```diff
- with (ROOT_DIR / "crates/ruff/src/codes.rs").open("r") as fp:
-     while (line := next(fp)).strip() != f"// {linter}":
-         text += line
-     text += line
- 
-     lines = []
-     while (line := next(fp)).strip() != "":
-         lines.append(line)
- 
-     variant = pascal_case(linter)
-     lines.append(
-         " " * 8
-         + f"""({variant}, "{code}") => (RuleGroup::Unspecified, Rule::{name}),\n""",
-     )
-     lines.sort()
+ with (ROOT_DIR / "crates/ruff/src/codes.rs").open("r") as fp:
+     while (line := next(fp)).strip() != f"// {linter}":
+         text += line
+     text += line
+ 
+     lines = []
+     while (line := next(fp)).strip() != "":
+         lines.append(line)
+ 
+     variant = pascal_case(linter)
+     lines.append(
+         " " * 8
+         + f"""({variant}, "{code}") => (RuleGroup::Unspecified, rules::{linter.split(" ")[0]}::rules::{name}),\n""",
+     )
+     lines.sort()
```

---

_@JonathanPlasse reviewed on 2023-05-25 11:50_

---

_Review comment by @konstin on `scripts/add_rule.py`:120 on 2023-05-25 12:02_

Thank you!

---

_@konstin reviewed on 2023-05-25 12:02_

---

_Comment by @charliermarsh on 2023-05-25 13:01_

(Please don't merge prior to my review, I'd like to have a chance to read this one.)

---

_Review requested from @charliermarsh by @konstin on 2023-05-25 13:12_

---

_Review comment by @JonathanPlasse on `scripts/add_rule.py`:142 on 2023-05-25 13:30_

`lines.sort()` should before `text += "".join(lines)`.

---

_@JonathanPlasse reviewed on 2023-05-25 13:30_

---

_@konstin reviewed on 2023-05-25 14:08_

---

_Review comment by @konstin on `scripts/add_rule.py`:142 on 2023-05-25 14:08_

oops yes

---

_Comment by @evanrittenhouse on 2023-05-25 17:17_

Nice work!

---

_Review comment by @charliermarsh on `crates/ruff_macros/src/map_codes.rs`:64 on 2023-05-26 18:56_

Can we make these methods on `Entry` instead? Like `entry.group()`, and then we can have `entry.rule_name()` and `entry.rule_id()`?

---

_Review comment by @charliermarsh on `crates/ruff_macros/src/map_codes.rs`:266 on 2023-05-26 18:57_

Nit: replace with `rule_name`, and move that up?

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:14 on 2023-05-26 19:01_

Can / should this and `impl Rule` be in `codes.rs`?

---

_@charliermarsh approved on 2023-05-26 19:01_

---

_Review comment by @konstin on `crates/ruff_macros/src/map_codes.rs`:64 on 2023-06-01 09:13_

refactored the data into one `Rule` struct

---

_@konstin reviewed on 2023-06-01 09:13_

---

_@konstin reviewed on 2023-06-01 09:14_

---

_Review comment by @konstin on `crates/ruff/src/registry.rs`:14 on 2023-06-01 09:14_

yes, but i'd do that in a separate PR

---

_Review requested from @charliermarsh by @konstin on 2023-06-01 19:36_

---

_Comment by @konstin on 2023-06-01 19:36_

the code changed a good bit since the last review, would be good if you good give the derive code another look

---

_@charliermarsh reviewed on 2023-06-02 02:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:4 on 2023-06-02 02:01_

(Should these be `//! `?)

---

_@charliermarsh approved on 2023-06-02 02:02_

This looks reasonable but I'd suggest doing some manual testing, just to ensure that the prefixes and selectors work as expected.

---

_@MichaReiser approved on 2023-06-02 06:09_

---

_Merged by @konstin on 2023-06-02 10:33_

---

_Closed by @konstin on 2023-06-02 10:33_

---

_Branch deleted on 2023-06-02 10:33_

---

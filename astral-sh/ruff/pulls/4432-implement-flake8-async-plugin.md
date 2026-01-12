```yaml
number: 4432
title: Implement flake8-async plugin
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/4246-implement-flake8-async
created_at: 2023-05-14T17:23:33Z
updated_at: 2023-05-15T21:38:43Z
url: https://github.com/astral-sh/ruff/pull/4432
synced_at: 2026-01-12T15:55:15Z
```

# Implement flake8-async plugin

---

_@qdegraaf_

### Summary

This PR adds the flake8-async plugin: https://github.com/cooperlees/flake8-async

First Ruff PR, relatively new to Rust as well so all feedback is welcomed. I checked the contributing guide but if I ended up missing a step anyway let me know.

Closes: https://github.com/charliermarsh/ruff/issues/4246

Marked as draft until:

- [x] Fixed bug where from style imports are not caught

---

_Comment by @github-actions[bot] on 2023-05-14 17:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.5±0.95ms     2.3 MB/sec    1.03     18.0±0.89ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.3±0.23ms     3.9 MB/sec    1.00      4.2±0.23ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   533.2±39.15µs     5.5 MB/sec    1.04   554.6±31.35µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.4±0.45ms     3.4 MB/sec    1.00      7.2±0.34ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.42ms     4.8 MB/sec    1.00      8.5±0.44ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1808.5±91.55µs     9.2 MB/sec    1.00  1816.0±85.27µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.02   219.5±13.51µs    13.4 MB/sec    1.00   215.9±16.08µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.18ms     6.5 MB/sec    1.00      3.8±0.21ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.36ms     6.0 MB/sec    1.00      6.7±0.39ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1388.5±67.48µs    12.0 MB/sec    1.00  1370.3±94.14µs    12.2 MB/sec
parser/numpy/globals.py                    1.01    137.7±9.72µs    21.4 MB/sec    1.00    136.4±7.38µs    21.6 MB/sec
parser/pydantic/types.py                   1.02      3.0±0.14ms     8.4 MB/sec    1.00      3.0±0.15ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.2±0.65ms  1963.7 KB/sec    1.00     21.3±0.72ms  1954.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.24ms     3.1 MB/sec    1.01      5.3±0.24ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   631.5±31.84µs     4.7 MB/sec    1.01   637.0±55.14µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.30ms     2.9 MB/sec    1.01      9.0±0.39ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.4±0.63ms     3.9 MB/sec    1.00     10.4±0.28ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.7 MB/sec    1.01      2.2±0.09ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.01   256.9±20.27µs    11.5 MB/sec    1.00   253.4±14.82µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.17ms     5.6 MB/sec    1.03      4.7±0.40ms     5.4 MB/sec
parser/large/dataset.py                    1.02      8.6±0.27ms     4.7 MB/sec    1.00      8.4±0.30ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.02  1628.6±69.83µs    10.2 MB/sec    1.00  1600.4±64.20µs    10.4 MB/sec
parser/numpy/globals.py                    1.01    164.5±9.63µs    17.9 MB/sec    1.00   162.7±10.36µs    18.1 MB/sec
parser/pydantic/types.py                   1.02      3.6±0.12ms     7.1 MB/sec    1.00      3.5±0.12ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @mara004 on 2023-05-14 19:53_

Looks like #4433 was submitted simultaneously?

---

_Comment by @qdegraaf on 2023-05-14 21:16_

> Looks like #4433 was submitted simultaneously?

Yeah, see Issue discussion at https://github.com/charliermarsh/ruff/issues/4246. 

Started on this about a week ago only mentioning it in Discord while I was still in looking-and-playing-around-a-lot stage of devving and unsure whether I could get it to a proper feature in a reasonable time frame, so made no commitment on the issue. Only finished it up today when https://github.com/charliermarsh/ruff/pull/4433 was also pushed after a mention on the issue, which I then saw after already having opened the PR. 

Happy to close if that implementation is preferred. Unsure what the protocol is in these scenarios.

---

_Converted to draft by @qdegraaf on 2023-05-14 22:15_

---

_Marked ready for review by @charliermarsh on 2023-05-15 02:45_

---

_@charliermarsh reviewed on 2023-05-15 02:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_async/rules.rs`:201 on 2023-05-15 02:46_

@qdegraaf - I changed this to use `resolve_call_path`, which will automatically detect situations like `from os import system` or even `from os import system as foo` or `import os as foo; foo.system`.

---

_@charliermarsh reviewed on 2023-05-15 02:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_async/rules.rs`:138 on 2023-05-15 02:46_

@qdegraaf - I opted to remove the extra struct to house the candidate members, it didn't feel like it was buying us much, and without it we can just do a simple `.contains` check.

---

_@charliermarsh reviewed on 2023-05-15 02:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:743 on 2023-05-15 02:46_

We don't have any codes this long, but I don't see a good reason to deviate from upstream in this case 🤷 

---

_Label `rule` added by @charliermarsh on 2023-05-15 02:47_

---

_Comment by @charliermarsh on 2023-05-15 02:47_

@qdegraaf - From my perspective this is pretty much good to go -- but let me know if you have any questions or want to handle any follow-ups from your end before I merge.

---

_@inikolaev reviewed on 2023-05-15 06:01_

---

_Review comment by @inikolaev on `crates/ruff/src/rules/flake8_async/rules.rs`:67 on 2023-05-15 06:01_

Adjust these comments since we use `ASYNC` for clarity? 
```suggestion
/// ASYNC100
```

---

_@inikolaev reviewed on 2023-05-15 06:03_

---

_Review comment by @inikolaev on `crates/ruff/src/rules/flake8_async/rules.rs`:224 on 2023-05-15 06:03_

@charliermarsh Do you see this as belonging to some utilities - might reusable in the future? Or how do you usually approach these?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_async/rules.rs`:223 on 2023-05-15 06:57_

Does `any` work here?

```suggestion
        .any(|scope| {
            if let ScopeKind::Function(FunctionDef { async_, .. }) = &scope.kind {
                *async_
            } else {
                false
            }
        })
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_async/rules.rs`:224 on 2023-05-15 06:58_

I tend to keep them local to rules until we see to use them in many places. But it could be helpful to have an `in_async` on `context` or `scopes`

---

_@MichaReiser approved on 2023-05-15 06:59_

---

_Comment by @qdegraaf on 2023-05-15 08:16_

> @qdegraaf - From my perspective this is pretty much good to go -- but let me know if you have any questions or want to handle any follow-ups from your end before I merge.

@charliermarsh Awesome, agree with all the edits, thanks for polishing this up. Had a quick look at the await validation mentioned in the issue, but the original plugin does not cover this either by the looks of it, so probably best left separate or as part of some other plugin. Feel free to merge at your leisure.

---

_Merged by @charliermarsh on 2023-05-15 13:15_

---

_Closed by @charliermarsh on 2023-05-15 13:15_

---

_Branch deleted on 2023-05-15 21:38_

---

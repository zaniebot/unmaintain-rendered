```yaml
number: 4589
title: Update to the new rule architecture
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: update-to-the-new-rule-architecture
created_at: 2023-05-22T22:14:36Z
updated_at: 2023-05-24T16:16:47Z
url: https://github.com/astral-sh/ruff/pull/4589
synced_at: 2026-01-12T15:55:15Z
```

# Update to the new rule architecture

---

_@JonathanPlasse_

The scripts `add_rule.py` only work correctly when we use the following rule architecture when there is a rules folder that contains a file for each rule. Like this adding a rule consist of adding an entry in mod.rs and a new rule file and some other changes.
Without this change `add_rule.py` will create the files like in the new architecture but it will not compile as it requires changing manually the generated files.

**New architecture**
```text
- crates/ruff/src/rules
  - <linter>
    - mod.rs
    - rules/
      - mod.rs
      - <rule_name.rs>
      -  <rule_name.rs>
      - ...
```

**Old architecture**
```text
- crates/ruff/src/rules
  - <linter>
    - mod.rs
    - rules.rs
```

Here is an example with flake8_async:

**flake8_async/rules/mod.rs**
```rust
pub(crate) use blocking_http_call::{blocking_http_call, BlockingHttpCallInAsyncFunction};
pub(crate) use blocking_os_call::{blocking_os_call, BlockingOsCallInAsyncFunction};
pub(crate) use open_sleep_or_subprocess_call::{
    open_sleep_or_subprocess_call, OpenSleepOrSubprocessInAsyncFunction,
};
pub(crate) use new_rule::{new_rule, NewRule};

mod blocking_http_call;
mod blocking_os_call;
mod open_sleep_or_subprocess_call;
mod new_rule;
```

**flake8_async/rules/new_rule.rs**
```rust
<File with rule definition>
```

---

_Comment by @github-actions[bot] on 2023-05-22 22:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.3±0.27ms     2.5 MB/sec    1.00     16.1±0.31ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.07ms     4.2 MB/sec    1.00      3.9±0.08ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   491.1±10.17µs     6.0 MB/sec    1.00   487.0±14.25µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.15ms     3.8 MB/sec    1.00      6.7±0.16ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.17ms     5.2 MB/sec    1.00      7.8±0.14ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1720.1±46.19µs     9.7 MB/sec    1.00  1722.9±45.39µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.9±5.20µs    15.1 MB/sec    1.02    198.9±6.10µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.08ms     7.1 MB/sec    1.01      3.6±0.07ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.0±0.11ms     6.8 MB/sec    1.00      6.0±0.11ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.01  1184.1±26.92µs    14.1 MB/sec    1.00  1171.5±31.02µs    14.2 MB/sec
parser/numpy/globals.py                    1.00    121.1±3.80µs    24.4 MB/sec    1.00    121.2±6.56µs    24.4 MB/sec
parser/pydantic/types.py                   1.02      2.6±0.06ms     9.7 MB/sec    1.00      2.6±0.06ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.9±0.27ms     2.6 MB/sec    1.01     16.0±0.32ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.09ms     4.2 MB/sec    1.00      3.9±0.13ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   461.8±13.52µs     6.4 MB/sec    1.01    466.0±9.84µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.15ms     3.8 MB/sec    1.00      6.6±0.18ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.13ms     5.3 MB/sec    1.00      7.7±0.11ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1656.3±37.67µs    10.1 MB/sec    1.00  1644.3±25.62µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.9±4.87µs    16.0 MB/sec    1.03    190.6±5.09µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.05ms     7.4 MB/sec    1.02      3.5±0.11ms     7.2 MB/sec
parser/large/dataset.py                    1.00      5.9±0.07ms     6.9 MB/sec    1.00      5.8±0.07ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1108.3±20.89µs    15.0 MB/sec    1.01  1114.2±29.28µs    14.9 MB/sec
parser/numpy/globals.py                    1.01    113.2±2.56µs    26.1 MB/sec    1.00    112.6±3.30µs    26.2 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.06ms    10.1 MB/sec    1.00      2.5±0.05ms    10.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-05-23 05:21_

Could you extend the summary with an explanation of what is changing and why? I don't understand the reason why some modules are moved, making it hard to review the pr

---

_Comment by @JonathanPlasse on 2023-05-23 07:33_

> Could you extend the summary with an explanation of what is changing and why? I don't understand the reason why some modules are moved, making it hard to review the pr

- The summary is updated, I invite you to use `add_rule.py` with the linter `flake8-async` and with the linter `flake8-pyi` with the branch from the PR #charliermarsh/ruff#3747. You will notice why we need to change the architecture to have a uniform architecture.

---

_Comment by @charliermarsh on 2023-05-23 22:57_

I'll take a look at this tonight.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-05-23 22:57_

---

_Comment by @charliermarsh on 2023-05-24 02:04_

Thank you for this! I'm comfortable merging the rule reorganization, which is great and much appreciated. I think we should discuss the CI scripts separately. The scripts seem useful, and they clearly work! But I'm worried that they will be difficult to maintain since they are so tightly coupled to the specific organization, structure, and syntax used in our code. I might prefer to run them periodically rather than enforce them on CI.

Can we move the scripts out to a separate PR, and merge the rule reorganization first?


---

_Comment by @charliermarsh on 2023-05-24 02:14_

Oh sorry, I see that #3747 contains the scripts themselves. Can we update this PR such that it's independently mergeable?

---

_Comment by @JonathanPlasse on 2023-05-24 12:01_

> Oh sorry, I see that #3747 contains the scripts themselves. Can we update this PR such that it's independently mergeable?

Just rebased this branch on main.

---

_Merged by @charliermarsh on 2023-05-24 15:30_

---

_Closed by @charliermarsh on 2023-05-24 15:30_

---

_Branch deleted on 2023-05-24 15:51_

---

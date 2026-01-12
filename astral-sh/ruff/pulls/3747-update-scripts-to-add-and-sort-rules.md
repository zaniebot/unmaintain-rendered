```yaml
number: 3747
title: Update scripts to add and sort rules
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
base: main
head: sort-rules
created_at: 2023-03-26T20:03:13Z
updated_at: 2023-10-17T11:07:07Z
url: https://github.com/astral-sh/ruff/pull/3747
synced_at: 2026-01-12T15:55:13Z
```

# Update scripts to add and sort rules

---

_@JonathanPlasse_

The aim of this PR is to reintroduce the use of `add_rule.py` and `add_plugin.py` in `CONTRIBUTING.md`.
This PR also fixes the sorting of rules, this means that all new code will be sorted correctly.
The CI job to test these scripts has been improved and should test all cases.

---

_Comment by @github-actions[bot] on 2023-03-26 20:16_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.35ms     3.9 MB/sec    1.00     10.5±0.38ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.09ms     8.0 MB/sec    1.00      2.1±0.12ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00   239.6±10.95µs    12.3 MB/sec    1.02   244.2±12.60µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.18ms     5.6 MB/sec    1.02      4.6±0.21ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.43ms     2.7 MB/sec    1.00     15.2±0.39ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.9±0.15ms     4.3 MB/sec    1.00      3.9±0.15ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   526.3±19.10µs     5.6 MB/sec    1.00   520.7±20.62µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.25ms     3.7 MB/sec    1.00      7.0±0.22ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      7.7±0.34ms     5.3 MB/sec    1.00      7.6±0.21ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1639.9±65.66µs    10.2 MB/sec    1.00  1643.8±56.36µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.9±9.24µs    15.1 MB/sec    1.00    196.6±7.92µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.11ms     7.3 MB/sec    1.00      3.5±0.12ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.4±0.15ms     3.6 MB/sec    1.04     11.9±0.14ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.5 MB/sec    1.03      2.3±0.03ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00    243.0±6.01µs    12.1 MB/sec    1.05   254.4±18.02µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.07ms     5.2 MB/sec    1.04      5.1±0.07ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.16ms     2.5 MB/sec    1.00     15.9±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    500.4±7.94µs     5.9 MB/sec    1.00    497.2±7.57µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.08ms     3.5 MB/sec    1.00      7.2±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.07ms     4.9 MB/sec    1.00      8.2±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1710.6±17.48µs     9.7 MB/sec    1.00  1703.6±16.06µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.2±4.01µs    15.3 MB/sec    1.00    192.4±3.41µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.05ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-05-20 22:27_

CI will now fail if the rules are not sorted.
[CI fail](https://github.com/charliermarsh/ruff/actions/runs/5034355069/jobs/9029131682?pr=3747)
[CI success](https://github.com/charliermarsh/ruff/actions/runs/5034442785/jobs/9029257386?pr=3747)

---

_Marked ready for review by @JonathanPlasse on 2023-05-20 22:27_

---

_Comment by @JonathanPlasse on 2023-05-21 00:42_

Only thing remaining, adding the instructions in `CONTRIBUTING.md` to use these scripts.

---

_Comment by @JonathanPlasse on 2023-05-21 01:12_

Currently there are 24 plugins that uses the old architecture:
- [x] flake8-async
- [x] flake8-builtins
- [x] flake8-blind-except
- [x] flake8-debugger
- [x] mccabe
- [x] flake8-tidy-imports
- [x] flake8-return
- [x] flake8-gettext
- [x] flake8-implicit-str-concat
- [x] flake8-quotes
- [x] flake8-annotations
- [x] flake8-future-annotations
- [x] flake8-2020
- [x] eradicate
- [x] flake8-boolean-trap
- [x] flake8-unused-arguments
- [x] flake8-datetimez
- [x] flake8-errmsg
- [x] flake8-pie
- [x] flake8-commas
- [x] flake8-no-pep420
- [x] flake8-use-pathlib
- [x] flake8-logging-format
- [x] flake8-todos

Ideally, all plugins should be updated to use the new rules architecture as currently the files will be generated but will need manual tweaking with the old architecture.
The CI `test_add_rule` only runs with rules with the new architecture.

---

_Comment by @JonathanPlasse on 2023-05-21 01:47_

Ready for review.

---

_Comment by @JonathanPlasse on 2023-05-21 09:45_

This is ready to merge as is.
Following PRs will be needed to:
- [ ] Sort all rules
- [ ] Enable keeping rules in CI
- [ ] JonathanPlasse/ruff#2

---

_Comment by @JonathanPlasse on 2023-05-22 22:07_

I finished converting all old architectures to the new one.
I would like both PRs to be merged to avoid having to rebase them.

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-24 06:35_

---

_Comment by @JonathanPlasse on 2023-05-24 12:03_

- You can now merge this PR or #4589 in any order.

---

_Converted to draft by @JonathanPlasse on 2023-05-30 14:45_

---

_Marked ready for review by @JonathanPlasse on 2023-05-30 20:54_

---

_Comment by @JonathanPlasse on 2023-06-02 11:48_

- Needs #4807 to be merged to work.

---

_@JonathanPlasse reviewed on 2023-06-02 16:09_

---

_Review comment by @JonathanPlasse on `scripts/_utils.py`:47 on 2023-06-02 16:09_

Would you like some documentation for the regex?

---

_Comment by @charliermarsh on 2023-07-21 20:10_

Hey @JonathanPlasse - sorry to let this one sit for so long. I really appreciate the work here, but after discussion with some other maintainers, we've opted not to merge this. The main concern is that we're signing up for ongoing maintenance of these Python scripts which are getting increasingly complex and will need to change whenever we change implementation details of the rule files. In other words: they're parsing a lot of Rust code via regex and otherwise, and that Python parsing code will likely need modifications over time. As one example: the current version doesn't handle feature flags, and so is broken in the Ruff rules. I'm sure we could fix that bug, but it's just an example of the kind of thing we'll run into over time. Again, thank you for putting it together and sorry for the extensive delay.

---

_Closed by @charliermarsh on 2023-07-21 20:10_

---

_Branch deleted on 2023-10-17 11:07_

---

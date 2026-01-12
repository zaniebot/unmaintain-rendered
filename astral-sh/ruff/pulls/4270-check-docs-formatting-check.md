```yaml
number: 4270
title: Check docs formatting check
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: check-docs-formatting
created_at: 2023-05-07T16:07:00Z
updated_at: 2023-05-10T14:55:22Z
url: https://github.com/astral-sh/ruff/pull/4270
synced_at: 2026-01-12T03:56:39Z
```

# Check docs formatting check

---

_Pull request opened by @calumy on 2023-05-07 16:07_

This PR: 
- Adds a script that runs black over the docs and returns the formatting violations and parse errors found
- Fixes some issues raised by the new formatting check

In some cases where black should not format the examples, then the rule should be added to `scripts/known_rule_formatting_violations.txt`. For the cases where there are known parse errors, then these rules should be added to `scripts/known_rule_parse_errors.txt`.

Reference: formatting section of #4168

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/compound_statements.rs`:82 on 2023-05-07 16:30_

Is it intentional to remove the semicolon from this example?

---

_@MichaReiser reviewed on 2023-05-07 16:30_

---

_Comment by @github-actions[bot] on 2023-05-07 16:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.0±0.10ms     2.7 MB/sec    1.00     14.8±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.8±1.35µs     8.1 MB/sec    1.00    363.4±1.93µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1540.0±5.13µs    10.8 MB/sec    1.00   1536.1±2.43µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    166.1±0.79µs    17.8 MB/sec    1.00    165.2±0.42µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.02ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.02ms     6.9 MB/sec    1.00      5.9±0.03ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1148.9±3.31µs    14.5 MB/sec    1.00   1150.7±1.16µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    117.5±0.30µs    25.1 MB/sec    1.00    117.3±0.39µs    25.2 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.07ms    10.1 MB/sec    1.00      2.5±0.09ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.4±0.63ms  1943.9 KB/sec    1.06     22.7±0.96ms  1837.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.33ms     2.9 MB/sec    1.05      5.9±0.29ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   635.3±45.32µs     4.6 MB/sec    1.16   734.9±84.88µs     4.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.47ms     2.7 MB/sec    1.06      9.9±0.41ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.36ms     3.8 MB/sec    1.12     12.2±0.54ms     3.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.10ms     7.4 MB/sec    1.14      2.6±0.18ms     6.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   269.7±18.57µs    10.9 MB/sec    1.07   288.8±22.55µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.22ms     5.3 MB/sec    1.09      5.3±0.27ms     4.8 MB/sec
parser/large/dataset.py                    1.02      9.6±0.35ms     4.3 MB/sec    1.00      9.4±0.32ms     4.3 MB/sec
parser/numpy/ctypeslib.py                  1.08  1967.0±87.30µs     8.5 MB/sec    1.00  1824.0±79.02µs     9.1 MB/sec
parser/numpy/globals.py                    1.00   170.2±14.92µs    17.3 MB/sec    1.00    169.8±8.32µs    17.4 MB/sec
parser/pydantic/types.py                   1.00      3.8±0.22ms     6.7 MB/sec    1.06      4.0±0.23ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@calumy reviewed on 2023-05-07 16:36_

---

_Review comment by @calumy on `crates/ruff/src/rules/pycodestyle/rules/compound_statements.rs`:82 on 2023-05-07 16:36_

No, reverted in e7a89f24a76bd17491f3267e65e01e8054aacc43

---

_Label `documentation` added by @MichaReiser on 2023-05-08 06:21_

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:165 on 2023-05-08 06:24_

Nit: It would be nice if the script could print the name of the duplicated entry.

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:131 on 2023-05-08 06:25_

Is my understanding correct that this defaults to `false` (we don't generate the docs in the CI)?

---

_@MichaReiser approved on 2023-05-08 06:26_

Nice, this is an excellent improvement! Looking forward to using our own formatter for this :)

---

_@calumy reviewed on 2023-05-08 11:43_

---

_Review comment by @calumy on `scripts/check_docs_formatted.py`:165 on 2023-05-08 11:43_

Updated in 673035f82328d4522253e92c0b3652017aa1cbf0

Duplicates are now listed as:

```bash
> python scripts/check_docs_formatted.py
Known formatting violations has duplicates:
  - useless-semicolon
Please remove them and re-run.
```

---

_@calumy reviewed on 2023-05-08 11:48_

---

_Review comment by @calumy on `scripts/check_docs_formatted.py`:131 on 2023-05-08 11:48_

Yes, that is correct. The previous CI stage generates the docs, so I thought that it would be best to keep them separate in CI as it will make it easier to narrow down the errors to the generation stage or the formatting check stage. 

If you would prefer that the CI is updated to generate and check the docs are formatted in one stage, please let me know and I can update this. 

The main use case that I see for the `--generate-docs` argument is it allows the user adding docs to quickly test that the docs they have written are formatted correctly without having to run both the `scripts/generate_mkdocs.py` and `scripts/check_docs_formatted.py` scripts. 


---

_@MichaReiser reviewed on 2023-05-08 11:49_

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:131 on 2023-05-08 11:49_

I like it!

---

_Comment by @charliermarsh on 2023-05-08 18:57_

Awesome!

---

_Review comment by @charliermarsh on `scripts/check_docs_formatted.py`:25 on 2023-05-08 18:57_

I opted to just inline these, it felt a little simpler than maintaining separate files that would only be used here, but let me know if you disagree.

---

_@charliermarsh reviewed on 2023-05-08 18:57_

---

_Merged by @charliermarsh on 2023-05-08 19:03_

---

_Closed by @charliermarsh on 2023-05-08 19:03_

---

_Branch deleted on 2023-05-10 14:55_

---

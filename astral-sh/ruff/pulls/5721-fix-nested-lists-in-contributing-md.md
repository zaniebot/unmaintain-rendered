```yaml
number: 5721
title: Fix nested lists in CONTRIBUTING.md
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/nested
created_at: 2023-07-13T02:58:30Z
updated_at: 2023-07-13T16:51:14Z
url: https://github.com/astral-sh/ruff/pull/5721
synced_at: 2026-01-12T03:30:21Z
```

# Fix nested lists in CONTRIBUTING.md

---

_Pull request opened by @charliermarsh on 2023-07-13 02:58_

## Summary

We have a lot of two-space-indented stuff, but apparently it needs to be four-space indented to render as expected in MkDocs? I'm really confused because `mdformat` is _still_ trying to reformat this with two- or three-space indentation for unordered and ordered lists respectively.

---

_Label `documentation` added by @charliermarsh on 2023-07-13 02:59_

---

_Comment by @github-actions[bot] on 2023-07-13 03:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.45ms     3.7 MB/sec    1.01     11.1±0.51ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.12ms     6.8 MB/sec    1.00      2.4±0.10ms     6.8 MB/sec
formatter/numpy/globals.py                 1.02   280.4±21.55µs    10.5 MB/sec    1.00   274.5±15.08µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.21ms     4.8 MB/sec    1.01      5.4±0.32ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     18.9±0.60ms     2.2 MB/sec    1.05     19.9±0.51ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.34ms     3.5 MB/sec    1.04      4.9±0.21ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   613.1±31.30µs     4.8 MB/sec    1.02   624.3±29.00µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.38ms     3.0 MB/sec    1.05      8.8±0.29ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.34ms     4.4 MB/sec    1.10     10.2±0.35ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1959.8±86.63µs     8.5 MB/sec    1.08      2.1±0.12ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   236.1±15.34µs    12.5 MB/sec    1.05   249.0±20.55µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.20ms     6.1 MB/sec    1.05      4.4±0.17ms     5.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     13.5±0.34ms     3.0 MB/sec    1.00     13.1±0.38ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.03      3.0±0.13ms     5.6 MB/sec    1.00      2.9±0.10ms     5.8 MB/sec
formatter/numpy/globals.py                 1.01   332.9±37.19µs     8.9 MB/sec    1.00   331.2±20.41µs     8.9 MB/sec
formatter/pydantic/types.py                1.01      6.4±0.19ms     4.0 MB/sec    1.00      6.3±0.24ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.00     21.9±0.40ms  1905.1 KB/sec    1.02     22.4±0.53ms  1862.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.23ms     2.9 MB/sec    1.01      5.7±0.18ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.03   709.2±54.13µs     4.2 MB/sec    1.00   690.9±33.55µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.1±0.46ms     2.5 MB/sec    1.00     10.1±0.25ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     11.1±0.25ms     3.7 MB/sec    1.02     11.3±0.24ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.07ms     7.1 MB/sec    1.00      2.3±0.07ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   284.2±10.53µs    10.4 MB/sec    1.00   283.4±12.06µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      5.0±0.16ms     5.1 MB/sec    1.00      5.0±0.12ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-07-13 06:32_

Four spaces or 1 tab seems about right. 

https://yakworks.github.io/docmark/cheat-sheet/#lists and here https://github.com/Python-Markdown/markdown/issues/451

Maybe we should consider using https://github.com/KyleKing/mdformat-mkdocs

Or use prettier, which respects the tab width https://github.com/prettier/prettier/pull/3694

---

_Comment by @konstin on 2023-07-13 06:56_

According to https://spec.commonmark.org/dingus/, 3 to 6 spaces make a sublist in commonmark, while the [tutorial](https://commonmark.org/help/tutorial/10-nestedLists.html) says "To nest one list within another, indent each item in the sublist by four spaces. You can also nest other elements like paragraphs, blockquotes or code blocks. ". mdformat claims to be commonmark compliant, so it's strange they'd reduce it to 2 spaces.

---

_Comment by @charliermarsh on 2023-07-13 16:19_

Ok, this adds the [mdformat-mkdocs](https://pypi.org/project/mdformat_mkdocs/) plugin which enforces 4-space indentation (which MkDocs requires, I guess).

---

_Merged by @charliermarsh on 2023-07-13 16:32_

---

_Closed by @charliermarsh on 2023-07-13 16:32_

---

_Branch deleted on 2023-07-13 16:33_

---

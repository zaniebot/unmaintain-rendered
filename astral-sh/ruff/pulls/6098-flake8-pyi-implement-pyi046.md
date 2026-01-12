```yaml
number: 6098
title: "[`flake8-pyi`] Implement `PYI046`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI046
created_at: 2023-07-26T15:13:36Z
updated_at: 2023-07-27T21:04:33Z
url: https://github.com/astral-sh/ruff/pull/6098
synced_at: 2026-01-12T15:55:20Z
```

# [`flake8-pyi`] Implement `PYI046`

---

_@LaBatata101_

## Summary
Checks for the presence of unused private `typing.Protocol` definitions.

ref #848 

## Test Plan

Snapshots and manual runs of flake8.


---

_Comment by @github-actions[bot] on 2023-07-26 15:45_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>typeshed (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/852882b8bfe9e1792448b94809351a392e83ce9e/stubs/Pillow/PIL/_imaging.pyi#L12'>stubs/Pillow/PIL/_imaging.pyi:12:7:</a> PYI046 Private protocol `_PixelAccessor` is never used
+ <a href='https://github.com/python/typeshed/blob/852882b8bfe9e1792448b94809351a392e83ce9e/stubs/bleach/bleach/callbacks.pyi#L9'>stubs/bleach/bleach/callbacks.pyi:9:7:</a> PYI046 Private protocol `_Callback` is never used
+ <a href='https://github.com/python/typeshed/blob/852882b8bfe9e1792448b94809351a392e83ce9e/stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi#L26'>stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi:26:7:</a> PYI046 Private protocol `_HasTagAndText` is never used
+ <a href='https://github.com/python/typeshed/blob/852882b8bfe9e1792448b94809351a392e83ce9e/stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi#L28'>stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi:28:7:</a> PYI046 Private protocol `_HasTagAndTextAndAttrib` is never used
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI046 | 4 | 4 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      8.9±0.13ms     4.6 MB/sec    1.00      8.5±0.10ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.04  1772.9±35.47µs     9.4 MB/sec    1.00  1704.3±32.30µs     9.8 MB/sec
formatter/numpy/globals.py                 1.05    198.6±6.10µs    14.9 MB/sec    1.00    189.9±6.31µs    15.5 MB/sec
formatter/pydantic/types.py                1.03      3.8±0.07ms     6.7 MB/sec    1.00      3.7±0.09ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     11.9±0.05ms     3.4 MB/sec    1.00     11.8±0.05ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.0±0.02ms     5.5 MB/sec    1.00      3.0±0.01ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    397.8±2.94µs     7.4 MB/sec    1.00    396.3±0.87µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.11ms     4.7 MB/sec    1.00      5.4±0.14ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.05ms     6.9 MB/sec    1.00      5.8±0.05ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1238.0±13.47µs    13.5 MB/sec    1.00  1227.0±10.41µs    13.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.5±1.73µs    21.9 MB/sec    1.00    133.9±0.32µs    22.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.05ms     9.8 MB/sec    1.00      2.6±0.05ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.09ms     4.0 MB/sec    1.02     10.4±0.11ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1956.3±26.17µs     8.5 MB/sec    1.01  1980.8±30.29µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    214.7±5.61µs    13.7 MB/sec    1.01    217.2±6.02µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     6.0 MB/sec    1.03      4.4±0.05ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.20ms     2.8 MB/sec    1.00     14.6±0.19ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.03ms     4.4 MB/sec    1.01      3.8±0.05ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.1±7.55µs     6.5 MB/sec    1.01    456.6±6.00µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.08ms     3.9 MB/sec    1.02      6.6±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.06ms     5.5 MB/sec    1.01      7.4±0.09ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1501.9±16.59µs    11.1 MB/sec    1.00  1507.0±15.83µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.2±3.07µs    17.5 MB/sec    1.02    172.1±3.02µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.04      3.4±0.04ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-07-26 16:04_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/unused_private_type_declaration.rs`:40 on 2023-07-26 16:04_

Should we note that it is private in this message? i.e. "Private protocol..."

---

_@zanieb reviewed on 2023-07-26 16:05_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/unused_private_type_declaration.rs`:12 on 2023-07-26 16:05_

Should we suggest that it could be made public as an alternative fix?

---

_Comment by @zanieb on 2023-07-26 16:08_

This looks good to me! Thank you :) 

It [looks like your docs need formatting](https://github.com/astral-sh/ruff/actions/runs/5670566518/job/15365743815?pr=6098). I have a couple minor questions too.

---

_Comment by @charliermarsh on 2023-07-26 16:13_

I will try to review the semantic model changes today.

---

_Comment by @LaBatata101 on 2023-07-26 16:20_

> This looks good to me! Thank you :)
> 
> It [looks like your docs need formatting](https://github.com/astral-sh/ruff/actions/runs/5670566518/job/15365743815?pr=6098). I have a couple minor questions too.

I'll fix the formating later today, I thought the `pre-commit` command would catch that type of error.

---

_@LaBatata101 reviewed on 2023-07-26 16:20_

---

_Review comment by @LaBatata101 on `crates/ruff/src/rules/flake8_pyi/rules/unused_private_type_declaration.rs`:40 on 2023-07-26 16:20_

That sounds like a good idea!

---

_@LaBatata101 reviewed on 2023-07-26 16:22_

---

_Review comment by @LaBatata101 on `crates/ruff/src/rules/flake8_pyi/rules/unused_private_type_declaration.rs`:12 on 2023-07-26 16:22_

That sounds good to me

---

_@charliermarsh requested changes on 2023-07-26 16:52_

(I left some feedback on the semantic model changes [here](https://github.com/astral-sh/ruff/pull/6018#pullrequestreview-1548195581), which I think applies to this PR as well. Thank you!)

---

_Review requested from @charliermarsh by @LaBatata101 on 2023-07-27 02:18_

---

_Review requested from @zanieb by @LaBatata101 on 2023-07-27 02:18_

---

_Label `rule` added by @charliermarsh on 2023-07-27 02:28_

---

_Merged by @charliermarsh on 2023-07-27 02:34_

---

_Closed by @charliermarsh on 2023-07-27 02:34_

---

_Branch deleted on 2023-07-27 21:04_

---

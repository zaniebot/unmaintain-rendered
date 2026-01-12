```yaml
number: 5048
title: Add JSON Lines (NDJSON) message serialization
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: jsonl
created_at: 2023-06-13T10:46:07Z
updated_at: 2023-06-13T14:37:03Z
url: https://github.com/astral-sh/ruff/pull/5048
synced_at: 2026-01-12T15:55:17Z
```

# Add JSON Lines (NDJSON) message serialization

---

_@akx_

## Summary

This adds `json-lines` (https://jsonlines.org/ or http://ndjson.org/) as an output format.

I'm sure you already know, but

* JSONL is more greppable (each record is a single line) than the pretty JSON
* JSONL is faster to ingest piecewise (and/or in parallel) than JSON

## Test Plan

Snapshot test in the new module :)

---

_Comment by @github-actions[bot] on 2023-06-13 10:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1392.8±4.72µs    12.0 MB/sec    1.01   1401.8±4.20µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    137.0±0.32µs    21.5 MB/sec    1.01    138.5±0.28µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.02ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.6±0.05ms     2.8 MB/sec    1.00     14.7±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.1±1.73µs     8.0 MB/sec    1.01    370.4±1.14µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.01ms     5.6 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1530.1±3.06µs    10.9 MB/sec    1.01   1541.2±3.23µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.6±0.52µs    17.8 MB/sec    1.00    165.8±0.47µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.05ms     5.3 MB/sec    1.01      7.7±0.06ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1563.7±18.20µs    10.6 MB/sec    1.00  1565.5±15.92µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    151.7±2.84µs    19.4 MB/sec    1.01    153.5±4.36µs    19.2 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.06ms     8.2 MB/sec    1.01      3.2±0.05ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.03     16.6±0.27ms     2.5 MB/sec    1.00     16.2±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    499.0±6.66µs     5.9 MB/sec    1.00    494.0±6.89µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.00      7.1±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.16ms     4.8 MB/sec    1.00      8.2±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1761.9±17.27µs     9.5 MB/sec    1.00  1744.4±14.91µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.7±3.32µs    14.7 MB/sec    1.01    202.5±6.43µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/message/jsonlines.rs`:16 on 2023-06-13 11:12_

nit: don't rename here

---

_@konstin approved on 2023-06-13 11:17_

---

_Review comment by @akx on `crates/ruff/src/message/jsonlines.rs`:16 on 2023-06-13 11:35_

@konstin If I don't, I'll get

```
12 |         writer: &mut dyn Write,
   |         ------ move occurs because `writer` has type `&mut dyn std::io::Write`, which does not implement the `Copy` trait
...
17 |             serde_json::to_writer(writer, &message_to_json_value(message))?;
   |                                   ------ value moved here
18 |             writer.write_all(b"\n")?;
   |             ^^^^^^^^^^^^^^^^^^^^^^^ value borrowed here after move
```


---

_@akx reviewed on 2023-06-13 11:35_

---

_@konstin reviewed on 2023-06-13 12:07_

---

_Review comment by @konstin on `crates/ruff/src/message/jsonlines.rs`:16 on 2023-06-13 12:07_

hm you can pass a `&mut *writer` there, but tbh i'm not sure if that's better than the extra binding you chose

---

_Comment by @akx on 2023-06-13 14:08_

@charliermarsh Missed a snapshot rename, fixed that 👍 

---

_Comment by @charliermarsh on 2023-06-13 14:09_

Thank you 🙏 

---

_Merged by @charliermarsh on 2023-06-13 14:15_

---

_Closed by @charliermarsh on 2023-06-13 14:15_

---

_Branch deleted on 2023-06-13 14:16_

---

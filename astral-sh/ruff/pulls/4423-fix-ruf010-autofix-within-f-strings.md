```yaml
number: 4423
title: "Fix `RUF010` autofix within f-strings"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-ruf010-auto-fix
created_at: 2023-05-13T23:09:34Z
updated_at: 2023-05-15T06:40:20Z
url: https://github.com/astral-sh/ruff/pull/4423
synced_at: 2026-01-12T15:55:15Z
```

# Fix `RUF010` autofix within f-strings

---

_@JonathanPlasse_

- Close #4414

---

_Comment by @github-actions[bot] on 2023-05-13 23:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.0±0.15ms     2.5 MB/sec    1.00     16.0±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.03ms     4.2 MB/sec    1.00      3.9±0.03ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    483.3±7.52µs     6.1 MB/sec    1.00    478.1±4.90µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.7±0.09ms     3.8 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.08ms     5.2 MB/sec    1.01      7.9±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1682.6±12.78µs     9.9 MB/sec    1.00  1690.4±15.98µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.9±2.41µs    15.8 MB/sec    1.00    187.3±3.34µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.00      3.5±0.03ms     7.2 MB/sec
parser/large/dataset.py                    1.00      6.2±0.05ms     6.6 MB/sec    1.00      6.2±0.06ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1206.2±11.30µs    13.8 MB/sec    1.01  1212.2±14.43µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    122.4±1.67µs    24.1 MB/sec    1.02    125.3±0.95µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.02ms     9.6 MB/sec    1.01      2.7±0.02ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.7±0.26ms     2.4 MB/sec    1.00     16.5±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.01      4.3±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.8±7.79µs     6.8 MB/sec    1.00    434.1±6.36µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.05ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1718.7±12.28µs     9.7 MB/sec    1.01  1738.3±13.32µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.1±1.28µs    15.9 MB/sec    1.03    190.4±7.00µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.8 MB/sec    1.00      3.8±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.8±0.03ms     6.0 MB/sec    1.00      6.7±0.25ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.02  1299.3±15.47µs    12.8 MB/sec    1.00  1273.7±11.49µs    13.1 MB/sec
parser/numpy/globals.py                    1.01    133.9±0.90µs    22.0 MB/sec    1.00    132.3±1.02µs    22.3 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.01ms     8.9 MB/sec    1.00      2.8±0.04ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:93 on 2023-05-14 01:53_

It looks like we now generate one diagnostic for the whole f-string (and that the diagnostic range is that of the entire f-string).

I think it'd be preferable to retain one diagnostic per formatted value, and limit the diagnostic to the formatted value. It's fine if the autofix contents include the whole string, even for each formatted value, but they should be separate diagnostics. (E.g., we could iterate over `formatted_values` and create one diagnostic per index, with a fix specific to that index.)

---

_@charliermarsh reviewed on 2023-05-14 01:53_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:91 on 2023-05-14 01:54_

(An alternative fix would be to just use `"foo"` directly given `str("foo")`, and then add the `!r` manually, instead of using `unparse`.) What you have here looks more robust, but the direct fix would also be safe (and is faster since we don't have to reparse the f-string). I leave it up to you!

---

_@charliermarsh reviewed on 2023-05-14 01:54_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/ruff/rules/explicit_f_string_type_conversion.rs`:91 on 2023-05-14 08:00_

I just did that.

---

_@JonathanPlasse reviewed on 2023-05-14 08:00_

---

_Renamed from "Fix RUF010 auto-fix" to "Fix `RUF010` autofix within f-strings" by @charliermarsh on 2023-05-15 01:57_

---

_Comment by @charliermarsh on 2023-05-15 01:57_

Awesome, thank you!

---

_Merged by @charliermarsh on 2023-05-15 02:08_

---

_Closed by @charliermarsh on 2023-05-15 02:08_

---

_Branch deleted on 2023-05-15 06:40_

---

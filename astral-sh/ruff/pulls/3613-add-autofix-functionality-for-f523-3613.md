```yaml
number: 3613
title: "Add autofix functionality for `F523` (#3613)"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - fixes
assignees: []
merged: true
base: main
head: f523-auto-fix
created_at: 2023-03-19T20:06:36Z
updated_at: 2023-03-21T07:18:11Z
url: https://github.com/astral-sh/ruff/pull/3613
synced_at: 2026-01-12T15:55:13Z
```

# Add autofix functionality for `F523` (#3613)

---

_@JonathanPlasse_

- Close #1186, the oldest auto-fix issue.
It was more complex than I thought.
You not only need to remove unused positional arguments but also update the field type indexes if the minimum index is not `0`.

---

_Comment by @github-actions[bot] on 2023-03-19 20:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.1±0.09ms     2.4 MB/sec    1.00     16.6±0.09ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.4±0.04ms     3.8 MB/sec    1.00      4.3±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    603.8±1.64µs     4.9 MB/sec    1.00    592.8±1.13µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.6±0.06ms     3.4 MB/sec    1.00      7.4±0.05ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.09      9.5±0.04ms     4.3 MB/sec    1.00      8.7±0.05ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.1±0.01ms     8.0 MB/sec    1.00   1958.6±2.75µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.03    226.4±0.55µs    13.0 MB/sec    1.00    220.2±0.52µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.3±0.02ms     5.9 MB/sec    1.00      4.1±0.03ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.04ms     2.8 MB/sec    1.00     14.8±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.01ms     4.1 MB/sec    1.00      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    447.5±4.18µs     6.6 MB/sec    1.00    449.1±4.01µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.02ms     3.9 MB/sec    1.01      6.6±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.01      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1735.4±4.81µs     9.6 MB/sec    1.01   1744.6±9.26µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.8±0.87µs    16.4 MB/sec    1.00    180.6±2.89µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.8 MB/sec    1.01      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-20 08:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/fixes.rs`:39 on 2023-03-20 08:27_

We should be able to avoid cloning by using `std::mem::take` and `into_iter`

```suggestion
	let current_elements = std::mem::take(&mut dict.elements);
    dict.elements = current_elements
        .into_iter()
        .filter_map(|e| match e {
            DictElement::Simple {
                key: Expression::SimpleString(name),
                ..
            } if unused_arguments.contains(&raw_contents(name.value)) => None,
            e => Some(e),
        })
        .collect();
```

Or you may be able to use `dict.elements.retain(|e| ...)` instead

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/fixes.rs`:73 on 2023-03-20 08:30_

Same as aboth: Can we use `Vec::retain` to "filter out" the unused arguments or `std::mem::take` with `into_iter` to avoid the `e.clone()`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/fixes.rs`:164 on 2023-03-20 08:33_

Nit: Can we use `retain` or `std::mem::take` with `into_iter`?

---

_@MichaReiser approved on 2023-03-20 08:33_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyflakes/fixes.rs`:39 on 2023-03-20 09:21_

Done. 

---

_@JonathanPlasse reviewed on 2023-03-20 09:22_

---

_@JonathanPlasse reviewed on 2023-03-20 09:24_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyflakes/fixes.rs`:164 on 2023-03-20 09:24_

Done.

---

_@JonathanPlasse reviewed on 2023-03-20 09:24_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyflakes/fixes.rs`:73 on 2023-03-20 09:24_

Done.

---

_Label `autofix` added by @charliermarsh on 2023-03-21 03:49_

---

_Renamed from "Add F523 auto-fix" to "Add autofix functionality for `F523` (#3613)" by @charliermarsh on 2023-03-21 03:49_

---

_Merged by @charliermarsh on 2023-03-21 03:55_

---

_Closed by @charliermarsh on 2023-03-21 03:55_

---

_Comment by @charliermarsh on 2023-03-21 03:55_

Thanks for this! 🎸 🎸 🎸 

---

_Branch deleted on 2023-03-21 07:18_

---

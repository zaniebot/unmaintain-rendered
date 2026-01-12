```yaml
number: 3552
title: "[`pylint`] invalid-characters-*"
type: pull_request
state: merged
author: r3m0t
labels:
  - rule
assignees: []
merged: true
base: main
head: chars-b
created_at: 2023-03-15T23:57:01Z
updated_at: 2023-03-17T19:54:11Z
url: https://github.com/astral-sh/ruff/pull/3552
synced_at: 2026-01-12T04:39:45Z
```

# [`pylint`] invalid-characters-*

---

_Pull request opened by @r3m0t on 2023-03-15 23:57_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-03-16 00:11_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/rule_table.rs`:32 on 2023-03-16 08:23_

Neat! 

We can improve the ergonomics (reduce the amount of `&` a caller must write) by: 

1. Change `Rule` to implement `Copy` (Rule is only 2 bytes large, which is cheaper than passing a reference that is 8 bytes) by changing the derive macro [here](https://github.com/charliermarsh/ruff/blob/aa51ecedc5ebbba5173350d45f0ea4ae9ae41df6/crates/ruff_macros/src/register_rules.rs#L32-L45) Edit: see #3556
2. Accept any iterable 
```suggestion
    pub fn any_enabled(&self, codes: impl IntoIterator<Item=Rule>) -> bool {
        codes.into_iter().any(|c| self.enabled.contains_key(c))
    }
```


You can then use it like this

```rust
let enforce_ambiguous_unicode_character = settings.rules.any_enabled([
        Rule::AmbiguousUnicodeCharacterString,
        Rule::AmbiguousUnicodeCharacterDocstring,
        Rule::AmbiguousUnicodeCharacterComment,
    ]);
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/tokens.rs`:51 on 2023-03-16 08:24_

Nit: Use `any_enabled`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:88 on 2023-03-16 08:26_

We prefer not to have an empty line between the doc comment and a type's attributes.

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:119 on 2023-03-16 08:26_

We prefer not to have an empty line between the doc comment and a type's attributes.

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:57 on 2023-03-16 08:26_

We prefer not to have an empty line between the doc comment and a type's attributes.

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:150 on 2023-03-16 08:27_

We prefer not to have an empty line between the doc comment and a type's attributes.

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:175 on 2023-03-16 08:29_

Use the universal lines iterator that also 
```suggestion
    for (row_offset, line) in UniversalNewlineIterator::from(text).enumerate() {
```

---

_@MichaReiser approved on 2023-03-16 08:31_

---

_Review requested from @MichaReiser by @r3m0t on 2023-03-16 21:57_

---

_Comment by @github-actions[bot] on 2023-03-16 22:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.07ms     3.0 MB/sec    1.01     13.8±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.7±1.06µs     5.9 MB/sec    1.00    501.6±1.28µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.03ms     5.4 MB/sec    1.01      7.6±0.02ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1682.3±2.35µs     9.9 MB/sec    1.01   1704.6±1.41µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.2±0.47µs    15.3 MB/sec    1.01    195.3±1.55µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.01ms     7.3 MB/sec    1.02      3.6±0.01ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.0±0.14ms     2.5 MB/sec    1.00     15.9±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     3.9 MB/sec    1.00      4.2±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    543.2±8.31µs     5.4 MB/sec    1.00    538.5±5.52µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.08ms     4.5 MB/sec    1.01      9.1±0.07ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1957.9±23.64µs     8.5 MB/sec    1.00  1953.3±15.28µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.3±3.94µs    13.8 MB/sec    1.02    217.3±9.23µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.04ms     6.2 MB/sec    1.01      4.2±0.17ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-17 03:50_

Sorry for the delay, will get to this tomorrow.

---

_Label `rule` added by @charliermarsh on 2023-03-17 03:51_

---

_Renamed from "[pylint] invalid-characters-*" to "[`pylint`] invalid-characters-*" by @charliermarsh on 2023-03-17 19:09_

---

_Merged by @charliermarsh on 2023-03-17 19:30_

---

_Closed by @charliermarsh on 2023-03-17 19:30_

---

```yaml
number: 3898
title: Fix unicode handling in PLE2515
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_unicode_in_PLE2515
created_at: 2023-04-06T10:59:06Z
updated_at: 2023-04-06T17:54:57Z
url: https://github.com/astral-sh/ruff/pull/3898
synced_at: 2026-01-12T04:28:19Z
```

# Fix unicode handling in PLE2515

---

_Pull request opened by @konstin on 2023-04-06 10:59_

Previously, we used byte indices when we should have used char indices, causing crashes when the were non-ascii characters before our replaces

Fixes #3716

---

_Review requested from @MichaReiser by @konstin on 2023-04-06 10:59_

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:198 on 2023-04-06 10:59_

@MichaReiser I've found this by guessing, i'm not sure if that's the correct way of computing offsets

---

_@konstin reviewed on 2023-04-06 10:59_

---

_@konstin reviewed on 2023-04-06 11:07_

---

_Review comment by @konstin on `crates/ruff/resources/test/fixtures/pylint/invalid_characters.py`:1 on 2023-04-06 11:07_

i also hope that this is the right terminology (github doesn't like the weird bytes inside)

---

_Comment by @github-actions[bot] on 2023-04-06 11:10_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -11, 0 error(s))

<details><summary>airflow (+0, -9)</summary>
<p>

```diff
- dev/breeze/src/airflow_breeze/params/build_prod_params.py:47:27: RUF201 Do not perform function call `get_airflow_extras` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:42:27: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:43:39: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:54:27: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:56:25: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:70:27: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:71:39: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:87:27: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:89:25: RUF201 Do not perform function call `os.environ.get` in dataclass defaults
```

</p>
</details>
<details><summary>scikit-build-core (+0, -2)</summary>
<p>

```diff
- src/scikit_build_core/file_api/model/toolchains.py:41:27: RUF201 Do not perform function call `APIVersion` in dataclass defaults
- tests/test_settings.py:23:17: RUF201 Do not perform function call `Path` in dataclass defaults
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.2±0.08ms     2.7 MB/sec    1.00     15.0±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    412.0±2.41µs     7.2 MB/sec    1.00    407.4±1.97µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.03ms     3.9 MB/sec    1.00      6.5±0.03ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.02      7.9±0.03ms     5.2 MB/sec    1.00      7.7±0.01ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1748.4±6.68µs     9.5 MB/sec    1.00   1713.4±1.76µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.6±0.58µs    16.2 MB/sec    1.00    181.5±0.55µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.26ms     2.8 MB/sec    1.00     14.5±0.18ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.09ms     4.5 MB/sec    1.00      3.7±0.06ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01    441.2±8.40µs     6.7 MB/sec    1.00    437.7±7.99µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.15ms     4.1 MB/sec    1.00      6.1±0.12ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.08ms     5.6 MB/sec    1.00      7.3±0.11ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1589.0±27.12µs    10.5 MB/sec    1.01  1607.6±30.01µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.8±7.51µs    17.1 MB/sec    1.01    174.0±5.52µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.08ms     7.7 MB/sec    1.01      3.3±0.06ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:198 on 2023-04-06 11:18_

It's the correct way for computing char offsets, which is slow. That's why I want to change `Location` to use byte offsets directly so that it simply becomes `line.len()` which is `O(1)`

The current solution has a worst-case complexity of `O(n^2)`  because `line[..column_bytes].chars().count()` iterates from the beginning of the text for every match. I would recommend not using `match_indices` and iterate over `chars` directly. 

```rust
let mut char_offset = 0;
for (byte_offset, char) in line.char_indices() {
	let replacement = match char {
		...patterns
		_ => {
			char_offset += 1;
			continue;
		}
	};

	// .. rest

	char_offset += 1;
}

```

---

_@MichaReiser approved on 2023-04-06 11:19_

---

_@konstin reviewed on 2023-04-06 12:39_

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/invalid_string_characters.rs`:198 on 2023-04-06 12:39_

updated, char() works

---

_Merged by @charliermarsh on 2023-04-06 17:54_

---

_Closed by @charliermarsh on 2023-04-06 17:54_

---

_Branch deleted on 2023-04-06 17:54_

---

_Label `bug` added by @charliermarsh on 2023-04-06 17:54_

---

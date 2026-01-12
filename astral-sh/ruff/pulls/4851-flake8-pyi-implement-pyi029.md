```yaml
number: 4851
title: "[`flake8-pyi`] Implement PYI029"
type: pull_request
state: merged
author: density
labels: []
assignees: []
merged: true
base: main
head: PYI029
created_at: 2023-06-04T15:08:33Z
updated_at: 2023-06-05T19:41:55Z
url: https://github.com/astral-sh/ruff/pull/4851
synced_at: 2026-01-12T15:55:16Z
```

# [`flake8-pyi`] Implement PYI029

---

_@density_

## Summary

Implement Y029  from https://github.com/PyCQA/flake8-pyi: `It is almost always redundant to define __str__ or __repr__ in a stub file, as the signatures are almost always identical to object.__str__ and object.__repr__.`

ref: #848 

## Test Plan

Added snapshots


---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI029_PYI029.pyi.snap`:12 on 2023-06-04 15:09_

Not sure why the underscores are lost in `__str__`

---

_@density reviewed on 2023-06-04 15:09_

---

_Comment by @github-actions[bot] on 2023-06-04 16:03_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>typeshed (+4, -0)</summary>
<p>

```diff
+ stdlib/builtins.pyi:108:9: PYI029 [*] Defining `__str__` in a stub is almost always redundant
+ stdlib/builtins.pyi:109:9: PYI029 [*] Defining `__repr__` in a stub is almost always redundant
+ stdlib/distutils/version.pyi:26:9: PYI029 [*] Defining `__str__` in a stub is almost always redundant
+ stdlib/distutils/version.pyi:35:9: PYI029 [*] Defining `__str__` in a stub is almost always redundant
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI029 | 4 | 4 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec  
formatter/numpy/ctypeslib.py               1.00   1378.8±2.10µs    12.1 MB/sec  
formatter/numpy/globals.py                 1.00    157.2±0.22µs    18.8 MB/sec  
formatter/pydantic/types.py                1.00      3.0±0.01ms     8.6 MB/sec  
linter/all-rules/large/dataset.py          1.00     16.8±0.11ms     2.4 MB/sec    1.02     17.1±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.01      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.3±0.86µs     6.0 MB/sec    1.02   499.7±14.63µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.04ms     3.6 MB/sec    1.01      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.03      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1718.7±12.60µs     9.7 MB/sec    1.02   1758.2±4.61µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.9±0.62µs    15.6 MB/sec    1.03    195.5±0.35µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.03      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                                                           1.00      6.3±0.01ms     6.5 MB/sec
parser/numpy/ctypeslib.py                                                         1.00   1227.5±1.33µs    13.6 MB/sec
parser/numpy/globals.py                                                           1.00    128.4±0.40µs    23.0 MB/sec
parser/pydantic/types.py                                                          1.00      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.7±0.12ms     7.2 MB/sec  
formatter/numpy/ctypeslib.py               1.00  1077.9±10.37µs    15.4 MB/sec  
formatter/numpy/globals.py                 1.00    117.8±1.29µs    25.0 MB/sec  
formatter/pydantic/types.py                1.00      2.4±0.02ms    10.6 MB/sec  
linter/all-rules/large/dataset.py          1.02     12.2±0.09ms     3.3 MB/sec    1.00     12.0±0.05ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.1±0.02ms     5.3 MB/sec    1.00      3.1±0.01ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    316.8±4.43µs     9.3 MB/sec    1.00    314.9±7.87µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.2±0.04ms     4.9 MB/sec    1.00      5.2±0.06ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.01      6.1±0.04ms     6.6 MB/sec    1.00      6.1±0.03ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1277.7±9.73µs    13.0 MB/sec    1.00   1268.1±7.30µs    13.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.4±1.25µs    22.0 MB/sec    1.01    135.9±3.51µs    21.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.8±0.02ms     9.1 MB/sec    1.00      2.7±0.02ms     9.3 MB/sec
parser/large/dataset.py                                                           1.00      4.8±0.02ms     8.4 MB/sec
parser/numpy/ctypeslib.py                                                         1.00    913.0±5.84µs    18.2 MB/sec
parser/numpy/globals.py                                                           1.00     95.2±2.16µs    31.0 MB/sec
parser/pydantic/types.py                                                          1.00      2.0±0.02ms    12.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-05 02:33_

Is this good to go or intentionally a draft?

---

_Marked ready for review by @density on 2023-06-05 02:45_

---

_Comment by @density on 2023-06-05 02:45_

@charliermarsh should be good now. just had to fix docs so tests would pass.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/str_or_repr_defined_in_stub.rs`:47 on 2023-06-05 06:06_

Should this rule also apply for `async` function definitions? 

If so, then the following could be useful (I thought we already had this, but I wasn't able to find it with a quick search). 

In `rustpython-ast`  define a new `AnyFunctionDef` node that is a union over sync and async function definitions

```rust
#[derive(Copy, Clone, Debug)]
pub enum AnyFunctionDefinition<'a> {
	FunctionDef(&'a StmtFunctionDef),
	AsyncFuncitonDef(&'a StmtAsyncFunctionDef)
}

impl AnyFunctionDefinition<'_> {
	pub fn body(&self) -> Suite {
		match self {
			Self::FunctionDef(StmtFunctionDef { body, .. }) | Self::AsyncFuncitonDef(StmtAsyncFunctionDef { body, .. }) => body
	}

	// other accessor methods

	pub fn into_async(self) -> Option<&StmtAsyncFunctionDef> {
		if let Self::AsyncFunctionDef(async) { Some(async) } else { None }
	}
}

impl AstNode for AnyFunctionDef<'_> {
	... 
}

impl<'a> From<&'a StmtFunctionDef> for AnyFunctionDefinition<'a> {
	fn from(value: &'a StmtFunctionDef) -> Self {
		Self::FunctionDef(value) 
	}
}

// Same for `Async`
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/str_or_repr_defined_in_stub.rs`:74 on 2023-06-05 06:06_

Nit: Moving this check to line 54 could speed up performance because it is cheaper than any semantic model check

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/str_or_repr_defined_in_stub.rs`:106 on 2023-06-05 06:07_

@charliermarsh is this still necessary after your isolation changes?

---

_@MichaReiser reviewed on 2023-06-05 06:07_

---

_@density reviewed on 2023-06-05 12:39_

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/str_or_repr_defined_in_stub.rs`:74 on 2023-06-05 12:39_

Thanks! Done

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/str_or_repr_defined_in_stub.rs`:47 on 2023-06-05 16:07_

(I can take a look at this separate from this PR.)

---

_@charliermarsh reviewed on 2023-06-05 16:07_

---

_@charliermarsh reviewed on 2023-06-05 16:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/str_or_repr_defined_in_stub.rs`:106 on 2023-06-05 16:07_

Yes although I'm tempted to just let it add `pass` and leave this to other PYI rules.

---

_Merged by @charliermarsh on 2023-06-05 19:21_

---

_Closed by @charliermarsh on 2023-06-05 19:21_

---

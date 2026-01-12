```yaml
number: 4409
title: Ignore ANN401 for overridden methods
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: ignore-ann401-for-overriden-methods
created_at: 2023-05-13T10:11:42Z
updated_at: 2023-05-13T15:42:38Z
url: https://github.com/astral-sh/ruff/pull/4409
synced_at: 2026-01-12T03:50:02Z
```

# Ignore ANN401 for overridden methods

---

_Pull request opened by @JonathanPlasse on 2023-05-13 10:11_

- Close #4408

---

_Comment by @github-actions[bot] on 2023-05-13 10:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.09     18.8±1.00ms     2.2 MB/sec     1.00     17.4±1.04ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      4.4±0.28ms     3.8 MB/sec     1.00      4.1±0.18ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.04   550.9±48.79µs     5.4 MB/sec     1.00   530.1±29.03µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.6±0.34ms     3.4 MB/sec     1.00      7.3±0.31ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.9±0.37ms     4.6 MB/sec     1.00      8.8±0.58ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1747.6±141.53µs     9.5 MB/sec    1.05  1833.2±157.58µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.03   222.3±11.67µs    13.3 MB/sec     1.00   216.6±14.24µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.17ms     6.7 MB/sec     1.06      4.1±0.45ms     6.3 MB/sec
parser/large/dataset.py                    1.02      7.2±0.35ms     5.7 MB/sec     1.00      7.0±0.44ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.01  1358.5±58.71µs    12.3 MB/sec     1.00  1346.6±96.27µs    12.4 MB/sec
parser/numpy/globals.py                    1.18   158.4±14.02µs    18.6 MB/sec     1.00    134.3±6.94µs    22.0 MB/sec
parser/pydantic/types.py                   1.17      3.4±0.26ms     7.5 MB/sec     1.00      2.9±0.12ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.4±0.04ms     2.5 MB/sec    1.00     16.2±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.01ms     3.9 MB/sec    1.00      4.2±0.01ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.3±3.82µs     6.9 MB/sec    1.00    428.8±4.24µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.05ms     5.0 MB/sec    1.00      8.1±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1703.8±6.11µs     9.8 MB/sec    1.00   1701.5±5.85µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.3±1.00µs    15.9 MB/sec    1.00    185.5±3.03µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.6±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1282.9±9.71µs    13.0 MB/sec    1.00   1264.2±4.60µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    133.2±0.92µs    22.2 MB/sec    1.00    131.5±1.11µs    22.4 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_annotations/helpers.rs`:11 on 2023-05-13 12:56_

#4359 introduced named types for each `StmtKind` variant. That means, we can now define custom unions:

```rust
enum MatchFunctionDef<'a> {
	FunctionDef(&'a ast::StmtFunctionDef),
	AsyncFunctionDef(&'a ast::StmtAsyncFunctionDef)
}

impl<'a> MatchFunctionDef<'a> {
	fn cast(stmt: &'a Stmt) -> Option<Self> {
		match &stmt.node {
			StmtKind::FunctionDef(def) => Some(Self::FunctionDef(def)),
			StmtKind::AsyncFunctionDef(def) => Some(Self::AsyncFunctionDef(def)),
			_ => None
		}
	},

	fn name(&self) -> &str {
		match self {
			Self::FunctionDef(def) => def.name(),
			Self::AsyncFunctionDef(def) => def.name(),
		}
	}

	...
}
```

I agree, it's definitely more code but it allows you to retrieve the underlying node when necessary and is more meaningful than a tuple with 5 fields.

---

_@MichaReiser approved on 2023-05-13 12:56_

---

_Renamed from "Ignore ANN401 for overriden methods" to "Ignore ANN401 for overridden methods" by @charliermarsh on 2023-05-13 15:14_

---

_Merged by @charliermarsh on 2023-05-13 15:20_

---

_Closed by @charliermarsh on 2023-05-13 15:20_

---

_Branch deleted on 2023-05-13 15:23_

---

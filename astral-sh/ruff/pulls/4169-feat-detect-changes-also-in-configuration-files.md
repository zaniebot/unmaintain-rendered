```yaml
number: 4169
title: "Feat: detect changes also in configuration files"
type: pull_request
state: merged
author: mikeleppane
labels: []
assignees: []
merged: true
base: main
head: feat/detect-changes-in-project-files
created_at: 2023-05-01T11:54:16Z
updated_at: 2023-05-09T17:01:03Z
url: https://github.com/astral-sh/ruff/pull/4169
synced_at: 2026-01-12T15:55:14Z
```

# Feat: detect changes also in configuration files

---

_@mikeleppane_

Detect changes also in configuration files. Detected config files are: pyproject.toml, ruff.toml,.ruff.toml

---

_Comment by @github-actions[bot] on 2023-05-01 12:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     19.4±0.47ms     2.1 MB/sec    1.00     18.8±0.45ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.6±0.23ms     3.6 MB/sec    1.00      4.4±0.12ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   552.8±19.44µs     5.3 MB/sec    1.00   543.8±13.65µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      8.0±0.36ms     3.2 MB/sec    1.00      7.7±0.25ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.04      9.3±0.28ms     4.4 MB/sec    1.00      9.0±0.42ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1992.2±70.52µs     8.4 MB/sec    1.00  1895.0±45.30µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    224.4±9.69µs    13.1 MB/sec    1.02   228.8±17.89µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.1±0.15ms     6.2 MB/sec    1.00      4.0±0.08ms     6.4 MB/sec
parser/large/dataset.py                    1.03      7.5±0.21ms     5.4 MB/sec    1.00      7.3±0.11ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.04  1488.1±89.71µs    11.2 MB/sec    1.00  1427.0±73.31µs    11.7 MB/sec
parser/numpy/globals.py                    1.00    141.7±7.21µs    20.8 MB/sec    1.01   142.6±10.78µs    20.7 MB/sec
parser/pydantic/types.py                   1.02      3.2±0.10ms     8.0 MB/sec    1.00      3.1±0.09ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.11ms     2.5 MB/sec    1.00     16.0±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   485.0±10.56µs     6.1 MB/sec    1.00    484.6±6.42µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.05ms     3.7 MB/sec    1.00      6.8±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.05ms     5.0 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.5±14.93µs     9.6 MB/sec    1.00  1725.1±15.37µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.6±2.89µs    15.4 MB/sec    1.02    196.3±8.04µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.01      3.7±0.09ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.06ms     6.1 MB/sec    1.01      6.7±0.05ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1265.3±15.28µs    13.2 MB/sec    1.01  1275.2±15.47µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    128.9±2.00µs    22.9 MB/sec    1.00    129.5±1.82µs    22.8 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.01      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:231 on 2023-05-01 12:06_

I think we have to recompute most of the variables defined above starting from line 115 whenever a `pyproject.toml` changes. For example, you can configure the output format in the settings and we should respect that change if we support reloading configuration files.

---

_@MichaReiser reviewed on 2023-05-01 12:06_

---

_@MichaReiser reviewed on 2023-05-01 12:08_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:56 on 2023-05-01 12:08_

Nit: I recommend matching on the extension or file name explicitly. E.g. we don't want to match a file named `py`

```suggestion
	if matches!(path.file_name(), Some("pyproject.toml" | "ruff.toml" |".ruff.toml")) {
		true
	} else if matches!(path.extension(), Some("py" | "pyi")) { 
		true
	} else { false }
```

---

_Review comment by @mikeleppane on `crates/ruff_cli/src/lib.rs`:56 on 2023-05-02 05:33_

Good catch! 

---

_@mikeleppane reviewed on 2023-05-02 05:34_

---

_@mikeleppane reviewed on 2023-05-02 05:46_

---

_Review comment by @mikeleppane on `crates/ruff_cli/src/lib.rs`:231 on 2023-05-02 05:46_

Absolutely! Is there now something more to be recalculated other than pyproject_strategy?

---

_@MichaReiser reviewed on 2023-05-02 07:14_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:231 on 2023-05-02 07:14_

I'm not too familiar with that code. A safe assumption would be to re-compute everything that is derived  from the `pyproject_strategy` (all settings that are supported in the configuration and aren't CLI options only). I wonder if we even have to be clever about it or if we can simply call `check` again. The main challenge that I see with that approach is that we might mess up the printer output because watch uses `write_continuously`

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:51 on 2023-05-03 09:59_

Making this configurable seems overkill for now. I would prefer matching statically on the file name and extension. That should also allow the compiler to optimize the code better (see my original comment on how to implement it with a match).

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:53 on 2023-05-03 10:02_

We can keep this a standalone function and instead introduce a new tri-boolean type to signal whether to ignore the change, it's a python file, or a configuration change

```rust
#[derive(Copy, Clone, Debug, Eq, PartialEq)]
enum ChangeKind {
	Configuration,
	SourceFile,
	Ignore
}

fn detect_change(paths: &[PathBuf]) -> ChangeKind {
	...
}
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:260 on 2023-05-03 10:04_

Only re-computing the `pyproject_strategy`, is, unfortunately, not sufficient. There are multiple derived variables on line 107-185 that need to be recomputed too

---

_@MichaReiser requested changes on 2023-05-03 10:04_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:260 on 2023-05-09 16:17_

I added a TODO for this.

---

_@charliermarsh reviewed on 2023-05-09 16:17_

---

_Merged by @charliermarsh on 2023-05-09 16:22_

---

_Closed by @charliermarsh on 2023-05-09 16:22_

---

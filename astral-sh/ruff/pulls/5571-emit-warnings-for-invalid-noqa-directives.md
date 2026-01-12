```yaml
number: 5571
title: "Emit warnings for invalid `# noqa` directives"
type: pull_request
state: merged
author: charliermarsh
labels:
  - suppression
assignees: []
merged: true
base: main
head: charlie/noqa-warnings
created_at: 2023-07-06T20:11:08Z
updated_at: 2023-07-13T00:19:49Z
url: https://github.com/astral-sh/ruff/pull/5571
synced_at: 2026-01-12T15:55:18Z
```

# Emit warnings for invalid `# noqa` directives

---

_@charliermarsh_

## Summary

This PR adds a `ParseError` type to the `noqa` parsing system to enable us to render useful warnings instead of silently failing when parsing `noqa` codes.

For example, given `foo.py`:

```python
# ruff: noqa: x

# ruff: noqa foo

# flake8: noqa: F401
import os  # noqa: foo-bar
```

We would now output:

```console
warning: Invalid `# noqa` directive on line 2: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 4: expected `:` followed by a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 6: Flake8's blanket exemption does not support exempting specific codes. To exempt specific codes, use, e.g., `# ruff: noqa: F401, F841` instead.
warning: Invalid `# noqa` directive on line 7: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

There's one important behavior change here too. Right now, with Flake8, if you do `# flake8: noqa: F401`, Flake8 treats that as equivalent to `# flake8: noqa` -- it turns off _all_ diagnostics in the file, not just `F401`. Historically, we respected this... but, I think it's confusing. So we now raise a warning, and don't respect it at all. This will lead to errors in some projects, but I'd argue that right now, those directives are almost certainly behaving in an unintended way for users anyway.

Closes https://github.com/astral-sh/ruff/issues/3339.


---

_Label `noqa` added by @charliermarsh on 2023-07-06 20:13_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-06 20:13_

---

_@charliermarsh reviewed on 2023-07-06 20:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:43 on 2023-07-06 20:14_

I find `Result<Option>` somewhat non-ergonomic, but...

---

_@charliermarsh reviewed on 2023-07-06 20:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:288 on 2023-07-06 20:20_

@MichaReiser - Is there any more succinct or idiomatic way to express this? I.e., propagate the `None`?

---

_Comment by @github-actions[bot] on 2023-07-06 20:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+21, -0, 0 error(s))

<details><summary>airflow (+21, -0)</summary>
<p>

```diff
+ airflow/__init__.py:129:1: I001 [*] Import block is un-sorted or un-formatted
+ airflow/__init__.py:131:36: F401 [*] `airflow.exceptions.AirflowException` imported but unused
+ airflow/__init__.py:132:40: F401 [*] `airflow.models.dataset.Dataset` imported but unused
+ airflow/__init__.py:18:1: D212 [*] Multi-line docstring summary should start at the first line
+ airflow/__init__.py:52:1: I001 [*] Import block is un-sorted or un-formatted
+ airflow/__init__.py:52:21: F401 [*] `airflow.configuration` imported but unused
+ airflow/__init__.py:60:67: PGH003 Use specific rule codes when ignoring type issues
+ airflow/__init__.py:90:5: ANN202 Missing return type annotation for private function `__getattr__`
+ airflow/__init__.py:94:15: TRY003 Avoid specifying long messages outside the exception class
+ airflow/__init__.py:94:30: EM102 [*] Exception must not use an f-string literal, assign to variable first
+ airflow/__init__.py:99:5: SIM108 [*] Use ternary operator `val = getattr(mod, attr_name) if attr_name else mod` instead of `if`-`else`-block
+ scripts/ci/pre_commit/pre_commit_insert_extras.py:1:1: INP001 File `scripts/ci/pre_commit/pre_commit_insert_extras.py` is part of an implicit namespace package. Add an `__init__.py`.
+ scripts/ci/pre_commit/pre_commit_insert_extras.py:33:1: E402 Module level import not at top of file
+ scripts/ci/pre_commit/pre_commit_insert_extras.py:34:1: E402 Module level import not at top of file
+ scripts/ci/pre_commit/pre_commit_insert_extras.py:62:41: SIM118 [*] Use `extra in EXTRAS_DEPENDENCIES` instead of `extra in EXTRAS_DEPENDENCIES.keys()`
+ scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:1:1: INP001 File `scripts/ci/pre_commit/pre_commit_local_yml_mounts.py` is part of an implicit namespace package. Add an `__init__.py`.
+ scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:25:1: E402 Module level import not at top of file
+ scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:29:65: COM812 [*] Trailing comma missing
+ scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:32:1: E402 Module level import not at top of file
+ scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:34:1: E402 Module level import not at top of file
+ scripts/ci/pre_commit/pre_commit_local_yml_mounts.py:57:14: COM812 [*] Trailing comma missing
```

</p>
</details>
Rules changed: 12

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| E402 | 5 | 5 | 0 |
| F401 | 3 | 3 | 0 |
| I001 | 2 | 2 | 0 |
| INP001 | 2 | 2 | 0 |
| COM812 | 2 | 2 | 0 |
| D212 | 1 | 1 | 0 |
| PGH003 | 1 | 1 | 0 |
| ANN202 | 1 | 1 | 0 |
| TRY003 | 1 | 1 | 0 |
| EM102 | 1 | 1 | 0 |
| SIM108 | 1 | 1 | 0 |
| SIM118 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.02ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1782.7±3.21µs     9.3 MB/sec    1.00   1778.5±4.77µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    196.3±0.48µs    15.0 MB/sec    1.00    195.8±1.19µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.00ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.02ms     2.9 MB/sec    1.01     14.0±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.2±1.80µs     8.1 MB/sec    1.00    364.3±1.13µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1464.8±3.69µs    11.4 MB/sec    1.00   1460.3±7.63µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.0±0.77µs    18.8 MB/sec    1.00    157.1±0.74µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     10.4±0.10ms     3.9 MB/sec    1.00      9.9±0.09ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.03ms     7.6 MB/sec    1.00      2.1±0.04ms     7.8 MB/sec
formatter/numpy/globals.py                 1.02    243.2±6.71µs    12.1 MB/sec    1.00    239.4±7.33µs    12.3 MB/sec
formatter/pydantic/types.py                1.04      4.9±0.05ms     5.2 MB/sec    1.00      4.7±0.06ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.12ms     2.6 MB/sec    1.00     15.7±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.01      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.8±5.87µs     5.9 MB/sec    1.01    503.1±7.18µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.01      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1701.1±16.16µs     9.8 MB/sec    1.01  1711.4±18.14µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.0±2.81µs    14.8 MB/sec    1.01    202.7±3.33µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.01      3.6±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-06 20:39_

Those errors in Airflow are legitimate. They're using `# flake8: noqa: F401` which causes _all_ diagnostics to be ignored in the file.

---

_@konstin approved on 2023-07-07 08:27_

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:288 on 2023-07-07 09:20_

Not that I know. I find it readable. 

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:277 on 2023-07-07 09:24_

What could help to clean this up potentially is to implement `ParsedFileExemption` as a "lexer" which returns different Tokens:

```
enum ExemptionToken {
	Whitespace,
	Flake8,
	Hash,
	Noqa,
	Code,
```

See `SimpleTokenizer` with its `Cursor` implementation for inspiration. `try_extract` is then a parser where you could have `eat` etc functions. But this may be very overkill.

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:402 on 2023-07-07 09:27_

Nit
```suggestion
		match chars.next() {
			Some(',') => chars.as.str(),
			Some(c) if is_python_whitespace(c) => chars.as_str(),
			_ => None
		}
```

---

_@MichaReiser approved on 2023-07-07 09:27_

This is cool!

---

_Review comment by @charliermarsh on `crates/ruff/src/noqa.rs`:277 on 2023-07-07 13:59_

Yeah in hindsight perhaps I should've done this. I may look into it as a follow-up PR.

---

_@charliermarsh reviewed on 2023-07-07 13:59_

---

_Merged by @charliermarsh on 2023-07-08 16:37_

---

_Closed by @charliermarsh on 2023-07-08 16:37_

---

_Branch deleted on 2023-07-08 16:37_

---

_Comment by @9128305 on 2023-07-12 23:00_

ruff 0.0.278 ignores non-code noqa. Is this a desired behavior?
```python
file = open('tmp/t.py', 'r') # noqa: open-file-with-context-handler
```
This works with 0.0.277 with `ruff --select SIM115`

---

_Comment by @charliermarsh on 2023-07-12 23:59_

@9128305 - Yes this is expected! Ruff was previously treating that as equivalent to `# noqa` ("Ignore _any_ errors on this line"). Flake8 has the same behavior. As of 0.0.278, we should be warning that that's an invalid `# noqa` code. You should instead use the numeric code for now to ignore SIM115 (e.g., `# noqa: SIM115`).

We will likely support using human-readable names in `# noqa` comments in the future, but as of now they aren't part of the public API.

---

_Comment by @9128305 on 2023-07-13 00:03_

Thanks for the clarification!

---

_Comment by @charliermarsh on 2023-07-13 00:19_

Any time, apologies for any confusion. I wrote a little more about it in the release blog post: https://astral.sh/blog/ruff-v0.0.278#invalid-noqa-directives-now-emit-warnings

---

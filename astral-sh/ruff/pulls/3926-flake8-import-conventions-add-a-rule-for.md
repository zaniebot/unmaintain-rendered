```yaml
number: 3926
title: "[`flake8-import-conventions`] Add a rule for `BannedImportAlias`"
type: pull_request
state: merged
author: stancld
labels: []
assignees: []
merged: true
base: main
head: rule/banned-aliases
created_at: 2023-04-10T11:32:28Z
updated_at: 2023-04-12T03:47:28Z
url: https://github.com/astral-sh/ruff/pull/3926
synced_at: 2026-01-12T04:28:19Z
```

# [`flake8-import-conventions`] Add a rule for `BannedImportAlias`

---

_Pull request opened by @stancld on 2023-04-10 11:32_

This PR adds a new rule into `flake8-import-conventions` allowing to ban of certain import aliases (`ICN002`), for example, due to violating some project's rules.

For example, with a given configuration:
```
[tool.ruff.flake8-import-conventions.banned-aliases]
"tensorflow.keras.backend" = ["K"]
```
the following import
```
>>> import tensorflow.keras.backend as K
```
raises.
```
`tensorflow.keras.backend` should not be imported as `K`
```

Closes #3892.

### Question:
- Since this rule is not supported directly in `flake8-import-conventions`, I'm not sure if adding this rule under this module is valid or not.

---

_Renamed from "rule[`flake8-import-conventions`]: Add a rule for `BannedImportAlias`" to "`flake8-import-conventions`: Add a rule for `BannedImportAlias`" by @stancld on 2023-04-10 11:34_

---

_Comment by @github-actions[bot] on 2023-04-10 11:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.48ms     2.4 MB/sec    1.01     16.8±0.39ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.03ms     3.9 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    547.5±1.59µs     5.4 MB/sec    1.00   535.0±12.63µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.3±0.03ms     3.5 MB/sec    1.00      7.1±0.16ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.6±0.04ms     4.7 MB/sec    1.00      8.5±0.15ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1917.1±19.61µs     8.7 MB/sec    1.00  1918.9±36.55µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.0±2.81µs    13.8 MB/sec    1.01    215.2±4.90µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.03ms     6.4 MB/sec    1.01      4.0±0.08ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.9±0.15ms     2.6 MB/sec    1.00     15.8±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    491.1±5.06µs     6.0 MB/sec    1.00    490.8±5.47µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.11ms     3.8 MB/sec    1.00      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.08ms     5.0 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1767.8±20.15µs     9.4 MB/sec    1.00  1770.9±19.10µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.2±3.38µs    15.4 MB/sec    1.01    194.6±7.86µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "`flake8-import-conventions`: Add a rule for `BannedImportAlias`" to "[`flake8-import-conventions`] Add a rule for `BannedImportAlias`" by @stancld on 2023-04-10 12:13_

---

_Marked ready for review by @stancld on 2023-04-10 12:26_

---

_Comment by @charliermarsh on 2023-04-10 13:57_

I think it's ok to add this as its own code, as you've done here -- we already deviated from the upstream plugin's codes (which we do very rarely) since `flake8-import-conventions` uses a separate rule for every individual import convention (whereas we wanted to support a single configurable rule for "bad aliases"). So deviating further here is fine.

---

_Comment by @charliermarsh on 2023-04-10 13:59_

One question on semantics. Given this:

```py
[tool.ruff.flake8-import-conventions.banned-aliases]
"tensorflow.keras.backend" = ["K"]
```

Is it correct that we'll flag `import tensorflow.keras.backend as K` but not `import tensorflow.keras.backend as B` etc.? In practice, do you all tend to alias `tensorflow.keras.backend` as something else, or prefer that it's not aliased at all? (Specifically, I'm wondering if it'd be useful to frame this as "disallow aliasing this import" rather than banning specific aliases.)


---

_Comment by @stancld on 2023-04-10 15:46_

My rationale when implementing this was to ban specific aliases I encounter during the reviewing of a project's codebase. (For example, those are aliases regularly used in tutorials, etc., but are considered unconventional in a given project.)
Didn't think about forbidding aliases for specific imports at all, but I can see your point now, and it makes sense to me as well. To be honest, I'm not sure then whether it's more appropriate to rely on a global ban for selected imports, or whether those rules are rather complementary.

---

_@MichaReiser reviewed on 2023-04-11 09:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_import_conventions/mod.rs`:76 on 2023-04-11 09:08_

You have to use  `assert_messages` after #3906 lands. It pretty prints the diagnostics instead of serializing them to yaml. 

```suggestion
        assert_messages!(snapshot, diagnostics);
```

---

_@stancld reviewed on 2023-04-11 14:19_

---

_Review comment by @stancld on `crates/ruff/src/rules/flake8_import_conventions/mod.rs`:76 on 2023-04-11 14:19_

@MichaReiser Thanks for pointing this out. Rebased & updated :]

---

_Comment by @charliermarsh on 2023-04-12 03:25_

I'm not sure either (whether the rules are complementary), but let's just run with this for now since we have a concrete use-case.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_import_conventions/rules/check_banned_import.rs`:51 on 2023-04-12 03:26_

I tweaked the logic here a bit, I think it was unintentionally flagging any non-aliased import.

---

_@charliermarsh reviewed on 2023-04-12 03:26_

---

_Merged by @charliermarsh on 2023-04-12 03:29_

---

_Closed by @charliermarsh on 2023-04-12 03:29_

---

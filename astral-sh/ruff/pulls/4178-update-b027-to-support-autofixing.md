```yaml
number: 4178
title: Update B027 to support autofixing
type: pull_request
state: merged
author: aacunningham
labels:
  - fixes
assignees: []
merged: true
base: main
head: b027-autofix
created_at: 2023-05-02T06:20:19Z
updated_at: 2023-05-07T04:59:30Z
url: https://github.com/astral-sh/ruff/pull/4178
synced_at: 2026-01-12T03:56:38Z
```

# Update B027 to support autofixing

---

_Pull request opened by @aacunningham on 2023-05-02 06:20_

Adding the ability to autofix B027 errors from bugbear by adding an `@abstractmethod` decorator above the offending function.

Resolves #4163 

---

_@aacunningham reviewed on 2023-05-02 06:22_

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:142 on 2023-05-02 06:22_

The end result looks pretty alright, just from eyeballing the snapshots. But not sure the best way to handle this `.unwrap()`, or if there's a better way to match the indentation/newline stuff. Thoughts?

---

_@aacunningham reviewed on 2023-05-02 06:23_

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:138 on 2023-05-02 06:23_

Do we need to be concerned about _also_ importing the decorator if it's not in the file? Seems weird that fixing the offending code could actually break it.

---

_Marked ready for review by @aacunningham on 2023-05-02 06:27_

---

_Comment by @github-actions[bot] on 2023-05-02 06:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.07ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.0±0.91µs     7.0 MB/sec    1.00    419.2±2.32µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.03ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      7.1±0.01ms     5.7 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1516.2±1.48µs    11.0 MB/sec    1.00   1485.6±1.38µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    169.2±0.26µs    17.4 MB/sec    1.00    166.2±0.45µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.01ms     8.0 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.01      5.5±0.01ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1053.6±1.95µs    15.8 MB/sec    1.01   1067.6±0.47µs    15.6 MB/sec
parser/numpy/globals.py                    1.00    107.7±0.53µs    27.4 MB/sec    1.02    109.8±0.52µs    26.9 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.01      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.7±0.38ms  2014.5 KB/sec    1.01     20.8±0.50ms  2002.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.20ms     3.2 MB/sec    1.00      5.2±0.15ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   620.3±25.30µs     4.8 MB/sec    1.01   627.5±30.10µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.35ms     2.9 MB/sec    1.00      8.8±0.27ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.5±0.51ms     3.9 MB/sec    1.00     10.5±0.23ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.4 MB/sec    1.00      2.2±0.07ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.5±13.48µs    11.6 MB/sec    1.04   262.5±21.54µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.22ms     5.4 MB/sec    1.00      4.7±0.18ms     5.4 MB/sec
parser/large/dataset.py                    1.01      8.6±0.15ms     4.7 MB/sec    1.00      8.5±0.23ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.01  1648.0±61.93µs    10.1 MB/sec    1.00  1624.5±74.30µs    10.3 MB/sec
parser/numpy/globals.py                    1.00    167.3±7.88µs    17.6 MB/sec    1.00    166.5±7.98µs    17.7 MB/sec
parser/pydantic/types.py                   1.01      3.6±0.11ms     7.0 MB/sec    1.00      3.6±0.12ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-02 06:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:138 on 2023-05-02 06:37_

Yeah we do need to worry about this! If you take a look at `crates/ruff/src/rules/pyupgrade/rules/lru_cache_with_maxsize_none.rs`, we have this call to `get_or_import_symbol` which will either import the symbol if it's not already imported, or give you the correct reference (e.g., `import abc as foo; foo.abstractmethod`).

---

_@charliermarsh reviewed on 2023-05-02 06:38_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:142 on 2023-05-02 06:38_

I think we should use an `if-let` here and only apply the fix if `indentation` returns `Some`. I'm trying to come up with a scenario in which `indentation` could be `None`... it may not be possible, but it's better to be defensive here.

---

_Review comment by @Skylion007 on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:138 on 2023-05-02 14:03_

@aacunningham Yep, the main reason this autofix wasn't implemented when B027 was added is because we did not have the `get_or_import_symbol` functionality implemented yet.

---

_@Skylion007 reviewed on 2023-05-02 14:03_

---

_@aacunningham reviewed on 2023-05-02 14:20_

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:138 on 2023-05-02 14:20_

Tbh that's a pretty good reason to pass on adding it 😃 

Let me take a look at that and squeeze it in here

---

_@aacunningham reviewed on 2023-05-02 21:17_

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B027_B027.py.snap`:20 on 2023-05-02 21:17_

In the fixture there's this at the top:

```py
from abc import abstractmethod
from abc import abstractmethod as notabstract
```

So `get_or_import_symbol` seems to prefer `notabstract` over `abstractmethod`. It makes this snap look a little funny, but hopefully this code isn't a real life example, and most cases will just import `abstractmethod`.

Should I see about making a separate fixture to assert that we add an import when it's not there?

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:138 on 2023-05-02 21:22_

Aight, "borrowed" inspiration from `lru_cache_with_maxsize_none.rs` (wish I had found that one sooner). Looking good, plus it gave me a clean answer for handling that one `Option` value, converting it to a Result inside that `try_set_fix` closure.

Thanks y'all, that `get_or_import_symbol` really made that too easy.

---

_@aacunningham reviewed on 2023-05-02 21:22_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B027_B027.py.snap`:20 on 2023-05-03 09:04_

Can we add a new test file specific to this rule without that import at the top to verify this is the case?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:80 on 2023-05-03 09:06_

Nit: We could also use `unwrap_or_default` because `indentation` returning `None` means that the `stmt` is at the beginning of the line.

---

_@MichaReiser approved on 2023-05-03 09:06_

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:80 on 2023-05-03 15:36_

Ooooh that makes a ton of sense, would explain the None. I guess that function only looks at the line of the Located, so it'd make sense if it failed for unindented lines.

I think end result will be the same regardless (this should never hit an unindented class method, so it should never be None), but unwrap_or_default feels like it'll read better anyways. Let me make that change real fast.

---

_@aacunningham reviewed on 2023-05-03 15:36_

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B027_B027.py.snap`:20 on 2023-05-04 04:06_

Donezo, lemme know if that test is what we're thinking.

---

_@aacunningham reviewed on 2023-05-04 04:06_

---

_Review comment by @aacunningham on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:80 on 2023-05-04 04:07_

Updated, looks good!

---

_@aacunningham reviewed on 2023-05-04 04:07_

---

_@MichaReiser reviewed on 2023-05-04 06:53_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/snapshots/ruff__rules__flake8_bugbear__tests__B027_B027.py.snap`:20 on 2023-05-04 06:53_

It is, thank you

---

_@charliermarsh reviewed on 2023-05-04 16:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/abstract_base_class.rs`:80 on 2023-05-04 16:32_

I think the `ok_or` version might actually be more appropriate, since if `indentation` returns `None`, it means there's non-whitespace content before the `stmt`.

---

_Label `autofix` added by @charliermarsh on 2023-05-04 16:32_

---

_Merged by @charliermarsh on 2023-05-04 16:36_

---

_Closed by @charliermarsh on 2023-05-04 16:36_

---

_Comment by @aacunningham on 2023-05-04 16:44_

Thanks for the feedback and updates! :100: 

---

_Comment by @charliermarsh on 2023-05-04 16:49_

Thanks for the contribution!

---

_Branch deleted on 2023-05-07 04:59_

---

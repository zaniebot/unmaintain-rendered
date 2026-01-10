```yaml
number: 9286
title: Allow config-file overrides in ecosystem checks
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/toml-updates-ecosystem
created_at: 2023-12-26T19:29:15Z
updated_at: 2023-12-28T16:44:52Z
url: https://github.com/astral-sh/ruff/pull/9286
synced_at: 2026-01-10T23:07:18Z
```

# Allow config-file overrides in ecosystem checks

---

_Pull request opened by @zanieb on 2023-12-26 19:29_

Adds the ability to override `ruff.toml` or `pyproject.toml` settings per-project during ecosystem checks.

Exploring this as a fix for the `setuptools` project error. 

Also useful for including Jupyter Notebooks in the ecosystem checks, see #9293

Note the remaining `sphinx` project error is resolved in #9294 

---

_Label `ci` added by @zanieb on 2023-12-26 19:29_

---

_Converted to draft by @zanieb on 2023-12-26 19:30_

---

_@zanieb reviewed on 2023-12-26 19:33_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/check.py`:482 on 2023-12-26 19:33_

Technically should be async but I don't mind blocking the event loop to read/write from the file in this simple code.

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:91 on 2023-12-26 19:34_

This seems a little confusing but something like this is needed to solve the problem.

---

_@zanieb reviewed on 2023-12-26 19:34_

---

_Comment by @github-actions[bot] on 2023-12-26 20:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: Selection of nursery rule `E211` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E273` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E228` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E117` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E262` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E223` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E112` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E203` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E116` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E221` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E227` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E242` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E271` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E275` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E261` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E222` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E111` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E202` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E115` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E226` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E231` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E241` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E224` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E113` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E252` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E265` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E272` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E274` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E266` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E225` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E201` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E114` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E211` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E273` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E228` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E117` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E262` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E223` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E112` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E203` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E116` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E221` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E227` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E242` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E271` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E275` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E261` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E222` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E111` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E202` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E115` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E226` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E231` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E241` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E224` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E113` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E252` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E265` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E272` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E274` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E266` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E225` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E201` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E114` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E211` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E273` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E228` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E117` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E262` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E223` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E112` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E203` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E116` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E221` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E227` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E242` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E271` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E275` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E261` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E222` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E111` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E202` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E115` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E226` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E231` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E241` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E224` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E113` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E252` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E265` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E272` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E274` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E266` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E225` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E201` without the `--preview` flag is deprecated.
warning: Selection of nursery rule `E114` without the `--preview` flag is deprecated.
warning: The following rules may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: The `flake8-quotes.inline-quotes="single"` option is incompatible with the formatter's `format.quote-style="double"`. We recommend disabling `Q000` and `Q003` when using the formatter, which enforces a consistent quote style. Alternatively, set both options to either `"single"` or `"double"`.
warning: Detected debug build without --no-cache.
error: Failed to read tests/roots/test-pycode/cp_1251_coded.py: stream did not contain valid UTF-8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: The following rules may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: The `flake8-quotes.inline-quotes="single"` option is incompatible with the formatter's `format.quote-style="double"`. We recommend disabling `Q000` and `Q003` when using the formatter, which enforces a consistent quote style. Alternatively, set both options to either `"single"` or `"double"`.
warning: Detected debug build without --no-cache.
error: Failed to read tests/roots/test-pycode/cp_1251_coded.py: stream did not contain valid UTF-8
```

</p>
</details>




---

_Marked ready for review by @zanieb on 2023-12-27 17:21_

---

_Renamed from "Allow toml-file overrides in ecosystem checks" to "Allow config-file overrides in ecosystem checks" by @zanieb on 2023-12-27 17:21_

---

_Review requested from @charliermarsh by @zanieb on 2023-12-28 01:50_

---

_Review comment by @MichaReiser on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:110 on 2023-12-28 10:21_

I'm sure this is correct, but it reads strangely. Why do we need to `yield` (nothing) right before returning?

---

_@MichaReiser approved on 2023-12-28 10:21_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/projects.py`:110 on 2023-12-28 16:43_

If we don't yield, the function is no longer a generator and the contextlib wrapper will complain.

```python
from contextlib import contextmanager


@contextmanager
def foo():
    return


with foo():
    pass
```

```
Traceback (most recent call last):
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 9, in <module>
    with foo():
  File "/Users/mz/.pyenv/versions/3.12.0/lib/python3.12/contextlib.py", line 137, in __enter__
    return next(self.gen)
           ^^^^^^^^^^^^^^
TypeError: 'NoneType' object is not an iterator
```

---

_@zanieb reviewed on 2023-12-28 16:43_

---

_Merged by @zanieb on 2023-12-28 16:44_

---

_Closed by @zanieb on 2023-12-28 16:44_

---

_Branch deleted on 2023-12-28 16:44_

---

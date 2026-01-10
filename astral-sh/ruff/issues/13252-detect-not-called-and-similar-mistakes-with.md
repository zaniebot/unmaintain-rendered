```yaml
number: 13252
title: "Detect `not_called()` and similar mistakes with Python mocks before Python 3.12"
type: issue
state: open
author: potiuk
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-09-05T11:31:59Z
updated_at: 2024-11-13T11:43:51Z
url: https://github.com/astral-sh/ruff/issues/13252
synced_at: 2026-01-10T11:09:55Z
```

# Detect `not_called()` and similar mistakes with Python mocks before Python 3.12

---

_Issue opened by @potiuk on 2024-09-05 11:31_

Python 3.12 started to report errors when mock assertion is done with a "common" mistake:

```
mock.not_called()
```

This does not do what you think it does, and when you run it with Python 3.12 you will get error:

```
'not_called' is not a valid assertion. Use a spec for the mock if 'not_called' is meant to be an attribute.
```

The correct way of calling it is:

```
mock.assert_not_called()
```

Similar for other methods. It's very easy to miss it during review and for Python < 3.12 that call gets silently ignored (obviously it looks like you are calling `not_called` method on mock and it will just record the call in the mock.

For projects like Apache Airflow where we optimize most PRs and run most tests with "lowest" supported Python (Python 3.8) this leads sometimes to false-positives where the tests start to fail in Python 3.12 "canary" build where it performs full suite of tests (not mentioning that this test passing on Python 3.8 does not test anything anyway because it does not make any assertion).

Example case where it caused our "canary" builds to fail https://github.com/apache/airflow/pull/42027 

It would be great to detect such "mistakes" via static checks with ruff as a new rule.


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Renamed from "Detect `not_called()` and similar mistakes in Python mocks before Python 3.12" to "Detect `not_called()` and similar mistakes with Python mocks before Python 3.12" by @potiuk on 2024-09-05 11:33_

---

_Comment by @AlexWaygood on 2024-09-06 08:58_

This might be a good addition to PGH005, assuming that that rule doesn't flag this already

---

_Comment by @AlexWaygood on 2024-09-06 09:37_

Indeed PGH005 does not currently flag this: https://play.ruff.rs/5f47abd1-efa4-4fd9-8077-616ae57edbad.

There's a possibility of false positives here as we don't yet have type inference in Ruff. If the object is not a mock, just a regular class that has a `not_called()` method, then we'd emit a spurious error on the `not_called()` call, because we currently only have limited capability to detect the type of an object. However, this seems relatively unlikely, given that not many classes have methods with names like `not_called()`. So I think this should be fine to implement now.

---

_Label `rule` added by @AlexWaygood on 2024-09-06 09:38_

---

_Label `help wanted` added by @AlexWaygood on 2024-09-06 09:38_

---

_Label `accepted` added by @AlexWaygood on 2024-09-06 09:38_

---

_Comment by @potiuk on 2024-09-06 10:46_

>  So I think this should be fine to implement now.

Would be fantastic :)

---

_Comment by @InSyncWithFoo on 2024-11-13 11:43_

@AlexWaygood Actually, `not_called` [<em>does</em> trigger a diagnostic](https://github.com/astral-sh/ruff/blob/f789b12705048a4f75cddeceb843fd4afbe783f1/crates/ruff_linter/src/rules/pygrep_hooks/rules/invalid_mock_access.rs#L91), but only [in the context of an `assert` statement](https://github.com/astral-sh/ruff/blob/f789b12705048a4f75cddeceb843fd4afbe783f1/crates/ruff_linter/src/checkers/ast/analyze/statement.rs#L1258-L1294).

---

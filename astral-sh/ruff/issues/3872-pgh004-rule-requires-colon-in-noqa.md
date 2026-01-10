```yaml
number: 3872
title: "PGH004 rule requires colon in `noqa`"
type: issue
state: closed
author: jurasofish
labels:
  - question
assignees: []
created_at: 2023-04-04T00:44:39Z
updated_at: 2023-04-04T09:06:32Z
url: https://github.com/astral-sh/ruff/issues/3872
synced_at: 2026-01-10T11:09:46Z
```

# PGH004 rule requires colon in `noqa`

---

_Issue opened by @jurasofish on 2023-04-04 00:44_

If you run `ruff check scratch_171.py --isolated --select PGH004` on the snippet below you get the output

```
scratch_171.py:2:8: PGH004 Use specific rule codes when using noqa
```


```python
a = 1  # noqa: blahblahblah
a = 1  # noqa blahblahblah
```


PGH004 is incorrectly detecting that `noqa blahblahblah` does not have a specific rule code, presumably because it does not have a colon.

```
ruff --version
ruff 0.0.260
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-04-04 02:01_

Hm, I think this is arguably correct. Both Ruff and Flake8 treat `# noqa blahblahblah` as equivalent to `# noqa`. Even `# noqa E501` is treated as identical to `# noqa` (that is, it's treated as a blanket ignore). So it seems right to be called out as such?

---

_Label `question` added by @charliermarsh on 2023-04-04 02:01_

---

_Comment by @jurasofish on 2023-04-04 09:06_

Ah you're right - I had assumed that `noqa blah` was equivalent to `noqa: blah` but it's actually a blanket ignore.

[PGH004](https://beta.ruff.rs/docs/rules/#pygrep-hooks-pgh) plus [RUF100](https://beta.ruff.rs/docs/rules/#ruff-specific-rules-ruf) protect against that pretty well then.

thank you


---

_Closed by @jurasofish on 2023-04-04 09:06_

---

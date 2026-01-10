```yaml
number: 4823
title: "F523: Single variable with single quotes triggers syntax violation"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-03T04:14:01Z
updated_at: 2023-06-03T19:53:59Z
url: https://github.com/astral-sh/ruff/issues/4823
synced_at: 2026-01-10T11:09:47Z
```

# F523: Single variable with single quotes triggers syntax violation

---

_Issue opened by @addisoncrump on 2023-06-03 04:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Reproduction

The following snippet is a minimal reproduction:

```py
x = 5
'".'.format(x)
```

The fix attempted:

```py
x = 5
"".".format()
```

## Command line

```bash
ruff check format.py --fix --isolated
```

## Version information

Built on e82160a83a76c11d04d727ee2349091351e63b32 (latest, bleeding).

## Misc

Discovered by #4822.

---

_Label `bug` added by @charliermarsh on 2023-06-03 15:57_

---

_Comment by @charliermarsh on 2023-06-03 15:57_

Thanks!

---

_Comment by @charliermarsh on 2023-06-03 15:58_

Looks like F523 _always_ uses double quotes: `new_format_string = format!(r#""{new_format_string}""#);`

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-03 19:27_

---

_Closed by @charliermarsh on 2023-06-03 19:53_

---

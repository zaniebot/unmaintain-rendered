```yaml
number: 6240
title: "Ignores `noqa` comment for PLR rule if not prefixed with `ruff: `"
type: issue
state: closed
author: chbndrhnns
labels:
  - suppression
assignees: []
created_at: 2023-08-01T12:57:55Z
updated_at: 2023-08-01T13:49:17Z
url: https://github.com/astral-sh/ruff/issues/6240
synced_at: 2026-01-12T15:54:45Z
```

# Ignores `noqa` comment for PLR rule if not prefixed with `ruff: `

---

_@chbndrhnns_

The [docs](https://beta.ruff.rs/docs/configuration/#error-suppression) say:

> To ignore an individual violation, add # noqa: {code} to the end of the line

This seems not work for this snippet. It only works when using `ruff: noqa: PLR0912`. 
That's surprising to me.

```py
# noqa: PLR0912
def _find_parent_by_value(data, look_for, parent=None, include_spec=False):
    if data and not isinstance(data, (str, int, float)):
        for key, val in data.items():
            if val == look_for:
                if include_spec:
                    if isinstance(parent, list):
                        yield from [parent]
                    else:
                        yield parent
                elif isinstance(parent, list):
                    yield []
                else:
                    yield from parent
            elif isinstance(val, dict):
                if look_for in val.values():
                    if include_spec:
                        yield {key: val}
                    else:
                        yield key
                else:
                    yield from _find_parent_by_value(
                        val, look_for, data, include_spec=include_spec
                    )
            elif isinstance(val, list):
                for item in val:
                    yield from _find_parent_by_value(
                        item, look_for, val, include_spec=include_spec
                    )

```

---

_Renamed from "Ignores `noqa` comment for PLR if not prefixed with `ruff: `" to "Ignores `noqa` comment for PLR rule if not prefixed with `ruff: `" by @chbndrhnns on 2023-08-01 13:00_

---

_Comment by @chbndrhnns on 2023-08-01 13:08_

I am starting to thing the docs might be off and `ruff: ` is required for ANY rule that should be disabled. :)

---

_Comment by @charliermarsh on 2023-08-01 13:09_

They do two different things:

- `# noqa: PLR0912` is an inline ignore. It will suppress a violation on a single line, and it needs to be placed at the end of the line on which the violation occurs. In your case, I think that's just after the `def _find_parent_by_value(data, look_for, parent=None, include_spec=False):  # noqa: PLR0912`.
- `# ruff: noqa: PLR0912` disables `PLR0912` for the entire file, and must be placed at the _start_ of a line.

---

_Comment by @charliermarsh on 2023-08-01 13:10_

So concretely, you either want:

```python
def _find_parent_by_value(data, look_for, parent=None, include_spec=False):  # noqa: PLR0912
    if data and not isinstance(data, (str, int, float)):
        for key, val in data.items():
            if val == look_for:
                if include_spec:
                    if isinstance(parent, list):
                        yield from [parent]
                    else:
                        yield parent
                elif isinstance(parent, list):
                    yield []
                else:
                    yield from parent
            elif isinstance(val, dict):
                if look_for in val.values():
                    if include_spec:
                        yield {key: val}
                    else:
                        yield key
                else:
                    yield from _find_parent_by_value(
                        val, look_for, data, include_spec=include_spec
                    )
            elif isinstance(val, list):
                for item in val:
                    yield from _find_parent_by_value(
                        item, look_for, val, include_spec=include_spec
                    )
```

Or (less recommended, only if you need to turn off a violation for a single file):

```python
# ruff: noqa: PLR0912
def _find_parent_by_value(data, look_for, parent=None, include_spec=False):
    if data and not isinstance(data, (str, int, float)):
        for key, val in data.items():
            if val == look_for:
                if include_spec:
                    if isinstance(parent, list):
                        yield from [parent]
                    else:
                        yield parent
                elif isinstance(parent, list):
                    yield []
                else:
                    yield from parent
            elif isinstance(val, dict):
                if look_for in val.values():
                    if include_spec:
                        yield {key: val}
                    else:
                        yield key
                else:
                    yield from _find_parent_by_value(
                        val, look_for, data, include_spec=include_spec
                    )
            elif isinstance(val, list):
                for item in val:
                    yield from _find_parent_by_value(
                        item, look_for, val, include_spec=include_spec
                    )
```

---

_Closed by @charliermarsh on 2023-08-01 13:10_

---

_Label `noqa` added by @charliermarsh on 2023-08-01 13:10_

---

_Comment by @chbndrhnns on 2023-08-01 13:27_

Thanks for explanation! 

I think this distinction between file-level and inline ignore could be a bit clearer in the docs. (I got it now.)
Also, I am wondering what this distinction (prefix vs no prefix) is required for: Wouldn't it be enough to use `# noqa: <rule>` at the beginning of the line to make it file-level?



---

_Comment by @zanieb on 2023-08-01 13:49_

We're planning on redesigning these suppression comments in the future. The current design imitates existing tooling for compatibility.

---

```yaml
number: 1529
title: "pypi-types: fix lenient requirement parsing"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-i1477
created_at: 2024-02-16T19:03:50Z
updated_at: 2024-02-16T20:52:45Z
url: https://github.com/astral-sh/uv/pull/1529
synced_at: 2026-01-12T16:04:38Z
```

# pypi-types: fix lenient requirement parsing

---

_@BurntSushi_

This fixes a bug where `uv pip install` failed to install `polars`:

```
$ uv pip install polars==0.14.0
error: Failed to download: polars==0.14.0
  Caused by: Couldn't parse metadata of polars-0.14.0-cp37-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.whl from https://files.pythonhosted.org/packages/5c/c6/749022b096895790c971338de93e610210331ea6cb7c1c2cc32b7f433c4f/polars-0.14.0-cp37-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.whl
  Caused by: Operator >= cannot be used with a wildcard version specifier
pyarrow>=4.0.*; extra == 'pyarrow'
       ^^^^^^^
```

Since `pyarrow>=4.0.*; extra == 'pyarrow'` is invalid *and* it comes
from the metadata of a dependency (that isn't under the control of the
end user), we actually attempt to "fix" it. Namely, wildcard
dependency specifications are only allowed with `==` and `!=`, as per
the [Version Specifiers spec]. (They aren't explicitly forbidden in
these cases, but instead only have specified behavior for the `==` and
`!=` operators.)

This is all fine, but it turns out that when we fix the `>=4.0.*`
component, we also strip the quotes around `pyarrow`. (Because some
dependency specifications include stray quotes.) We fix this by making
our quote stripping a bit more selective. (We require that it appear
adjacent to a digit or a `*`.)

Note that #1477 also reports this error:

```
$ uv pip install 'requests>=2.30.*'
error: Failed to parse `requests>=2.30.*`
  Caused by: Operator >= cannot be used with a wildcard version specifier
requests>=2.30.*
```

However, we specifically keep that error message since it's something
under the end user's control. And similarly for a dependency
specification in a `requirements.txt` file.

Fixes #1477

[Version Specifiers spec]: https://packaging.python.org/en/latest/specifications/version-specifiers/


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-16 19:04_

---

_@charliermarsh reviewed on 2024-02-16 20:23_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/lenient_requirement.rs`:25 on 2024-02-16 20:23_

Nit: remove?

---

_@charliermarsh approved on 2024-02-16 20:23_

---

_Label `bug` added by @charliermarsh on 2024-02-16 20:23_

---

_Merged by @BurntSushi on 2024-02-16 20:52_

---

_Closed by @BurntSushi on 2024-02-16 20:52_

---

_Branch deleted on 2024-02-16 20:52_

---

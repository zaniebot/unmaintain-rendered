```yaml
number: 17310
title: "Show package constraints (similar to `poetry show`)"
type: issue
state: open
author: jack-mcivor
labels:
  - enhancement
assignees: []
created_at: 2026-01-02T19:44:53Z
updated_at: 2026-01-13T16:05:43Z
url: https://github.com/astral-sh/uv/issues/17310
synced_at: 2026-01-13T16:27:41Z
```

# Show package constraints (similar to `poetry show`)

---

_@jack-mcivor_

### Summary

Coming from poetry, one thing I use a lot is `poetry show <pkg>`. The output shows the constraints on packages in the dependencies and required-by sections, which is helpful to understand why a particular version is installed. For example (from the [poetry docs](https://python-poetry.org/docs/cli/#show)):
```sh
$ poetry show pendulum

name        : pendulum
version     : 1.4.2
description : Python datetimes made easy

dependencies
 - python-dateutil >=2.6.1
 - tzlocal >=1.4
 - pytzdata >=2017.2.2

required by
 - calendar requires >=1.4.0
```

I'd like to request a similar feature for uv 🙏. I know of `uv pip show <pkg>` and `uv pip tree --package <pkg>`, but those don't show constraints.

One extension which might be useful is showing which constraints are binding. Quite often an upper bound causes a surprising version to be chosen. It can be tough to understand why, especially if the binding constraint is on a transitive dependency. An example to mind is `numba==0.63.1` which requires `numpy >=1.22,<2.4`.


### Example

_No response_

---

_Label `enhancement` added by @jack-mcivor on 2026-01-02 19:44_

---

_Comment by @powercoconola on 2026-01-12 23:16_

Does `uv tree` solve your issue?

---

_Comment by @jack-mcivor on 2026-01-13 16:05_

> Does uv tree solve your issue?

I don't think so? AFAICT that shows installed versions, but does not display constraints at all

---

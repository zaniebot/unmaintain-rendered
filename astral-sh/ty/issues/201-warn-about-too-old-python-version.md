```yaml
number: 201
title: Warn about too old python-version
type: issue
state: open
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2025-02-11T10:57:26Z
updated_at: 2025-11-13T17:34:28Z
url: https://github.com/astral-sh/ty/issues/201
synced_at: 2026-01-10T02:06:24Z
```

# Warn about too old python-version

---

_Issue opened by @MichaReiser on 2025-02-11 10:57_

It might be good to print a warning to the terminal if we resolve the user's configuration settings to a `requires-python` version less than typeshed's oldest supported version. Typeshed exposes this information in [this field here](https://github.com/python/typeshed/blob/24c78b9e0dff457275092025a14387dac206f5d6/pyproject.toml#L173-L174) in its pyproject.toml file (the intention is that it could be used by tools vendoring typeshed as well as by typeshed's internal tools). It doesn't look like we currently vendor typeshed's pyproject.toml file as part of our typeshed syncs, but we could easily start doing so. The reason why I think we should print a warning is that people might think that we'll accurately understand the Python 3.7 stdlib if they put `requires-python = ">= 3.7"` in their pyproject.toml file -- but we won't, because typeshed deleted the pre-Python-3.8 branches when it dropped support for Python 3.7. The reason why it should be a suppressible warning rather than an error is that the user might have Python <3.8 branches in their own code.

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/16028#discussion_r1950623156_
            

---

_Renamed from "[red-knot] Warn about too old python-version" to "Warn about too old python-version" by @MichaReiser on 2025-05-07 15:26_

---

_Label `configuration` added by @carljm on 2025-11-13 00:44_

---

_Assigned to @Gankra by @Gankra on 2025-11-13 17:34_

---

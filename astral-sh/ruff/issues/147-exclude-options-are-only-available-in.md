```yaml
number: 147
title: Exclude options are only available in configuration file
type: issue
state: closed
author: Jackenmen
labels: []
assignees: []
created_at: 2022-09-11T02:37:05Z
updated_at: 2022-09-11T14:44:24Z
url: https://github.com/astral-sh/ruff/issues/147
synced_at: 2026-01-10T15:56:05Z
```

# Exclude options are only available in configuration file

---

_Issue opened by @Jackenmen on 2022-09-11 02:37_

It would be useful to be able to pass `--exclude` to the CLI, rather than only have it be configurable with a config file.
Ideally, ruff would, similarly to flake8, also support `--extend-exclude` to allow extending configured exclusions from CLI rather than override whatever was configured in the file.

---

_Renamed from "Exclude options are only available in configuration" to "Exclude options are only available in configuration file" by @Jackenmen on 2022-09-11 02:37_

---

_Label `enhancement` added by @charliermarsh on 2022-09-11 13:43_

---

_Closed by @charliermarsh on 2022-09-11 14:44_

---

```yaml
number: 1487
title: "Fix typos & add pre-commit configuration"
type: pull_request
state: merged
author: akx
labels:
  - internal
assignees: []
merged: true
base: main
head: pre-commit
created_at: 2024-02-16T12:44:39Z
updated_at: 2024-02-17T17:16:56Z
url: https://github.com/astral-sh/uv/pull/1487
synced_at: 2026-01-10T15:33:24Z
```

# Fix typos & add pre-commit configuration

---

_Pull request opened by @akx on 2024-02-16 12:44_

The pre-commit configuration was adapted from Ruff's.

Typos needed to be fixed because they'd fail pre-commit otherwise now. 😁 

---

_@MichaReiser reviewed on 2024-02-16 13:51_

---

_Review comment by @MichaReiser on `.pre-commit-config.yaml`:14 on 2024-02-16 13:51_

No ruff? ;)

---

_@akx reviewed on 2024-02-16 14:06_

---

_Review comment by @akx on `.pre-commit-config.yaml`:14 on 2024-02-16 14:06_

Oh, fine, this time, if you insist... 😂 

---

_Review requested from @MichaReiser by @akx on 2024-02-16 14:07_

---

_@zanieb reviewed on 2024-02-16 14:31_

---

_Review comment by @zanieb on `_typos.toml`:12 on 2024-02-16 14:31_

```suggestion
seeked = "seeked" # special term used for streams

```

---

_@zanieb approved on 2024-02-16 14:31_

Sweet!

---

_@zanieb reviewed on 2024-02-16 14:32_

---

_Review comment by @zanieb on `.pre-commit-config.yaml`:12 on 2024-02-16 14:32_

Do we need this one?

---

_@zanieb reviewed on 2024-02-16 14:32_

---

_Review comment by @zanieb on `.pre-commit-config.yaml`:12 on 2024-02-16 14:32_

I think Ruff has support for this?

---

_@henryiii reviewed on 2024-02-16 16:13_

---

_Review comment by @henryiii on `.pre-commit-config.yaml`:12 on 2024-02-16 16:13_

Validate-pyproject validates schemas. Ruff just makes sure the pyproject is parsable, as far as I know, and checks its own section. It doesn't check schemas (yet?).

It's not that useful[^1] though unless you add the schemas from SchemaStore, which can be done either with `--store` (slow, pulls from network) or `validate-pyproject-schema-store`, which is a regularly updated copy of the pyproject related schemas. Currently about half of the most popular projects are supported with schemas: https://github.com/SchemaStore/schemastore/issues/3564.

[^1]: By default, it validates `project`, `build-system`, and `tool.setuptools`.

---

_@henryiii reviewed on 2024-02-16 16:15_

---

_Review comment by @henryiii on `.pre-commit-config.yaml`:39 on 2024-02-16 16:15_

```suggestion
        args: [--fix, --exit-non-zero-on-fix, --show-fixes]
```

Also, I believe I've been told `ruff-format` should follow `ruff`.

---

_@akx reviewed on 2024-02-16 21:43_

---

_Review comment by @akx on `.pre-commit-config.yaml`:12 on 2024-02-16 21:43_

Yeah, this was literally just copied from Ruff :D 

---

_@akx reviewed on 2024-02-16 21:43_

---

_Review comment by @akx on `.pre-commit-config.yaml`:39 on 2024-02-16 21:43_

Also copied from Ruff, no opinions either way. 

---

_@MichaReiser approved on 2024-02-17 17:16_

---

_Merged by @MichaReiser on 2024-02-17 17:16_

---

_Closed by @MichaReiser on 2024-02-17 17:16_

---

_Label `internal` added by @MichaReiser on 2024-02-17 17:16_

---

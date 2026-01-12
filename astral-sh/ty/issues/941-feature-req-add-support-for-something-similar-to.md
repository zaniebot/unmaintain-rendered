```yaml
number: 941
title: "[Feature Req] Add support for something similar to `python.analysis.extraPaths`"
type: issue
state: closed
author: ragvri
labels: []
assignees: []
created_at: 2025-08-05T15:32:31Z
updated_at: 2025-08-06T03:44:33Z
url: https://github.com/astral-sh/ty/issues/941
synced_at: 2026-01-12T15:54:24Z
```

# [Feature Req] Add support for something similar to `python.analysis.extraPaths`

---

_@ragvri_

Sorry, not sure if this is already being tracked (I searched and was not able to find anything similar) but just thought there is no harm in creating a feature request here. 

Currently, there is no way for me to add extra paths for import resolution, would be great to have this

---

_Comment by @AlexWaygood on 2025-08-05 15:35_

> Currently, there is no way for me to add extra paths for import resolution

Good news: there is, actually! It has the same name as the pyright option

- https://docs.astral.sh/ty/reference/configuration/#extra-paths
- https://docs.astral.sh/ty/reference/cli/#ty-check--extra-search-path

---

_Closed by @AlexWaygood on 2025-08-05 15:35_

---

_Comment by @ragvri on 2025-08-05 15:48_

Ah thanks, I guess I missed looking at the docs. IIUC, this is a `toml` file configuration and not a vscode settings configuration, right?

---

_Comment by @AlexWaygood on 2025-08-05 15:53_

That's correct, yes. I'd assume the setting would still be read from a pyproject.toml file or ty.toml file even in the context of an IDE such as VSCode, though. @MichaReiser or @dhruvmanila know more about how our editor integration works; they might be able to say more.

---

_Comment by @MichaReiser on 2025-08-05 16:17_

> Ah thanks, I guess I missed looking at the docs. IIUC, this is a `toml` file configuration and not a vscode settings configuration, right?

For now yes, but exposing some settings as editor settings is something we want to do. We just didn't get to it yet

---

_Comment by @dhruvmanila on 2025-08-06 03:44_

For this specific setting, refer to https://github.com/astral-sh/ty-vscode/issues/22

---

```yaml
number: 10457
title: PEP 723 configuration support (inline script metadata)
type: issue
state: open
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2024-03-18T13:56:33Z
updated_at: 2025-01-07T15:35:59Z
url: https://github.com/astral-sh/ruff/issues/10457
synced_at: 2026-01-12T15:54:50Z
```

# PEP 723 configuration support (inline script metadata)

---

_@MichaReiser_

[PEP 723](https://peps.python.org/pep-0723/) introduced configuration support for single-file projects without requiring an external pyproject.toml. We should explore how to support PEP 723 in ruff.



---

_Label `configuration` added by @MichaReiser on 2024-03-18 13:56_

---

_Comment by @janlarres on 2024-09-22 05:32_

This would be quite useful to be able to specify the `requires-python` version. I just had a case where I was using the zoneinfo module that was added to the standard library in 3.9, and I was surprised that Ruff sorted it under third-party imports (see also https://github.com/astral-sh/ruff/issues/6475 and https://github.com/astral-sh/ruff/issues/13354). So I think it would be great if Ruff could support the metadata comment block for that.

---

_Comment by @InSyncWithFoo on 2024-11-11 16:58_

@dhruvmanila Will an ad-hoc fix for `I001` be accepted as a PR, or are some high-level design choices needed to be made first?

---

_Comment by @MichaReiser on 2024-11-11 18:28_

@InSyncWithFoo a short design outline would be great. Which configuration options are supported (what about `exclude`?), how do we plan on integrating it (short outline)

---

_Comment by @InSyncWithFoo on 2024-11-11 19:11_

@MichaReiser I'm afraid I don't follow. What I meant to say is that I got a fix working locally for the `organize_import()` function (and thus `I001` in general), and I'm asking whether I should submit a PR, because, if the function needs to be redesigned entirely, the fix will only be a maintenance burden.

The fix is just to parse the metadata and override `settings.target_version` with its `requires_python` if that is found. You can see it for yourself [here](https://github.com/InSyncWithFoo/ruff/commit/0b00c81b76d5c1f032d9015dd709355fd7975107).

---

_Comment by @MichaReiser on 2024-11-12 08:19_

@InSyncWithFoo I think it should be solved holistically. It's otherwise confusing why some rules respect the script's target versions but others don't. I'm open to only supporting a subset of the configuration options.

---

_Renamed from "PEP 723 configuration support" to "PEP 723 configuration support (inline script metadata)" by @AlexWaygood on 2025-01-07 15:35_

---

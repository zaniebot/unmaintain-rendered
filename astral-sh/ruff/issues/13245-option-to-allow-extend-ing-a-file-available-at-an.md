```yaml
number: 13245
title: "Option to allow `extend`ing a file available at an HTTPS URL"
type: issue
state: closed
author: lengau
labels: []
assignees: []
created_at: 2024-09-04T15:03:59Z
updated_at: 2024-09-04T15:19:43Z
url: https://github.com/astral-sh/ruff/issues/13245
synced_at: 2026-01-12T15:54:52Z
```

# Option to allow `extend`ing a file available at an HTTPS URL

---

_@lengau_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The current `extend` configuration is great for large monorepos where we want to extend configuration from a higher level, but for teams that have multiple separate repos, but still want a unified base configuration, it would be useful to allow `extend` to extend a remote file (with that file being cached for offline use).

For example:

```toml
[tool.ruff]
extend = "https://raw.githubusercontent.com/canonical/starbase/v1/pyproject.toml"
```

This is similar to [Mend Renovate's `extends`](https://docs.renovatebot.com/configuration-options/#extends) but without the complexity of converting repository descriptions to URLs.

---

_Comment by @MichaReiser on 2024-09-04 15:11_

Thanks for opening this feature request. I'll merge it with https://github.com/astral-sh/ruff/issues/12352.

---

_Closed by @MichaReiser on 2024-09-04 15:11_

---

_Comment by @lengau on 2024-09-04 15:19_

Thanks! I missed that when searching for dupes.

---

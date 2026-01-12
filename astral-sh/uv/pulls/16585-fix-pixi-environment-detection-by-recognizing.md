```yaml
number: 16585
title: "Fix pixi environment detection by recognizing Conda prefixes with `conda-meta/pixi`"
type: pull_request
state: merged
author: terror
labels:
  - virtualenv
assignees: []
merged: true
base: main
head: pixi-env-detection
created_at: 2025-11-04T06:54:19Z
updated_at: 2025-11-10T04:16:18Z
url: https://github.com/astral-sh/uv/pull/16585
synced_at: 2026-01-12T16:12:20Z
```

# Fix pixi environment detection by recognizing Conda prefixes with `conda-meta/pixi`

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16295

This PR updates the Conda base-environment heuristic to recognize Pixi-managed environments by checking for the `conda-meta/pixi` marker file. Pixi default environments now resolve as isolated child environments instead of system installations, restoring the expected uv pip behavior without the `--system` flag.

---

_Label `virtualenv` added by @samypr100 on 2025-11-07 02:53_

---

_@zanieb reviewed on 2025-11-07 15:31_

---

_Review comment by @zanieb on `crates/uv-python/src/virtualenv.rs`:91 on 2025-11-07 15:31_

We should probably have a comment here? Like 
```suggestion
        // Pixi never creates "base" environments but uses the name "default" for project environments
        // which breaks our heuristics, so we special case them
        if is_pixi_environment(path) {
```

---

_@zanieb approved on 2025-11-07 16:25_

---

_Merged by @zanieb on 2025-11-10 04:16_

---

_Closed by @zanieb on 2025-11-10 04:16_

---

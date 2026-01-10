```yaml
number: 9293
title: Add jupyter notebooks to ecosystem checks
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/jupyter-ecosystem
created_at: 2023-12-27T17:07:00Z
updated_at: 2024-01-16T20:04:42Z
url: https://github.com/astral-sh/ruff/pull/9293
synced_at: 2026-01-10T22:57:09Z
```

# Add jupyter notebooks to ecosystem checks

---

_Pull request opened by @zanieb on 2023-12-27 17:07_

Implements https://github.com/astral-sh/ruff/pull/8873 via https://github.com/astral-sh/ruff/pull/9286

---

_Label `ci` added by @zanieb on 2023-12-27 17:10_

---

_Comment by @github-actions[bot] on 2023-12-27 17:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @zanieb on 2023-12-27 17:29_

Are these real issues with our formatter? cc @dhruvmanila 

---

_Comment by @konstin on 2023-12-28 19:19_

I checked some of the notebooks and they did contain syntax errors.

The error messages should have a cell number though.

---

_Comment by @zanieb on 2023-12-29 17:15_

Yes I can confirm the huggingface notebooks are all over the place and have invalid syntax. We should improve the formatter error messages in those cases, but that's a separate concern (#9311). I've fixed the errors in https://github.com/zanieb/notebooks/commit/68cd6fa1a2831c5189f85257c13d691cb76292db but contributing upstream seems like a pain since the notebooks are synced from other repositories and the content is full of problems.

I opened a fix for the openai-cookbook error at https://github.com/openai/openai-cookbook/pull/964

---

_Comment by @zanieb on 2023-12-29 20:46_

Hm very interesting, do we mutate cell `id`s?

```diff
  "cells": [
   {
    "cell_type": "markdown",
-   "id": "3f738710-b5ef-46c4-b874-f13221ee9f76",
+   "id": "754e1af3-4775-4211-a476-d0f698c81cd4",
    "metadata": {
     "colab_type": "text",
     "id": "view-in-github"
```

---

_Review requested from @dhruvmanila by @MichaReiser on 2023-12-30 11:10_

---

_Comment by @zanieb on 2023-12-30 17:24_

We create IDs if they do not exist at https://github.com/astral-sh/ruff/blob/c01bb0d485fb2bca14cf89aa4da2cdcd60fe0bbe/crates/ruff_notebook/src/notebook.rs#L155-L158

Presumably this just differs on each run of the Ruff formatter. Hm. I guess we could seed for determinism?

---

_Comment by @zanieb on 2024-01-02 21:26_

Rebasing onto #9359 

---

_Marked ready for review by @zanieb on 2024-01-04 15:20_

---

_Comment by @zanieb on 2024-01-04 18:59_

See fix for ibis #9392 

---

_@charliermarsh reviewed on 2024-01-04 21:33_

---

_Review comment by @charliermarsh on `python/ruff-ecosystem/ruff_ecosystem/defaults.py`:12 on 2024-01-04 21:33_

Why these?

---

_@charliermarsh approved on 2024-01-04 21:33_

---

_@zanieb reviewed on 2024-01-04 21:36_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/defaults.py`:12 on 2024-01-04 21:36_

@dhruvmanila chose them — they're rules that have different behavior for notebooks or something? Idk if we really need to select a subset though?

---

_Merged by @zanieb on 2024-01-04 21:38_

---

_Closed by @zanieb on 2024-01-04 21:38_

---

_Branch deleted on 2024-01-04 21:38_

---

_@charliermarsh reviewed on 2024-01-04 21:40_

---

_Review comment by @charliermarsh on `python/ruff-ecosystem/ruff_ecosystem/defaults.py`:12 on 2024-01-04 21:40_

Yeah I'd probably just expand them, but nbd either way.

---

_@zanieb reviewed on 2024-01-04 22:18_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/defaults.py`:12 on 2024-01-04 22:18_

Like extend select? I'll just wait for Dhruv to be available — not crtical.

---

_@dhruvmanila reviewed on 2024-01-16 20:04_

---

_Review comment by @dhruvmanila on `python/ruff-ecosystem/ruff_ecosystem/defaults.py`:12 on 2024-01-16 20:04_

> they're rules that have different behavior for notebooks or something?

Yeah, I chose them for this exact reason but it's reasonable to just expand them and use `extend-select` if possible.

---

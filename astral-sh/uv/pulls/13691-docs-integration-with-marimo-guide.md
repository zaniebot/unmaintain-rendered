```yaml
number: 13691
title: "docs: integration with marimo guide"
type: pull_request
state: merged
author: akshayka
labels:
  - documentation
assignees: []
merged: true
base: main
head: aka/docs-integration-marimo
created_at: 2025-05-27T21:52:51Z
updated_at: 2025-05-27T23:43:43Z
url: https://github.com/astral-sh/uv/pull/13691
synced_at: 2026-01-12T16:10:48Z
```

# docs: integration with marimo guide

---

_@akshayka_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change adds a new integration guide, on using uv with marimo notebooks. It is similar to but simpler than the existing Jupyter guide, since marimo stores notebooks as Python files and also integrates tightly with uv for package management.

The guide showcases four ways of using uv with marimo:

1. marimo as a standalone tool (`uvx`)
2. managing inline script metadata (an alternative to Jupyter kernels, marimo has no concept of kernels)
3. in project environments
4. in non-project environments

## Test Plan

N/A as this is a docs-only change. 


---

_Assigned to @zanieb by @zanieb on 2025-05-27 22:01_

---

_@zanieb reviewed on 2025-05-27 22:02_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:93 on 2025-05-27 22:02_

Does `uv run marimo edit` not work here?

---

_@zanieb reviewed on 2025-05-27 22:04_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:87 on 2025-05-27 22:04_

I would try to avoid this toggle if you can. If `uv run` doesn't work, you could link out to the docs on activating the virtual environment and say "After activating the environment, run `marimo edit`"? 

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:20 on 2025-05-27 22:04_

```suggestion
For ad-hoc access to marimo notebooks, start a marimo server at any time in an isolated environment
```

---

_@zanieb reviewed on 2025-05-27 22:04_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:27 on 2025-05-27 22:04_

We end these with a `:` generally

---

_@zanieb reviewed on 2025-05-27 22:04_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:48 on 2025-05-27 22:05_

```suggestion
marimo will automatically use uv to start your notebook in an isolated virtual environment with
```

---

_@zanieb reviewed on 2025-05-27 22:05_

---

_@zanieb reviewed on 2025-05-27 22:05_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:48 on 2025-05-27 22:05_

We generally avoid "continuing the sentence" like this.

---

_@zanieb reviewed on 2025-05-27 22:06_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:78 on 2025-05-27 22:06_

Same as https://github.com/astral-sh/uv/pull/13691/files#r2110376751

---

_@zanieb reviewed on 2025-05-27 22:06_

---

_Review comment by @zanieb on `docs/guides/integration/index.md`:7 on 2025-05-27 22:06_

Would it be reasonable to say "Jupyter notebooks" and "marimo notebooks" here?

---

_@zanieb reviewed on 2025-05-27 22:07_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:15 on 2025-05-27 22:07_

Maybe?
```suggestion
You can readily use marimo as a standalone tool, as self-contained scripts, in
```

---

_@zanieb reviewed on 2025-05-27 22:08_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:36 on 2025-05-27 22:08_

"For example, to ...:" would be nice

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:42 on 2025-05-27 22:08_

```suggestion
To run a notebook containing inline script metadata, use
```

---

_@zanieb reviewed on 2025-05-27 22:08_

---

_Review comment by @zanieb on `docs/guides/integration/marimo.md`:98 on 2025-05-27 22:09_

How does this section differ from `Using marimo with inline script metadata`? You may need to explain that here

---

_@zanieb reviewed on 2025-05-27 22:09_

---

_Label `documentation` added by @zanieb on 2025-05-27 22:11_

---

_Review comment by @akshayka on `docs/guides/integration/marimo.md`:98 on 2025-05-27 22:39_

I've added more context — notebooks can be run as scripts even if they don't have inline script metadata. If you think this is redundant, or obvious, I am happy to remove this section.

---

_@akshayka reviewed on 2025-05-27 22:39_

---

_@akshayka reviewed on 2025-05-27 22:39_

---

_Review comment by @akshayka on `docs/guides/integration/marimo.md`:87 on 2025-05-27 22:39_

`uv run` does in fact work. Updated, thank you.

---

_@akshayka reviewed on 2025-05-27 22:39_

---

_Review comment by @akshayka on `docs/guides/integration/marimo.md`:93 on 2025-05-27 22:39_

It does work in fact, thank you, updated.

---

_@akshayka reviewed on 2025-05-27 22:40_

---

_Review comment by @akshayka on `docs/guides/integration/marimo.md`:27 on 2025-05-27 22:40_

Done

---

_Review comment by @akshayka on `docs/guides/integration/index.md`:7 on 2025-05-27 22:40_

Done

---

_@akshayka reviewed on 2025-05-27 22:40_

---

_Comment by @akshayka on 2025-05-27 22:42_

@zanieb Thanks so much for the thorough review. Ready for another look.

---

_@zanieb approved on 2025-05-27 23:43_

Thanks for the prompt updates!

---

_Merged by @zanieb on 2025-05-27 23:43_

---

_Closed by @zanieb on 2025-05-27 23:43_

---

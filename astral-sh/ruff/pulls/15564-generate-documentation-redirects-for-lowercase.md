```yaml
number: 15564
title: Generate documentation redirects for lowercase rule codes
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-redirects
created_at: 2025-01-18T00:52:57Z
updated_at: 2025-01-18T05:15:14Z
url: https://github.com/astral-sh/ruff/pull/15564
synced_at: 2026-01-10T20:05:43Z
```

# Generate documentation redirects for lowercase rule codes

---

_Pull request opened by @InSyncWithFoo on 2025-01-18 00:52_

## Summary

Resolves #15016.

## Test Plan

None.


---

_@charliermarsh approved on 2025-01-18 00:53_

Thanks.

---

_Comment by @InSyncWithFoo on 2025-01-18 00:54_

I couldn't actually verify if this works locally (even before the change), but theoretically it should.

---

_Comment by @github-actions[bot] on 2025-01-18 01:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_@dhruvmanila approved on 2025-01-18 04:38_

Thanks!

I tested it out locally using by generating the docs using:
```console
uv run --with-requirements docs/requirements-insiders.txt scripts/generate_mkdocs.py
```

and, checking whether the mapping was created in `mkdocs.generated.yml` and running the server using:

```console
uvx --with-requirements docs/requirements-insiders.txt -- mkdocs serve -f mkdocs.insiders.yml -o
```

---

_Label `documentation` added by @dhruvmanila on 2025-01-18 04:38_

---

_Merged by @dhruvmanila on 2025-01-18 04:39_

---

_Closed by @dhruvmanila on 2025-01-18 04:39_

---

_Branch deleted on 2025-01-18 05:15_

---

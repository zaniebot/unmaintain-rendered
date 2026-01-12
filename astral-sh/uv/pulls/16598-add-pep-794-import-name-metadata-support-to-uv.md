```yaml
number: 16598
title: Add PEP 794 import name metadata support to uv build backend
type: pull_request
state: open
author: terror
labels: []
assignees: []
base: main
head: pep-794
created_at: 2025-11-05T05:17:58Z
updated_at: 2025-11-19T10:08:41Z
url: https://github.com/astral-sh/uv/pull/16598
synced_at: 2026-01-12T16:12:20Z
```

# Add PEP 794 import name metadata support to uv build backend

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16435

This first pass extends the build backend to parse, validate, and emit `project.import-names` / `project.import-namespaces`, including the new error cases and prefix checks (outlined in the [PEP 794](https://peps.python.org/pep-0794/) document), and bumps metadata output to 2.5 when either field is present. It also expands the shared metadata types so we can round-trip `Import-Name` headers and keeps the publish path tolerant of them.

There are a few follow-up PRs to consider here, namely:
- Threading the new fields through the resolver/database/lock layers so we can surface them in front-end commands.
- Exposing the metadata in user-facing tooling (uv pip show, diagnostics, etc.).
- Considering a linter or helper for computing import names automatically.

I would be willing to get these in here as well, but it may balloon the diff making it harder to review.

---

_Marked ready for review by @terror on 2025-11-05 05:53_

---

_@konstin reviewed on 2025-11-19 10:08_

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:570 on 2025-11-19 10:08_

Note that shipping this is blocked on adding support in warehouse (https://github.com/pypi/warehouse/issues/19083)

---

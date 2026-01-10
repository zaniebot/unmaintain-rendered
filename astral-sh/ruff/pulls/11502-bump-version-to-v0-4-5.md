```yaml
number: 11502
title: "Bump version to `v0.4.5`"
type: pull_request
state: merged
author: snowsignal
labels:
  - release
assignees: []
merged: true
base: main
head: release/045
created_at: 2024-05-23T00:40:55Z
updated_at: 2024-05-23T01:25:26Z
url: https://github.com/astral-sh/ruff/pull/11502
synced_at: 2026-01-10T22:05:26Z
```

# Bump version to `v0.4.5`

---

_Pull request opened by @snowsignal on 2024-05-23 00:40_

_No description provided._

---

_Renamed from "Bump version to v0.4.5" to "Bump version to `v0.4.5`" by @snowsignal on 2024-05-23 00:41_

---

_@snowsignal reviewed on 2024-05-23 00:42_

---

_Review comment by @snowsignal on `CHANGELOG.md`:68 on 2024-05-23 00:42_

@charliermarsh Should we include this, and if so, what would be a good section for it?

---

_@charliermarsh reviewed on 2024-05-23 00:43_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:68 on 2024-05-23 00:43_

I think we stopped doing this

---

_@charliermarsh reviewed on 2024-05-23 00:54_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:68 on 2024-05-23 00:54_

Sorry, I thought this was on the blog post (I saw it on my phone). Lets put this under `Documentation` I guess?

---

_@charliermarsh reviewed on 2024-05-23 00:54_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:58 on 2024-05-23 00:54_

Lets remove this section and "Internal" since it doesn't affect users.

---

_@charliermarsh reviewed on 2024-05-23 00:55_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:8 on 2024-05-23 00:55_

Can we saw something like:

> `ruff server` supports the same feature set as `ruff-lsp`, but with superior performance and no installation required. We'd love your feedback.

Just to match the language we added to the https://github.com/astral-sh/ruff-lsp README and elsewhere.

---

_Review comment by @charliermarsh on `CHANGELOG.md`:50 on 2024-05-23 00:56_

Nit: "Add `--preview` to the README"

---

_@charliermarsh reviewed on 2024-05-23 00:56_

---

_@charliermarsh reviewed on 2024-05-23 00:56_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:8 on 2024-05-23 00:56_

Lets add a second paragraph that's just like:

You can enable `ruff server` in the [VS Code extension](https://github.com/astral-sh/ruff-vscode?tab=readme-ov-file#enabling-the-rust-based-language-server) today.

What do you think?

---

_@snowsignal reviewed on 2024-05-23 00:58_

---

_Review comment by @snowsignal on `CHANGELOG.md`:8 on 2024-05-23 00:58_

Should we mention that `ruff server` has several brand-new features too?

---

_@charliermarsh reviewed on 2024-05-23 00:59_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:8 on 2024-05-23 00:59_

I just want there to be a call-to-action without going through the blog post, since most users won't click through.

---

_@charliermarsh reviewed on 2024-05-23 01:00_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:8 on 2024-05-23 01:00_

Actually:

> `ruff server` supports the same feature set as `ruff-lsp`, powering linting, formatting, and code fixes in Ruff's editor integrations -- but with superior performance and no installation required. We'd love your feedback.

Something to make clear it's not a general LSP?

---

_@snowsignal reviewed on 2024-05-23 01:00_

---

_Review comment by @snowsignal on `CHANGELOG.md`:8 on 2024-05-23 01:00_

Ah, to be clear, this was a question for the first suggestion.

---

_@snowsignal reviewed on 2024-05-23 01:02_

---

_Review comment by @snowsignal on `CHANGELOG.md`:8 on 2024-05-23 01:02_

@charliermarsh That sounds good!

---

_@charliermarsh approved on 2024-05-23 01:05_

---

_Label `release` added by @charliermarsh on 2024-05-23 01:05_

---

_Merged by @snowsignal on 2024-05-23 01:09_

---

_Closed by @snowsignal on 2024-05-23 01:09_

---

_Branch deleted on 2024-05-23 01:09_

---

_Comment by @github-actions[bot] on 2024-05-23 01:25_

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

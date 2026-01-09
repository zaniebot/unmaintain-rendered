---
number: 16660
title: "Add package change summaries to `uv sync` json output"
type: pull_request
state: closed
author: terror
labels:
  - preview
assignees: []
draft: true
base: main
head: package-sync-install-summary
created_at: 2025-11-10T04:50:07Z
updated_at: 2025-12-09T01:22:08Z
url: https://github.com/astral-sh/uv/pull/16660
synced_at: 2026-01-07T13:12:19-06:00
---

# Add package change summaries to `uv sync` json output

---

_Pull request opened by @terror on 2025-11-10 04:50_

Initial work for https://github.com/astral-sh/uv/issues/16653

This PR adds a lightweight install summary to `uv sync --output-format json` by threading the changelog returned from the pip operations into the sync report: the JSON now includes a `sync.packages` array detailing each added, removed, or reinstalled distribution (with version and optional source URL), so users can inspect the plan without parsing stderr.

As stated in https://github.com/astral-sh/uv/issues/16653, this is a preview change. We'll likely need to iterate on this in a few other changes (or keep adding onto this one).

Some ideas:
- Wire this up with `--dry-run`
- Richer context (dependency group, editable/file path, source index, etc)

---

_@zanieb reviewed on 2025-11-10 14:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1299 on 2025-11-10 14:46_

`PackageName` supports serialization, no? We shouldn't resolve to strings here unless we need to.

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1253 on 2025-11-10 14:47_

After looking at it... I wonder if we should call it `changes`? I worry `packages` could be confusing because it doesn't imply it's only the packages that changed

---

_@zanieb reviewed on 2025-11-10 14:47_

---

_@zanieb reviewed on 2025-11-10 14:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:1253 on 2025-11-10 14:48_

Unless we _also_ want to enumerate all the packages that are unchanged here, with `action: check`... hm.

---

_Comment by @zanieb on 2025-11-10 15:02_

What's your plan for dry-run? I think a clear path forward there is important to understand, even if we don't implement it here.

---

_Label `preview` added by @zanieb on 2025-11-10 15:06_

---

_Referenced in [astral-sh/uv#16981](../../astral-sh/uv/pulls/16981.md) on 2025-12-04 15:49_

---

_Comment by @terror on 2025-12-04 16:16_

Closing in favour of https://github.com/astral-sh/uv/pull/16981 :)

---

_Closed by @terror on 2025-12-04 16:16_

---

_Branch deleted on 2025-12-09 01:22_

---

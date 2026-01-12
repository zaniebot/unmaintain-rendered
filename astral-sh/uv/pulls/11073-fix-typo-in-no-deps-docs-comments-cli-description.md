```yaml
number: 11073
title: Fix typo in no-deps docs/comments/cli description
type: pull_request
state: merged
author: pocotiti
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-no-deps-docs
created_at: 2025-01-29T17:36:38Z
updated_at: 2025-01-29T18:03:16Z
url: https://github.com/astral-sh/uv/pull/11073
synced_at: 2026-01-12T16:09:39Z
```

# Fix typo in no-deps docs/comments/cli description

---

_@pocotiti_

## Summary
Fixes a recurring typo.

## Details
There's a typo appearing in a particular sentence...

> Ignore package dependencies, instead only add those packages explicitly listed on the command line to the resulting **the** requirements file.

... used in:
* `crates/uv-cli/src/lib.rs`
* `crates/uv-settings-src-settings.rs`
* `docs/reference/settings.md`
* `uv.schem.json`

Docs, comments and a CLI command description seem affected.

This PR fixes it.


---

_@zanieb approved on 2025-01-29 17:38_

Thank you!

---

_Label `documentation` added by @zanieb on 2025-01-29 17:39_

---

_Comment by @zanieb on 2025-01-29 17:39_

It looks like there's a difference between the generated pages and their sources, can you check on that? `cargo dev generate-all` will regenerate.

---

_Comment by @pocotiti on 2025-01-29 17:44_

> It looks like there's a difference between the generated pages and their sources, can you check on that? `cargo dev generate-all` will regenerate.

Regenerated the docs and there is one indeed. Mea culpa. Should be OK now.

---

_Merged by @zanieb on 2025-01-29 17:55_

---

_Closed by @zanieb on 2025-01-29 17:55_

---

_Branch deleted on 2025-01-29 18:03_

---

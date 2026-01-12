```yaml
number: 1596
title: Use async zip streaming for non-range-request supporting registries
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - registry
assignees: []
created_at: 2024-02-17T15:27:48Z
updated_at: 2024-02-21T03:12:22Z
url: https://github.com/astral-sh/uv/issues/1596
synced_at: 2026-01-12T15:58:30Z
```

# Use async zip streaming for non-range-request supporting registries

---

_@charliermarsh_

For registries that support range requests, we grab the wheel metadata using range requests, which is much faster than downloading the wheel.

For registries that don’t, we fall back to downloading the wheel, and reading the metadata from disk.

Instead, we should be able to stream the wheel in-memory until we get to the METADATA entry, then return it without writing to disk (and without downloading the entire wheel). I don’t know where the METDATA is typically located (e.g., if it’s at the front of the archive, this will be significantly faster; if it’s at the end, it won’t be that different).

---

_Label `enhancement` added by @charliermarsh on 2024-02-17 15:27_

---

_Comment by @zanieb on 2024-02-17 16:32_

Is this a wish or something we think we should do soon?

---

_Comment by @charliermarsh on 2024-02-17 18:35_

I'd like to do it soon since it does affect a lot of registries. I think I know how, I just need to find time to test it.

---

_Comment by @notatallshaw on 2024-02-18 02:38_

btw, I think this is somewhat related work on Pip side: https://github.com/pypa/pip/pull/12208, or at least I think it's related.

I think the author has seen examples of metadata at both the front and back of the zip file.

Sorry if it's not actually that closely related, my mind was triggered from reading this and remembering that PR, but I only have time to skim over things.

---

_Label `index` added by @zanieb on 2024-02-18 07:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 14:58_

---

_Closed by @charliermarsh on 2024-02-21 03:12_

---

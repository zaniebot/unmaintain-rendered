---
number: 16545
title: "Update UV's JSON Schema on JSON Schema Store"
type: issue
state: closed
author: FishAlchemist
labels:
  - enhancement
assignees: []
created_at: 2025-11-01T05:29:25Z
updated_at: 2025-11-02T18:27:15Z
url: https://github.com/astral-sh/uv/issues/16545
synced_at: 2026-01-10T01:26:07Z
---

# Update UV's JSON Schema on JSON Schema Store

---

_Issue opened by @FishAlchemist on 2025-11-01 05:29_

### Summary

The JSON schema for `uv` on SchemaStore is currently outdated.

<img width="192" height="60" alt="Image" src="https://github.com/user-attachments/assets/65e7d101-7905-4787-9de9-d0411282f2cd" />

**Reason for Update:**
1. The popular VS Code extension **Even Better TOML** reads schemas by default from SchemaStore.
2. When [#12994 (comment)](https://github.com/astral-sh/uv/issues/12994#issuecomment-3467997747) was mentioned 


**Note:** I think the community should be able to handle it, since there is already an existing JSON Schema (uv.schema.json).
### Example

_No response_

---

_Label `enhancement` added by @FishAlchemist on 2025-11-01 05:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-01 19:41_

---

_Comment by @charliermarsh on 2025-11-01 19:41_

https://github.com/SchemaStore/schemastore/pull/5091

---

_Comment by @samypr100 on 2025-11-01 19:51_

I wonder if schemastore has some form of automation for keeping schemas up-to-date that we could integrate with or contribute?

---

_Comment by @charliermarsh on 2025-11-01 20:11_

Typically you can have it point to an external file (e.g., the schema we check-in to our repo). But IIRC because we support nested schemas or something, we can’t use that. Specifically, I believe it’s because we want our schema to both work in uv.toml and pyproject.toml.

---

_Closed by @charliermarsh on 2025-11-02 18:27_

---

_Referenced in [astral-sh/uv#12994](../../astral-sh/uv/issues/12994.md) on 2025-11-03 13:28_

---

---
number: 11897
title: "Support `workspace/didChangeConfiguration` for `ruff server`"
type: issue
state: open
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2024-06-17T06:17:16Z
updated_at: 2025-04-03T13:48:10Z
url: https://github.com/astral-sh/ruff/issues/11897
synced_at: 2026-01-07T13:12:15-06:00
---

# Support `workspace/didChangeConfiguration` for `ruff server`

---

_Issue opened by @dhruvmanila on 2024-06-17 06:17_

Tracking issue to add support for `workspace/didChangeConfiguration` for `ruff server`.

Related issue in `ruff-lsp`: https://github.com/astral-sh/ruff-lsp/issues/42

This is a low priority feature for now.

## Todo

- [ ] Implement in https://github.com/astral-sh/ruff/blob/84e87771cba841c08ac7d5d1017050b8bd22837e/crates/ruff_server/src/server/api/notifications/did_change_configuration.rs
- [ ] Update [Neovim documentation](https://github.com/astral-sh/ruff/blob/355d26f05cf2388a3d1b38819def0c4206be8bc1/crates/ruff_server/docs/setup/NEOVIM.md#L4) to suggest using the `settings` key instead of `init_options`

---

_Label `server` added by @dhruvmanila on 2024-06-17 06:17_

---

_Comment by @snowsignal on 2024-06-17 16:30_

I think this is a duplicate of https://github.com/astral-sh/ruff/issues/10797, but I'll close the former since this has a more through description.

---

_Referenced in [astral-sh/ruff#10797](../../astral-sh/ruff/issues/10797.md) on 2024-06-17 16:31_

---

_Added to milestone `Ruff Server: Post-Stable` by @snowsignal on 2024-06-17 16:34_

---

_Assigned to @snowsignal by @snowsignal on 2024-06-18 03:43_

---

_Referenced in [astral-sh/ruff#11217](../../astral-sh/ruff/issues/11217.md) on 2024-06-18 06:03_

---

_Unassigned @snowsignal by @dhruvmanila on 2024-07-10 02:48_

---

_Referenced in [astral-sh/ruff#12344](../../astral-sh/ruff/pulls/12344.md) on 2024-07-18 07:42_

---

_Removed from milestone `Ruff Server: Post-Stable` by @dhruvmanila on 2025-04-03 13:48_

---

```yaml
number: 16590
title: Clarify user-level and system-level configuration file locations
type: pull_request
state: open
author: my1e5
labels:
  - documentation
assignees: []
base: main
head: clarify-config-file-docs
created_at: 2025-11-04T14:57:31Z
updated_at: 2025-11-18T02:50:09Z
url: https://github.com/astral-sh/uv/pull/16590
synced_at: 2026-01-10T05:58:11Z
```

# Clarify user-level and system-level configuration file locations

---

_Pull request opened by @my1e5 on 2025-11-04 14:57_

## Summary

A small tweak to the layout of the configuration file documentation to help visually clarify that macOS/Linux and Windows have different file locations. I accidently misread this section at first and got confused why my Windows PC wasn't respecting `~/.config/uv/uv.toml`. The refactor uses a tabbed layout similar to [Getting Started->Installation ](https://docs.astral.sh/uv/getting-started/installation/).

## Test Plan

Built the docs locally and this is the old/new layout:

### Old

<img width="777" height="211" alt="image" src="https://github.com/user-attachments/assets/87e1a075-c450-45cb-bb28-a8a7446c1749" />

### New

<img width="774" height="288" alt="image" src="https://github.com/user-attachments/assets/ec59c2d5-692b-491b-84e6-0cd06a7fa2dc" />




---

_Review comment by @samypr100 on `docs/concepts/configuration-files.md`:57 on 2025-11-05 03:48_

```suggestion
User-level and system-level configuration must use the `uv.toml` format, rather than the `pyproject.toml`
```

---

_@samypr100 reviewed on 2025-11-05 03:48_

---

_Label `documentation` added by @samypr100 on 2025-11-07 03:01_

---

_Comment by @samypr100 on 2025-11-18 02:50_

@my1e5 see https://github.com/astral-sh/uv/pull/15954


---

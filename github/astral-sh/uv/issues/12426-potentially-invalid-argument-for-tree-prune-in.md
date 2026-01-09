---
number: 12426
title: "Potentially invalid argument for tree --prune in the CLI's help menu"
type: issue
state: closed
author: nickjj
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2025-03-24T11:06:13Z
updated_at: 2025-03-24T15:45:30Z
url: https://github.com/astral-sh/uv/issues/12426
synced_at: 2026-01-07T13:12:18-06:00
---

# Potentially invalid argument for tree --prune in the CLI's help menu

---

_Issue opened by @nickjj on 2025-03-24 11:06_

### Summary

Hi,

`uv tree --help` produces this line:

```
--prune <PRUNE>                      Prune the given package from the display of the dependency tree
```

I believe this should be `<PACKAGE>` not `<PRUNE>` based on the description. I don't know Rust well enough to know where to update this, otherwise I would have opened a PR. If you're up for guidance on what to change, I'd be open to put together a PR for this.

After poking around the code, I do see a bunch of ` prune: Vec<PackageName>` references which indicates prune does expect a package not a "prune".

### Platform

Ubuntu 22.04

### Version

0.6.9

---

_Label `bug` added by @nickjj on 2025-03-24 11:06_

---

_Comment by @charliermarsh on 2025-03-24 13:11_

Yeah that's right. If you look at `ExportArgs` in `crates/uv-cli/src/lib.rs`, you can add a `value_name`.

---

_Label `good first issue` added by @charliermarsh on 2025-03-24 13:11_

---

_Label `help wanted` added by @charliermarsh on 2025-03-24 13:11_

---

_Referenced in [astral-sh/uv#12432](../../astral-sh/uv/pulls/12432.md) on 2025-03-24 15:30_

---

_Comment by @nickjj on 2025-03-24 15:33_

Thanks for the tip!

---

_Closed by @charliermarsh on 2025-03-24 15:45_

---

_Closed by @charliermarsh on 2025-03-24 15:45_

---

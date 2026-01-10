```yaml
number: 14985
title: "Environment removal should use `remove_virtualenv`"
type: issue
state: open
author: zanieb
labels:
  - good first issue
  - internal
assignees: []
created_at: 2025-07-31T02:27:11Z
updated_at: 2025-10-05T10:53:41Z
url: https://github.com/astral-sh/uv/issues/14985
synced_at: 2026-01-10T03:23:54Z
```

# Environment removal should use `remove_virtualenv`

---

_Issue opened by @zanieb on 2025-07-31 02:27_

at https://github.com/astral-sh/uv/blob/6856a27711910d240a488d53d7967b73340b1524/crates/uv/src/commands/project/mod.rs#L1575 we're not using the `remove_virtualenv` which should always be used for removing virtual environment directories

---

_Label `good first issue` added by @zanieb on 2025-07-31 02:27_

---

_Label `internal` added by @zanieb on 2025-07-31 02:27_

---

_Comment by @zanieb on 2025-07-31 02:28_

same at https://github.com/astral-sh/uv/blob/bfb4bc2aebe1dbff9aeba84d0c96e3ffcbf0ddd1/crates/uv-tool/src/lib.rs#L266

---

_Renamed from "Script environment removal should use `remove_virtualenv`" to "Environment removal should use `remove_virtualenv`" by @zanieb on 2025-07-31 02:32_

---

_Comment by @zanieb on 2025-07-31 02:34_

I found these while auditing some `uv_virtualenv::OnExisting::Remove` uses for #14734 and looks like we often remove the virtual environment manually beforehand. I'm... not actually sure we should be doing that? because we handle that case inside `create_venv` which always uses `remove_virtualenv`.


---

_Comment by @Zaloog on 2025-07-31 20:18_

Hi @zanieb I made a PR for this one, as it looked pretty straight forward.

Should I write something here before starting to work on that for the next time?
Best regards


---

_Comment by @zanieb on 2025-07-31 20:19_

Usually it's fine to start with a pull request for small tasks. If it's bigger, it can good to put a heads up just so nobody else duplicates work on it. There's no need to ask for permission on anything that has a `help wanted` or `good first issue` tag though.

---

_Comment by @VINAYAK-N-MAGAJIKONDI on 2025-08-01 08:29_

> at
> 
> [uv/crates/uv/src/commands/project/mod.rs](https://github.com/astral-sh/uv/blob/6856a27711910d240a488d53d7967b73340b1524/crates/uv/src/commands/project/mod.rs#L1575)
> 
> Line 1575 in [6856a27](/astral-sh/uv/commit/6856a27711910d240a488d53d7967b73340b1524)
> 
>  let replaced = match fs_err::remove_dir_all(&root) { 
> we're not using the `remove_virtualenv` which should always be used for removing virtual environment directories

Iam working on it. assign it to me. Thank You


---

_Comment by @VINAYAK-N-MAGAJIKONDI on 2025-08-01 08:32_

my bad i had not seen the latest update .



---

_Comment by @Zaloog on 2025-08-04 09:30_

Hi Zanie,
is this one completed with #15007, or should I add something else? 

---

_Comment by @MitchellBerend on 2025-10-05 10:53_

Hello, is this issue still relevant? I looked around in the codebase for `::remove_dir` and I found some hits that might still need to be migrated.

https://github.com/astral-sh/uv/blob/00d3aa3780816f5b3da40c0815579ca359653de1/crates/uv-tool/src/lib.rs#L245-L263
---

These still use `fs::remove_dir_all` instead of `fs_err::remove_dir_all` it's maybe not relevant to this issue but I didn't understand if it is by choice or not.

https://github.com/astral-sh/uv/blob/00d3aa3780816f5b3da40c0815579ca359653de1/crates/uv-install-wheel/src/install.rs#L116-L136

https://github.com/astral-sh/uv/blob/00d3aa3780816f5b3da40c0815579ca359653de1/crates/uv-install-wheel/src/uninstall.rs#L69-L86

https://github.com/astral-sh/uv/blob/00d3aa3780816f5b3da40c0815579ca359653de1/crates/uv-install-wheel/src/uninstall.rs#L107-L133

https://github.com/astral-sh/uv/blob/00d3aa3780816f5b3da40c0815579ca359653de1/crates/uv-virtualenv/src/virtualenv.rs#L612-L632
---

I think `fs_err` is a drop in replacement and having nice errors when something goes wrong with the filesystem nice to have. Not sure if this affects anything performance wise though. I can open an issue for this (and a pr) if that is something that might add some value.

---

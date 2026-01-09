---
number: 1326
title: "`uv venv` does not suggest how to activate"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - virtualenv
assignees: []
created_at: 2024-02-15T19:05:15Z
updated_at: 2024-02-20T12:39:43Z
url: https://github.com/astral-sh/uv/issues/1326
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv venv` does not suggest how to activate

---

_Issue opened by @zanieb on 2024-02-15 19:05_

```
❯ uv venv
Using Python 3.11.7 interpreter at /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: .venv

❯ source .venv/bin/activate
```

We should prompt activation

---

_Label `enhancement` added by @zanieb on 2024-02-15 19:05_

---

_Comment by @zanieb on 2024-02-15 19:06_

`python -m venv` is silent; `virtualenv` is verbose but not helpful

```
❯ virtualenv foo
created virtual environment CPython3.12.0.final.0-64 in 250ms
  creator CPython3Posix(dest=/Users/mz/eng/src/astral-sh/uv/foo, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, via=copy, app_data_dir=/Users/mz/Library/Application Support/virtualenv)
    added seed packages: pip==23.3.2
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
```

---

_Referenced in [astral-sh/uv#1386](../../astral-sh/uv/issues/1386.md) on 2024-02-15 23:39_

---

_Comment by @0v00 on 2024-02-16 01:31_

@zanieb For prompting activation, would it be sufficient to add the following to `crates/uv/src/commands/venv.rs` inside of `venv_impl`?

```rust
    writeln!(
        printer,
        "Activate virtualenv: source {}/bin/activate",
        path.normalized_display().cyan()
    )
    .into_diagnostic()?;
```

Happy to make a small PR/update tests if so.

---

_Comment by @zanieb on 2024-02-16 01:54_

I think so! Maybe styled like "Activate with `source ....`"?

---

_Comment by @zanieb on 2024-02-16 14:49_

Note we also need a separate hint to Windows users

---

_Referenced in [astral-sh/uv#1580](../../astral-sh/uv/pulls/1580.md) on 2024-02-17 10:03_

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:39_

---

_Closed by @AlexWaygood on 2024-02-20 12:39_

---

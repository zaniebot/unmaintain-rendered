---
number: 4823
title: "`uv tool list -v` should show paths"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-07-05T07:55:45Z
updated_at: 2024-07-17T23:11:14Z
url: https://github.com/astral-sh/uv/issues/4823
synced_at: 2026-01-10T01:23:41Z
---

# `uv tool list -v` should show paths

---

_Issue opened by @konstin on 2024-07-05 07:55_

It would be great if `uv tool list` would have a verbose mode that shows paths, maybe reusing `-v`, maybe something like `--paths`.

Current output:

```
$ cargo run -q tool list -v --preview
DEBUG uv 0.2.21
DEBUG Acquired lock for `/home/konsti/.local/share/uv/tools`
warning: Ignoring malformed tool `interpreter-v2`: missing receipt
Python interpreter not found at `/home/konsti/.local/share/uv/tools/azure-cli/bin/python3`
black v24.4.2
    black
    blackd
```

Desired output
```
$ cargo run -q tool list -v --preview
DEBUG uv 0.2.21
DEBUG Acquired lock for `/home/konsti/.local/share/uv/tools`
warning: Ignoring malformed tool `interpreter-v2`: missing receipt
Python interpreter not found at `/home/konsti/.local/share/uv/tools/azure-cli/bin/python3`
black v24.4.2 (/home/konsti/.local/share/uv/tools/black/)
    black (/home/konsti/.local/bin/black)
    blackd (/home/konsti/.local/bin/blackd)
```



---

_Label `enhancement` added by @konstin on 2024-07-05 07:55_

---

_Label `preview` added by @konstin on 2024-07-05 07:55_

---

_Comment by @zanieb on 2024-07-05 15:43_

Seems kind of nice, but won't the paths be mostly redundant since they're all going to be in the same bin directory?

---

_Comment by @konstin on 2024-07-05 15:45_

If we just show the data and the bin directory that's fine too

---

_Referenced in [astral-sh/uv#4867](../../astral-sh/uv/issues/4867.md) on 2024-07-07 21:04_

---

_Comment by @charliermarsh on 2024-07-08 01:50_

`uv tool dir` shows the directory into which we _install_ the tools. But it should probably show the bin directory...?

---

_Comment by @zanieb on 2024-07-08 02:02_

That might make more sense although we're calling the install directory the "uv tool directory" whereas the bin directory is the "tool executable directory". Maybe we need a toggle? Maybe we should have two commands with clearer names?

---

_Comment by @charliermarsh on 2024-07-17 02:35_

Any further thoughts here? I would be ok showing the paths with `-v` but it feels "surprising".

---

_Comment by @zanieb on 2024-07-17 17:28_

I think `uv tool dir` should have a `--bin` flag that shows the binary directory instead?

Regarding paths of tools, Cargo doesn't show them

```
❯ cargo install --list -v
cargo-dist v0.17.0:
    cargo-dist
cargo-insta v1.38.0:
    cargo-insta
cargo-nextest v0.9.72:
    cargo-nextest
cargo-shear v0.0.26:
    cargo-shear
hyperfine v1.18.0:
    hyperfine
mdbook v0.4.40:
    mdbook
ripgrep v14.1.0:
    rg
typos-cli v1.22.4:
    typos
```

I'd be okay with showing the paths with `-v` or a `--show-paths` flag. In this case, I'd think we'd show the paths for both the executables and the environment e.g.

```
typos-cli v1.22.4 (/Users/zb/.local/tools/typos-cli):
    typos (/Users/zb/.local/bin/typos)
```



---

_Comment by @charliermarsh on 2024-07-17 17:32_

Okay, I'm fine with `--bin` and `--show-paths`.

---

_Comment by @charliermarsh on 2024-07-17 17:32_

I can do them

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 17:32_

---

_Referenced in [astral-sh/uv#5164](../../astral-sh/uv/pulls/5164.md) on 2024-07-17 18:27_

---

_Closed by @charliermarsh on 2024-07-17 23:11_

---

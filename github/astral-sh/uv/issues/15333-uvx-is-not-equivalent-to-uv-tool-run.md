---
number: 15333
title: "`uvx` is not equivalent to `uv tool run`"
type: issue
state: closed
author: RafalSkolasinski
labels:
  - question
assignees: []
created_at: 2025-08-17T21:25:40Z
updated_at: 2025-08-18T14:54:37Z
url: https://github.com/astral-sh/uv/issues/15333
synced_at: 2026-01-07T13:12:19-06:00
---

# `uvx` is not equivalent to `uv tool run`

---

_Issue opened by @RafalSkolasinski on 2025-08-17 21:25_

### Question

When reading the [docs](https://docs.astral.sh/uv/guides/tools/) I came across
```
$ uvx ruff
This is exactly equivalent to:
$ uv tool run ruff
uvx is provided as an alias for convenience.
```

But that does not to seem to be fully the case. At least not after running `uv tool install ...` command.

I have been experimenting with IPython and I do get following:

1. First `uvx` - seems it runs from temporary cache environment. 
```bash
uvx ipython -c 'import IPython; print(IPython.__file__)'   
/Users/rskolasinski/.cache/uv/archive-v0/5mledyZnvLs75xlCg-2oW/lib/python3.13/site-packages/IPython/__init__.py
```

2. Using `uv tool run` - after installing the tool - seems to run from tool location
```bash
uv tool run ipython -c 'import IPython; print(IPython.__file__)' 
/Users/rskolasinski/.local/share/uv/tools/ipython/lib/python3.13/site-packages/IPython/__init__.py
```

and the same if I just run ipython now
```bash
ipython -c 'import IPython; print(IPython.__file__)'         
/Users/rskolasinski/.local/share/uv/tools/ipython/lib/python3.13/site-packages/IPython/__init__.py
```

Note: this is what lead me to thinking that `uvx` and is not really an alias for `uv tool run ...`...
I also tried without the tool installed:

3. Indeed when `ipython` is not installed as a tool I get the same as in 1.
```bash
uv tool uninstall ipython
...

uv tool run ipython -c 'import IPython; print(IPython.__file__)'
/Users/rskolasinski/.cache/uv/archive-v0/5mledyZnvLs75xlCg-2oW/lib/python3.13/site-packages/IPython/__init__.py
```

---

TLDR; it does seem that when a tool is installed then `uvx` is not 100% just an alias to `uv tool run` as stated in docs. It'd be great to clarify this.

### Platform

macOS 15.6 arm64 (Darwin 24.6.0 arm64)

### Version

uv 0.8.8 (9a54754b0 2025-08-08)

---

_Label `question` added by @RafalSkolasinski on 2025-08-17 21:25_

---

_Comment by @charliermarsh on 2025-08-18 13:44_

I believe it’s literally an alias. Or, specifically, uvx is a separate script that just calls uv tool run. Do which uv and which uvx show different locations, perhaps? (I’m not at a computer but will try to reproduce later.)

---

_Comment by @zanieb on 2025-08-18 13:49_

`uvx` does literally invoke `uv tool uvx` 

https://github.com/astral-sh/uv/blob/041c7a5e63d5559ce5571a30e177c072b1b357b2/crates/uv/src/bin/uvx.rs#L87-L96

which runs the same implementation as `uv tool run`

https://github.com/astral-sh/uv/blob/23245c63e9d63c4916328060aed3f45c03a11a9b/crates/uv/src/lib.rs#L1113-L1115

just using the `uvx` component of the command to track that `uvx` was used to improve error messages, e.g.,

https://github.com/astral-sh/uv/blob/58c7cc0e0f10c0c8a904d5276c40c81c6eccd776/crates/uv/src/commands/tool/run.rs#L214-L225

---

_Comment by @zanieb on 2025-08-18 13:51_

I cannot reproduce the behavior

```
❯ uvx ipython -c 'import IPython; print(IPython.__file__)'
/private/tmp/example/cache/archive-v0/-3NtPdstD88xs5BnNrCPM/lib/python3.13/site-packages/IPython/__init__.py
❯ uv tool run ipython -c 'import IPython; print(IPython.__file__)'
/private/tmp/example/cache/archive-v0/-3NtPdstD88xs5BnNrCPM/lib/python3.13/site-packages/IPython/__init__.py
❯ uv tool install ipython
Resolved 16 packages in 14ms
Installed 16 packages in 27ms
 + asttokens==3.0.0
 + decorator==5.2.1
 + executing==2.2.0
 + ipython==9.4.0
 + ipython-pygments-lexers==1.1.1
 + jedi==0.19.2
 + matplotlib-inline==0.1.7
 + parso==0.8.4
 + pexpect==4.9.0
 + prompt-toolkit==3.0.51
 + ptyprocess==0.7.0
 + pure-eval==0.2.3
 + pygments==2.19.2
 + stack-data==0.6.3
 + traitlets==5.14.3
 + wcwidth==0.2.13
Installed 2 executables: ipython, ipython3
❯ uvx ipython -c 'import IPython; print(IPython.__file__)'
/Users/zb/.local/share/uv/tools/ipython/lib/python3.13/site-packages/IPython/__init__.py
❯ uv tool run ipython -c 'import IPython; print(IPython.__file__)'
/Users/zb/.local/share/uv/tools/ipython/lib/python3.13/site-packages/IPython/__init__.py
```

Maybe share the output of

```
❯ which uv
/Users/zb/.local/bin/uv
❯ which uvx
/Users/zb/.local/bin/uvx
❯ uvx --version
uvx 0.8.11 (f892276ac 2025-08-14)
❯ uv --version
uv 0.8.11 (f892276ac 2025-08-14)
```

to see if they're using different uv binaries?

---

_Comment by @RafalSkolasinski on 2025-08-18 14:54_

Actually, what I documented is consistent with your test. In notes (well, the issue above -) I am missing case with `uvx` without the tool installed. When I am testing it now I do have consistent behaviour for both:

```
# when tool installed
/Users/rskolasinski/.local/share/uv/tools/ipython/lib/python3.13/site-packages/IPython/__init__.py

# when uninstalled
/Users/rskolasinski/.cache/uv/archive-v0/5mledyZnvLs75xlCg-2oW/lib/python3.13/site-packages/IPython/__init__.py
```

I would swear I had the case when `uvx` and `uv tool run` was giving me different output without running install/uninstall in between but also cannot reproduce it now...



---

_Closed by @RafalSkolasinski on 2025-08-18 14:54_

---

_Comment by @RafalSkolasinski on 2025-08-18 14:54_

Thanks for double checking @zanieb!

---

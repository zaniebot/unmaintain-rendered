```yaml
number: 13571
title: "`uv pip install --group` fails when used with a subfolder"
type: issue
state: open
author: Artalus
labels:
  - error messages
assignees: []
created_at: 2025-05-21T11:44:33Z
updated_at: 2025-05-21T17:08:15Z
url: https://github.com/astral-sh/uv/issues/13571
synced_at: 2026-01-12T16:01:32Z
```

# `uv pip install --group` fails when used with a subfolder

---

_@Artalus_

### Summary

Was demonstrating `dependency-groups` to a colleague, and got a bit confused.
`uv pip install ./subdir` works fine - yet if you do `uv pip install ./subdir --group x`, it fails with `error: File not found: pyproject.toml`.
If I do `cd ./subdir` and then `uv pip install . --group x`, it works as expected. Same if `-e .` is used instead of `.`, and even if I use just `uv pip install --group x`
`uv pip install --help` suggests that I probably misunderstood something, as it indicates that  _either_ `PACKAGE` _or_ `--group <GROUP>` should be used:
```
Usage: uv pip install [OPTIONS] <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>|--group <GROUP>>
Arguments:
  [PACKAGE]...  Install all listed packages
Options:
      --group <GROUP>                          Install the specified dependency group from a `pyproject.toml`
```
Given the novelty of PEP-735 and the engaging discussion on supporting it in pip, I am a bit unsure whether it's actually a bug, or if I should do more reading to understand the intention behind the PEP and/or this feature.

---

Full MRE:

```
[artalus@idpad uv]$ uv --version
uv 0.7.6

[artalus@idpad uv]$ uv venv
Using CPython 3.13.2 interpreter at: /usr/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

[artalus@idpad uv]$ uv init ./foo
Initialized project `foo` at `/tmp/uv/foo`

[artalus@idpad uv]$ echo -e '\n[dependency-groups]\ndev=["mypy"]' >> foo/pyproject.toml

[artalus@idpad uv]$ uv pip install -e ./foo
...
 + foo==0.1.0 (from file:///tmp/uv/foo)

[artalus@idpad uv]$ uv pip install -e ./foo --group dev
error: File not found: `pyproject.toml`

[artalus@idpad uv]$ cd foo

[artalus@idpad foo]$ uv pip install -e . --group dev
Using Python 3.13.2 environment at: /tmp/uv/.venv
...
 ~ foo==0.1.0 (from file:///tmp/uv/foo)
 + mypy==1.15.0
 + mypy-extensions==1.1.0
 + typing-extensions==4.13.2
```

### Platform

MacOS, Windows, Manjaro Linux

### Version

uv 0.7.6

### Python version

Python 3.13.2

---

_Label `bug` added by @Artalus on 2025-05-21 11:44_

---

_Comment by @charliermarsh on 2025-05-21 11:58_

\cc @Gankra 

---

_Comment by @Gankra on 2025-05-21 12:24_

This is unfortunately the behaviour that `pip` itself implements: the full syntax is `--group path:groupname`, and when `path:` is omitted it's implicitly `./pyproject.toml`. This is because pip does not want to be project aware. Also the `--group` flags are essentially completely independent directives to grab a dependency-group from that file, similar to passing paths to multiple requirements.txt files. So the fact that you passed `./subdir` doesn't really change anything (on the other hand `uv pip install --directory ./subdir . --group x` would do what you want).

We intentionally opted to be compatible with this behaviour even though it's not really great for ux and we have better info to do a nicer thing. We really need to give a better diagnostic though.

---

_Comment by @Artalus on 2025-05-21 12:27_

Doing some follow-up reading on #11686, and Gankra correctly predicted this exact ticket 😁 

> case (e) below may get complaints from people who aren't well-versed in dependency-groups-as-they-pertain-to-wheels.
> --8<--
> d) `uv pip install mypackage --group path/to/pyproject.toml:mygroup` much like multiple instances of `--group` the two requests here are essentially completely independent: pleases install `mypackage`, and please also install `path/to/pyproject.toml:mygroup`.
>
> e) `uv pip install mypackage --group mygroup` is exactly the same, but this is where it becomes possible for someone to be a little confused, as you might think `mygroup` is supposed to refer to `mypackage` in some way (it can't). But no, it's sourcing `pyproject.toml:mygroup` from the current working directory.

I suppose we got even extra confused, since `--editable` (which I used in MRE as well as in the original talk with my colleague) is not a flag but _also_ takes a parameter; ` -e, --editable <EDITABLE>  Install the editable package based on the provided local file path`. So I completely mis-parsed the original expression of `uv pip install -e ./foo --group dev` 😅 

And since the current behavior is intentional (if maybe not satisfying for every party involved), I'd myself settle on just a better diagnostic.

---

_Label `bug` removed by @Gankra on 2025-05-21 17:08_

---

_Label `error messages` added by @Gankra on 2025-05-21 17:08_

---

_Assigned to @Gankra by @Gankra on 2025-05-21 17:08_

---

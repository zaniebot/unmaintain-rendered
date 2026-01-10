---
number: 1331
title: "Provide `pip` -> `uv pip` alias in virtual environments"
type: issue
state: closed
author: vitalik
labels:
  - enhancement
assignees: []
created_at: 2024-02-15T20:22:30Z
updated_at: 2025-04-01T17:55:47Z
url: https://github.com/astral-sh/uv/issues/1331
synced_at: 2026-01-10T01:23:05Z
---

# Provide `pip` -> `uv pip` alias in virtual environments

---

_Issue opened by @vitalik on 2024-02-15 20:22_

mabye when uv creates virtualenv it should also create "symlink" for pip to `uv pip` ?

```
$ uv venv
...

$ uv pip install ...
...

$ source .venv/bin/activate
...

$ pip freeze
uv==0.1.0

# ^ this is a bit unexpected  <---

$ uv pip freeze
annotated-types==0.6.0
asgiref==3.7.2
certifi==2024.2.2
charset-normalizer==3.3.2
django==5.0.2
django-ninja==1.1.0
idna==3.6
pydantic==2.6.1
pydantic-core==2.16.2
requests==2.31.0
sqlparse==0.4.4
typing-extensions==4.9.0
urllib3==2.2.0
```

```
$ which pip
/Users/vitaliy/.pyenv/shims/pip #  global (unexpected)

$ which python
/private/tmp/test/.venv/bin/python # from venv (good)
```

---

_Comment by @zanieb on 2024-02-15 20:24_

Hi! That's kind of a fun idea! I'm a little hesitant to step on the `pip` namespace _automatically_, but maybe there's something here.

You can seed `pip` if you want with `uv venv --seed` but of course we think `uv pip` is better to use :)

---

_Label `enhancement` added by @zanieb on 2024-02-15 20:24_

---

_Renamed from "pip freeze" to "Provide `pip` -> `uv pip` alias in virtual environments" by @zanieb on 2024-02-15 20:25_

---

_Comment by @vitalik on 2024-02-15 20:28_

but this is what default `python -m venv` does - it creates a bin/pip which looks like this:

```
#!/path/to/your/.venv/bin/python
# -*- coding: utf-8 -*-
import re
import sys
from pip._internal.cli.main import main
if __name__ == '__main__':
    sys.argv[0] = re.sub(r'(-script\.pyw|\.exe)?$', '', sys.argv[0])
    sys.exit(main())
```

while uv does not do that - so now if I accidentally run "pip install something" it will be installed globally

---

_Comment by @zanieb on 2024-02-15 20:31_

Yeah I that's fair — I think #1330 may address a small part of that concern but I like your idea too.

---

_Comment by @pfmoore on 2024-02-15 21:17_

(Pip maintainer here) I'd be strongly against this because `pip` and `uv pip` have different command sets and behaviours. It would be far too easy to end up with users reporting issues with pip when they are actually using `uv pip`.

---

_Comment by @vitalik on 2024-02-15 21:33_

@pfmoore @zanieb ok... so maybe `--seed` should be enabled by default ?

I think it would be the most annoying thing - activating venv and then installing stuff to global with global pip

---

_Comment by @jgauth on 2024-02-15 22:57_

+1 that the default behavior is quite confusing/unexpected:

```bash
$ uv venv -p 3.11

Using Python 3.11.8 interpreter at /opt/Python3.11.8/bin/python3.11
Creating virtualenv at: .venv

$ uv pip freeze
# no output -- good, no packages have been installed to this venv

$ source .venv/bin/activate

$ pip freeze
# outputs all the packages installed on my system installation, even with venv active
annotated-types==0.6.0
ansible-base==2.10.8
...

$ which pip
/home/jgauthier/.local/bin/pip
# venv doesn't change what `pip` I'm using
```
compared to what I was expecting, based on how `python3 -m venv` works:
```
$ python3.11 -m venv .venv

$ source .venv/bin/activate


$ pip freeze
# no output (no packages installed)

$ which pip
/home/jgauthier/.local/bin/pip
# venv _does_ change what `pip` I'm using
```

`--seed` does fix this, but not having it default seems likely to lead to confusion, and possibly even people installing packages to the system, when they think they're installing to the venv e.g.:
```
$ uv venv -p 3.11
Using Python 3.11.8 interpreter at /opt/Python3.11.8/bin/python3.11
Creating virtualenv at: .venv

$ source .venv/bin/activate

$ pip install redis
# oh no, this installed to system packages
```
This ^ could be quite destructive if you're unlucky / not careful

---

_Comment by @woutervh on 2024-02-15 22:59_

IMHO uv should be symlink into the virtualenv, and operate automatically in that virtualenv

now, I'm stuck if you don't activate  the venv  (whould should be optional):
```
> uv venv foo
> cd foo

# this currently fails
> uv pip install pytest
    error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.

# hopefully this works in the future
> bin/uv pip install pytest
```




---

_Comment by @pfmoore on 2024-02-15 23:11_

Maybe the correct approach is to not call the `uv` subcommand `uv pip` in the first place? Then the possibility of confusing the two is substantially reduced.

---

_Comment by @MithicSpirit on 2024-02-15 23:46_

> while uv does not do that - so now if I accidentally run "pip install something" it will be installed globally

I recommend using `alias pip='python -m pip'` globally (i.e., in your shell's rc file) so that it always uses the pip module from the current python environment (and fails if one does not exist).


---

_Comment by @pfmoore on 2024-02-15 23:58_

There's also the [zipapp distribution of pip](https://pip.pypa.io/en/stable/installation/#standalone-zip-application) which will get executed by the active Python environment.

The reality here, though, is that pip is designed to be installed in every environment. That's a historical consequence of a number of things, and it's not ideal, but it's really hard to change. And there's never been any real incentive to do so, as there's never been any viable alternative to pip until now. Simply by *being* an alternative to pip, `uv` is going to need to address some of the consequences of disrupting the status quo, where it's expected that you can always run an installer via `subprocess.run([sys.executable, "-m", "pip", ...])`, or `/path/to/env/python -m pip`. The problem is wider than simply adding a `pip` shim that runs `uv`.

There's a discussion at https://discuss.python.org/t/pip-plans-to-introduce-an-alternative-zipapp-deployment-method/17431 which adds some context to all of this.

---

_Comment by @zanieb on 2024-02-16 00:28_

There are some [very fair concerns](https://discuss.python.org/t/uv-another-rust-tool-written-to-replace-pip/46039/10) about this idea. I think we are unlikely to do it; solving this is going to require more discussion and consideration.

---

_Comment by @HarlanHeilman on 2024-02-16 04:02_

Perhaps a solution would be a `uv init` that works similarly to the zoxide init https://github.com/ajeetdsouza/zoxide (If you are unfamiliar, it allows you to map over the cd command). This way the user can choose "at their own risk" to map over pip and venv with uv's implementation. 

---

_Comment by @pfmoore on 2024-02-16 08:57_

I’m not sure why there is any need to overwrite the pip command at all. In spite of the current name, `uv pip` and `pip` are completely separate and independent commands/tools. I don’t think we want `uv pip` to be constrained to follow pip’s interface, for example - it already has some options pip doesn’t have, and omits some pip functionality that it may never want to add.

---

_Comment by @woutervh on 2024-02-16 11:37_

maybe rename "uv pip" to "uv rip"  :)

---

_Comment by @pfmoore on 2024-02-16 12:03_

All the good names [are taken](https://prefix.dev/blog/introducing_rip), aren't they? :-)

---

_Comment by @layday on 2024-02-16 14:21_

Uh, `uv pipish`? It's pip...ish 🤯 

---

_Referenced in [astral-sh/uv#1657](../../astral-sh/uv/issues/1657.md) on 2024-02-18 17:25_

---

_Comment by @zanieb on 2024-02-18 17:25_

I do not think we're going to do this.

See instead:

- https://github.com/astral-sh/uv/issues/1632
- https://github.com/astral-sh/uv/issues/1657


---

_Closed by @zanieb on 2024-02-18 17:25_

---

_Referenced in [astral-sh/uv#6593](../../astral-sh/uv/issues/6593.md) on 2024-08-25 14:07_

---

_Referenced in [astral-sh/uv#7534](../../astral-sh/uv/issues/7534.md) on 2024-09-19 09:55_

---

_Referenced in [astral-sh/uv#10949](../../astral-sh/uv/issues/10949.md) on 2025-01-25 16:23_

---

_Referenced in [astral-sh/uv#9452](../../astral-sh/uv/issues/9452.md) on 2025-01-25 16:24_

---

_Referenced in [astral-sh/uv#11550](../../astral-sh/uv/issues/11550.md) on 2025-02-16 05:31_

---

_Referenced in [astral-sh/uv#12335](../../astral-sh/uv/issues/12335.md) on 2025-03-20 13:15_

---

_Referenced in [astral-sh/uv#12604](../../astral-sh/uv/issues/12604.md) on 2025-04-01 17:54_

---

_Comment by @zanieb on 2025-04-01 17:55_

I've opened another tracking issue for this concept (with some context on our understanding of the experience here): https://github.com/astral-sh/uv/issues/12604

---

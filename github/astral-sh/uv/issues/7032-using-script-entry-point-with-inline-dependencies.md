---
number: 7032
title: Using script entry point with inline dependencies 
type: issue
state: open
author: samsja
labels:
  - cli
assignees: []
created_at: 2024-09-04T16:07:24Z
updated_at: 2024-12-30T05:57:59Z
url: https://github.com/astral-sh/uv/issues/7032
synced_at: 2026-01-07T13:12:17-06:00
---

# Using script entry point with inline dependencies 

---

_Issue opened by @samsja on 2024-09-04 16:07_

First thanks for the great work on uv !


I have a python script that need pytorch. The dependency is defined inside the python script using the inline uv [syntax](https://docs.astral.sh/uv/guides/scripts/?query=script+dependencies#declaring-script-dependencies. The problem is that my entry point to run my code is not `python myscript.py` but `torchrun myscript.py` where torchrun is a script entrypoint install with pytorch. 

It's an egg and chicken problem because pytorch is not installed so the command will fail, but it won't install pytorch unless I can run the script ...

I would expect to do something like:

```bash
uv run --with -R myscript.py torchrun myscript.py
```

that would read and install the dependencies from `myscript.py`. I could eventually distribute a separate requirement.txt but It some much cleaner in some cases to only share one file.

Thanks in advance :pray: 

---

_Comment by @zanieb on 2024-09-04 16:14_

I think what you're looking for is a `--script-runner` option https://github.com/astral-sh/uv/issues/6542#issuecomment-2307709992

---

_Comment by @samsja on 2024-09-04 16:51_

> I think what you're looking for is a `--script-runner` option [#6542 (comment)](https://github.com/astral-sh/uv/issues/6542#issuecomment-2307709992)

oh yes exactly what I am looking for !

---

_Comment by @zanieb on 2024-09-04 17:03_

Awesome. Yeah I guess we have two options here... `uv run --script-runner torchrun myscript.py` or `uv run --with-requirements myscript.py torchrun myscript.py`

@charliermarsh thoughts?

The latter is nice and general. People have been asking for the ability to extract requirements from a script.

---

_Comment by @charliermarsh on 2024-09-04 17:07_

The latter seems reasonable, but should it be a dedicated flag? Like `--with-script-requirements`? I know that's long, but otherwise we have to do extension sniffing, and then it doesn't extend to scripts that (for whatever reason) don't end in a known extension.

---

_Comment by @zanieb on 2024-09-04 17:09_

Don't we already do extension sniffing for `--with-requirements pyproject.toml` (and for scripts in `uv run`?). Definitely hear what you're saying, but it seems okay to have it "just work"? `--with-script-requirements` is also pretty reasonable as an explicit option.

---

_Label `cli` added by @zanieb on 2024-09-04 17:10_

---

_Comment by @charliermarsh on 2024-09-04 17:11_

We do match `pyproject.toml` exactly, I think that's ok because it's a standardized file name. I guess it's ok to just sniff `--with-requirements foo.py`, just worried about edge cases. Do we also detect the embedded settings?

---

_Comment by @zanieb on 2024-09-04 17:14_

It seems safest to do the explicit option. We can always add sniffing later. Eek settings... we probably should? I don't love the complexity but `uvx --script-runner pytorch script.py` would definitely respect the settings.

---

_Comment by @sigridjineth on 2024-12-30 05:57_

any update here, for introducing script-runner here?

---

_Referenced in [astral-sh/uv#10381](../../astral-sh/uv/issues/10381.md) on 2025-01-08 04:49_

---

_Referenced in [astral-sh/uv#12763](../../astral-sh/uv/pulls/12763.md) on 2025-04-09 00:55_

---

_Referenced in [astral-sh/uv#15296](../../astral-sh/uv/issues/15296.md) on 2025-11-16 09:48_

---

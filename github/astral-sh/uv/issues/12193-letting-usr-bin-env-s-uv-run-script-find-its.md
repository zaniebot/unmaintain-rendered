---
number: 12193
title: "Letting `#!/usr/bin/env -S uv run` script find its project even if run from outside the project"
type: issue
state: open
author: andersk
labels:
  - enhancement
assignees: []
created_at: 2025-03-15T19:54:48Z
updated_at: 2025-07-08T16:45:27Z
url: https://github.com/astral-sh/uv/issues/12193
synced_at: 2026-01-07T13:12:18-06:00
---

# Letting `#!/usr/bin/env -S uv run` script find its project even if run from outside the project

---

_Issue opened by @andersk on 2025-03-15 19:54_

### Summary

I have a script `script.py` with package requirements listed in `pyproject.toml` and `uv.lock`. A `#!/usr/bin/env -S uv run` shebang line works well for this workflow:

```console
$ git clone https://github.com/some/project
# cd project
$ ./script.py
```

But I want to enable this workflow:

```console
$ git clone https://github.com/some/project
$ project/script.py
```

Unfortunately, `uv run` locates the project configuration from the process’s current working directory, not from the location of the script. Similarly, if I provide a relative path to options like `--directory`, `--project`, and `--config-file`, they’re parsed relative to the process’s current working directory. This can’t work if the current working directory is outside the project, and we don’t know in advance what path the project is checked out to.

I could use PEP 723 metadata and `script.py.lock`, but then I need to maintain separate lock files and get separate ephemeral virtual environments for all of these scripts in my project.

Am I missing another good strategy? I think my preferred resolution would be to fix `uv run` to locate the project from the location of the script, and parse relative path options relative to the location of the script.

### Example

In the [Zulip server](https://github.com/zulip/zulip), we currently achieve this using `#!/usr/bin/env python3` and a [horrendous function](https://github.com/zulip/zulip/blob/main/scripts/lib/setup_path.py) that locates and invokes `.venv/bin/activate_this.py`. But that has a number of drawbacks and won’t work at all if we want to use an uv-managed Python with a different version than the system Python.

---

_Label `enhancement` added by @andersk on 2025-03-15 19:54_

---

_Comment by @zanieb on 2025-03-15 20:08_

I think this can be achieved with

```
#!/bin/sh
'''exec' uv run --script --project "$(dirname -- "$(realpath -- "$0")")" "$0" "$@"
' '''
import sys

print(f'hello world from {sys.executable}')
```

I think both resolving relative to the script's directory and relative to the working directory are reasonable. I'm hesitant to change the default behavior. We _could_ consider adding a flag, but let's see how the above example works for people (this is roughly what we use for "relocatable" virtual environment entrypoints)

---

_Comment by @andersk on 2025-03-16 01:47_

I’m not at all excited about having to teach new contributors how to correctly write shell/Python polyglots, or about editors misdetecting the language, linters warning about the “unused” code, formatters breaking it with reformatting, etc.

A new flag would allow solving the problem. But I couldn’t come up with use case where one might prefer the old behavior; if you wanted a script’s environment to depend on the process working directory, why would it be using an `uv` shebang?

---

_Referenced in [astral-sh/uv#12303](../../astral-sh/uv/issues/12303.md) on 2025-03-19 01:08_

---

_Referenced in [astral-sh/uv#11302](../../astral-sh/uv/issues/11302.md) on 2025-03-23 01:16_

---

_Comment by @karnigen on 2025-03-25 22:48_

I haven't seen such a _sophisticated trick_ in a long time; it's exactly how it should work. 
On the other hand, it would probably be more practical if `uv` used a switch to switch the project according to the script's path, like:

`#!/usr/bin/env -S uv run -r`

where `-r` would mean 'resolve project dir from script'.

It would also be worthwhile to create an independent script that would resolve `--project` for `uv` and/or detect other managers (poetry, etc.) and appropriately call the script, for example, like the `uvr` script.

```
#!/usr/bin/env python
import os, sys
script = os.path.realpath(sys.argv[1])
scriptDir = os.path.dirname(script)
os.execlp('uv', 'uv', 'run', '--project', scriptDir, script, *sys.argv[2:])
```
To use `uvr`, ensure it's on your `PATH` and add a _shebang_ to your script.
`#!/usr/bin/env uvr`

---

_Comment by @zanieb on 2025-04-01 17:41_

Changing the `uv run` behavior can be tracked in https://github.com/astral-sh/uv/issues/11302

Here, we'll continue to focus on discussion of shebang-specific behaviors.

---

_Comment by @gregglind on 2025-07-08 16:45_

The proposed shebang exec solution is pretty fragile and hard to grok.  I am sure I also got some spacing wrong in the copied line!

When I am using uv as a shebang, I want to it resolve a project to incorporate my personal code and libraries.

---

---
number: 8470
title: Confusing behavior of uvx and uv tool paths.
type: issue
state: open
author: warsaw
labels:
  - documentation
  - question
assignees: []
created_at: 2024-10-22T16:54:33Z
updated_at: 2024-10-23T20:23:56Z
url: https://github.com/astral-sh/uv/issues/8470
synced_at: 2026-01-07T13:12:17-06:00
---

# Confusing behavior of uvx and uv tool paths.

---

_Issue opened by @warsaw on 2024-10-22 16:54_

I'm trying to convert from pipx to uvx, but running into some confusion about how the commands should work.

```
% uv --version
uv 0.4.25 (Homebrew 2024-10-21)
```

I am using `/bin/bash` on macOS 14.7

I wrote a little script called `world` which [looks up country codes](https://world.readthedocs.io/en/stable/).  If I install this with pipx, it gets put in my `~/.local/bin` directory, which I can put on my `$PATH` and then can invoke directly instead of through pipx.  If I try to do something similar with uvx, I get some confusing behavior.  This might be *intended* behavior but for someone moving from pipx to uvx it feels like friction.

```
% uvx world it
Installed 2 packages in 3ms
it originates from Italy
% uvx world de
de originates from Germany
```

So far so good.  However, `uv tool list` doesn't show that `world` is installed:

```
% uv tool list
No tools installed
```

I'd also like to be able to invoke `world` directly, which I assumed would be possible if uvx's bin directory were on my `$PATH`, but this doesn't work:

```
% uv tool dir --bin
/Users/mylogin/.local/bin
% uv tool update-shell
Executable directory /Users/mylogin/.local/bin is already in PATH
% world fr
-bash: world: command not found
% ls -l $(uv tool dir --bin) | grep world
%
```

If I install the tool before trying to run it though, it works in the obvious ways:

```
% uv tool install world
Resolved 2 packages in 293ms
Installed 2 packages in 6ms
 + atpublic==5.0
 + world==5.1.1
Installed 1 executable: world
% world it
it originates from Italy
% uv tool list
world v5.1.1
- world
% ls -l $(uv tool dir --bin) | grep world
lrwxrwxr-x  1 mylogin  staff  50 Oct 22 09:49 world@ -> /Users/mylogin/.local/share/uv/tools/world/bin/world
```

It almost seems like the default behavior for uvx is to run with the `--isolated` flag enabled by default.


---

_Comment by @zanieb on 2024-10-22 17:04_

Yes, `uvx` does not install a tool by design — it uses an isolated environment.

---

_Label `question` added by @zanieb on 2024-10-22 17:04_

---

_Comment by @zanieb on 2024-10-22 17:04_

(`uvx` is equivalent to `uv tool run` which is roughly equivalent to `pipx run` which also does not install the tool in your bin directory)

---

_Comment by @warsaw on 2024-10-22 17:14_

I had a feeling this was working by design, thank you for confirmation!

My only suggestion to resolve this ticket is to add some documentation to clarify that `uvx`/`uv tool run` uses isolated environments by default, and that *if* you want to put the tool in your `uv tool dir --bin` directory and have `uv tool list` show it is to `uv tool install` first.

This is especially useful for tools where you need the uvx `--from` flag, e.g. [pepotron](https://pypi.org/project/pepotron/) but don't if you `uv tool install` first, e.g. `v tool run --from pepotron pep`.

Thanks!

---

_Label `documentation` added by @zanieb on 2024-10-22 17:58_

---

_Assigned to @zanieb by @zanieb on 2024-10-22 17:58_

---

_Comment by @zanieb on 2024-10-22 18:01_

> This is especially useful for tools where you need the uvx --from flag, e.g. [pepotron](https://pypi.org/project/pepotron/) but don't if you uv tool install first, e.g. v tool run --from pepotron pep.

Yeah this actually sort of incidental, in that we only check if the requested binary belongs to the package if it fails — so you can do things like `uvx --from black bash -c "black"`.

> My only suggestion to resolve this ticket is to add some documentation to clarify that uvx/uv tool run uses isolated environments by default, ...

I'm planning on doing some documentation work this week, I'll try to roll this into that.

---

_Comment by @powercoconola on 2024-10-23 20:23_

I think it's consistently confusing that `uvx` doesn't just mean `uv tool`
It seems more intuitive than for it to only mean `uv tool run`, making it a niche shorthand.
I try to remember that `uvx` is not a thing as to not mess up my commands 

---

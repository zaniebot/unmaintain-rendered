```yaml
number: 16241
title: "Add a single command equivalent to \"uv run --script\""
type: issue
state: open
author: treyhunner
labels:
  - enhancement
assignees: []
created_at: 2025-10-10T21:47:43Z
updated_at: 2025-10-22T13:58:34Z
url: https://github.com/astral-sh/uv/issues/16241
synced_at: 2026-01-10T03:23:54Z
```

# Add a single command equivalent to "uv run --script"

---

_Issue opened by @treyhunner on 2025-10-10 21:47_

### Summary

Add a `uvs` command so that `uv init --script` could add a `#!/usr/bin/env uvs` line without standardization concerns.

**Reason**: I always forget the spelling of the `#!/usr/bin/env -S uv run --script` incantation I need to add to the bottom of each script after initialization.

### Longer Explanation

I would love to see `uv init --script` add a shebang line (either by default or in a `--with-shebang` if hiding behind an option flag is deemed necessary). The issue with this is that `-S` is a non-standard `/usr/bin/env` option.

In [#11876](https://github.com/astral-sh/uv/issues/11876#issuecomment-2745912913)  @magistau suggested adding a single command (e.g. `uvs`) to fix this:

> It's hard to implement properly, because `-S` flag is not a part of [POSIX standard][env]. That is, different `env` implementations may treat it differently, potentially causing issues. IMO a better solution would be introducing a helper executable (say, `uvs`) that could be used instead of `uv run` when a shebang is needed.
> 
> [env]: https://pubs.opengroup.org/onlinepubs/9799919799/ 

### Example

```bash
$ uv init --script ~/bin/new_script
$ head -n 5 ~/bin/new_script
#!/usr/bin/env uvs
# /// script
# requires-python = ">=3.14"
# dependencies = []
# ///
```

---

_Label `enhancement` added by @treyhunner on 2025-10-10 21:47_

---

_Comment by @treyhunner on 2025-10-11 20:23_

As both a proof of concept and a useful tool to help me work with uv scripts more easily, I published [uvrs](https://github.com/treyhunner/uvrs). If this feature is intergrated into uv directly (and I hope it will be!) I will sunset that tool.

---

_Renamed from "Add uvs command equivalent to "uv run --script"" to "Add a single command equivalent to "uv run --script"" by @treyhunner on 2025-10-11 20:45_

---

_Comment by @treyhunner on 2025-10-17 03:13_

I think a `uvs <path>` command should be equivalent to `uv run --exact --script <path>` (in the spirit of "principle of least astonishment").

Having a different default semantic than for `uv run` seems acceptable since the script virtual environment is more carefully and deliberately hidden and "behind the scenes" than project virtual environments are.

---

_Comment by @konstin on 2025-10-17 08:31_

> The issue with this is that `-S` is a non-standard `/usr/bin/env` option.

For determining the impact, which shells don't implement the `-S` option?

---

_Comment by @treyhunner on 2025-10-17 20:50_

> > The issue with this is that `-S` is a non-standard `/usr/bin/env` option.
> 
> For determining the impact, which shells don't implement the `-S` option?

I have been wondering this too. Here is my attempt at researching this from web searches...

These systems seem to have `/usr/bin/env` that supports `-S` with the semantics that the uv documentation assumes:

1. Linux by default, as of 2018 or so
2. Mac OS, though I don't know when it was added
3. FreeBSD
4. Various WSL tools, thanks to support in Linux

I believe these systems don't support `/usr/bin/env -S`:

1. Most BSD systems (Open BSD & Net BSD for example)
2. Alpine Linux and some other deliberately small Linux distributions
3. Other POSIX systems in general (haven't looked into this)

---

_Comment by @apfelchips on 2025-10-18 21:08_

@bahamas10 made a nice video about shebang argument handling on different Systems: https://www.youtube.com/watch?v=aoHMiCzqCNw&t=1000s

TLDW: You can't trust what and how arguments are passed and for maximum portability you shouldn't use more than one.

---

_Comment by @zanieb on 2025-10-18 22:56_

There's a non-zero cost to us adding additional binaries to the uv installation, but we can consider this.

There was quite a bit of discussion about the shebang in https://github.com/astral-sh/uv/pull/11553

---

_Comment by @davep on 2025-10-22 13:58_

> Mac OS, though I don't know when it was added

@treyhunner If it helps, the HISTORY section of the man page for `env` on macOS says:

> The env command appeared in 4.4BSD.  The -P, -S and -v options were added in FreeBSD 6.0.  The -C option was added in FreeBSD 14.2.

This is as documented in macOS 26.0, with the man page last updated October 8, 2024.

---

```yaml
number: 14547
title: "`uv` installer (https://astral.sh/uv/install.sh) creating `.zshrc` even in bash"
type: issue
state: open
author: alichaudry
labels:
  - bug
assignees: []
created_at: 2025-07-10T17:34:42Z
updated_at: 2025-07-10T20:16:14Z
url: https://github.com/astral-sh/uv/issues/14547
synced_at: 2026-01-12T16:01:51Z
```

# `uv` installer (https://astral.sh/uv/install.sh) creating `.zshrc` even in bash

---

_@alichaudry_

### Summary

I installed `uv` in a bash shell running in Ubuntu on Windows 11 (WSL2) a few weeks ago, and I just noticed that the installer had also created a `.zshrc` file. This is surely a bug, right? As I wouldn't expect the installer to create a `.zshrc` file in bash (and vice versa, a `.bashrc` in zsh).

These are the contents of the files:

```shell
❯ cat ~/.zshrc
. "$HOME/.local/bin/env"
```

```shell
❯ ls ~/.local/bin/
env  env.fish  uv  uvx
```

```shell
❯ cat ~/.local/bin/env
#!/bin/sh
# add binaries to PATH if they aren't added yet
# affix colons on either side of $PATH to simplify matching
case ":${PATH}:" in
    *:"$HOME/.local/bin":*)
        ;;
    *)
        # Prepending path in case a system-installed binary needs to be overridden
        export PATH="$HOME/.local/bin:$PATH"
        ;;
esac
```

It also looks like `env` and `env.fish` (presumably for fish shell?) were installed in addition to `uv` and `uvx`.

1. I did search documentation and found [at least one issue](https://github.com/astral-sh/uv/issues/10413) that touches on a similar topic, but my concern here is not that it modified the `.bashrc`, but that it also created a `.zshrc`. My `.bashrc` was also appended with `. "$HOME/.local/bin/env"`.
2. For changes made by the `uv` installer, it would be nice to add a comment saying what added the `. "$HOME/.local/bin/env"` and when, like:

```shell
# added by https://astral.sh/uv/install.sh on 6/12/2025
. "$HOME/.local/bin/env"
```

Other than that, thank you for creating an incredible product. It's actually re-energized my interest in python as I no longer have to fiddle with a mix of pip-tools and conda for my use cases.

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.7.18

### Python version

Python 3.13.3

---

_Label `bug` added by @alichaudry on 2025-07-10 17:34_

---

_Comment by @zanieb on 2025-07-10 17:57_

I think it adds a shell hook for all known shells, because detecting the relevant subset is hard? This behavior is currently implemented via https://github.com/axodotdev/cargo-dist

cc @Gankra 

Thanks for the kind words!

---

_Comment by @alichaudry on 2025-07-10 20:16_

Ah okay, that makes sense! Do you think there's a possibility to use something like [what `brew` does](https://github.com/Homebrew/brew/blob/main/Library/Homebrew/cmd/shellenv.sh) to figure out the active shell? Not sure if it would work in all scenarios; even for me `echo "$(/bin/ps -p "${PPID}" -c -o comm=)"` is not saying `bash` but `Relay(360)` instead (I'm guessing this is a WSL2 process, but technically the way the script is written, the catch-all `case` would still do the right thing for `bash` but perhaps not for `zsh`).

But maybe it works well-enough in a majority of cases that in the typical scenario we don't have to create all the extra files for the unrecognized shells, and only as a backup/catch-all we can do the current default?

And as a side-note, it would still be great to add a date/timestamp + authorship message from uv as a comment above the uv-inserted lines in system/user configuration files.

---

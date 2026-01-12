```yaml
number: 15766
title: "Install shell completions in `uv tool install`"
type: issue
state: open
author: sondhg
labels:
  - wish
  - needs-design
assignees: []
created_at: 2025-09-10T09:10:08Z
updated_at: 2025-09-11T14:55:58Z
url: https://github.com/astral-sh/uv/issues/15766
synced_at: 2026-01-12T16:02:17Z
```

# Install shell completions in `uv tool install`

---

_@sondhg_

### Question

I don't know why shell completions for tools installed via `uv tool install` do not work. An example will be easier to explain the problem I'm facing.

I'll use a random Python CLI tool `gallery-dl` (I aliased it to `gdl`) as example, with comparison between two installation methods.
 
1. Case 1: Install via Homebrew `brew install gallery-dl`. When I type `gallery-dl --` then hit <kbd>Tab</kbd>, flags/options for the command are suggested.

<img width="760" height="609" alt="Image" src="https://github.com/user-attachments/assets/0868af21-299b-411d-898d-266afbb6f759" />

2. Case 2: Install via uv `uv tool install gallery-dl`. I performed the similar action, but shell completions do not work.

<img width="719" height="55" alt="Image" src="https://github.com/user-attachments/assets/4d88a3f3-62a5-4227-84eb-6398f5d9bbaf" />

After a bit of search, I find the supposed-to-be shell completion file `_gallery-dl` in `~/.local/share/uv/tools/gallery-dl/share/zsh/site-functions`. 

<img width="1920" height="295" alt="Image" src="https://github.com/user-attachments/assets/c23ab781-0b94-4170-8a54-0d33b9ab9d3e" />

Is the reason for this problem is because `uv` doesn't add the directories storing these completion files to `$fpath` like Homebrew does? Here is my `$fpath` currently:

<img width="651" height="192" alt="Image" src="https://github.com/user-attachments/assets/cbf834ca-0c49-453d-8bbf-c090b9fd6d67" />

So is there a proper solution to enable shell completions for CLI tools installed via `uv tool install`? It would be much appreciated! Right now I either have to manually add `~/.local/share/uv/tools/gallery-dl/share/zsh/site-functions` to `$fpath` (by adding the code below to `.zshrc`), or copy  `_gallery-dl` to one of the directories presented in `$fpath`. Both are not ideal, I assume.

```shell
# Add uv tool site-functions to fpath
for d in ~/.local/share/uv/tools/*/share/zsh/site-functions; do
  [[ -d $d ]] && fpath+=$d
done
```

### Platform

Linux 6.8.0-79-generic x86_64 GNU/Linux

### Version

uv 0.8.15 (Homebrew 2025-09-03)

---

_Label `question` added by @sondhg on 2025-09-10 09:10_

---

_Comment by @zanieb on 2025-09-10 13:08_

Thanks for the details!

Does this work with `pipx`?

---

_Comment by @sondhg on 2025-09-10 15:09_

If you mean `pipx` by default, then no. I'd need to add this path to `$fpath` by adding the similar code as above to `.zshrc` 

<img width="874" height="54" alt="Image" src="https://github.com/user-attachments/assets/0ebdb9d6-39f4-44d8-b659-5ad2dc1c3c4b" />



---

_Label `question` removed by @zanieb on 2025-09-10 16:08_

---

_Label `wish` added by @zanieb on 2025-09-10 16:08_

---

_Renamed from "Shell completions do not work for CLI tools installed via `uv tool install`" to "Install shell completions in `uv tool install`" by @zanieb on 2025-09-10 16:08_

---

_Comment by @zanieb on 2025-09-10 16:09_

I'm not sure how we'd implement this, though it does seem nice to have.

---

_Label `needs-design` added by @zanieb on 2025-09-10 16:09_

---

_Comment by @sondhg on 2025-09-11 14:55_

Thank you for the reply. It's good to know that this isn't a feature yet. I was thinking this feature already existed and I may have missed it while reading the `uv` documentation 😅 . I'm glad that wasn't the case.

Seems like many tools installed via `uv tool install <PACKAGE_NAME>` already have their completion script file `_<PACKAGE_NAME>` installed to `~/.local/share/uv/tools/<PACKAGE_NAME>/share/zsh/site-functions` (I use `zsh` so I don't know where the script would be stored in other shells). Unlike Homebrew, these completion script files are not exposed to $fpath by default. So for now the simplest solution I've found is to `echo` the code I wrote in previous comment into `.zshrc`:

```shell
for d in ~/.local/share/uv/tools/*/share/zsh/site-functions; do
  [[ -d $d ]] && fpath+=$d
done
```

It would be convenient to see the this sort of code for each shell included somewhere in the docs. Or maybe a `uv tool enable-tool-completions` command to do the same thing. Just my suggestion! I hope this feature will be available in the future. Been loving uv so far.

---

```yaml
number: 12465
title: "uv's python appears to eat ansi escape sequences on WSL? (looks like cmd.Cmd)"
type: issue
state: open
author: waynew
labels:
  - bug
  - windows
  - uv python
assignees: []
created_at: 2025-03-25T14:20:46Z
updated_at: 2025-05-11T10:28:36Z
url: https://github.com/astral-sh/uv/issues/12465
synced_at: 2026-01-12T16:01:03Z
```

# uv's python appears to eat ansi escape sequences on WSL? (looks like cmd.Cmd)

---

_@waynew_

### Summary

I'm definitely seeing some weird behavior here. At first I thought it was something with `uv` generally, but that does not appear to be the case. I could be doing something wrong, but...

```
pipx install shibboleth==25.3.0b1
shibboleth

Welcome to Shibboleth 25.3.0b1, the tool designed to be *your*
secret weapon.

Your editor is currently vim. If you don't like that, you
should change or set your EDITOR environment variable.

⇀shibboleth:/home/wayne
```

This is the expected output. If I run
```
uv tool install shibboleth==25.3.0b1
shibboleth
```
On my `Linux 6.13.8-arch1-1 x86_64 GNU/Linux` machine, this works the same. However, after a `pipx uninstall shibboleth`

```
uvx --from=shibboleth==25.3.0b1 shibboleth

Welcome to Shibboleth 25.3.0b1, the tool designed to be *your*
secret weapon.

Your editor is currently vim. If you don't like that, you
should change or set your EDITOR environment variable.

⇀[34mshibboleth[0m:/home/wayne>
```

It appears to be escaping the ansi escape sequences. `uv tool install shibboleth==25.3.0b1` has the same effect `shibboleth` ansi colors get borked. 

But when I use my system Python:

```
✦ ❯ PYTHONPATH=/home/wayne/.local/share/uv/tools/shibboleth/lib64/python3.13/site-packages/ python $(which shibboleth)

Welcome to Shibboleth 25.3.0b1, the tool designed to be *your*
secret weapon.

Your editor is currently vim. If you don't like that, you
should change or set your EDITOR environment variable.

⇀shibboleth:/home/wayne
>
```
Everything is great!

So it appears to have something to do with `uv`s python. Indeed:

```
/home/wayne/.local/share/uv/tools/shibboleth/bin/python ~/.local/share/uv/tools/shibboleth/lib/python3.13/site-packages/shibboleth.py
```

is borked, but system python works.

Knowing that I went ahead and pulled some of the colorizing code from shibboleth and adapted it a bit:

```
print(f'\x1b[32mThis is sad\x1b[0m' )
```

![Image](https://github.com/user-attachments/assets/f9949710-1dc2-4199-8958-73ab7bfb616b)

But... that worked. Putting it in a script worked. What other difference was there? OOooo. cmd.Cmd

```
import cmd
bad =  f'\x1b[32mThis is sad\x1b[0m'


class Ok(cmd.Cmd):
    prompt = bad

    def do_quit(self, line):
        return True

print(bad)
Ok().cmdloop()
print(bad)
```

![Image](https://github.com/user-attachments/assets/cb2cf8e6-5a2b-4c8e-a691-fcc2adae9f31)

There it is. Something within uv doesn't play well with cmd.Cmd.

But then things get *very* interesting! With a lil' tweak to the script to make this clear:

![Image](https://github.com/user-attachments/assets/7d24d6ec-52e6-4360-a53d-620294b2c19e)

Interactively, the colors are correctly displayed with the uv tool python. Non-interactively? Something goes haywire, at least under WSL 🤷 

If there's a command or env var, or flag I can add to the shibboleth entrypoint to try and make the colors work... let me know!

### Platform

Linux 5.15.167.4-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

0.5.7

### Python version

Python 3.13.1

---

_Label `bug` added by @waynew on 2025-03-25 14:20_

---

_Label `windows` added by @zanieb on 2025-04-01 18:43_

---

_Label `uv python` added by @zanieb on 2025-04-01 18:43_

---

_Comment by @zanieb on 2025-04-01 18:43_

I suspect this is a `python-build-standalone` quirk, it might be a bit until we can dig deeper.

---

_Comment by @hibukki on 2025-05-11 10:28_

We're seeing similar behavior in a very different situation:

On macos 15.0.1, when using a uv-provided python

```
uv add visidata
uv run vd
```

shows colors in the menu bar and popup, and you can change the default color and see everything in a different color, but visidata somehow *thinks* it doesn't have colors so it displays all cell contents and popup contents in the configured default color (Shift-O opens the options, where you should see a few things in color if running in a pyenv- or brew-supplied python, but they all appear in the default color in a uv-supplied python).

(To quit visidata, press q a few times)

---

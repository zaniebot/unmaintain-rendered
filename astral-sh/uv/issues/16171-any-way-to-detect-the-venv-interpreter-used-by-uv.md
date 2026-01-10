---
number: 16171
title: "Any way to detect the venv/interpreter used by `uv run --script my_script.py` (to enable IDE linting)"
type: issue
state: open
author: aaronsteers
labels:
  - question
assignees: []
created_at: 2025-10-08T01:51:25Z
updated_at: 2025-10-09T00:04:05Z
url: https://github.com/astral-sh/uv/issues/16171
synced_at: 2026-01-10T01:26:04Z
---

# Any way to detect the venv/interpreter used by `uv run --script my_script.py` (to enable IDE linting)

---

_Issue opened by @aaronsteers on 2025-10-08 01:51_

### Question

I'm using uv and portable uv-executed scripts, as I recently wrote about here: [The Modern Ways to (not) Install Python and Virtual Environments - DEV Community](https://dev.to/aaronsteers/the-modern-ways-to-not-install-python-1e85)

Now, I'd like to give myself and my team a way to maintain uv scripts with the help of IDE linting. I think this is possible if I locate the uv-managed venv. Is there a way to locate this path? (With the understanding that the path may be ephemeral, changing without notice?)

### Platform

Darwin 25.0.0 x86_64

### Version

uv-self 0.6.12

---

_Label `question` added by @aaronsteers on 2025-10-08 01:51_

---

_Comment by @zanieb on 2025-10-08 04:14_

You can do something like

```
❯ uv sync --script example.py
Creating script environment at: /Users/zb/.cache/uv/environments-v2/example-7a04b142bfbd2723
Resolved in 8ms
Audited in 0.38ms
❯ uv python find --script example.py
/Users/zb/.cache/uv/environments-v2/example-7a04b142bfbd2723/bin/python3
```

or

```
❯ uv sync --dry-run --script example.py --output-format json --quiet | jq -r .sync.environment.path
/Users/zb/.cache/uv/environments-v2/example-7a04b142bfbd2723
```

The path is stable.


---

_Comment by @aaronsteers on 2025-10-08 23:39_

@zanieb - Thanks very much for your reply. I really like the simplicity of the `uv python find` syntax, but it doesn't work correctly in my trials. Instead, it prints a uv-managed python path that does not have hash parts. (Perhaps it is resolving the symlink?)

```
aj:repos aj.steers$ uv python find --script=airbyte/poe-tasks/get_connector_matrix.py
/Users/aj.steers/.local/share/uv/python/cpython-3.12.0-macos-aarch64-none/bin/python3.12
```

The second option does work, although a bit more verbose and clunky if typing by hand.

This version (with or without `--dry-run`) seems to be a balance of sane-to-type while also not an insane amount of output:

```
aj:repos aj.steers$ uv sync --script airbyte/poe-tasks/get_connector_matrix.py | grep "environment"
Using script environment at: /Users/aj.steers/.cache/uv/environments-v2/get-connector-matrix-1bd340654832c662
Resolved 13 packages in 2ms
Audited 13 packages in 0.26ms
```

Any ideas on whether `uv python find` could be updated to not traverse symlinks (if that's indeed what is happening), and/or have a another option to use that command with the --script arg to locate the script's venv?

Totally okay either way - I'm unblocked now, and I see that `sync` operations print it by default, which is handy for what I'm hoping to train team members on.

Much thanks!
AJ

---

_Comment by @aaronsteers on 2025-10-08 23:43_

For documentation purposes: 

Confirmed that appending `/bin/python` to the end of the discovered path, and then passing that to VS Code as its interpreter works beautifully to get linting working. 💎 ✨ 💎 ✨ 

Screenshot or it didn't happen 😄 

> <img width="868" height="531" alt="Image" src="https://github.com/user-attachments/assets/091421d6-8576-4886-8084-efb9c8dcb91b" />

Note the intellisense for the `typer` library works correctly, despite being a third party library that is only used in that script's venv.

Note also in the bottom status bar the text "`3.12 (get_connector_matrix.py)`" indicating the found Python version, as well as the helpful indicator of the venv corresponding to my script. 🎉 

I'll leave this issue open for replies to my inquiries in my prior comment - but feel free to close/resolve after replying. Thanks!! 

---

_Comment by @zanieb on 2025-10-09 00:04_

>  Instead, it prints a uv-managed python path that does not have hash parts. (Perhaps it is resolving the symlink?)

Did you run `uv sync --script ...` first?

---

---
number: 9123
title: ruff VSCode extension erases entire file on save
type: issue
state: closed
author: bdewilde
labels:
  - bug
  - cli
assignees: []
created_at: 2023-12-14T02:54:32Z
updated_at: 2023-12-14T05:38:30Z
url: https://github.com/astral-sh/ruff/issues/9123
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff VSCode extension erases entire file on save

---

_Issue opened by @bdewilde on 2023-12-14 02:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

After updating/reloading a couple extensions in VSCode -- one of which may have been Ruff, I'm not 100% sure -- the Ruff extension suddenly started erasing entire Python files on save. I've disabled/re-enabled and uninstalled/re-installed, but that doesn't change the behavior. I've further narrowed it down to the "format imports" command -- "format document" does not erase the module -- but disabling the "organize imports" behavior in settings somehow does not stop saves from erasing the file. The corresponding configs in my VSCode settings and `pyproject.toml` tools have not changed.

I don't have a code snippet, but I do have a gif demonstrating the phenomenon:

![ruff-surprise](https://github.com/astral-sh/ruff/assets/2514535/ec86f690-6dc0-45e5-afbc-2cdddb690f5c)

This is quite unexpected, and I do not understand what's gone wrong. I have the latest version of both the `ruff` VSCode extension (v2023.52.0) and Python package (v0.1.8) installed. Here's the relevant config:

```json
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": "always",
      "source.organizeImports.ruff": "always"
    },
  }
```

and

```toml
[tool.ruff]
# ignore line-length violations, None comparisons
ignore = ["E501", "E711"]
line-length = 88
indent-width = 4
target-version = "py310"

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"

[tool.ruff.lint.isort]
lines-after-imports = 2

[tool.ruff.per-file-ignores]
"__init__.py" = ["E402", "F401"] # imports not at top of cell and unused imports
```

Assuming this must be user error, but gosh I don't know what or how. Thanks in advance for your help.

---

_Comment by @charliermarsh on 2023-12-14 03:10_

Let me try this now -- I thought we had fixed this but it may've regressed.

---

_Comment by @charliermarsh on 2023-12-14 03:12_

Is this happening in every file, or only certain files?

---

_Comment by @charliermarsh on 2023-12-14 03:15_

I assume this persists across restarting VS Code?

---

_Comment by @bdewilde on 2023-12-14 03:16_

Hi! Thanks very much for the quick response. This does happen across VSCode restarts -- I even restarted my laptop, was getting desperate. This is happening for every Python file that I try to save.

---

_Comment by @charliermarsh on 2023-12-14 03:18_

Do you mind checking the version of Ruff that's running as reported by the extension in the Ruff logs?

<img width="727" alt="Screen Shot 2023-12-13 at 10 17 44 PM" src="https://github.com/astral-sh/ruff/assets/1309177/f0dd3d65-f979-4b92-a71f-5816cb20fb95">

I can't reproduce with the latest extension and Ruff version, but if you're continuing to see this then I'll revert the change that likely caused it and re-release while we figure out what's gone wrong.


---

_Comment by @charliermarsh on 2023-12-14 03:23_

Also, if you could confirm that downgrading to the previous release fixes it for you, that'd be super helpful too!

---

_Comment by @bdewilde on 2023-12-14 03:27_

Nice, I didn't realize you could peep on logs from particular extensions in VSCode. Here's what I see there re: version:

```
2023-12-13 22:19:28.313 [info] Name: Ruff
2023-12-13 22:19:28.313 [info] Module: ruff
2023-12-13 22:19:28.313 [info] Python extension loading
2023-12-13 22:19:28.313 [info] Waiting for interpreter from python extension.
2023-12-13 22:19:28.313 [info] Python extension loaded
2023-12-13 22:19:28.313 [info] Server run command: /Users/burtondewilde/.pyenv/versions/3.10.12/envs/colandr-py310/bin/python /Users/burtondewilde/.vscode/extensions/charliermarsh.ruff-2023.52.0-darwin-x64/bundled/tool/server.py
2023-12-13 22:19:28.313 [info] Server: Start requested.
2023-12-13 22:19:29.271 [info] 2023-12-13 22:19:29,269 INFO Starting IO server
```

Scrolling through, I also see an error -- maybe this is normal, but figured I'd call it out:

```
2023-12-13 22:19:29.415 [info] [Trace - 10:19:29 PM] Received notification 'window/logMessage'.
2023-12-13 22:19:29.415 [info] Running Ruff with: /Users/burtondewilde/.pyenv/versions/3.10.12/envs/colandr-py310/bin/ruff ['--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--line-length=88', '--indent-width=4', '--stdin-filename', '/Users/burtondewilde/Desktop/example.py']
2023-12-13 22:19:29.428 [info] [Trace - 10:19:29 PM] Received notification 'window/logMessage'.
2023-12-13 22:19:29.428 [info] error: unexpected argument '--indent-width' found

  tip: to pass '--indent-width' as a value, use '-- --indent-width'
```

Which should I try downgrading: the package (from v0.1.8 to v0.1.7) or the extension?

---

_Comment by @charliermarsh on 2023-12-14 03:28_

Interesting, do you have `--indent-width` passed in as a command-line argument in your settings? (I was suggesting downgrading the _extension_ to 2023.50.0.)

---

_Comment by @charliermarsh on 2023-12-14 03:29_

Ahhh okay, I think the issue is:

```json
"ruff.lint.args": ["--indent-width", "4"]
```

Or similar in your `settings.json`. We're wiping the file on error, apparently. I'll fix this tonight.

---

_Comment by @bdewilde on 2023-12-14 03:31_

Aha, yeah, I have these at the bottom of my VSCode user settings -- forgot about them, since I set them up a long while back when I was still trying to figure out if `ruff` could replace `black`+`isort` :)

```
  "ruff.lint.run": "onSave",
  "ruff.lint.args": [
    "--line-length=88",
    "--indent-width=4"
  ],
```

Should I remove them? Restructure the JSON as your example suggests?

_update:_ Removing them stops the files from being erased on save, but changing the structure of the json does not.

---

_Comment by @charliermarsh on 2023-12-14 03:40_

Yeah you need to remove `--indent-width=4` -- we only support a subset of arguments on the command-line, so Ruff is failing when it sees `--indent-width=4`, and we're (erroneously) wiping your file instead of surfacing the error.

---

_Comment by @bdewilde on 2023-12-14 03:41_

Understood. Thanks a ton for helping to resolve this so quickly!

---

_Comment by @charliermarsh on 2023-12-14 03:42_

No prob. Sorry for all the trouble... Will get this fixed for good ASAP.

---

_Label `bug` added by @MichaReiser on 2023-12-14 03:43_

---

_Label `cli` added by @MichaReiser on 2023-12-14 03:43_

---

_Referenced in [astral-sh/ruff-lsp#341](../../astral-sh/ruff-lsp/pulls/341.md) on 2023-12-14 03:59_

---

_Closed by @charliermarsh on 2023-12-14 04:41_

---

_Comment by @charliermarsh on 2023-12-14 05:38_

Should be fixed in the latest version, just published.

---

_Referenced in [astral-sh/ruff#15734](../../astral-sh/ruff/issues/15734.md) on 2025-01-24 22:39_

---

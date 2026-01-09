---
number: 16041
title: Unexpected relative path handling in native server mode
type: issue
state: closed
author: richtea
labels:
  - bug
  - server
assignees: []
created_at: 2025-02-07T22:47:41Z
updated_at: 2025-02-10T04:57:16Z
url: https://github.com/astral-sh/ruff/issues/16041
synced_at: 2026-01-07T13:12:16-06:00
---

# Unexpected relative path handling in native server mode

---

_Issue opened by @richtea on 2025-02-07 22:47_

[Edit - see [my comment](https://github.com/astral-sh/ruff/issues/16041) below for problem description]

I just had a notification that ruff-lsp is deprecated, so I removed my setting `"ruff.nativeServer": "off"`. Now, I don't get any highlighting in Python documents, however when I run `poetry run ruff check` it lists violations.

When native server is enabled, I've checked the logs and can't see any obvious suggestions as to why nothing gets highlighted. In the Ruff output window, it displays `Server: Start requested.` and nothing more. In the Ruff Language Server output window it displays

```text
WARN ruff:main ruff_server::server::api: Received notification $/setTrace which does not have a handler.
```

multiple times.

If I switch back to `"ruff.nativeServer": "off"` then the highlighting is restored.

The related settings in `settings.json` are:

```json
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.codeActionsOnSave": {
      "source.fixAll": "never",
      "source.organizeImports": "never",
      "source.organizeImports.ruff": "always"
    },
    "editor.insertSpaces": true,
    "editor.formatOnSave": false,
    "editor.formatOnPaste": true,
    "editor.formatOnType": false
  },
```

---

_Comment by @dhruvmanila on 2025-02-08 03:31_

Do you have any other VS Code settings? For example, any related to `ruff.*`.

And, can you try setting the log level to debug as stated [here](https://github.com/astral-sh/ruff-vscode#troubleshooting) and provide all the logs?

---

_Comment by @MichaReiser on 2025-02-08 08:00_

By highlighting, do you meant syntax highlighting or that ruff doesn't show any warnings or errors in the editor?

---

_Comment by @richtea on 2025-02-08 10:49_

Thanks for the suggestions - having increased the log level I now have an idea what the problem is (see below).

> By highlighting, do you meant syntax highlighting or that ruff doesn't show any warnings or errors in the editor?

I mean no warnings or errors ("squiggly underlines").

> Do you have any other VS Code settings? For example, any related to ruff.*.

None to affect this, mainly `python.analysis` and `python.testing`.

> And, can you try setting the log level to debug 

Thanks, I'd found the `ruff.trace.server` setting but not the `ruff.logLevel` setting - this one had the messages I needed.

## The problem

I think the difference in behaviour between LSP and native server relates to relative path handling in the `exclude` setting. I had a relative directory named `play` configured to exclude, and by coincidence (or perhaps because I play a lot :grinning:) the repo itself lives some levels below a directory also called `play`. 

Below are some logs, both while running with native server on. Note that the `pyproject.toml` is in the root repo directory called `polebot` (full repo is [here](https://github.com/bwcc-clan/polebot)).

The first is with setting `exclude = ["play"]`:

```text
8848.627385916s DEBUG ThreadId(106) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/[me]/dev/play/bwcc/bwcc-clan/polebot/play
8848.638075750s DEBUG  ruff:worker:4 ruff_server::resolve: Ignored path via `exclude`: /Users/[me]/dev/play/bwcc/bwcc-clan/polebot/src/utils/log_tools.py
```

If the setting is `exclude = ["./play"]` then it behaves as I would expect:

```text
8449.275811916s DEBUG ThreadId(94) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/[me]/dev/play/bwcc/bwcc-clan/polebot/play
8449.285933083s DEBUG  ruff:worker:3 ruff_server::resolve: Included path via `include`: /Users/[me]/dev/play/bwcc/bwcc-clan/polebot/src/utils/log_tools.py
```

## Is it a bug?

Note that the first `play` directory in the path is above the `pyproject.toml` and therefore, by my understanding of the [docs](https://docs.astral.sh/ruff/configuration/#config-file-discovery), should be ignored in the context of relative path handling. The second `play` directory (in the first log entry in both examples above) is excluded in both cases, as expected because it's below the repo root directory.

I would argue that this is a bug, for a few reasons:

1. Any part of a file path that is above the directory containing the config file should be ignored in the context of relative path handling.

2. The current behaviour within the editor is different from the behaviour when running `ruff check` from the command line.

3. The current behaviour of the native server is different from how it works in the LSP, in this case effectively turning Ruff off for the entire repo.


---

_Renamed from "Ruff not highlighting in native server mode" to "Unexpected relative path handling in native server mode" by @richtea on 2025-02-08 10:50_

---

_Comment by @MichaReiser on 2025-02-08 16:00_

Oh nice find. I'm curious, what ruff version are you using?

---

_Comment by @richtea on 2025-02-08 16:21_

> what ruff version are you using?

0.9.5

---

_Comment by @dhruvmanila on 2025-02-08 17:35_

Oh, great find. Thank you for debugging. This seems like a bug.

---

_Label `bug` added by @dhruvmanila on 2025-02-08 17:35_

---

_Label `server` added by @dhruvmanila on 2025-02-08 17:35_

---

_Referenced in [astral-sh/ruff#16043](../../astral-sh/ruff/pulls/16043.md) on 2025-02-08 17:59_

---

_Closed by @dhruvmanila on 2025-02-10 04:57_

---

_Closed by @dhruvmanila on 2025-02-10 04:57_

---

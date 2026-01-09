---
number: 11587
title: "Server and CLI don' t exclude the same directories"
type: issue
state: closed
author: EricDepagne
labels:
  - bug
  - server
assignees: []
created_at: 2024-05-28T21:36:18Z
updated_at: 2024-10-15T10:14:50Z
url: https://github.com/astral-sh/ruff/issues/11587
synced_at: 2026-01-07T13:12:15-06:00
---

# Server and CLI don' t exclude the same directories

---

_Issue opened by @EricDepagne on 2024-05-28 21:36_

I' m using ruff server in vscode.
Ruff version: 0.4.5
VSCode version : 1.89.1 (Universal)

My ruff.toml contains the following exclusion:
exclude = ["\*/tests/\*"]


I run ruff in a terminal at the same time as the ruff server in vscode.
The intersting part of the output of ruff -w is:

```
[23:26:28 PM] Starting linter in watch mode...

warning: Unexpected `# ruff: noqa` directive at conftest.py:10. File-level suppression comments must appear on their own line. For line-level suppression, omit the `ruff:` prefix.
warning: Unexpected `# ruff: noqa` directive at bikeanalysis/activities/models/activities.py:228. File-level suppression comments must appear on their own line. For line-level suppression, omit the `ruff:` prefix.
[23:26:28 PM] Found 19 errors. Watching for file changes.
```
Only 19 errors found

In vscode, the same errors are flagged, as expected, but I have also all the warnings for all my test files, that are in one of the tests/ directories.

For instance, this one:
```

def test_missing_all_streams():
    streams = []
    assert len(get_missing_streams(streams=streams)) == 11
```
triggers D103.

The ruff that runs in my terminal does not show any errors for files in my tests/ directory as expected from the configuration.

Am I missing something when configuring ruff server in VSCode?


---

_Label `server` added by @charliermarsh on 2024-05-28 21:43_

---

_Comment by @charliermarsh on 2024-05-28 22:29_

Might be a real bug in the new server. I'll try to repro tonight.

---

_Comment by @charliermarsh on 2024-05-28 23:56_

`exclude = ["*/tests/*"]` doesn't actually work for me, even on the command-line. `exclude = ["**/tests/*"]` does work though.

---

_Label `bug` added by @charliermarsh on 2024-05-28 23:56_

---

_Comment by @charliermarsh on 2024-05-28 23:59_

I can reproduce with `exclude = ["**/tests/*"]` though.

---

_Comment by @charliermarsh on 2024-05-29 00:05_

I think `ruff server` just doesn't respect exclusions yet if you open an excluded file directly.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-29 00:15_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-05-29 00:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-29 00:38_

---

_Referenced in [astral-sh/ruff#11590](../../astral-sh/ruff/pulls/11590.md) on 2024-05-29 00:45_

---

_Closed by @charliermarsh on 2024-05-29 02:58_

---

_Comment by @messense on 2024-08-21 05:49_

#11590 breaks my format workflow, I'd expect the vscode `format document` and `format cell` commands to format the python/notebook file even if it's excluded in configuration file, because I'm explicitly asking vscode to format it.

BTW, `ruff format <path to file>` works regardless of file exclusions config, so IMO manually call vscode `format document` should have the same behavior.

---

_Comment by @MichaReiser on 2024-08-21 06:33_

> https://github.com/astral-sh/ruff/pull/11590 breaks my format workflow, I'd expect the vscode format document and format cell commands to format the python/notebook file even if it's excluded in configuration file, because I'm explicitly asking vscode to format it.

Hmm, I do understand the desire that explicitly issuing format document or format cells should format the code even if it is excluded. The challenge we face is that the LSP request, as far as I know, contains no information whether the request was triggered implicitly (by enabling format on save) or explicitly (by issuing the command). Implicit request should respect the ignores, I'm open to change that explicit requests don't. 

The alternative is that we introduce a new option. 

---

_Comment by @messense on 2024-08-22 01:42_

An new option would be nice for people don't use `format on save`.

---

_Comment by @dhruvmanila on 2024-08-22 03:10_

> The challenge we face is that the LSP request, as far as I know, contains no information whether the request was triggered implicitly (by enabling format on save) or explicitly (by issuing the command)

Yeah, this is correct. The server has no way of knowing whether the request was triggered automatically or manually.

It seems reasonable to add a new option and is related to https://github.com/astral-sh/ruff/issues/12176. @messense Is it that the extension should format / lint _every_ document that's opened in the editor, thus not considering the includes / excludes ?

---

_Comment by @masaccio on 2024-10-14 17:16_

Is this really about files that are opened explicitly? I'm finding that VScode flags warnings from files that are not open but are in my workspace. In my case these files:

<img width="267" alt="Screenshot 2024-10-14 at 18 15 23" src="https://github.com/user-attachments/assets/73f904dd-7f0d-475f-9c33-02f17dc41c2a">

Despite:

``` tool
[tool.ruff]
exclude = [
  # Machine-generated files
  "**/.bootstrap/*",
  "**/.tox/*",
  "**/.vscode/*",
  "**/src/numbers_parser/generated/*",
]
```

Whether they're included by the server is a bit random. Is there a VScode log that would be helpful?

---

_Comment by @dhruvmanila on 2024-10-15 10:14_

> Is this really about files that are opened explicitly? I'm finding that VScode flags warnings from files that are not open but are in my workspace.

Can you say more about this? Is it that the diagnostics window includes violations from files that aren't opened in the editor? Are those diagnostics coming from Ruff because we don't support workspace wide diagnostics? Do you have `python.analysis.diagnosticMode` set to `workspace`? That setting is used by the Python extension to include workspace wide diagnostics.

---

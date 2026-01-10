---
number: 2310
title: VS Code Extension regularly crashes
type: issue
state: closed
author: npaolini2634
labels:
  - server
  - fatal
assignees: []
created_at: 2026-01-02T22:37:38Z
updated_at: 2026-01-09T07:49:42Z
url: https://github.com/astral-sh/ty/issues/2310
synced_at: 2026-01-10T01:48:23Z
---

# VS Code Extension regularly crashes

---

_Issue opened by @npaolini2634 on 2026-01-02 22:37_

### Summary

I've been using ty and the vscode extension for a few months now. I pretty regularly get the message in vscode saying "ty has crashed 5 times and will not be restarted".  Previously, I never looked at the logs, but it happened again just now and I've attached the log. It looks like many files take over 100ms, then a worker gets a stack overflow. Sorry if this is the wrong place to put vscode extension issues.

[ty Language Server.log](https://github.com/user-attachments/files/24412542/ty.Language.Server.log)

### Version

ty 0.0.8

---

_Label `fatal` added by @AlexWaygood on 2026-01-02 22:40_

---

_Comment by @MichaReiser on 2026-01-03 10:29_

> Sorry if this is the wrong place to put vscode extension issues.

No need to be sorry. We'll move issues if they aren't in the right place (but yours is).

This is tricky to narrow down without access to the code. Can you try to run `ty` on the CLI with `TY_MAX_PARALLELISM=1 ty check -vv`. It should then run in single-threaded mode and log what files it's currently checking. That should help us to narrow down which (first-party file) contains the code on which ty currently stack overflows.

There's also an existing issue, any chance this is something you use in your code? https://github.com/astral-sh/ty/issues/2039

---

_Label `needs-info` added by @MichaReiser on 2026-01-03 10:29_

---

_Comment by @npaolini2634 on 2026-01-06 21:14_

Okay thanks, I've attached the output of `TY_MAX_PARALLELISM=1 ty check -vv`

We do little to no recursion so I don't think recursive callables would be the source. I had a quick look and didn't find anything matching that pattern.

If you wanted to see the contents of one or two files, I could probably share that.

[ty -vv.txt](https://github.com/user-attachments/files/24459898/ty.-vv.txt)

---

_Comment by @npaolini2634 on 2026-01-06 21:26_

Is it helpful to have anymore logs of overflows? Here's another one that's a bit shorter that happened today.

[2026-01-06 ty.log](https://github.com/user-attachments/files/24460092/2026-01-06.ty.log)

---

_Renamed from "VS Code Extension regular crashes" to "VS Code Extension regularly crashes" by @AlexWaygood on 2026-01-06 21:36_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-07 00:21_

---

_Comment by @MichaReiser on 2026-01-07 09:25_

Thanks. The logs are very useful and allowed me to submit what I think should fix this issue, see https://github.com/astral-sh/ruff/pull/22433

What makes me draw this conclusion is that we only see the stack overflow when using the server, but not when using the CLI. I also assume that you use `diagnosticMode: "openFilesOnly"` (the default), in which case checking doesn't run on a rayon thread (for which we already set the higher stack size), but on one of the LSPs worker threads.


---

_Label `needs-info` removed by @MichaReiser on 2026-01-07 09:26_

---

_Label `needs-info` added by @MichaReiser on 2026-01-07 09:26_

---

_Label `server` added by @MichaReiser on 2026-01-07 09:26_

---

_Label `needs-info` removed by @MichaReiser on 2026-01-07 09:26_

---

_Closed by @MichaReiser on 2026-01-07 12:55_

---

_Comment by @npaolini2634 on 2026-01-07 17:53_

Okay I'm happy to try again whenever any new changes are merged in. I do use `"ty.diagnosticMode": "workspace",` however.

The other two settings changes I have are `"ty.inlayHints.variableTypes": false,`, and `"ty.inlayHints.callArgumentNames": false,`

---

_Comment by @MichaReiser on 2026-01-08 08:06_

Hmm, so maybe that wasn't it. We did a release yesterday. Could you give that a try?

---

_Comment by @npaolini2634 on 2026-01-08 18:32_

Okay I updated to the new version and after about 30 minutes I got a similar error. This one popped up different warning messages in vscode, but it looks like there's still an overflow in there.

[2026-01-08.log](https://github.com/user-attachments/files/24501866/2026-01-08.log)

---

_Comment by @MichaReiser on 2026-01-08 18:41_

The logs suggest that you're still on 0.0.8. Any chance you have ty installed globally or in your venv?

---

_Comment by @npaolini2634 on 2026-01-09 00:45_

Oops, I'm used to Pylance that uses its own bundled version by default. Will ty only use the bundled version if it doesn't find another version installed?

I've updated my venv to 0.0.10, and haven't had the language server crash yet today. I'll report back if it stays reliable or it fails again in the next few days.

---

_Comment by @MichaReiser on 2026-01-09 07:49_

> Oops, I'm used to Pylance that uses its own bundled version by default. Will ty only use the bundled version if it doesn't find another version installed?

There's a setting controlling the [extensions behavior](https://docs.astral.sh/ty/reference/editor-settings/#importstrategy). The default is to:

* The version installed in the selected venv
* A system wide installation
* The bundled version

ty always uses the bundled version when opening an untrusted workspace.

---

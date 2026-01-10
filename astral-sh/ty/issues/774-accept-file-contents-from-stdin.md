```yaml
number: 774
title: accept file contents from stdin
type: issue
state: open
author: jleechpe
labels:
  - cli
  - wish
assignees: []
created_at: 2025-07-07T21:07:26Z
updated_at: 2025-10-06T15:31:24Z
url: https://github.com/astral-sh/ty/issues/774
synced_at: 2026-01-10T02:06:24Z
```

# accept file contents from stdin

---

_Issue opened by @jleechpe on 2025-07-07 21:07_

### Question

I'm trying to test out `ty` using Emacs Flymake to display diagnostics.  With `ruff` I can pass the modified contents as `stdin` and use `--stdin-filename` to get it to behave as expected prior to saving the modified file.  With `ty` I only get diagnostics after saving since there doesn't seem to be any way to give it file contents over stdin and then get a report.

### Version

ty 0.0.1-alpha.13

---

_Label `question` added by @jleechpe on 2025-07-07 21:07_

---

_Comment by @MichaReiser on 2025-07-08 06:24_

Checking files from stdin is currently not supported. We could add support for checking files from stdin if there's a good use case for it. 

I don't think running ty's regular CLI in an editor gives a great user experience. It may work fine for very small projects but it will become noticably laggy once working on large projects because ty has to start its analysis from scratch everytime you save a file. While ty's fast, this will take significantely longer than when you using ty's LSP (long running server process). 

I'm not an emacs user but does flymake, by any chance, support connecting to an lsp.

---

_Comment by @AlexWaygood on 2025-07-08 11:58_

We previously discussed this a bit internally at https://discord.com/channels/1039017663004942429/1228460843033821285/1358449954397491231 (most folks external to Astral won't be able to use that link -- sorry!). Mypy and Python itself don't support reading code from stdin but they both have a [`-c` flag](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-c) which I use a lot. I frequently find myself wanting something similar when debugging issues with ty, though I don't mind too much whether it's an option to read from stdin, a `-c` flag similar to Python/mypy, or both.

---

_Comment by @dhruvmanila on 2025-07-08 12:30_

> I'm trying to test out `ty` using Emacs Flymake to display diagnostics.

You could use [Eglot](https://github.com/joaotavora/eglot) in Emacs to use ty's language server. We don't have docs for Emacs setup using ty server but it should be similar to the one in Ruff (https://docs.astral.sh/ruff/editors/setup/#emacs). Sorry, I'm not an Emacs user. @ntBre might be able to help if you have any questions.

---

_Comment by @ntBre on 2025-07-08 12:41_

I haven't set up ty in Emacs yet, and I don't use flymake/flycheck very much, but in my flycheck buffer it looks like all of my diagnostics are coming from the LSP, so hopefully that is a promising suggestion.

But yes, happy to help!

---

_Comment by @jleechpe on 2025-07-08 13:19_

> > I'm trying to test out `ty` using Emacs Flymake to display diagnostics.
> 
> You could use [Eglot](https://github.com/joaotavora/eglot) in Emacs to use ty's language server. We don't have docs for Emacs setup using ty server but it should be similar to the one in Ruff (https://docs.astral.sh/ruff/editors/setup/#emacs). Sorry, I'm not an Emacs user. [@ntBre](https://github.com/ntBre) might be able to help if you have any questions.

The problem is that Eglot only directly supports one LSP server at a time (it looks like there may finally be a tool ([lspx](https://github.com/thefrontside/lspx)) to help with that).  Right now I have a toggle to cycle between pyright and ruff depending on needs, adding one for ty would be more complex than just adding it to flymake (although then I found out about https://github.com/emacs-lsp/lsp-mode/issues/2808 where flymake/eglot don't support multiple diagnostics for the same line coming from the same source.

I'll test with flycheck to see if my diagnostics look right (with lspx I can see the ty diagnostics pop up momentarily then pyright overwrites them since it returns slower)

---

_Comment by @MeGaGiGaGon on 2025-10-04 00:29_

I would very much like this. Even if `-c` is already taken, `ty` could support passing `-` as the file name, which is the other usual take from stdin. Sometimes I just want to write a quick sanity check, so it would be nice to be able to do `echo <code here> | ty check -`

---

_Renamed from "Can ty accept file contents from stdin?" to "accept file contents from stdin" by @carljm on 2025-10-06 15:31_

---

_Label `question` removed by @carljm on 2025-10-06 15:31_

---

_Label `cli` added by @carljm on 2025-10-06 15:31_

---

_Label `wish` added by @carljm on 2025-10-06 15:31_

---

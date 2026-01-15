```yaml
number: 22568
title: "Formatter: Less strict enforcement of blank lines"
type: issue
state: open
author: MichaReiser
labels:
  - formatter
  - needs-decision
  - style
assignees: []
created_at: 2026-01-14T10:53:01Z
updated_at: 2026-01-15T07:02:10Z
url: https://github.com/astral-sh/ruff/issues/22568
synced_at: 2026-01-15T07:48:15Z
```

# Formatter: Less strict enforcement of blank lines

---

_@MichaReiser_

Add a formatter option so that it is less strict about the number of empty lines between statements. More specifically: Preserve the number of empty lines but no more than 2.

Related to https://github.com/astral-sh/ruff/issues/18448


The original request:

Opening this to get some feedback on if this is a feature that would be more widely useful

Similar to Black's `--pyi` mode:

```
  --pyi                           Format all input files like typing stubs
                                  regardless of file extension. This is useful
                                  when piping source on standard input.
```

We use it in mdtests today to get more concise formatting. 

Or add a similar option that allows for more concise formatting in documentation (markdown files, code snippets in docstrings)

A design question here is: If a user specifies `--pyi`, does that take precedence over the language specified on the fenced code block? If yes, this becomes useless for ty's mdtests 😆 

---

_Label `cli` added by @MichaReiser on 2026-01-14 10:53_

---

_Label `formatter` added by @MichaReiser on 2026-01-14 10:53_

---

_Comment by @MichaReiser on 2026-01-14 12:16_

Having thought more about this, I'm coming to the conclusion that we definitely shouldn't support `--pyi` because:

* Ruff uses `--stdin-filename` to determine the extension of files when passing them via stdin
* Looking at the diff in https://github.com/astral-sh/ruff/pull/22566, the main concern is that the "normal" mode is stricter about where it inserts blank lines and line breaks. One solution to this problem is to make the normal mode less strict about enforcing blank lines. Something I'm very empathetic with, but this is a larger discussion. However, allowing users to specify `--pyi` is effectively asking the formatter to support two formatting styles: "compact" and "normal". I'm not sure if the pyi formatting captures "compact" well (it's more compact, but I'm not sure if all the special casing for pyi should apply to all Python code in documentation). It also opens us op to supporting even more style guides which has a pretty high bar for me. 

TLDR: I'd be open to adding an option that makes Ruff less strict about enforcing blank lines (specifically, allowing 1-2 empty lines at the statement level, everywhere, and up to 1 blank line between expressions, matching Prettier's behavior). But that's a separate feature request and something entirely separate of formatting fenced code blocks in markdown files


Related to https://github.com/astral-sh/ruff/issues/18448

---

_Closed by @MichaReiser on 2026-01-14 12:16_

---

_Reopened by @MichaReiser on 2026-01-14 12:17_

---

_Label `cli` removed by @MichaReiser on 2026-01-14 12:17_

---

_Label `style` added by @MichaReiser on 2026-01-14 12:17_

---

_Renamed from "Formatter: Support `--pyi` mode" to "Formatter: Less strict enforcement of blank lines" by @MichaReiser on 2026-01-14 12:18_

---

_Comment by @MichaReiser on 2026-01-14 12:45_

For markdown files. The solution is to change the language on the fenced code block from `py` to `pyi`. 

Unfortunately, this might not always be a feasible option in ty if it's a fenced code block without a preceding file name (`**file.py**`). But it's probably feasible in many instances, which should be enough to mitigate the worst blank line regressions

---

_Comment by @leonace924 on 2026-01-15 05:05_

Good morning @MichaReiser,
would you assign me this issue as well?


---

_Comment by @MichaReiser on 2026-01-15 07:01_

Sorry. This is not ready for implementation 

---

_Label `needs-decision` added by @MichaReiser on 2026-01-15 07:02_

---

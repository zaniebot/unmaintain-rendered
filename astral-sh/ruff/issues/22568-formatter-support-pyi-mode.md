```yaml
number: 22568
title: "Formatter: Support `--pyi` mode"
type: issue
state: open
author: MichaReiser
labels:
  - cli
  - formatter
assignees: []
created_at: 2026-01-14T10:53:01Z
updated_at: 2026-01-14T10:58:27Z
url: https://github.com/astral-sh/ruff/issues/22568
synced_at: 2026-01-14T11:33:16Z
```

# Formatter: Support `--pyi` mode

---

_@MichaReiser_

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

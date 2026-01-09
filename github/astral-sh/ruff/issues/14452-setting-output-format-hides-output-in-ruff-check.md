---
number: 14452
title: setting output format hides output in ruff check
type: issue
state: open
author: kaddkaka
labels:
  - cli
  - needs-design
assignees: []
created_at: 2024-11-19T13:37:45Z
updated_at: 2024-11-27T14:37:28Z
url: https://github.com/astral-sh/ruff/issues/14452
synced_at: 2026-01-07T13:12:16-06:00
---

# setting output format hides output in ruff check

---

_Issue opened by @kaddkaka on 2024-11-19 13:37_

When running a `ruff check --fix-only` there is no output about what errors were fixed. This surprised me, I thought it would still print the fixed error since the [documentation about `--fix-only`](https://docs.astral.sh/ruff/settings/#fix-only) says "Like [fix](https://docs.astral.sh/ruff/settings/#fix), but disables reporting on leftover violation. Implies [fix](https://docs.astral.sh/ruff/settings/#fix)."

Maybe supplying `--show-fixes` will fix it? Yes,

`ruff check --fix-only --show-fixes` shows the fixed errors, great!

Can we also get this info in json output format? No,

`ruff check --fix-only --show-fixes --output-format json`  print nothing! `--output-format junit` has the same issue.

Ruff version: 0.7.0

---

_Comment by @zanieb on 2024-11-19 20:30_


```
❯ echo "import b, a" > example.py
❯ ruff check example.py --select I
example.py:1:1: I001 [*] Import block is un-sorted or un-formatted
  |
1 | / import b, a
  |
  = help: Organize imports

Found 1 error.
[*] 1 fixable with the `--fix` option.

❯ ruff check example.py --select I --fix-only
Fixed 1 error.

❯ echo "import b, a" > example.py
❯ ruff check example.py --select I --fix
Found 1 error (1 fixed, 0 remaining).

❯ ruff check example.py --select I --fix-only --output-format json

❯ echo "import b, a" > example.py
❯ ruff check example.py --select I --fix --output-format json
[]%                        

❯ echo "import b, a" > example.py
❯ ruff check example.py --select I --fix --output-format json --show-fixes
[]%                          

❯ echo "import b, a" > example.py
❯ ruff check example.py --select I --fix-only --show-fixes

Fixed 1 error:
- example.py:
    1 × I001 (unsorted-imports)

Fixed 1 error.                                                                                                                                  
```

I don't think this has to do with `--fix-only`, i.e., it applies equally to `--fix`. I think we should improve the JSON output here, though I don't think it should rely on the `--show-fixes` flag.

---

_Label `cli` added by @MichaReiser on 2024-11-20 08:04_

---

_Label `needs-design` added by @MichaReiser on 2024-11-20 08:04_

---

_Comment by @MichaReiser on 2024-11-20 08:06_

I suspect that Ruff throws away the diagnostics after each fix cycle. We would need to preserve them and pass them through to the json emitter. This likely requires some special casing in the CLI because we don't want to emit the diagnostics for all output formats (which makes the experience somewhat inconsistent)

---

_Comment by @kaddkaka on 2024-11-27 14:37_

> I suspect that Ruff throws away the diagnostics after each fix cycle. We would need to preserve them and pass them through to the json emitter. This likely requires some special casing in the CLI because we don't want to emit the diagnostics for all output formats (which makes the experience somewhat inconsistent)

Don't you want to keep all diagnostics until the end and then emit or not depending on the output format? 

---

_Referenced in [astral-sh/ruff#16831](../../astral-sh/ruff/issues/16831.md) on 2025-03-18 16:02_

---

_Referenced in [astral-sh/ruff#20443](../../astral-sh/ruff/pulls/20443.md) on 2025-10-02 20:27_

---

---
number: 16239
title: PEP723-style comments inside a string literal still causes script-mode execution
type: issue
state: closed
author: flaport
labels:
  - bug
assignees: []
created_at: 2025-10-10T20:17:51Z
updated_at: 2025-10-14T11:32:12Z
url: https://github.com/astral-sh/uv/issues/16239
synced_at: 2026-01-07T13:12:19-06:00
---

# PEP723-style comments inside a string literal still causes script-mode execution

---

_Issue opened by @flaport on 2025-10-10 20:17_

### Summary

I have a script in my repo which generates uv scripts. Notice that it has a shebang, but not a shebang with `--script`. It also has some script templates defined as multi-line string inside.

```
#!/usr/bin/env uv run

my_script = '''
# /// script
# requires-python = ">=3.11"
# dependencies = ["requests"]
# ///
...

import project_dependency
...
```

where `project_dependency` is any dependency other than 'requests'.

We get an import error, even though `project_dependency` is listed in the pyproject.toml of my project (and since I'm calling uv run without the --script tag it should get it from there)

When I remove the multi-line string it works!

I think uv might be parsing the PEP723-style comments from anywhere in the file, even inside multi-line strings.


### Platform

macos

### Version

0.9.2

### Python version

3.12.10

---

_Label `bug` added by @flaport on 2025-10-10 20:17_

---

_Comment by @geofft on 2025-10-14 11:32_

This is, unfortunately for your use case, intentional on the part of the [PEP 723](https://peps.python.org/pep-0723/) specification, which specifically does not require Python parsing to determine whether a block is actually a PEP 723 comment or not: "Furthermore, Python’s syntax changes in every release. If extracting dependency data needs a Python parser, the parser will need to know which version of Python the script is written for, and the overhead for a generic tool of having a parser that can handle _multiple_ versions of Python is unsustainable." See also the reference implementation and reference regular expression, which do not do anything special to avoid finding these comment blocks inside multi-line string literals.

There was an extended previous discussion in #12303 with some input from the author of the PEP. uv's behavior here is consistent with other tools that implement PEP 723 (with one exception I know of, pex-tool/pex#2722), and if nothing else, I think it would be more confusing if we deviated from the spec / the consensus reading of it.

See if you can figure out some way to make that inline block not match the spec, e.g. indenting it and wrapping it with `textwrap.dedent` or something.

---

_Closed by @geofft on 2025-10-14 11:32_

---

```yaml
number: 13278
title: "ERA001 stops parsing an inline script metadata block at the first `# ///` line"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2024-09-07T14:07:54Z
updated_at: 2024-09-09T18:47:41Z
url: https://github.com/astral-sh/ruff/issues/13278
synced_at: 2026-01-10T11:09:55Z
```

# ERA001 stops parsing an inline script metadata block at the first `# ///` line

---

_Issue opened by @dscorbett on 2024-09-07 14:07_

[ERA001](https://docs.astral.sh/ruff/rules/commented-out-code/) skips PEP 723 inline script metadata blocks as of #10455, except that it stops skipping after the first `# ///` line. However, a metadata block continues if the following lines are valid embedded content followed by another `# ///` line. ERA001 should skip over the full metadata block.

For example, this script (named era001.py) contains metadata with an embedded `# ///` line.
```python
# /// script
# [tool.uv]
# extra-index-url = ["https://pypi.org/simple", """\
# https://example.com/
# ///
# """
# ]
# ///
print(1)
```

The script works, which shows that the first `# ///` line does not end the block. (I am using uv, but this could work with any tool that interprets inline metadata.)
```console
$ uv version
uv 0.4.7 (a178051e8 2024-09-07)
$ uv run era001.py
Reading inline script metadata from: era001.py
1
```

However, Ruff reports an error.
```console
$ ruff --version
ruff 0.6.4
$ ruff check --isolated --select ERA001 era001.py
era001.py:7:1: ERA001 Found commented-out code
  |
5 | # ///
6 | # """
7 | # ]
  | ^^^ ERA001
8 | # ///
9 | print(1)
  |
  = help: Remove commented-out code

Found 1 error.
```

---

_Label `bug` added by @MichaReiser on 2024-09-08 07:47_

---

_Label `rule` added by @MichaReiser on 2024-09-08 07:47_

---

_Comment by @MichaReiser on 2024-09-08 07:47_

Thanks. From the PEP:

> Precedence for an ending line # /// is given when the next line is not a valid embedded content line as described above. For example, the following is a single fully valid block:

```python
# /// some-toml
# embedded-csharp = """
# /// <summary>
# /// text
# ///
# /// </summary>
# public class MyClass { }
# """
# ///
```

---

_Assigned to @MichaReiser by @MichaReiser on 2024-09-08 07:47_

---

_Closed by @MichaReiser on 2024-09-09 18:47_

---

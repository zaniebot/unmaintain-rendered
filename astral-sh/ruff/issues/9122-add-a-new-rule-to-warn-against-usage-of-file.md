---
number: 9122
title: "Add a new rule to warn against usage of `__file__` module attribute"
type: issue
state: open
author: edgarrmondragon
labels:
  - rule
assignees: []
created_at: 2023-12-13T23:24:13Z
updated_at: 2024-07-29T01:04:20Z
url: https://github.com/astral-sh/ruff/issues/9122
synced_at: 2026-01-10T01:22:48Z
---

# Add a new rule to warn against usage of `__file__` module attribute

---

_Issue opened by @edgarrmondragon on 2023-12-13 23:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Using the `__file__` module attribute can be problematic in library code. According to PyOxydizer:

<blockquote>
<p class="last">Use of <code class="docutils literal notranslate"><span class="pre">__file__</span></code> is commonly encountered in code loading <em>resource
files</em>. See <a class="reference internal" href="oxidized_importer_resource_files.html#resource-files"><span class="std std-ref">Loading Resource Files</span></a> for more on this topic, including
how to port code to more modern Python APIs for loading resources.</p>
</blockquote>

This is also a problem for [zipapps](https://docs.python.org/3/library/zipapp.html).

Using `__file__` is fine for scripts, so if this accepted I'm not sure what _plugin_ this make the most sense to live in.

---

_Renamed from "Add a new rule to warn against usage of `__file__`" to "Add a new rule to warn against usage of `__file__` module attribute" by @edgarrmondragon on 2023-12-13 23:24_

---

_Label `rule` added by @charliermarsh on 2024-01-25 05:12_

---

_Comment by @nathanjmcdougall on 2024-07-26 03:09_

Some further writeup on the issue:
https://github.com/indygreg/PyOxidizer/issues/69#issue-463012985

---

_Comment by @edgarrmondragon on 2024-07-29 01:04_

I wonder if this makes sense as part of `RUF`

---

```yaml
number: 4034
title: "Support `flake8-typing-as-t`"
type: issue
state: closed
author: edgarrmondragon
labels:
  - rule
assignees: []
created_at: 2023-04-20T00:55:12Z
updated_at: 2023-04-23T04:40:38Z
url: https://github.com/astral-sh/ruff/issues/4034
synced_at: 2026-01-12T15:54:44Z
```

# Support `flake8-typing-as-t`

---

_@edgarrmondragon_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

PyPI: https://pypi.org/project/flake8-typing-as-t/

<blockquote>
<p>This is a <code>flake8</code> plugin which ensures that imports from the <code>typing</code> library must be written using <code>import typing as t</code>.</p>

<ul>
<li><code>TYT01</code>: Bare <code>import typing</code> usage</li>
<li><code>TYT02</code>: <code>import typing as X</code> where <code>X</code> is not literal <code>t</code></li>
<li><code>TYT03</code>: <code>from typing import X</code> usage</li>
</ul>
</blockquote>

`TYT01` and `TYT02` could be covered by [`ICN001`](https://beta.ruff.rs/docs/rules/unconventional-import-alias/#aliases) if the following config is added:

```toml
[tool.ruff.flake8-import-conventions.extend-aliases]
"typing" = "t"
```

but `TYT03` is still valuable to have.

Another option is to extend `flake-import-conventions` to flag cases like

```python
from pandas import DataFrame  # Use `import as pd` and `pd.DataFrame` instead
```

---

_Comment by @charliermarsh on 2023-04-20 03:44_

My vote would be to find a way to enable this within the import conventions plugin since it's so closely related.

---

_Label `rule` added by @charliermarsh on 2023-04-20 03:44_

---

_Comment by @edgarrmondragon on 2023-04-21 23:50_

I've gone for a new `ICN003` rule that bans `from ... import ...` for any configured modules

---

_Closed by @charliermarsh on 2023-04-23 04:40_

---

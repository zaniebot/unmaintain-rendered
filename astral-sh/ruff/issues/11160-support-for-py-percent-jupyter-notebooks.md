```yaml
number: 11160
title: "Support for py:percent jupyter notebooks"
type: issue
state: closed
author: bluss
labels: []
assignees: []
created_at: 2024-04-26T08:46:47Z
updated_at: 2025-12-15T09:42:13Z
url: https://github.com/astral-sh/ruff/issues/11160
synced_at: 2026-01-12T15:54:50Z
```

# Support for py:percent jupyter notebooks

---

_@bluss_

Does ruff support py:percent files? py:percent is one of the plain python formats jupytext supports, and it's the one I use for jupyter notebooks in git (not using .ipynb other as an intermediate product in execution/build), and it's also supported by vscode.

Something like

- automatically using jupyter style rules for py:percent files
- Or, identify py:percent files as a separate kind of file and allow setting separate rules for these
- Or, allow inline code comment to set jupyter style rules for a source file
- The formatter removes extra `;` but this rule should be suppressed in a notebook 


About the py:percent format: https://jupytext.readthedocs.io/en/latest/formats-scripts.html

---

_Comment by @dhruvmanila on 2024-04-26 14:44_

Hey, thanks for opening this issue!

By "py:percent" files, do you mean a regular Python files containing `%%` comments as a way of creating the cell boundaries? We don't currently support that. I think https://github.com/astral-sh/ruff/issues/8800 is a similar request.

---

_Comment by @dhruvmanila on 2024-04-26 14:46_

I'll merge this with the linked issue.

---

_Closed by @dhruvmanila on 2024-04-26 14:46_

---

_Comment by @bluss on 2024-04-26 15:18_

Ok, fine for me. I found the previous issue but it seemed to be about markdown-framed formats, so was not sure if this would be included.

---

_Comment by @dhruvmanila on 2024-04-26 15:45_

No worries. I updated the issue title to contain all formats supported by Jupytext which will be easier to track :)

---

_Comment by @Norlandz on 2025-12-13 09:59_

Was trying to find a way to tell Ruff to stop complaining about the Jupyter magic commands with `%`. In jupytext py:percent files.
Not sure if this is the right place to ask.

eg something like:
```
# %%

# ruff ignore start
%load_ext autoreload
%autoreload 2
# ruff ignore end

do_this()

# %%
import os
do_that()
```

---

_Comment by @dhruvmanila on 2025-12-15 09:42_

Ruff supports parsing those magic commands if they're in a Jupyter notebook but it doesn't recognize the py:percent format in Jupytext. Refer to #8800 

---

```yaml
number: 1052
title: Reopening notebook breaks ty language server partly
type: issue
state: closed
author: Xiao-Chenguang
labels:
  - needs-info
  - server
assignees: []
created_at: 2025-08-19T15:51:22Z
updated_at: 2025-08-19T17:32:45Z
url: https://github.com/astral-sh/ty/issues/1052
synced_at: 2026-01-10T02:06:24Z
```

# Reopening notebook breaks ty language server partly

---

_Issue opened by @Xiao-Chenguang on 2025-08-19 15:51_

### Summary

Reopening a `notebook` containing same code blocks in another `.py` file breaks ty language server.

In the `example.ipynb` and `example.py` containing the same code block:
```python
import pandas as pd

df = pd.read_csv("data.csv")
print(df.head())
```

Open the `example.py` and then `example.ipynb`, ty works as expected as the figure

<img width="1615" height="544" alt="Image" src="https://github.com/user-attachments/assets/c698a340-fa0b-426c-9276-bd59c5b89046" />

Then colse and reopen `example.ipynb` will cause wrong segment highlight and wrong hover respond as the figure:
<img width="1620" height="549" alt="Image" src="https://github.com/user-attachments/assets/a0d8892f-0d7c-47dd-8cb3-1c8f7583ef1d" />

Tested on version **2025.35.12261122** and **2025.37.12311339**, the issue persists. I did not dig deep into the problem, while the trace of ty language server looks normal. Looks like some thing wrong with the cache used by ty.

### Version

0.0.1-alpha.19 with ty-vscode 2025.37.12311339

---

_Comment by @dhruvmanila on 2025-08-19 15:58_

Thank you for the report!

Currently, ty doesn't support notebooks, there's a tracking issue over at #173.

Can you clarify which screenshot is the one with the notebook as it's hard to understand? 😅

@MichaReiser If the client is still sending requests for notebooks to ty like hover, semantic tokens, etc., I think we might want to restrict the snapshots to only be created for Python files. Currently, they would be created for notebooks as well which might be confusing to users when ty hasn't declared official support for notebooks. I haven't checked this yet.

---

_Comment by @dhruvmanila on 2025-08-19 16:02_

Huh, not sure why is VS Code sending requests for notebooks when the server hasn't registered for it. I guess it might be because each cell is still a Python text document while the server doesn't receive any requests related to notebooks.

<img width="2560" height="1412" alt="Image" src="https://github.com/user-attachments/assets/0e606e35-f1be-4d37-ae49-485df535176a" />

---

_Label `server` added by @carljm on 2025-08-19 16:04_

---

_Comment by @Xiao-Chenguang on 2025-08-19 16:07_

> Can you clarify which screenshot is the one with the notebook as it's hard to understand? 😅

Both screenshot is with a notebook. The difference is the first comes from the notebook is opened the first time. The sccond screenshot is a result of closing and reopening the notebook.

Steps:
1. Open `.py` -> works fine.
2. Open `.notebook` containing same code block as `.py` -> works fine. (screenshot 1)
3. Close and reopen `.notebook` -> ty breaks. (screenshot 2)

---

_Comment by @MichaReiser on 2025-08-19 16:49_

Maybe it's because of 

https://github.com/astral-sh/ty-vscode/blob/3b77f79626a64b7e0d07d172ad06e9d3d98fe95b/package.json#L53-L57

---

_Comment by @MichaReiser on 2025-08-19 17:07_

The first screenshot doesn't look like ty to me. ty doesn't support multiline formatting of types (there's a PR but it's not yet merged https://github.com/astral-sh/ruff/pull/19979). So I suspect the first screenshot is a different LSP providing the hover over ty. Do you have pylance or pyright (or any other python LSP installed?)

---

_Comment by @Xiao-Chenguang on 2025-08-19 17:10_

> The first screenshot doesn't look like ty to me. ty doesn't support multiline formatting of types (there's a PR but it's not yet merged [astral-sh/ruff#19979](https://github.com/astral-sh/ruff/pull/19979)). So I suspect the first screenshot is a different LSP providing the hover over ty. Do you have pylance or pyright (or any other python LSP installed?)

Yes I have Pylance alongside ty. I thought ty would mute pylance automatically. I'll try reproduce the issue with pylance disabled manually.

---

_Comment by @Xiao-Chenguang on 2025-08-19 17:18_

> > The first screenshot doesn't look like ty to me. ty doesn't support multiline formatting of types (there's a PR but it's not yet merged [astral-sh/ruff#19979](https://github.com/astral-sh/ruff/pull/19979)). So I suspect the first screenshot is a different LSP providing the hover over ty. Do you have pylance or pyright (or any other python LSP installed?)
> 
> Yes I have Pylance alongside ty. I thought ty would mute pylance automatically. I'll try reproduce the issue with pylance disabled manually.

I can confirm that without Pylance, screenshot 1 is the same as 2. That' to say:

1. Open `.py` -> (screenshot 2)
2. Open `.notebook` containing same code block as `.py` ->  (screenshot 2)
3. Close and reopen `.notebook` -> (screenshot 2)

Now the problem becomes the semantic highlight for `pandas` and reference to `pd.read_csv` is not displayed as expected.


---

_Comment by @MichaReiser on 2025-08-19 17:22_

Hmm, I must be doing something wrong because I'm not able to reproduce this.

https://github.com/user-attachments/assets/f57a43d9-4891-49ba-b999-1efb6c91ce99



---

_Comment by @MichaReiser on 2025-08-19 17:23_

Edit: Or are you saying that the issue now just is that `pandas` doesn't use the same highlighting as Pylance?

---

_Label `needs-info` added by @MichaReiser on 2025-08-19 17:23_

---

_Comment by @Xiao-Chenguang on 2025-08-19 17:27_

Yes I have being using ty along pylance for a while, I got exactly the same result as you now with pylance disabled. I thought the highlight and reference in screenshot 1 was provided by ty while it's not.

If the semantic highlight and multiline is still in development, pleaes feel free to close this.

---

_Comment by @MichaReiser on 2025-08-19 17:32_

Okay, that makes sense. I'll merge this into https://github.com/astral-sh/ty/issues/791 as highlights for symbols from other modules is explicitly listed as a TODO

---

_Closed by @MichaReiser on 2025-08-19 17:32_

---

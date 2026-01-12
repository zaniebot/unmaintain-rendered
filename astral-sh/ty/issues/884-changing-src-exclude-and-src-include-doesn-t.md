```yaml
number: 884
title: "Changing `src.exclude` and `src.include` doesn't reload the project files"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-07-24T14:19:47Z
updated_at: 2025-08-22T12:54:16Z
url: https://github.com/astral-sh/ty/issues/884
synced_at: 2026-01-12T15:54:24Z
```

# Changing `src.exclude` and `src.include` doesn't reload the project files

---

_@MichaReiser_

### Summary

I noticed this in VS Code. Diagnostics for files in `.github` didn't get cleared after I added `.github` to `src.exclude`. I'm not sure if it's related to workspace diagnostics. It does seem that the configuration is correctly reloaded.

### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-07-24 14:19_

---

_Label `server` added by @MichaReiser on 2025-07-24 14:19_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:28_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:38_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:38_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-08-22 12:39_

---

_Comment by @MichaReiser on 2025-08-22 12:54_

I'm no longer able to reproduce this anymore. What I tried:

* added a `.github` folder
* added a `.github/test.py` file with `print(a)`
* Added `.github` to `exclude`

I also opened langchain and added 

```toml
[tool.ty.src]
exclude = ["scripts", ".github", "**/*.ipynb", "docs", "**/integration_template"]

[tool.ty.environment]
root = [
    ".",
    "libs/core", 
    "libs/langchain", 
    "libs/text-splitters", 
    "libs/standard-tests",
    "libs/cli",
    "libs/partners/openai",
    "libs/partners/anthropic", 
    "libs/partners/chroma",
    "libs/partners/fireworks",
    "libs/partners/groq",
    "libs/partners/mistralai"
]
```

which resolved all errors in the files in the `.github` folder


So much to fixing something today...

---

_Closed by @MichaReiser on 2025-08-22 12:54_

---

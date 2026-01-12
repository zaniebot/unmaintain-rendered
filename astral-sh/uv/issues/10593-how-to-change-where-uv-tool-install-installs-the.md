```yaml
number: 10593
title: "How to change where `uv tool install` installs the binaries?"
type: issue
state: closed
author: dezlymacauley
labels:
  - question
  - configuration
assignees: []
created_at: 2025-01-14T12:36:14Z
updated_at: 2025-01-14T16:39:49Z
url: https://github.com/astral-sh/uv/issues/10593
synced_at: 2026-01-12T16:00:17Z
```

# How to change where `uv tool install` installs the binaries?

---

_@dezlymacauley_

**uv version:** 
0.5.18 (27d1bad55 2025-01-11)
___
When installing packages like litecli using the following command:
`uv tool install litecli`

This is the default behavior of uv:
```
❯ uv tool dir
/home/dezly-macauley/.local/share/uv/tools

~
❯ uv tool dir --bin
/home/dezly-macauley/.local/bin
```
___
I have created a custom directory that I want uv to use instead:
`.pypi-global-pkgs`

I added this to my .zshrc
export UV_TOOL_DIR="$HOME/.pypi-global-pkgs"

___
**When I run these commands again, I get this output:**
```
❯ uv tool dir
/home/dezly-macauley/.pypi-global-pkgs

~
❯ uv tool dir --bin
/home/dezly-macauley/.local/bin
```

How do I set `uv tool dir --bin` to use

home/dezly-macauley/.pypi-global-pkgs/bin ?


---

_Comment by @FishAlchemist on 2025-01-14 13:08_

Is [UV_TOOL_BIN_DIR](https://docs.astral.sh/uv/configuration/environment/#uv_tool_bin_dir)?

---

_Comment by @charliermarsh on 2025-01-14 13:42_

Yeah, you want https://docs.astral.sh/uv/configuration/environment/#uv_tool_bin_dir.

---

_Closed by @charliermarsh on 2025-01-14 13:42_

---

_Label `question` added by @charliermarsh on 2025-01-14 13:42_

---

_Label `configuration` added by @charliermarsh on 2025-01-14 13:42_

---

_Comment by @dezlymacauley on 2025-01-14 16:39_

@FishAlchemist and @charliermarsh 

🤘 Thank you! My uv setup is working now!

---

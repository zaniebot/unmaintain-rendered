```yaml
number: 12268
title: Fast way to run inline scripts from git
type: issue
state: open
author: mrkbac
labels:
  - question
assignees: []
created_at: 2025-03-18T09:52:56Z
updated_at: 2025-03-28T18:51:18Z
url: https://github.com/astral-sh/uv/issues/12268
synced_at: 2026-01-12T16:00:59Z
```

# Fast way to run inline scripts from git

---

_@mrkbac_

### Question

Hey,

I have a repo with multiple script with inline meta data, I was wondering if there is a fast way to directly run them from git.

For example with `uvx` I could use the `git+http://....` syntax, but it always expects a package not a script. Is there a way to easily do with one uv command?

Something like this `uvx git+https://github.com/user/repo/my_script.py`

Preferably it should use git in the background, since vscode automatically authenticated me for github. 

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @mrkbac on 2025-03-18 09:52_

---

_Comment by @konstin on 2025-03-28 18:51_

Using the URL from the raw view on github, you can use `uv run`:

```
uv run https://raw.githubusercontent.com/konstin/say-hi/refs/heads/main/say-hi.py
```

We currently don't support this for the git protocol though.

---

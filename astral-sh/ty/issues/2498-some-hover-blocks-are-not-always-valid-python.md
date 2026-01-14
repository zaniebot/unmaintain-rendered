```yaml
number: 2498
title: Some hover blocks are not always valid Python source
type: issue
state: open
author: rchl
labels: []
assignees: []
created_at: 2026-01-14T19:27:10Z
updated_at: 2026-01-14T19:28:31Z
url: https://github.com/astral-sh/ty/issues/2498
synced_at: 2026-01-14T19:38:28Z
```

# Some hover blocks are not always valid Python source

---

_@rchl_

### Summary

Code snippet displayed in the hover popups is not always a valid python source code. Some editors have more lenient syntax highlighting that makes it not really an issue, other have more strict highlighting that will introduce red error highlighting to indicate that the code is not valid. For example in ST we get something like this in some cases:

<img width="698" height="215" alt="Image" src="https://github.com/user-attachments/assets/08f69ac5-f2da-4fe9-a3d5-ddcbb75070d3" />


Perhaps `ty` can tell whether the snippet is a valid code or not and based on that return either a python code block or plain text code block?

### Version

0.0.10

---

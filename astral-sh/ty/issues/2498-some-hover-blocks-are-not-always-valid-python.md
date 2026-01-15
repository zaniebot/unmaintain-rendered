```yaml
number: 2498
title: Some hover blocks are not always valid Python source
type: issue
state: open
author: rchl
labels:
  - server
assignees: []
created_at: 2026-01-14T19:27:10Z
updated_at: 2026-01-15T05:15:21Z
url: https://github.com/astral-sh/ty/issues/2498
synced_at: 2026-01-15T05:50:18Z
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

_Label `server` added by @AlexWaygood on 2026-01-14 20:45_

---

_Comment by @dhruvmanila on 2026-01-15 05:15_

I think we use the `xml` language tag for such code blocks, I see the following hover content in Neovim:

````
```xml
<module 'collections.abc'>
```
````

Is it that your editor's syntax highlighting doesn't support XML tags correctly? Can you try opening a XML file with the above content and see how does your editor highlight that?

---

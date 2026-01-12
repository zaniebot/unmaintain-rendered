```yaml
number: 1012
title: "Language server offers code completions after `.` inside strings"
type: issue
state: closed
author: GriceTurrble
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-08-15T15:50:19Z
updated_at: 2025-10-17T08:11:28Z
url: https://github.com/astral-sh/ty/issues/1012
synced_at: 2026-01-12T15:54:24Z
```

# Language server offers code completions after `.` inside strings

---

_@GriceTurrble_

### Summary

With the VS Code extension enabled with language features, `ty` will attempt code completions from inside a string.

Typing the following into the editor:

```py
a = "something.
```

...shows completions starting with `ArithmeticError`.

This can also be reproduced in the playground:

<img width="586" height="409" alt="Image" src="https://github.com/user-attachments/assets/35d48d16-a09f-48fa-a676-0a4522d37e8e" />

This is particularly annoying if I'm typing a multiline docstring, which if I'm not watching the screen too closely ends up as:

```py
def something():
    """Whatever we want here.Args

    And more so.ArithmeticError
    and another line.ArithmeticError
    """
```

(muscle memory makes me now hit `Esc` after each `.` I type, which is getting annoying)

This is something that used to crop up due to the Jupyter extension, see this older SO link for details: https://stackoverflow.com/a/72825719

- disabling the `ty` extension makes this issue go away
- disabling language server features with the setting `"ty.disableLanguageServices": true` also makes this issue go away.

### Version

ty-vscode 2025.35.12261122

---

_Label `bug` added by @MichaReiser on 2025-08-15 15:51_

---

_Label `server` added by @MichaReiser on 2025-08-15 15:51_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 15:51_

---

_Comment by @MichaReiser on 2025-08-15 15:51_

Thanks for reporting this issue.

CC: @BurntSushi 

---

_Assigned to @BurntSushi by @BurntSushi on 2025-08-15 15:52_

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:49_

---

_Comment by @MichaReiser on 2025-10-17 08:11_

This is now fixed because the parser has now better handling for unclosed string literals (the fix was released with alpha.23)

---

_Closed by @MichaReiser on 2025-10-17 08:11_

---

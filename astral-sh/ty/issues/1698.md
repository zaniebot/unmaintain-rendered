```yaml
number: 1698
title: Make semantic highlighting optional
type: issue
state: closed
author: juhannc
labels:
  - needs-info
  - server
assignees: []
created_at: 2025-12-01T09:41:27Z
updated_at: 2025-12-01T11:15:44Z
url: https://github.com/astral-sh/ty/issues/1698
synced_at: 2026-01-10T01:58:59Z
```

# Make semantic highlighting optional

---

_Issue opened by @juhannc on 2025-12-01 09:41_

I've grown accustomed to the same semantic colors for years now. And while I really like ty for its speed, I don't like to have my colors changed. I propose making semantic coloring optional and not forcing the overwrite of the existing semantic coloring rules

---

_Comment by @MichaReiser on 2025-12-01 09:58_

Can you say more about what you used before? Did you use Pylance's semantic highlighting, and ty's highlighting is different from it? If so, could you share examples where the highlighting differs? This might also just be a bug in ty, where ty misclassifies a token.

Or did you not use any semantic highlighting before?

Note, that VS Code allows you to disable semantic highlighting, see https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide#enablement-of-semantic-highlighting

---

_Label `needs-info` added by @MichaReiser on 2025-12-01 09:58_

---

_Label `server` added by @MichaReiser on 2025-12-01 09:58_

---

_Comment by @juhannc on 2025-12-01 10:54_

This is super embarrassing, but it's working now 🙈
I uninstalled ty after my first test, and when I reinstalled it now to provide further info, it worked just fine, and my colors stayed as they were.
Sorry for troubling you!

---

_Closed by @juhannc on 2025-12-01 10:54_

---

_Comment by @MichaReiser on 2025-12-01 11:15_

No worries. I'm glad to hear that it's now working as expected

---

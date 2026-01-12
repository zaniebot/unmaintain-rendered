```yaml
number: 453
title: "`unresolved-import`: Add note if no third-party search path is configured"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-05-19T15:53:10Z
updated_at: 2025-05-19T21:24:26Z
url: https://github.com/astral-sh/ty/issues/453
synced_at: 2026-01-12T15:54:23Z
```

# `unresolved-import`: Add note if no third-party search path is configured

---

_@MichaReiser_

Let's add a note to the `unresolved-import` diagnostic hinting users towards configuring the path to their virtual environment, or setting `VIRTUAL_ENV` if no third party search path is set.

---

_Label `help wanted` added by @MichaReiser on 2025-05-19 15:53_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-19 15:53_

---

_Comment by @EmilyBZhang on 2025-05-19 17:07_

I can take a look at this one!

---

_Assigned to @EmilyBZhang by @AlexWaygood on 2025-05-19 17:09_

---

_Comment by @EmilyBZhang on 2025-05-19 19:13_

How do we want to display this warning to the user?

There are three options that I can think of right now:
1. In the description of the error (pictured, might be too wordy if it's printed every time)
2. Somewhere around the blue "info" shown here
3. At the end of the checks if there were any `unresolved-import` errors

<img width="988" alt="Image" src="https://github.com/user-attachments/assets/03d38b73-450c-410b-a70a-f6c5a4cdf824" />

---

_Comment by @MichaReiser on 2025-05-19 19:20_

The same as the info message. You can create one by adding a sub diagnostic with a info severity 

---

_Closed by @dcreager on 2025-05-19 21:24_

---

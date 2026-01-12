```yaml
number: 12040
title: "Remove `output-format=text`"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
assignees: []
created_at: 2024-06-26T08:14:11Z
updated_at: 2025-06-26T23:46:06Z
url: https://github.com/astral-sh/ruff/issues/12040
synced_at: 2026-01-12T15:54:51Z
```

# Remove `output-format=text`

---

_@MichaReiser_

https://github.com/astral-sh/ruff/pull/12010 (shipped in Ruff 0.5) made using `output-format=text` a hard error. The next deprecation step is to remove the option entirely in 0.6 or 0.7

---

_Added to milestone `v0.6` by @MichaReiser on 2024-06-26 08:14_

---

_Label `cli` added by @dhruvmanila on 2024-06-28 06:19_

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-08-12 14:01_

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-12 14:01_

---

_Closed by @MichaReiser on 2024-10-08 12:04_

---

_Comment by @simsong on 2025-06-26 19:43_

so how do you turn off all of the TTY escape codes in the output?

---

_Comment by @simsong on 2025-06-26 19:47_

Apparently you need to run with `NO_COLOR=1` environment variable set. ruff is using some magic Rust thing to figure out if it should colorize the output, and this fails when running inside an emacs compile window. You get stuff like this:

![Image](https://github.com/user-attachments/assets/8b26cd80-08fa-4ba9-accc-d8239e30ab26)

---

_Comment by @ntBre on 2025-06-26 20:09_

Another option, if you do want the colors, is to add this to your config:

```elisp
(require 'ansi-color)
(add-hook 'compilation-filter-hook 'ansi-color-compilation-filter)
```

That's what I have in my Emacs config!

![Image](https://github.com/user-attachments/assets/a393a7b9-4baa-43d8-be78-2d7a09e5d5ad)

I double-checked with `emacs -nw -Q` to make sure that was the important piece of configuration and that it works in the terminal.

---

_Comment by @simsong on 2025-06-26 23:46_

Woah. Thanks.

---

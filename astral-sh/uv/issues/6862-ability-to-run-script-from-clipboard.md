```yaml
number: 6862
title: Ability to run script from clipboard
type: issue
state: open
author: valentincalomme
labels:
  - wish
  - cli
assignees: []
created_at: 2024-08-30T12:31:59Z
updated_at: 2024-09-05T00:21:02Z
url: https://github.com/astral-sh/uv/issues/6862
synced_at: 2026-01-12T15:59:08Z
```

# Ability to run script from clipboard

---

_@valentincalomme_

I love that we can now run scripts that define their dependencies in a top-level comment. A feature that would be even more "fun" is adding the ability to run a script from the clipboard.

I often see code snippets online etc. and they typically have a "copy" button added to it. I would love it if I could just run it without having to first create a file etc.

On Mac, I currently do `pbpaste | uv run python` and it does do the trick, but it would be even better if `uv` had some sort of flag so that I could just run `uv run --from-clipboard` or even an alias `uvc run`

---

_Label `cli` added by @charliermarsh on 2024-08-30 12:51_

---

_Comment by @zanieb on 2024-09-03 13:51_

I think the pipe is more appropriate here, I don't really want to own reading from the clipboard.

Do you know of other tools that support this?

---

_Label `wish` added by @zanieb on 2024-09-05 00:21_

---

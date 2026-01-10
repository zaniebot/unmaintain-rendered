```yaml
number: 1568
title: "Add context sensitive keyword completions for `in` and `as` in some cases"
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-11-14T21:41:58Z
updated_at: 2025-11-14T21:41:58Z
url: https://github.com/astral-sh/ty/issues/1568
synced_at: 2026-01-10T02:06:25Z
```

# Add context sensitive keyword completions for `in` and `as` in some cases

---

_Issue opened by @BurntSushi on 2025-11-14 21:41_

Like we did in https://github.com/astral-sh/ruff/pull/21291, we should be able to provide very specific keyword completions for `in`. For example, `for name <CURSOR>`.

We might be able to do the same for `import module <CURSOR>`, but IMO that might be over-stepping a bit. The user could also just hit enter to go to the next line. The additional whitespace I think _suggests_ `as`, but it isn't required. We could offer `as` for `import module a<CURSOR>` though. And similarly for `from module import foo a<CURSOR>`. I think similar reasoning applies to `with`, `except` and `match` statements.

---

_Added to milestone `Stable` by @BurntSushi on 2025-11-14 21:41_

---

_Label `server` added by @BurntSushi on 2025-11-14 21:42_

---

_Label `completions` added by @BurntSushi on 2025-11-14 21:42_

---

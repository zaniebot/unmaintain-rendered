```yaml
number: 2464
title: "New CLI --field-{context,match}-separator-{prefix,postfix} flags"
type: issue
state: closed
author: vorburger
labels:
  - wontfix
assignees: []
created_at: 2023-03-19T13:48:57Z
updated_at: 2023-10-19T12:15:36Z
url: https://github.com/BurntSushi/ripgrep/issues/2464
synced_at: 2026-01-12T16:13:24Z
```

# New CLI --field-{context,match}-separator-{prefix,postfix} flags

---

_@vorburger_

Following up on #1842, it could be great and very useful if, in addition to the current flags introduced in #1871:

    --field-match-separator
    --field-context-separator

there were, in addition, also:

    --field-match-separator-postfix
    --field-match-separator-prefix
    --field-context-separator-prefix
    --field-context-separator-postfix

This would help when invoking ripgrep in an IDE, such as Visual Studio Code (VSC), to allow clicking on links detected in terminals. This is because with the defaulf of `rg` default, e.g.:

    .bazelrc:4:common --enable_bzlmod

link detection does not work. Using `--field-match-separator=": "` or `--field-match-separator=" :"` isn't right and doesn't work either.

What one would ideally want is (something like): `--field-match-separator-prefix=":" --field-match-separator-postfix=" "` so that it would print:

    .bazelrc:4 common --enable_bzlmod

PS: I guess one could work around this either by post-processing `rg` output, or perhaps with [a custom problem matcher in VSC](https://code.visualstudio.com/docs/editor/tasks#_defining-a-problem-matcher), but the flags would be neat.

---

_Comment by @BurntSushi on 2023-03-19 13:54_

This looks like a duplicate of #665 to me. You're suggesting a different approach here, but the high level problem you're trying to solve appears to be same.

IMO, VS Code should adapt to the format. It's standard grep format. VS Code should understand that. Many other editors already do.

---

_Comment by @BurntSushi on 2023-10-19 12:15_

Hyperlinks will be available as an opt-in feature in ripgrep 14. Given that your ultimate goal here is the ability to click on things and that's better solved by hyperlinks, I'm going to close this.

I probably would wind up closing this anyway as I'm not keen on just adding to a never-ending list of output control functions. This feature request is proposing _four_ new flags that are, to a first approximation, never going to be used. Not worth it.

---

_Closed by @BurntSushi on 2023-10-19 12:15_

---

_Label `wontfix` added by @BurntSushi on 2023-10-19 12:15_

---

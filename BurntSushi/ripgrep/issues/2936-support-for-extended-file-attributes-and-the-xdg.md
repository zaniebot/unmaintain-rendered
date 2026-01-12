```yaml
number: 2936
title: Support for extended file attributes and the xdg.robots recommendation.
type: issue
state: closed
author: blueforesticarus
labels:
  - wontfix
assignees: []
created_at: 2024-11-23T16:30:32Z
updated_at: 2024-11-23T16:57:22Z
url: https://github.com/BurntSushi/ripgrep/issues/2936
synced_at: 2026-01-12T16:13:25Z
```

# Support for extended file attributes and the xdg.robots recommendation.

---

_@blueforesticarus_

Most filesystems support extended attributes. 
Tooling around this feature is lacking, despite great potential for organizing files. 

Two examples:
1. We'd like to be able to set an attribute on folders to get ripgrep to skip them.
2. We'd like to be able to set metadata like user.role=notes on files, and tell ripgrep to only search them.

See: https://github.com/sharkdp/fd/issues/830#issuecomment-2492367688

Freedesktop recommends `user.xdg.robots.index`
https://www.freedesktop.org/wiki/CommonExtendedAttributes/


---

_Comment by @BurntSushi on 2024-11-23 16:57_

I basically agree with sharkdp that this should be left to an external tool. There's a _reason_ why tooling doesn't usually support attributes like this: it's expensive. ripgrep does a lot of its filtering without doing any stat calls on each file.

The main way to tell ripgrep to skip things is with a `.gitignore`, `.ignore` or `.rgignore` file.

---

_Closed by @BurntSushi on 2024-11-23 16:57_

---

_Label `wontfix` added by @BurntSushi on 2024-11-23 16:57_

---

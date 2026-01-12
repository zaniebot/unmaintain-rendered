```yaml
number: 1227
title: How to disable auto completion overload
type: issue
state: closed
author: klonuo
labels:
  - question
  - server
  - completions
assignees: []
created_at: 2025-09-21T15:14:10Z
updated_at: 2025-10-03T12:19:05Z
url: https://github.com/astral-sh/ty/issues/1227
synced_at: 2026-01-12T15:54:24Z
```

# How to disable auto completion overload

---

_@klonuo_

### Question

Hi,

I use latest a21 version and I think that this issue started with this version - for example just typing some keyword and lengthy auto-complete list with unrelated helpers pops up:

<img width="946" height="653" alt="Image" src="https://github.com/user-attachments/assets/a7452eb5-36dd-4659-9807-fc94ccac42fa" />

I couldn't find setting to disable this in the docs, while i tried to enable/disable [`autoimport`](https://docs.astral.sh/ty/reference/editor-settings/#autoimport) not knowing what this setting is even supposed to do, but it didn't change anything.

So how to disable this feature

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Label `question` added by @klonuo on 2025-09-21 15:14_

---

_Label `server` added by @AlexWaygood on 2025-09-21 15:19_

---

_Comment by @BurntSushi on 2025-09-21 15:27_

Hmmm, this looks like it's behaving as intended? It's the same for me, with `autoImport` disabled:

https://github.com/user-attachments/assets/35e74d1e-ac92-4d24-934f-c83b3b3f8334

What you're seeing in your completions are symbols that are actually in scope at your cursor's position. This includes everything from `builtins`.

Now the _ranking_ may be questionable. For example, you might expect to see completions for items in your local scope before items from `builtins`. But we haven't really addressed ranking yet.

---

_Closed by @klonuo on 2025-09-21 17:34_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:19_

---

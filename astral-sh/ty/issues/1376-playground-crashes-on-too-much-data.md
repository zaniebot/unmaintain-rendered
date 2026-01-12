```yaml
number: 1376
title: Playground crashes on too much data
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - playground
  - fatal
assignees: []
created_at: 2025-10-17T04:45:24Z
updated_at: 2025-10-17T07:15:35Z
url: https://github.com/astral-sh/ty/issues/1376
synced_at: 2026-01-12T15:54:25Z
```

# Playground crashes on too much data

---

_@MeGaGiGaGon_

### Summary

The playground crashes with the error `Uncaught DOMException: The quota has been exceeded.` when you put in too much data (see attached video).

The crash happens by putting more than 8 MB of data into the playground. I used this script to generate 4 MB `python -c 'open("issue.py", "w").write("a" * 2**22)'`, which when pasted twice into the playground crashes it.

It would be nice if like the `ruff` playground it didn't crash, or even better showed an error popup too.

<details>
<summary>Full crash traceback</summary>

```
Uncaught DOMException: The quota has been exceeded.
    z0 https://play.ty.dev/assets/index-C9G0QtaD.js:64
    m1 https://play.ty.dev/assets/index-C9G0QtaD.js:107
    ll https://play.ty.dev/assets/index-C9G0QtaD.js:49
    gg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    nn https://play.ty.dev/assets/index-C9G0QtaD.js:49
    gg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    nn https://play.ty.dev/assets/index-C9G0QtaD.js:49
    gg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    Dg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    zg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    Ee https://play.ty.dev/assets/index-C9G0QtaD.js:26
    Y_ https://play.ty.dev/assets/index-C9G0QtaD.js:26
    Y_ https://play.ty.dev/assets/index-C9G0QtaD.js:26
    Z_ https://play.ty.dev/assets/index-C9G0QtaD.js:26
    F_ https://play.ty.dev/assets/index-C9G0QtaD.js:42
    W_ https://play.ty.dev/assets/index-C9G0QtaD.js:50
    <anonymous> https://play.ty.dev/assets/index-C9G0QtaD.js:50
[index-C9G0QtaD.js:64](https://play.ty.dev/assets/index-C9G0QtaD.js)
    z0 https://play.ty.dev/assets/index-C9G0QtaD.js:64
    m1 https://play.ty.dev/assets/index-C9G0QtaD.js:107
    ll https://play.ty.dev/assets/index-C9G0QtaD.js:49
    gg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    nn https://play.ty.dev/assets/index-C9G0QtaD.js:49
    gg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    nn https://play.ty.dev/assets/index-C9G0QtaD.js:49
    gg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    Dg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    zg https://play.ty.dev/assets/index-C9G0QtaD.js:49
    Ee https://play.ty.dev/assets/index-C9G0QtaD.js:26
    (Async: EventHandlerNonNull)
    Y_ https://play.ty.dev/assets/index-C9G0QtaD.js:26
    Y_ https://play.ty.dev/assets/index-C9G0QtaD.js:26
    Z_ https://play.ty.dev/assets/index-C9G0QtaD.js:26
    F_ https://play.ty.dev/assets/index-C9G0QtaD.js:42
    W_ https://play.ty.dev/assets/index-C9G0QtaD.js:50
    <anonymous> https://play.ty.dev/assets/index-C9G0QtaD.js:50
```

</details>

https://github.com/user-attachments/assets/1e687845-523e-467d-b9de-fcb5a2b993de

### Version

Playground (64edfb6ef)

---

_Label `playground` added by @AlexWaygood on 2025-10-17 06:38_

---

_Label `fatal` added by @AlexWaygood on 2025-10-17 06:38_

---

_Comment by @MichaReiser on 2025-10-17 07:03_

The issue here is that there's a quote limit on how much we can store in local storage. 

We can skip local persistence if the files become very large. This fixes the crash but the playground definitely becomes sluggish with large files (we'd have to move the check phase to a worker, which isn't worth the complexity, as it isn't intended as a full IDE).

---

_Closed by @MichaReiser on 2025-10-17 07:15_

---

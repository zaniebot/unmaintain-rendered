---
number: 2307
title: "Feature: Auto Format F-strings"
type: issue
state: open
author: AndBoyS
labels:
  - server
  - wish
assignees: []
created_at: 2026-01-02T19:42:17Z
updated_at: 2026-01-06T11:02:20Z
url: https://github.com/astral-sh/ty/issues/2307
synced_at: 2026-01-10T01:51:14Z
---

# Feature: Auto Format F-strings

---

_Issue opened by @AndBoyS on 2026-01-02 19:42_

Add an option to LSP: prefix string with f if `{` is typed inside a string (like in https://github.com/DetachHead/basedpyright/issues/280)

---

_Comment by @MeGaGiGaGon on 2026-01-02 20:30_

As a basedpyright user, this can be very nice, but can also be very annoying at times. If this does get implemented into ty, one change that would be very nice is not making the string an f-string if it has existing `{}`s in it (so in the string `""` typing `{` at the end would turn it into `f"{}"`, while in the string `"{}"` typing `"{"` would leave it as a normal string)

---

_Label `server` added by @mtshiba on 2026-01-02 21:12_

---

_Label `wish` added by @MichaReiser on 2026-01-03 10:24_

---

_Comment by @dhruvmanila on 2026-01-05 10:16_

This is available in Ruff using https://docs.astral.sh/ruff/rules/missing-f-string-syntax/ which is in [preview](https://docs.astral.sh/ruff/preview/). Once you turn on preview rules, the LSP should also provide you with a code action to fix this but (I think?) this will require turning on [unsafe fixes](https://docs.astral.sh/ruff/settings/#unsafe-fixes) as the fix is unsafe.

---

_Comment by @MeGaGiGaGon on 2026-01-05 17:26_

That's similar but not exactly it - with how basedpyright does it there's no need to apply a code action, it does the change at the same time as you type the `{`:

https://github.com/user-attachments/assets/e5ff3d9b-af0d-4284-b76a-1435b559a299

---

_Comment by @AlexWaygood on 2026-01-05 17:30_

I really wouldn't want that, personally. There are lots of strings that contain `{}` in them but aren't necessarily meant to be f-strings. That's why https://docs.astral.sh/ruff/rules/missing-f-string-syntax/ is still in preview: it's really difficult to know for sure whether a string with `{}` is meant to be an f-string or not! The Ruff rule has too many false positives to be considered stable, and it's hard to know how to improve it (I put up https://github.com/astral-sh/ruff/pull/13076 ages ago, but I'm unlikely to get back to it any time soon)

---

_Comment by @MichaReiser on 2026-01-05 17:34_

I find this a reasonable opt-in feature. I certainly wouldn't enable it by default. 

---

_Comment by @dhruvmanila on 2026-01-06 11:02_

> That's similar but not exactly it - with how basedpyright does it there's no need to apply a code action, it does the change at the same time as you type the `{`:

Interesting! My guess would be that this is done using `textDocument/onTypeFormatting` (https://github.com/astral-sh/ruff/issues/16829).

---

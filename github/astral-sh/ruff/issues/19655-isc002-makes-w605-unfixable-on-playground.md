---
number: 19655
title: ISC002 makes W605 unfixable on playground
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - playground
assignees: []
created_at: 2025-07-31T01:18:42Z
updated_at: 2025-09-23T13:15:33Z
url: https://github.com/astral-sh/ruff/issues/19655
synced_at: 2026-01-07T13:12:16-06:00
---

# ISC002 makes W605 unfixable on playground

---

_Issue opened by @MeGaGiGaGon on 2025-07-31 01:18_

### Summary

I'm not sure why this happens, but if both [invalid-escape-sequence (W605)](https://docs.astral.sh/ruff/rules/invalid-escape-sequence/#invalid-escape-sequence-w605) and [multi-line-implicit-string-concatenation (ISC002)](https://docs.astral.sh/ruff/rules/multi-line-implicit-string-concatenation/#multi-line-implicit-string-concatenation-isc002) trigger on the same string on the playground, `W605` will not be fixable using the On Hover context menu -> Quick Fix.

https://play.ruff.rs/83a23eba-a1c5-4394-aab9-dcd20e098a55

<img width="584" height="143" alt="Image" src="https://github.com/user-attachments/assets/199cc5b8-d1f0-41fc-8fe6-13ef9957c36b" />

This seems to just happen with the playground hover menu, the fix works fine locally in PowerShell with `--fix`, the "Quick Fix" shows up on hover locally in VSC with the VSC extension, and when using the `ctrl+.` shortcut with the cursor over the `W605` error to open the Quick Fix menu directly.

No clue if this is something that is fixable/is a problem on ruff's end, it could also be something in Monaco.

### Version

Playground

---

_Label `playground` added by @MichaReiser on 2025-07-31 06:32_

---

_Comment by @MichaReiser on 2025-07-31 06:33_

Interesting. I first thought this might be related to https://github.com/astral-sh/ruff/issues/16266 but, after a closer look, I don't think that's the case

---

_Referenced in [astral-sh/ruff#20527](../../astral-sh/ruff/pulls/20527.md) on 2025-09-23 02:51_

---

_Closed by @MichaReiser on 2025-09-23 13:15_

---

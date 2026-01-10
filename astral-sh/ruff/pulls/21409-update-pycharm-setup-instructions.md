```yaml
number: 21409
title: Update PyCharm setup instructions
type: pull_request
state: merged
author: charliecloudberry
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2025-11-12T19:52:48Z
updated_at: 2025-11-13T17:54:08Z
url: https://github.com/astral-sh/ruff/pull/21409
synced_at: 2026-01-10T16:53:55Z
```

# Update PyCharm setup instructions

---

_Pull request opened by @charliecloudberry on 2025-11-12 19:52_

## Summary

Ruff support was recently added to PyCharm and I want to update Ruff's documentation accordingly.

## Test Plan

n/a


---

_Comment by @ntBre on 2025-11-12 20:42_

My understanding based on https://github.com/astral-sh/ruff/issues/10102#issuecomment-3488551126 was that the Ruff support was still in preview, and this seems to be supported by the warning at the top of the [docs](https://www.jetbrains.com/help/pycharm/2025.3/lsp-tools.html#ruff) you linked:

> Because PyCharm 2025.3 is still in development, this documentation may not be entirely accurate and is subject to change.

Should we hold off until the Ruff support is stabilized, or do most users have access to the preview features?

---

_Label `documentation` added by @ntBre on 2025-11-12 20:42_

---

_Comment by @charliecloudberry on 2025-11-12 21:08_

@ntBre Hi! Ruff support is already available in the EAP version of PyCharm (anyone can install and use this version if they want to), and the major release is planned before the end of 2025. 
Also I was not sure how long it would take for the changes to be merged and published, so I decided to submit the PR now.
If you think we should wait, I can submit my changes later.

---

_Comment by @ntBre on 2025-11-12 21:22_

Ah okay, I wasn't sure exactly how preview worked, thanks!

And thank you for opening this by the way, we're excited for the official support too!

If we merge this today it would go out in the weekly patch release tomorrow, though, so I would probably lean toward waiting or at least keeping the old setup instructions too until stabilization.

But I'm curious what others think too (cc @MichaReiser)

---

_Comment by @MichaReiser on 2025-11-13 08:13_

Nice! I agree with @ntBre. I'd prefer to keep the old documentation around for now and mention that the builtin Ruff integration is only available with Pycharm verison X or newer

---

_Comment by @charliecloudberry on 2025-11-13 11:24_

Hi! Thank you for the review and suggestions. I’ve restored the old documentation and updated the text to specify the PyCharm version.
I’ll also submit a new PR to update the link to the PyCharm docs after the major release.

---

_Comment by @MichaReiser on 2025-11-13 17:54_

Thank you

---

_Merged by @MichaReiser on 2025-11-13 17:54_

---

_Closed by @MichaReiser on 2025-11-13 17:54_

---

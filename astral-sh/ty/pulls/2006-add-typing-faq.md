```yaml
number: 2006
title: Add typing FAQ
type: pull_request
state: merged
author: sharkdp
labels:
  - documentation
assignees: []
merged: true
base: main
head: david/add-typing-faq
created_at: 2025-12-17T12:40:25Z
updated_at: 2025-12-17T13:46:57Z
url: https://github.com/astral-sh/ty/pull/2006
synced_at: 2026-01-12T15:54:28Z
```

# Add typing FAQ

---

_@sharkdp_

Rendered: https://shark.fish/ty-faq/reference/typing-faq/

---

_Label `documentation` added by @sharkdp on 2025-12-17 12:40_

---

_@sharkdp reviewed on 2025-12-17 12:41_

---

_Review comment by @sharkdp on `docs/reference/typing-faq.md`:119 on 2025-12-17 12:41_

This is not my area of expertise. Can someone proof-read and possibly extend this?

---

_@sharkdp reviewed on 2025-12-17 12:41_

---

_Review comment by @sharkdp on `docs/reference/typing-faq.md`:146 on 2025-12-17 12:41_

Similar here. Should we be advertising these workarounds?

---

_@AlexWaygood reviewed on 2025-12-17 12:48_

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:119 on 2025-12-17 12:48_

This looks very solid to me. You could possibly link to https://github.com/astral-sh/ty/issues/487 here as the tracking issue for improving our diagnostics when C extensions are installed without stubs.

https://github.com/astral-sh/ty/issues/585 is also a very common issue (local package is installed editably, but using a dynamic setuptools editable install rather than a "traditional" editable install using a `.pth` file, so we can't see that the module is installed and can't resolve the import).

---

_@AlexWaygood reviewed on 2025-12-17 12:49_

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:146 on 2025-12-17 12:49_

@zsol?

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:5 on 2025-12-17 12:49_

(https://github.com/astral-sh/ty/issues/2003 will also help a lot with confusion around this, I think)

---

_@AlexWaygood reviewed on 2025-12-17 12:49_

---

_@AlexWaygood approved on 2025-12-17 12:50_

This is awesome, thank you!!

---

_@sharkdp reviewed on 2025-12-17 13:14_

---

_Review comment by @sharkdp on `docs/reference/typing-faq.md`:119 on 2025-12-17 13:14_

> This looks very solid to me. You could possibly link to #487 here as the tracking issue for improving our diagnostics when C extensions are installed without stubs.

Done

> #585 is also a very common issue (local package is installed editably, but using a dynamic setuptools editable install rather than a "traditional" editable install using a `.pth` file, so we can't see that the module is installed and can't resolve the import).

I'm not sure I would do a good job trying to explain this. But that issue only has two upvotes, and one is from Carl, so I think it's fine to leave it out of the FAQs for now :smile: 

---

_@sharkdp reviewed on 2025-12-17 13:20_

---

_Review comment by @sharkdp on `docs/reference/typing-faq.md`:146 on 2025-12-17 13:20_

Merging for now, we can always remove it later.

---

_Merged by @sharkdp on 2025-12-17 13:20_

---

_Closed by @sharkdp on 2025-12-17 13:20_

---

_Branch deleted on 2025-12-17 13:20_

---

_@AlexWaygood reviewed on 2025-12-17 13:21_

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:119 on 2025-12-17 13:21_

It's come up a bunch of times on discord too. But don't worry about it, I can add something as a followup!

---

_@zsol reviewed on 2025-12-17 13:46_

---

_Review comment by @zsol on `docs/reference/typing-faq.md`:146 on 2025-12-17 13:46_

It's not a big deal but I would probably not advertise the second option here - I can follow up with improvements once I've converted pyx to use `uv workspace` to iterate over members. 

---

```yaml
number: 689
title: Add simple option/feature flag to disable memory leaks
type: issue
state: closed
author: qarmin
labels:
  - testing
assignees: []
created_at: 2025-06-21T13:01:23Z
updated_at: 2025-08-15T12:25:50Z
url: https://github.com/astral-sh/ty/issues/689
synced_at: 2026-01-12T15:54:23Z
```

# Add simple option/feature flag to disable memory leaks

---

_@qarmin_

According to https://github.com/astral-sh/ruff/issues/14945, memory leaks at the end of executing ty are expected due to the lack of cleanup of the db variable in:

https://github.com/astral-sh/ruff/blob/ea812d0813faf6e0ed2d39354e85d08f9b857c90/crates/ty/src/lib.rs#L148

However, even with ASAN_OPTIONS="detect_leaks=0" set, I sometimes observe memory leaks reported by AddressSanitizer—probably because I run the application in a somewhat unusual way.

Would it be possible to add a non-default feature flag to properly clean up the leaked variable?

---

_Label `memory` added by @AlexWaygood on 2025-06-21 13:05_

---

_Comment by @MichaReiser on 2025-06-21 15:26_

I don't think it makes sense to have a CLI or configuration option for this. It would only confuse users and I don't see a case where they should enable this option. 

Would a rust feature flag work for you? 

---

_Label `needs-info` added by @MichaReiser on 2025-06-21 15:26_

---

_Comment by @qarmin on 2025-06-22 06:48_

Yes, I use a self-compiled build for most of my testing, so a Rust feature flag is a perfectly fine solution for me.

---

_Comment by @MichaReiser on 2025-06-22 07:50_

Great, that would work for us. Not sure what to call the feature flag but I would accept such a PR

---

_Label `needs-info` removed by @MichaReiser on 2025-06-22 07:50_

---

_Label `memory` removed by @MichaReiser on 2025-07-10 16:23_

---

_Label `testing` added by @MichaReiser on 2025-07-10 16:23_

---

_Comment by @MichaReiser on 2025-08-15 12:25_

I'll close this issue as this isn't something we plan on implementing ourselves but I'd accept a contributor contribution that adds this.

---

_Closed by @MichaReiser on 2025-08-15 12:25_

---

```yaml
number: 4485
title: Feature gate term_width and max_term_width APIs
type: issue
state: closed
author: SUPERCILEX
labels:
  - C-bug
  - M-breaking-change
  - S-waiting-on-decision
  - A-builder
assignees: []
created_at: 2022-11-16T03:58:33Z
updated_at: 2022-11-21T12:44:06Z
url: https://github.com/clap-rs/clap/issues/4485
synced_at: 2026-01-12T16:14:16Z
```

# Feature gate term_width and max_term_width APIs

---

_@SUPERCILEX_

They currently do nothing unless `wrap_help` is enabled which is extremely confusing. Those APIs should only be available when `wrap_help` is enabled.

---

_Label `C-bug` added by @epage on 2022-11-16 12:49_

---

_Label `S-waiting-on-decision` added by @epage on 2022-11-16 12:49_

---

_Label `A-builder` added by @epage on 2022-11-16 12:49_

---

_Label `M-breaking-change` added by @epage on 2022-11-16 12:49_

---

_Added to milestone `5.0` by @epage on 2022-11-16 12:49_

---

_Comment by @epage on 2022-11-16 12:50_

Yes, hadn't considered this when making the other changes.  My preference would be to only gate the setters and not the getters so code operating on clap doesn't have to take it into account.

---

_Comment by @SUPERCILEX on 2022-11-16 18:17_

Sounds good. I again wasted ~15mins last night trying to figure out why nothing would wrap until I remembered the feature flag, so anything to prevent that works.

---

_Comment by @epage on 2022-11-17 15:08_

Something I should add is that this can be implemented today, behind the `unstable-v5` feature flag.  In this case, it would be something like  `#[cfg(any(not(feature = "unstable-v5"), feature = "wrap_help"))]`.

---

_Comment by @SUPERCILEX on 2022-11-21 03:33_

Sounds good, done: https://github.com/clap-rs/clap/pull/4495

---

_Closed by @epage on 2022-11-21 12:44_

---

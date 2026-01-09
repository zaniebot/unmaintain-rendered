---
number: 6111
title: "feat: mark `StyledStr::push_str` functions public"
type: pull_request
state: merged
author: eatradish
labels: []
assignees: []
merged: true
base: master
head: public-styledstr-method
created_at: 2025-08-25T07:00:54Z
updated_at: 2025-08-26T16:15:31Z
url: https://github.com/clap-rs/clap/pull/6111
synced_at: 2026-01-07T13:12:20-06:00
---

# feat: mark `StyledStr::push_str` functions public

---

_Pull request opened by @eatradish on 2025-08-25 07:00_

Mark `StyledStr::push_str` as public, so that `ErrorFormatter` trait available outside of clap crate.

https://github.com/clap-rs/clap/issues/380#issuecomment-3217975004

---

_@epage reviewed on 2025-08-25 15:22_

---

_Review comment by @epage on `clap_builder/src/error/mod.rs`:307 on 2025-08-25 15:22_

Is this needed?  I would assume within an application, you would know your own `Styles`

---

_@epage reviewed on 2025-08-25 15:29_

---

_Review comment by @epage on `clap_builder/src/builder/styled_str.rs`:40 on 2025-08-25 15:29_

I'm ok with `push_str` but not `push_string` being `pub`

---

_@epage reviewed on 2025-08-25 15:29_

---

_Review comment by @epage on `clap_builder/src/builder/styled_str.rs`:143 on 2025-08-25 15:29_

I'm ok with `push_str` but not `push_styled` being `pub`

---

_@epage reviewed on 2025-08-25 15:29_

---

_Review comment by @epage on `clap_builder/src/builder/styled_str.rs`:40 on 2025-08-25 15:29_

Looks like the error formatter doesn't use it.

---

_Review comment by @epage on `clap_builder/src/builder/styled_str.rs`:143 on 2025-08-25 15:29_

Looks like its only used in a couple of places so shouldn't be too bad to switch

---

_@epage reviewed on 2025-08-25 15:29_

---

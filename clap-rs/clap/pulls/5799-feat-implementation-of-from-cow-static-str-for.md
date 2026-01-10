---
number: 5799
title: "feat: Implementation of `From<Cow<'static, str>>` for `StyledStr`"
type: pull_request
state: closed
author: zaira-bibi
labels: []
assignees: []
base: master
head: implement-for-styledstr
created_at: 2024-11-01T07:28:00Z
updated_at: 2025-04-21T11:51:25Z
url: https://github.com/clap-rs/clap/pull/5799
synced_at: 2026-01-10T01:28:24Z
---

# feat: Implementation of `From<Cow<'static, str>>` for `StyledStr`

---

_Pull request opened by @zaira-bibi on 2024-11-01 07:28_

Implemented `From<Cow<'static, str>>` for `StyledStr` under #5785.

(I'm not sure where I was supposed to write the tests for it, so guidance in that would be really appreciated. Thanks!)

---

_@epage reviewed on 2024-11-06 15:12_

---

_Review comment by @epage on `clap_builder/src/builder/styled_str.rs`:216 on 2024-11-06 15:12_

Please use `.into_owned()`

`to_string` can work on any `display` and can make code more sloppy on refactor.

(sorry, I overlooked this last time and apparently a clippy lint didn't catch this case)

---

_@yhx-12243 reviewed on 2024-11-25 12:21_

---

_Review comment by @yhx-12243 on `clap_builder/src/builder/styled_str.rs`:216 on 2024-11-25 12:21_

No, `Cow<'static, str>::to_string` will cause unnecessary clone on owned case, `into_owned` is indeed what we want.

---

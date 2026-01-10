---
number: 2305
title: impl Send for App
type: issue
state: closed
author: tbraun96
labels: []
assignees: []
created_at: 2021-01-22T00:25:14Z
updated_at: 2021-01-22T12:17:24Z
url: https://github.com/clap-rs/clap/issues/2305
synced_at: 2026-01-10T01:27:15Z
---

# impl Send for App

---

_Issue opened by @tbraun96 on 2021-01-22 00:25_

I am using clap to parse arguments in a custom terminal environment. I initialize ``App`` once, then use ``get_matches_from_safe_borrow``in order to not consume ``self``, thus allowing me to reuse ``App``. When initializing ``App``, I place it inside lazy_static block wrapped in a ``Mutex`` to ensure no race conditions. However, I am surprised to see that ``App`` does not implement ``Send``, so really, I can't safely use App. Inside your code, replace the ``Rc``'s with ``Arc``'s, thus giving us more usability. Thanks

---

_Label `T: new feature` added by @tbraun96 on 2021-01-22 00:25_

---

_Comment by @pksunkara on 2021-01-22 10:39_

It's already done. #1771. Please search before creating issues. And you haven't even used our issue template.

---

_Closed by @pksunkara on 2021-01-22 10:39_

---

_Comment by @tbraun96 on 2021-01-22 11:52_

It's been over 9 months and no progress on a trivial addition

---

_Comment by @pksunkara on 2021-01-22 12:17_

It has already been done. What do you mean no progress?

If something is actually missing and you consider it a trivial addition, why don't you actually make a PR then instead of being aggressive towards the maintainers?

---

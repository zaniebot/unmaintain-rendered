```yaml
number: 16537
title: "Allow Python requests to include `+gil` to require a GIL-enabled interpreter"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/gil-request
created_at: 2025-10-31T15:03:18Z
updated_at: 2025-11-03T19:35:54Z
url: https://github.com/astral-sh/uv/pull/16537
synced_at: 2026-01-12T16:12:18Z
```

# Allow Python requests to include `+gil` to require a GIL-enabled interpreter

---

_@zanieb_

Addresses https://github.com/astral-sh/uv/pull/16142#issuecomment-3390586957

Nobody seems to have a good idea about how to spell this. "not free-threaded" would be the most technically correct, but I think "gil" will be more intuitive.

---

_Label `enhancement` added by @zanieb on 2025-10-31 15:03_

---

_@geofft approved on 2025-10-31 18:25_

This seems fine. I think there is no obviously good syntax for this and we do need _something_, and this seems like the least annoying one / closest to what we already do.

---

_Comment by @jezdez on 2025-10-31 18:43_

I'm not sure if that's the best name, since the GIL can be enabled in free-threaded Python during runtime and is enabled when importing C-API extension modules. Maybe `+default` might be a good name?

---

_Comment by @zanieb on 2025-10-31 19:42_

That's why I said "not free-threaded" would be the most technically correct in the pull request body.

I don't think `+default` works, it's not specific enough. e.g., imagine a world where the free-threaded builds become the default and there's a legacy GIL-enabled build around. I guess we could have the `+default` modifier change its meaning depending on the Python version, but that seems questionable.

I'd love a better name, but I can't really think of one and want to ship something to unblock users. I think `+gil` is the most obvious name that someone would look for, even if there's some technical nuance.

I tried asking the free-threaded development group and didn't really get anywhere.



---

_Merged by @zanieb on 2025-11-03 19:35_

---

_Closed by @zanieb on 2025-11-03 19:35_

---

_Branch deleted on 2025-11-03 19:35_

---

```yaml
number: 17463
title: "Retry on \"too many open file\" errors when uninstalling Python"
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: claude/retry-open-files-error-o5T7E
created_at: 2026-01-14T14:15:43Z
updated_at: 2026-01-14T15:12:59Z
url: https://github.com/astral-sh/uv/pull/17463
synced_at: 2026-01-14T15:39:55Z
```

# Retry on "too many open file" errors when uninstalling Python

---

_@zanieb_

_No description provided._

---

_Label `bug` added by @zanieb on 2026-01-14 14:15_

---

_Comment by @konstin on 2026-01-14 14:20_

Can we try instead increasing the ulimit (https://github.com/astral-sh/uv/issues/16999#issuecomment-3641417484)? There's multiple locations where we could run into these kinds of errors, a higher limits avoids them occurring in the first place and is also more performant.

---

_Comment by @zanieb on 2026-01-14 14:23_

What about both? I think adjusting the ulimit is scarier and I'd want to gate that with preview to start. This feels safe, though it can be less performant in the bad case.

---

_Comment by @konstin on 2026-01-14 14:33_

I'd start with increasing the ulimit, I've collected the precedent of other popular applications increasing the ulimit in https://github.com/astral-sh/uv/issues/16999#issuecomment-3641417484, this seems to be much more widely used than retrying. The other aspect is that these errors can occur in different parts of uv, and it could also happen that one operation opening lots of files crashes a usually less affected operation opening a file in parallel.

---

_Comment by @zanieb on 2026-01-14 14:45_

I still don't think I want to ship increasing the ulimit outside of preview to start and would like to unblock this clearly broken specific case.

---

_Comment by @zanieb on 2026-01-14 14:50_

Drafting that at https://github.com/astral-sh/uv/pull/17464 (though it's not preview gated right now)

---

_Comment by @konstin on 2026-01-14 14:53_

What problems do you see occurring when increasing the ulimit?

We know that too many file errors can occur in at least uninstall and bytecode compilation, only handling them for uninstallation and only through retry doesn't seem a robust solution to me. If we want a more immediate fix, we could also consider setting a lower parallelism limit for it based on the ulimit.

---

_Comment by @zanieb on 2026-01-14 14:58_

> What problems do you see occurring when increasing the ulimit?

There's scary stuff in the refs you linked, like

```
                    // apparently, requesting too high of a number can cause other processes to not start.
                    // https://discord.com/channels/876711213126520882/1316342194176790609/1318175562367242271
                    // https://github.com/postgres/postgres/blob/fee2b3ea2ecd0da0c88832b37ac0d9f6b3bfb9a9/src/backend/storage/file/fd.c#L1072
```

---

_Comment by @konstin on 2026-01-14 15:08_

Fair point, we definitely want to more conservative than `@max(limit.max, 163840)`.

---

_Comment by @zanieb on 2026-01-14 15:12_

They only do that for musl libc, but yeah.

---

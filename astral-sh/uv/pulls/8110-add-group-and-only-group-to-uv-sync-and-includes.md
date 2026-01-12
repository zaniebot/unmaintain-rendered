```yaml
number: 8110
title: "Add `--group` and `--only-group` to `uv sync` and includes all groups in `uv lock`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: tracking/735
head: zb/735-sync-lock
created_at: 2024-10-10T21:08:03Z
updated_at: 2024-10-16T22:53:27Z
url: https://github.com/astral-sh/uv/pull/8110
synced_at: 2026-01-12T16:08:10Z
```

# Add `--group` and `--only-group` to `uv sync` and includes all groups in `uv lock`

---

_@zanieb_

Part of #8090 

Adds the ability to include a group (`--group`) in the sync or _only_ sync a group (`--only-group`). Includes all groups in the resolution, which will have the same limitations as extras as described in #6981.

There's a great deal of refactoring of the "development" concept into "groups" behind the scenes that I am continuing to defer here to minimize the diff.

Additionally, this does not yet resolve interactions with the existing `dev` group — we'll tackle that separately as well. I probably won't merge the stack until that design is resolved. The current proposal is that we'll just "combine' the `dev-dependencies` contents into the `dev` group.

---

_@zanieb reviewed on 2024-10-10 21:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:281 on 2024-10-10 21:08_

This took me a while to track down — this is critical to actually respect groups.

---

_Marked ready for review by @zanieb on 2024-10-15 00:28_

---

_@charliermarsh approved on 2024-10-16 21:26_

---

_Comment by @charliermarsh on 2024-10-16 21:37_

I guess we also need this on `uv run`, and maybe also in `uv pip`?

---

_Comment by @alex on 2024-10-16 21:40_

I'm interested in it for uv pip compile

On Wed, Oct 16, 2024, 5:38 PM Charlie Marsh ***@***.***>
wrote:

> I guess we also need this on uv run, and maybe also in uv pip?
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/pull/8110#issuecomment-2418011254>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAAGBD5ABDMXDAPRTU7KBTZ33MEZAVCNFSM6AAAAABPXUA2NOVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDIMJYGAYTCMRVGQ>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
>


---

_Merged by @zanieb on 2024-10-16 22:53_

---

_Closed by @zanieb on 2024-10-16 22:53_

---

_Branch deleted on 2024-10-16 22:53_

---

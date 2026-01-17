```yaml
number: 17532
title: Remove some abandoned dependencies
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/abandoned-deps
created_at: 2026-01-16T16:54:35Z
updated_at: 2026-01-17T05:30:30Z
url: https://github.com/astral-sh/uv/pull/17532
synced_at: 2026-01-17T06:04:36Z
```

# Remove some abandoned dependencies

---

_@zanieb_

#2658 reports these as abandoned, so let's move off them?

---

_Label `internal` added by @zanieb on 2026-01-16 16:54_

---

_Marked ready for review by @zanieb on 2026-01-16 17:18_

---

_Review comment by @EliteTK on `.github/workflows/build-dev-binaries.yml`:242 on 2026-01-16 18:22_

Works as it is, so you can ignore this.

But if you wanted our CI to demonstrate our advanced sed skills you could do this instead:

```suggestion
          sed -n '/rust-version/s/.*"\([^"]*\)".*/value=\1/p' Cargo.toml >> "$GITHUB_OUTPUT"
```

---

_Review comment by @EliteTK on `.github/workflows/check-lint.yml`:66 on 2026-01-16 18:24_

We should probably check that what we downloaded matches a specified hash, to prevent this being a supply chain attack route. But I'll grant you that the original action didn't do this, so this strictly isn't making things worse.

---

_@EliteTK approved on 2026-01-16 18:26_

just nitpicks

---

_@zanieb reviewed on 2026-01-16 18:31_

---

_Review comment by @zanieb on `.github/workflows/build-dev-binaries.yml`:242 on 2026-01-16 18:31_

Is this easier to read?

I actually do not have an opinion. They both hurt my eyes.

---

_@zanieb reviewed on 2026-01-16 18:32_

---

_Review comment by @zanieb on `.github/workflows/check-lint.yml`:66 on 2026-01-16 18:32_

I'm not sure how we can do that in a way supported by renovate without also fetching the hash from GitHub which seems a bit moot, but can open an issue to track it.

---

_@EliteTK reviewed on 2026-01-16 18:34_

---

_Review comment by @EliteTK on `.github/workflows/build-dev-binaries.yml`:242 on 2026-01-16 18:34_

Yeah I don't think you will find readability in `sed` (or I guess regex adjacent things in general).

I am not sure if it's easier to read. I'd say the former requires knowing a little bit less about `sed`. But it's forbidden toml parsing either way.

It wasn't entirely a serious suggestion ;)

---

_Review comment by @EliteTK on `.github/workflows/check-lint.yml`:66 on 2026-01-16 18:46_

Hmm, I missed the renovate comment, didn't realise that was how it worked.

Spent a couple of minutes looking into it and I can't see an obvious way to do this either which won't incur some kind of cost every time renovate updates it (on the other hand, shellcheck releases don't happen that often).

Let's just make an issue for now.

---

_@EliteTK reviewed on 2026-01-16 18:46_

---

_Merged by @zanieb on 2026-01-16 18:49_

---

_Closed by @zanieb on 2026-01-16 18:49_

---

_Branch deleted on 2026-01-16 18:49_

---

_@samypr100 reviewed on 2026-01-17 05:30_

---

_Review comment by @samypr100 on `.github/workflows/check-lint.yml`:66 on 2026-01-17 05:30_

Another potential approach is shellcheck supporting GH's new immutable releases, although less explored

---

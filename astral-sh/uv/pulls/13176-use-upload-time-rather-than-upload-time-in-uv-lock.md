```yaml
number: 13176
title: "Use `upload-time` rather than `upload_time` in `uv.lock`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/kebab
created_at: 2025-04-28T13:27:37Z
updated_at: 2025-05-05T11:46:58Z
url: https://github.com/astral-sh/uv/pull/13176
synced_at: 2026-01-10T11:10:41Z
```

# Use `upload-time` rather than `upload_time` in `uv.lock`

---

_Pull request opened by @charliermarsh on 2025-04-28 13:27_

## Summary

In https://github.com/astral-sh/uv/pull/12968, we added support for upload time to `uv.lock`, but stylized as `upload_time`. The other keys in `uv.lock` use kebab casing, as in common in Python formats, so this really should've been `upload-time`. I want to change it ASAP to minimize churn for users. Any users that already upgraded will of course experience churn in their files a second time. But if we don't change it now, we'll only increase the surface area of affected users.

So, this PR uses `upload-time` instead, but continues reading `upload_time` to make it non-breaking.


---

_Review requested from @BurntSushi by @charliermarsh on 2025-04-28 13:37_

---

_Review requested from @Gankra by @charliermarsh on 2025-04-28 13:37_

---

_Review requested from @zanieb by @charliermarsh on 2025-04-28 13:37_

---

_Marked ready for review by @charliermarsh on 2025-04-28 13:37_

---

_@Gankra approved on 2025-04-28 13:41_

hmm does this need a lock version bump? a previous version of uv will fail to parse `upload-time`.

---

_Comment by @charliermarsh on 2025-04-28 13:50_

I think a previous version will just ignore it.

---

_@BurntSushi approved on 2025-04-28 13:51_

---

_Comment by @Gankra on 2025-04-28 13:51_

Yeah, as long as the field being missing on a version that knows about the field is fine, then this is fine.

---

_Comment by @BurntSushi on 2025-04-28 13:53_

Yeah versions of `uv` before `upload_time` will ignore. Versions of `uv` that know about `upload_time` but not `upload-time` will read `upload_time` but ignore `upload-time`.

I think the main "bad" thing that can happen is when multiple versions of `uv` are used to operate on the same lock file. So a newer version of `uv` with this change would write `upload-time`, but an older version would ignore that and then probably write `upload_time`. So then they would ping-pong. But other than that churn, I don't think anything bad can happen.

---

_Comment by @charliermarsh on 2025-04-28 14:27_

Yeah confirmed old versions can read this (they just ignore it). It's only used in the PEP 751 export, so I think that's ok.

---

_Merged by @charliermarsh on 2025-04-28 15:01_

---

_Closed by @charliermarsh on 2025-04-28 15:01_

---

_Branch deleted on 2025-04-28 15:01_

---

_Comment by @dimaqq on 2025-05-01 07:12_

What you probably have not anticipated are lock files checked into version control, where one developer uses slightly older and another slightly newer version of `uv` and the lock files goes through a ping-pong of `-` and `_` for a while.

Then again, this pushes folks to upgrade, that it's a win? 😆 

---

_Comment by @Viicos on 2025-05-01 16:06_

Maybe it's worth adding a note in the changelog whenever the lock file's version/revision is bumped, together with these kind of changes. 

---

_Comment by @zanieb on 2025-05-01 16:07_

@dimaqq we did consider that, see https://github.com/astral-sh/uv/pull/13176#issuecomment-2835330647

@Viicos can you share more about what you want to see in the changelog and how it would help?

---

_Comment by @Viicos on 2025-05-01 16:12_

Thanks for answering @zanieb,

in our case we have `tool.uv.required-version` set in our pyproject, so that all members of the team use the same uv version (or at least the same minimum version). We don't have any automated setup to update it but I regularly watch for new uv releases, and so if something like "warning: this uv version changes the lockfile output" is added in the release notes, I can easily catch it and bump our minimum version.

---

_Comment by @zanieb on 2025-05-01 17:22_

Alright. Maybe we can add a dedicated "Lockfile" section — I'm not sure. We really try to avoid any churn here though, so it should be rare.

---

_Comment by @michelhe on 2025-05-05 11:46_

Yeah, we also experienced the format inconsistency issues when working against a committed `uv.lock` file due to every developer having a different version of uv and used `tool.uv.required-version` to force everyone to upgrade.

---

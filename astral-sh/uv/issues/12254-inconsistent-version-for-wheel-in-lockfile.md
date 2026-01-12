```yaml
number: 12254
title: Inconsistent version for wheel in lockfile updated by Dependabot
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2025-03-17T23:52:52Z
updated_at: 2025-03-20T15:24:44Z
url: https://github.com/astral-sh/uv/issues/12254
synced_at: 2026-01-12T16:00:58Z
```

# Inconsistent version for wheel in lockfile updated by Dependabot

---

_@zanieb_

Dependabot recently launched support for updating versions in `uv.lock` files (see https://github.com/dependabot/dependabot-core/issues/10478). However, the latest version of Dependabot:

- Edits the lockfile directly instead of using uv to resolve new metadata
- Updates the `<package>.version` field (but not the associated distributions

(see the [implementation](https://github.com/dependabot/dependabot-core/blob/1cd6fc59d7dae25d46255550528dd056c97c46d8/uv/lib/dependabot/uv/file_updater/lock_file_updater.rb#L102-L143))

Previously, we've assumed we are the only writer of the lockfile and that the metadata inside it was consistent. Since the distribution files were not updated by Dependabot, uv would continue to use the old package version despite the Dependabot change. This means that Dependabot pull requests updating the versions of packages in the `uv.lock` had no effect.

It looks like this may be a Dependabot regression introduced by https://github.com/dependabot/dependabot-core/pull/11810. It's not clear if the change in behavior was intentional.

In https://github.com/astral-sh/uv/pull/12235, we've added validation that the package version in a lockfile matches its associated wheels. In uv 0.6.7, an error will be raised if this is detected. This will prevent invalid changes to the lockfile. Unfortunately, this means that some existing lockfiles will be invalidated. If a lockfile has been updated by the latest version of Dependabot, it will need to be reverted.

Note the validation is currently only scoped to distributions with _wheels_. If there are not wheels published for the package, the described problem is still present. We are exploring validation of source distribution versions in https://github.com/astral-sh/uv/pull/12237 — but we are worried this may disrupt other workflows.

We continue to be excited about Dependabot support for uv. If you're working on a fix for this issue in Dependabot, feel free to ping me for review.

---

_Comment by @zanieb on 2025-03-20 15:24_

I'm not seeing much activity here — so unpinning this and closing.

---

_Closed by @zanieb on 2025-03-20 15:24_

---

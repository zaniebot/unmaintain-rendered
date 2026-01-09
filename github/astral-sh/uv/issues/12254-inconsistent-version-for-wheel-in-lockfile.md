---
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
synced_at: 2026-01-07T13:12:18-06:00
---

# Inconsistent version for wheel in lockfile updated by Dependabot

---

_Issue opened by @zanieb on 2025-03-17 23:52_

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

_Referenced in [astral-sh/uv#12235](../../astral-sh/uv/pulls/12235.md) on 2025-03-17 23:55_

---

_Referenced in [astral-sh/uv#12164](../../astral-sh/uv/issues/12164.md) on 2025-03-17 23:55_

---

_Referenced in [dependabot/dependabot-core#10478](../../dependabot/dependabot-core/issues/10478.md) on 2025-03-17 23:56_

---

_Referenced in [Homebrew/homebrew-core#211299](../../Homebrew/homebrew-core/pulls/211299.md) on 2025-03-18 02:41_

---

_Referenced in [johnthagen/python-blueprint#238](../../johnthagen/python-blueprint/issues/238.md) on 2025-03-18 12:12_

---

_Referenced in [astral-sh/uv#12276](../../astral-sh/uv/issues/12276.md) on 2025-03-18 13:46_

---

_Comment by @zanieb on 2025-03-20 15:24_

I'm not seeing much activity here — so unpinning this and closing.

---

_Closed by @zanieb on 2025-03-20 15:24_

---

_Referenced in [ToucanToco/weaverbird#2372](../../ToucanToco/weaverbird/pulls/2372.md) on 2025-03-21 14:20_

---

---
number: 4779
title: "`uv pip install -U somepkg` should only update the specified package, unless additional upgrades are necessary"
type: issue
state: open
author: ThiefMaster
labels:
  - compatibility
  - needs-design
assignees: []
created_at: 2024-07-03T15:52:13Z
updated_at: 2024-12-28T22:30:46Z
url: https://github.com/astral-sh/uv/issues/4779
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv pip install -U somepkg` should only update the specified package, unless additional upgrades are necessary

---

_Issue opened by @ThiefMaster on 2024-07-03 15:52_

Classic `pip` did the same until a few years ago where they changed the default to `--upgrade-strategy only-if-needed` for which `uv` currently does not have an equivalent. The only way to get this behavior is `uv pip install -P somepkg somepkg` which is a bit ugly since I need to specify the package name twice.

I think this behavior would be more desirable than "upgrade everything" because:

- In most proper production deployments you have lock files anyway and will not use `uv pip install -U` at all
- If you have some extra packages in such an environment, you do NOT want updating that package to also update some random dependency (that's actually specified in another package's lock file)
- In a development environment, I guess it's a matter of preference


My suggestion would be to:

- Change the default to match `--upgrade-strategy only-if-needed` (or at least implement this option)
- Display a notice if any (direct?) dependencies of the specified package could also be updated (maybe only if this does not conflict with version pins of already-installed packages?)

---

_Comment by @zanieb on 2024-07-03 16:05_

Sounds like you're just looking for `--upgrade-package`? Why is it a big deal to specify the name twice? Isn't it harder for users to do `uv pip install <package> --upgrade --upgrade-strategy eager` to update their environment? I've been bitten by this surprising behavior in `pip` several times in CI.

---

_Label `question` added by @zanieb on 2024-07-03 16:05_

---

_Comment by @zanieb on 2024-07-03 16:16_

I'm not particularly opposed to `--upgrade` applying to all of the "given packages" if we can find a nice way to specify upgrading the environment. I guess `--upgrade-strategy` isn't bad.

---

_Label `needs-design` added by @zanieb on 2024-07-03 16:16_

---

_Referenced in [astral-sh/uv#6100](../../astral-sh/uv/issues/6100.md) on 2024-08-15 01:22_

---

_Referenced in [mosaicml/ci-testing#30](../../mosaicml/ci-testing/pulls/30.md) on 2024-09-03 13:55_

---

_Referenced in [astral-sh/uv#8260](../../astral-sh/uv/issues/8260.md) on 2024-10-16 16:34_

---

_Label `question` removed by @zanieb on 2024-10-21 21:23_

---

_Label `compatibility` added by @zanieb on 2024-10-21 21:23_

---

_Comment by @zanieb on 2024-10-21 21:24_

Related https://github.com/astral-sh/uv/issues/7176

---

_Comment by @jph00 on 2024-12-28 22:30_

Also related: #10097 

---

_Referenced in [astral-sh/uv#14162](../../astral-sh/uv/issues/14162.md) on 2025-06-20 16:30_

---

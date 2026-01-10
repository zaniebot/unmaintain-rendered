---
number: 2686
title: Allow per-package overrides
type: issue
state: open
author: adzenith
labels:
  - enhancement
assignees: []
created_at: 2024-03-27T05:07:07Z
updated_at: 2025-08-27T23:48:19Z
url: https://github.com/astral-sh/uv/issues/2686
synced_at: 2026-01-10T01:23:20Z
---

# Allow per-package overrides

---

_Issue opened by @adzenith on 2024-03-27 05:07_

[This comment](https://github.com/astral-sh/uv/issues/511#issuecomment-1831526605) discusses a second type of dependency overriding:
> Replace the requirement on foo==1.2.3 in bar with foo==4.5.6. With this, you say "While bar wants foo 1.2.3, i have confirmed that it is actually compatible with foo 4.5.6, too". It requires more effort on the user side, but is less risky and gives good errors messages when new requirements on foo are added outside of bar.

The parent ticket was resolved based on the first idea (global overrides) and I didn't see another ticket to track this second idea, which as stated is less risky and could give better error messages. I would love to see this feature!

Thanks! I've been having an amazing time with `uv` so far.

---

_Label `enhancement` added by @charliermarsh on 2024-03-29 15:59_

---

_Referenced in [astral-sh/uv#4422](../../astral-sh/uv/issues/4422.md) on 2024-06-20 11:27_

---

_Comment by @hmc-cs-mdrissi on 2024-08-09 18:42_

Supportive of this and hit it today. I have several libraries that depend on library A with various constraints. Most of them have flexible enough constraints that I want them to be used and resolved. A few have constraints that are too tight and I've tested and have confidence it's ok to relax a bit.

I ended up working around this by dropping dependencies I knew I could relax, resolving, then adding them back and adding override. Would be safer experience if I could say this directly.

---

_Comment by @charliermarsh on 2024-11-20 00:33_

This might now exist as `[[tool.uv.dependency-metadata]]`?

https://docs.astral.sh/uv/concepts/resolution/#dependency-metadata

---

_Comment by @Kitryn on 2025-01-03 08:25_

ran into this today and not sure if I'm understanding the solution correctly

issue: we have some dependency `solders` that both `anchorpy` and `solana` depend on. `solana==0.36.1` depends on `solders~=0.23.0`, whereas `anchorpy==0.20.1` depends on `solders~=0.21.0`, thus uv fails as expected, but we know (assume...) that `anchorpy==0.20.1` in fact can work with `solders~=0.23.0`

We can solve this with global overrides, but ideally we'd like to do this with per-package overrides as previously mentioned. Tried using `[[tool.uv.dependency-metadata]]`:

```
[[tool.uv.dependency-metadata]]
name = "anchorpy"
version = "0.20.1"
requires-dist = ["solders~=0.23.0"]
```

Is this what the above solution was referring to? Does that mean we have to include `anchorpy`'s entire list of dependencies in this field? Since if we don't, this replaces the entire dependency list of `anchorpy` with the contents of `requires-dist` so other dependencies don't get installed

example pyproject.toml:
```
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "solana~=0.36.1",
    "anchorpy~=0.20.0"
]

[[tool.uv.dependency-metadata]]
name = "anchorpy"
version = "0.20.0"
requires-dist = ["solders~=0.23.0"]
```

Resultant `uv.lock` snippet:
```
...
[[manifest.dependency-metadata]]
name = "anchorpy"
version = "0.20.0"
requires-dist = ["solders~=0.23.0"]

[[package]]
name = "anchorpy"
version = "0.20.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "solders" },
]
sdist = { url = "https://files.pythonhosted.org/packages/1b/52/9a2a6a515d5f8aec195c691f26222ef2788f442e867bf3ef6de742b972e2/anchorpy-0.20.0.tar.gz", hash = "sha256:4bb4b558f3d566b23e7438b78b441040a52d980ac573fdf3897600b555d37fd9", size = 48047 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/17/18/9ff75cbcbdcdce7c79daf7932ef26e168e5ba6ddd09b4cc01eb6339c4092/anchorpy-0.20.0-py3-none-any.whl", hash = "sha256:173cfbe9bab0ec0ea2548fc82864077880168dc62270b291192b73bb821bb13c", size = 63048 },
]
...
```

Also (don't know if related) - if `solana` was commented out in the pyproject.toml (i.e. single `anchorpy` dependency with the `tool.uv.dependency-metadata` field still included, attempting to override `solders` version), then the resultant uv.lock is as if there's no override (`solders==0.21.0` gets installed)

Thanks! `uv` has been really great in our monorepo.



---

_Referenced in [stfc/janus-core#420](../../stfc/janus-core/issues/420.md) on 2025-02-14 16:38_

---

_Referenced in [stfc/janus-core#421](../../stfc/janus-core/pulls/421.md) on 2025-02-14 17:43_

---

_Comment by @rinarakaki on 2025-02-22 09:26_

I want this feature when I know I can safely ignore dependency conflicts.

---

_Comment by @Kitryn on 2025-05-20 04:36_

any update on this? thanks

---

_Referenced in [astral-sh/uv#12512](../../astral-sh/uv/issues/12512.md) on 2025-05-30 16:41_

---

_Comment by @jiridanek on 2025-08-27 11:04_

* https://github.com/astral-sh/uv/issues/4422#issuecomment-2638910299

```toml
[[tool.uv.dependency-metadata]]
name = "anchorpy"
version = "0.20.1"
requires-dist = ["solders~=0.23.0"]
```

> Is this what the above solution was referring to? Does that mean we have to include anchorpy's entire list of dependencies in this field?

Yes, we'd have to include anchorpy's entire list of dependencies in this field, besides the changed one we're overriding. So while this can be one off fix for problems related to this issue and also for

* https://github.com/astral-sh/uv/issues/4422

the result is hard-to-maintain because error-prone pyproject.toml.

---

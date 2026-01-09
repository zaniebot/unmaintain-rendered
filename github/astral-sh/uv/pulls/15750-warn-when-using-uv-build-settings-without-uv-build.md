---
number: 15750
title: "Warn when using `uv_build` settings without `uv_build`"
type: pull_request
state: open
author: konstin
labels:
  - error messages
  - build-backend
assignees: []
base: main
head: konsti/warn-on-unused-build-backend-settings
created_at: 2025-09-09T12:23:49Z
updated_at: 2025-12-11T14:50:41Z
url: https://github.com/astral-sh/uv/pull/15750
synced_at: 2026-01-07T13:12:19-06:00
---

# Warn when using `uv_build` settings without `uv_build`

---

_Pull request opened by @konstin on 2025-09-09 12:23_

To help with cases such as https://github.com/astral-sh/uv/issues/15655.

A question is when to show this warning. I've used sources as a proxy as URL dependencies with enabled sources are likely those controlled by the user, and they include workspace and `git clone`d path dependencies.

The first commit is a refactoring, the second commit the implementation.

Fixes https://github.com/astral-sh/uv/issues/15740

---

_Label `error messages` added by @konstin on 2025-09-09 12:23_

---

_Label `build-backend` added by @konstin on 2025-09-09 12:23_

---

_@konstin reviewed on 2025-09-12 14:14_

---

_Review comment by @konstin on `crates/uv-build-frontend/src/lib.rs`:604 on 2025-09-12 14:14_

One potential drawback is that we assume that all future versions will have `build-system = "uv_build"`.

---

_@zsol approved on 2025-09-16 15:27_

I've been wanting to make this refactor for a while now

<a href="https://gitme.me/image?url=https%3A%2F%2Fmedia2.giphy.com%2Fmedia%2FyyZRSvISN1vvW%2Fgiphy.gif&token=nice" data-gitmeme-token="nice"><img src="https://media2.giphy.com/media/yyZRSvISN1vvW/giphy.gif" title="Created by gitme.me with /nice"
        alt="nice"/></a>

---

_@zanieb reviewed on 2025-09-16 15:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/build_backend.rs`:1061 on 2025-09-16 15:53_

Can we avoid warning multiple times here?

---

_@zanieb reviewed on 2025-09-16 15:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/build_backend.rs`:1073 on 2025-09-16 15:53_

I think I'd cut some of this redundant text

```suggestion
    warning: There are settings for `uv_build` defined in `tool.uv.build-backend`, but the project does not use the `uv_build` backend: [TEMP_DIR]/pyproject.toml
```

---

_Review comment by @zanieb on `crates/uv/tests/it/build_backend.rs`:1059 on 2025-09-16 15:54_

We might want to rephrase so it's clearer what we're referring to at the end, e.g., "but the `uv_build` backend is not used by the project at: PATH"

---

_@zanieb reviewed on 2025-09-16 15:54_

---

_@konstin reviewed on 2025-09-17 10:50_

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:1061 on 2025-09-17 10:50_

I need to figure our something that works despite the different paths

---

_Comment by @HACK55515 on 2025-10-26 12:02_

aok -_-

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:1061 on 2025-11-24 10:11_

I've removed the path, it's clearer this was got Git dependencies too.

---

_@konstin reviewed on 2025-11-24 10:11_

---

_Referenced in [astral-sh/uv#16928](../../astral-sh/uv/pulls/16928.md) on 2025-12-02 14:27_

---

_Referenced in [astral-sh/uv#16961](../../astral-sh/uv/issues/16961.md) on 2025-12-03 11:20_

---

_Referenced in [astral-sh/uv#16962](../../astral-sh/uv/issues/16962.md) on 2025-12-03 12:54_

---

_Review comment by @zanieb on `crates/uv-build-frontend/src/lib.rs`:608 on 2025-12-03 13:09_

(I'm not sure I'd understand this comment without the context from this pull request review)

---

_@zanieb reviewed on 2025-12-03 13:09_

---

_@zanieb reviewed on 2025-12-03 13:12_

---

_Review comment by @zanieb on `crates/uv/tests/it/build_backend.rs`:1147 on 2025-12-03 13:12_

Does it make sense to say "`pyproject.toml` of"? Isn't that the only place it can be defined?

Does it make sense to say the version? If it's first-party, isn't the version just noise? (and it's present in the built file name too if really helpful).

I think I'd be confused about what it means that it doesn't use uv_build, maybe it'd help to show the other backend?

I think I might say

> project-name defines settings for `uv_build` in `tool.uv.build-backend`, but does not use `uv_build` (<hint>)

where `<hint>` is like

> (no `build-system` is defined)

or

> (`build-system` uses `hatchling`)

---

_@konstin reviewed on 2025-12-11 14:50_

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:1147 on 2025-12-11 14:50_

I've updated the hint to show only the package name and also show the actual build backend

---

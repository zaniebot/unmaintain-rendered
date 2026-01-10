```yaml
number: 13563
title: "Build backend: Support stubs packages"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - build-backend
assignees: []
merged: true
base: main
head: konsti/stubs-in-the-build-backend
created_at: 2025-05-20T21:35:21Z
updated_at: 2025-05-22T17:02:23Z
url: https://github.com/astral-sh/uv/pull/13563
synced_at: 2026-01-10T11:10:41Z
```

# Build backend: Support stubs packages

---

_Pull request opened by @konstin on 2025-05-20 21:35_

Stubs packages are different in that their name ends with `-stubs`, their module is `<module name>-stubs` (with a dash, not the generally legal underscore) and their modules contain a `__init__.pyi` instead of an `__init__.py` (https://typing.python.org/en/latest/spec/distributing.html#stub-only-packages).

We add support in the uv build backend by detecting the `-stubs` suffix.

Fixes #13546

---

_Review requested from @BurntSushi by @konstin on 2025-05-20 21:35_

---

_Review requested from @AlexWaygood by @konstin on 2025-05-20 21:35_

---

_Label `enhancement` added by @konstin on 2025-05-20 21:35_

---

_Label `build-backend` added by @konstin on 2025-05-20 21:35_

---

_Review comment by @AlexWaygood on `crates/uv-build-backend/src/lib.rs`:244 on 2025-05-20 22:11_

```suggestion
        // someone had a regular package with `-stubs`.
```

---

_@AlexWaygood approved on 2025-05-20 22:15_

This LGTM as far as I can tell!

If you want to see how ty handles this in its module resolver, you can grep for uses of [this struct](https://github.com/astral-sh/ruff/blob/d098118e37be9437345d527c29a5aeea91b676a6/crates/ty_python_semantic/src/module_resolver/resolver.rs#L879-L905) in the `crates/ty_python_semantic/src/module_resolver` directory in the Ruff repo

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:245 on 2025-05-21 12:44_

If a false positive happens, is there a way for a user to override that and say, "no, this is not a stubs package"?

Conversely, is there a way for a package that doesn't end with `-stubs` to be treated as a stubs package?

From this PR, it looks like the answer to both above is "no." Are we sure that's okay?

---

_@BurntSushi reviewed on 2025-05-21 12:44_

---

_@konstin reviewed on 2025-05-21 15:47_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:245 on 2025-05-21 15:47_

In one direction, https://typing.python.org/en/latest/spec/distributing.html#stub-only-packages is a clear MUST about the `-stubs` suffix. In the other direction, I think I'd try this out, and if there are really packages with let's say `foo_stubs` we can add an override (`tool.uv.build-backend.stubs = False`). If we can have one option less by having this inference I'd prefer that.

Another option would be to require stubs packages to always declare a module name, so we can tell `-stubs` (stubs package) from `_stubs` (regular module).

---

_@BurntSushi reviewed on 2025-05-21 15:50_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:245 on 2025-05-21 15:50_

> In one direction, https://typing.python.org/en/latest/spec/distributing.html#stub-only-packages is a clear MUST about the `-stubs` suffix. In the other direction, I think I'd try this out, and if there are really packages with let's say `foo_stubs` we can add an override (`tool.uv.build-backend.stubs = False`). If we can have one option less by having this inference I'd prefer that.

I think that's good enough for me.

> Another option would be to require stubs packages to always declare a module name, so we can tell `-stubs` (stubs package) from `_stubs` (regular module).

I do like this idea! But I agree that inference probably makes more sense, and if necessary, we can add a way to override it if it comes up.

---

_@BurntSushi approved on 2025-05-21 15:51_

---

_Comment by @konstin on 2025-05-21 15:53_

Added some docs, mainly to make searching for "stubs" show that we support them.

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:245 on 2025-05-22 17:01_

I realized that we actually do have overrides: Setting the module name to `_stubs` vs. `-stubs` will trigger regular package vs. stubs package behavior.

---

_@konstin reviewed on 2025-05-22 17:01_

---

_Merged by @konstin on 2025-05-22 17:02_

---

_Closed by @konstin on 2025-05-22 17:02_

---

_Branch deleted on 2025-05-22 17:02_

---

_@BurntSushi reviewed on 2025-05-22 17:02_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:245 on 2025-05-22 17:02_

Nice. Yeah I think that SGTM!

---

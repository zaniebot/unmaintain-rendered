```yaml
number: 1268
title: "Implement pip compatible `--no-binary` and `--only-binary` options"
type: pull_request
state: merged
author: zanieb
labels:
  - compatibility
assignees: []
merged: true
base: main
head: zb/pip-compatible-specifier
created_at: 2024-02-08T20:31:38Z
updated_at: 2024-02-12T01:31:42Z
url: https://github.com/astral-sh/uv/pull/1268
synced_at: 2026-01-10T15:33:24Z
```

# Implement pip compatible `--no-binary` and `--only-binary` options

---

_Pull request opened by @zanieb on 2024-02-08 20:31_

Updates our `--no-binary` option and adds a `--only-binary` option for compatibility with `pip` which uses `:all:`, `:none:` and `<name>` for specifying packages.

This required adding support for `--only-binary <name>` into our resolver, previously it was only a boolean toggle.

Retains`--no-build` which is equivalent to `--only-binary :all:`. This is common enough for safety that I would prefer it is available without pip's awkward `:all:` syntax.

---

_Label `compatibility` added by @zanieb on 2024-02-08 20:32_

---

_Comment by @zanieb on 2024-02-08 20:35_

I'd like to improve test coverage here (particularly in unsat cases) but I'll need to add support for no-binary / only-binary to packse and iterate from that.

---

_Review comment by @konstin on `crates/puffin-dispatch/src/lib.rs`:261 on 2024-02-08 20:38_

We should keep the version information so the user can look into the index what failed, something like "{dist} is only available as source distribution, but building {} is disabled".

---

_Review comment by @konstin on `crates/puffin-dispatch/src/lib.rs`:268 on 2024-02-08 20:38_

```suggestion
                        "Only editable builds are exempt from 'no build' checks"
```

---

_Review comment by @konstin on `crates/puffin-traits/src/lib.rs`:203 on 2024-02-08 20:39_

Do we intentionally allow combining them?

---

_Review comment by @konstin on `crates/puffin/src/main.rs`:274 on 2024-02-08 20:42_

What's the use case for this behavior? This seems prone to accidents

---

_Review comment by @konstin on `crates/puffin/src/main.rs`:373 on 2024-02-08 20:43_

I'd prefer removing it instead of keeping it as a hidden opt, same for `no_build`.

---

_Review comment by @konstin on `crates/puffin/tests/pip_install.rs`:707 on 2024-02-08 20:44_

Could we pick an example with less deps to speed up the test suite a bit? Maybe one of the dependencies of flask instead of flask itself.

---

_@konstin approved on 2024-02-08 20:44_

---

_@zanieb reviewed on 2024-02-08 20:49_

---

_Review comment by @zanieb on `crates/puffin-dispatch/src/lib.rs`:261 on 2024-02-08 20:49_

I thought the user should never see this and this was just a guard?

---

_@zanieb reviewed on 2024-02-08 20:49_

---

_Review comment by @zanieb on `crates/puffin/src/main.rs`:274 on 2024-02-08 20:49_

This is `pip`'s interface we're just providing a compatible interface here. I don't think we should do this in our own interface.

---

_Review comment by @zanieb on `crates/puffin-traits/src/lib.rs`:203 on 2024-02-08 20:50_

Same as https://github.com/astral-sh/puffin/pull/1268#discussion_r1483574874

---

_@zanieb reviewed on 2024-02-08 20:50_

---

_@zanieb reviewed on 2024-02-08 20:51_

---

_Review comment by @zanieb on `crates/puffin/src/main.rs`:373 on 2024-02-08 20:51_

Did you mean something else here? This is `no_build`. We could remove it or show it. I think it's a more sensible flag than pip's frankly so maybe we should just keep it and have it be visible.

---

_@konstin reviewed on 2024-02-08 20:54_

---

_Review comment by @konstin on `crates/puffin-dispatch/src/lib.rs`:261 on 2024-02-08 20:54_

Oh right that's true

---

_@konstin reviewed on 2024-02-08 20:55_

---

_Review comment by @konstin on `crates/puffin/src/main.rs`:373 on 2024-02-08 20:55_

I'm fine with either, i'd only like to avoid having used-but-undocumented interfaces.

---

_@konstin reviewed on 2024-02-10 20:20_

---

_Review comment by @konstin on `crates/puffin/tests/pip_install.rs`:728 on 2024-02-10 20:20_

If that still stack overflows, it should use the `PUFFIN_STACK_SIZE` workaround.

```suggestion
```

---

_Merged by @zanieb on 2024-02-12 01:31_

---

_Closed by @zanieb on 2024-02-12 01:31_

---

_Branch deleted on 2024-02-12 01:31_

---

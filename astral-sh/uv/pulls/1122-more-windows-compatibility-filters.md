```yaml
number: 1122
title: More windows compatibility filters
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konsti/more-windows-tests
created_at: 2024-01-26T13:40:00Z
updated_at: 2024-04-29T13:02:05Z
url: https://github.com/astral-sh/uv/pull/1122
synced_at: 2026-01-12T16:04:26Z
```

# More windows compatibility filters

---

_@konstin_

Add windows compatibility filters that filter out windows only deps and reduce the package count appropriately and use normalized path display in tests to ensure a UNC `\\?\` prefix doesn't cause a mismatch. This significantly reduces the number of failing windows tests.

The `pip_uninstall`, `puffin_interpreter` and `requirements_txt` test modules now all pass on Windows.

Summary: 416 tests run: 347 passed, 69 failed, 1 skipped

---

_@konstin reviewed on 2024-01-26 13:40_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/lib.rs`:81 on 2024-01-26 13:40_

The error displays in theory have the same problem as the log messages

---

_@charliermarsh reviewed on 2024-01-26 13:58_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/lib.rs`:81 on 2024-01-26 13:58_

Yes thank you! What's with the underscore?

---

_@konstin reviewed on 2024-01-26 16:22_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/lib.rs`:81 on 2024-01-26 16:22_

`0` isn't a valid ident / already has a value assigned and that seems to be the workaround. [The docs](https://docs.rs/thiserror/latest/thiserror/#details) write about `.0` instead, i guess both work.

---

_@charliermarsh reviewed on 2024-01-26 17:10_

---

_Review comment by @charliermarsh on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-26 17:10_

@zanieb - Do you want to take a look at this too?

---

_@zanieb reviewed on 2024-01-26 18:54_

---

_Review comment by @zanieb on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-26 18:54_

Providing the extra package count expected on Windows is a pretty awkward API. Can we do better?

Also in #1112 I experience a lot of awkwardness because we cannot do any processing between creating the command and the snapshot. Perhaps we should devote some resources to inserting ourselves between the command and its snapshot? Then we could process the snapshot and identify which windows-only dependencies are present and adjust the number accordingly.

---

_@zanieb reviewed on 2024-01-26 18:54_

---

_Review comment by @zanieb on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-26 18:54_

(This does not need to block this pull request)

---

_Review comment by @charliermarsh on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-27 11:19_

I feel like we should at least wrap this in some slightly different and more explicit API. Could we instead make this a bitflags or something that includes the expected packages to remove for the given test?

---

_@charliermarsh reviewed on 2024-01-27 11:19_

---

_@konstin reviewed on 2024-01-27 12:27_

---

_Review comment by @konstin on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-27 12:27_

I'm considering doing our string processing, it seems insta doesn't support programmatic filters 

---

_@zanieb reviewed on 2024-01-27 18:49_

---

_Review comment by @zanieb on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-27 18:49_

I think that makes sense in the long term.

---

_@konstin reviewed on 2024-01-27 19:18_

---

_Review comment by @konstin on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-27 19:18_

I'm torn on writing our own `with_settings! { assert_cmd_snapshot! }` macro, it would fit our testing setup much better but otoh all the support features in insta_cmd are private, so we'll have to duplicate them and lose the other features of that crate.

---

_@zanieb reviewed on 2024-01-27 19:20_

---

_Review comment by @zanieb on `crates/puffin/tests/common/mod.rs`:29 on 2024-01-27 19:20_

I wouldn't really mind vendoring parts of `assert_cmd_snapshot` it's a critical part of our testing and it's pretty inflexible.

---

_Closed by @konstin on 2024-02-05 19:55_

---

_Branch deleted on 2024-04-29 13:02_

---

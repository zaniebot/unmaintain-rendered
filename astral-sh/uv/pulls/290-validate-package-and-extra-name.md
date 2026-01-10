```yaml
number: 290
title: Validate package and extra name
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: validate-package-and-extra-name
created_at: 2023-11-02T12:24:37Z
updated_at: 2023-11-06T10:04:33Z
url: https://github.com/astral-sh/uv/pull/290
synced_at: 2026-01-10T15:50:28Z
```

# Validate package and extra name

---

_Pull request opened by @konstin on 2023-11-02 12:24_

`PackageName` and `ExtraName` can now only be constructed from valid names. They share the same rules, so i gave them the same implementation. Constructors are split between `new` (owned) and `from_str` (borrowed), with the owned version avoiding allocations.

Closes #279

---

_Review requested from @zanieb by @konstin on 2023-11-02 12:24_

---

_@konstin reviewed on 2023-11-02 12:25_

---

_Review comment by @konstin on `crates/puffin-normalize/src/lib.rs`:51 on 2023-11-02 12:25_

I thought about tracking whether it's a package or an extra name error but decided it's not worth it

---

_@zanieb reviewed on 2023-11-02 13:50_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__invalid_extra_name.snap`:20 on 2023-11-02 13:50_

It bothers me that this error repeats the invalid value part — can we make it do a different error message for Clap? 😬 Could be a follow-up

---

_@zanieb reviewed on 2023-11-02 13:51_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:116 on 2023-11-02 13:51_

It does feel like validation of the name shouldn't be a hard blocker _unless_ an extra has been requested so I'm kind of happy it's raw still?

---

_@konstin reviewed on 2023-11-02 14:15_

---

_Review comment by @konstin on `crates/puffin-cli/src/requirements.rs`:116 on 2023-11-02 14:15_

In this case the pyproject.toml isn't valid wrt to PEP 621, so i think we should throw an error

---

_@konstin reviewed on 2023-11-02 14:24_

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_compile__invalid_extra_name.snap`:20 on 2023-11-02 14:24_

Done

---

_@zanieb reviewed on 2023-11-02 14:56_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:116 on 2023-11-02 14:56_

I don't know if being pedantic about that is better for the user 🤷‍♀️ 

---

_@zanieb approved on 2023-11-02 14:57_

---

_@charliermarsh reviewed on 2023-11-02 15:30_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:642 on 2023-11-02 15:30_

Can these be expects instead?

---

_@charliermarsh reviewed on 2023-11-02 15:31_

---

_Review comment by @charliermarsh on `crates/puffin-normalize/src/lib.rs`:51 on 2023-11-02 15:31_

Will this get duplicated with `extra_name_with_clap_error`?

---

_@zanieb reviewed on 2023-11-02 15:49_

---

_Review comment by @zanieb on `crates/pep508-rs/src/lib.rs`:642 on 2023-11-02 15:49_

That'd be nice like 
```
expect("`ExtraName` validation should match PEP 508 parsing")
```

---

_@konstin reviewed on 2023-11-02 15:59_

---

_Review comment by @konstin on `crates/puffin-cli/src/requirements.rs`:116 on 2023-11-02 15:59_

It is! Otherwise users never realize that their pyproject.toml is broken and don't understand why their extra never worked as intended.

---

_@zanieb reviewed on 2023-11-02 16:11_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:116 on 2023-11-02 16:11_

But if they didn't request an extra then we've just forced them to deal with a totally irrelevant issue?

---

_@konstin reviewed on 2023-11-06 09:57_

---

_Review comment by @konstin on `crates/puffin-normalize/src/lib.rs`:51 on 2023-11-06 09:57_

No, the `invalid_extra_name` test checks that

---

_@konstin reviewed on 2023-11-06 09:59_

---

_Review comment by @konstin on `crates/puffin-cli/src/requirements.rs`:116 on 2023-11-06 09:59_

We already require that all the other fields are valid through serde (which is good, it avoids broken configuration going unnoticed, i had that problem with poetry), so doing this here too is more consistent.

Ideally pyproject-toml-rs would use `ExtraName`, but we would need to export them first.

---

_Merged by @konstin on 2023-11-06 10:04_

---

_Closed by @konstin on 2023-11-06 10:04_

---

_Branch deleted on 2023-11-06 10:04_

---

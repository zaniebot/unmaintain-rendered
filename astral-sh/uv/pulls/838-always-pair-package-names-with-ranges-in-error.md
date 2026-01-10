```yaml
number: 838
title: Always pair package names with ranges in error messages
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/available
created_at: 2024-01-08T18:28:49Z
updated_at: 2024-01-08T22:11:11Z
url: https://github.com/astral-sh/uv/pull/838
synced_at: 2026-01-10T15:44:44Z
```

# Always pair package names with ranges in error messages

---

_Pull request opened by @zanieb on 2024-01-08 18:28_

Adjusts display of "no versions available" in error messages to be consistent with other package/range pairings i.e. we usually display "<package-name><range>".

---

_@charliermarsh approved on 2024-01-08 18:29_

---

_Label `error messages` added by @charliermarsh on 2024-01-08 18:29_

---

_@zanieb reviewed on 2024-01-08 18:31_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:85 on 2024-01-08 18:31_

I'm actually so-so on this being easier to read. I know I want to pair the package name and range but I'm not sure what else should be done to make this clearer.

---

_@BurntSushi reviewed on 2024-01-08 18:38_

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_install_scenarios.rs`:85 on 2024-01-08 18:38_

I'm not sure either, but I think the spacing might be contributing to the problem here. When I look at `a<1.0.0 | >1.0.0`, there is a part of my brain that wants to read it as two components: `a<1.0.0 | >` and `1.0.0`. There is another part of my brain that wants to read it as `a<1.0.0` "or" `>1.0.0`, and then my brain wonders, "wait does the `>1.0.0` apply to `a` too?"

---

_@zanieb reviewed on 2024-01-08 18:53_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:85 on 2024-01-08 18:53_

That will be addressed in #810, might be easier to tell what we should do here once we have that.

---

_@zanieb reviewed on 2024-01-08 22:06_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:85 on 2024-01-08 22:06_

Happier with https://github.com/astral-sh/puffin/pull/838/commits/5c2b2bfd57036afcfa22dad2a4b82178f73c4a44 as a start

---

_Merged by @zanieb on 2024-01-08 22:11_

---

_Closed by @zanieb on 2024-01-08 22:11_

---

_Branch deleted on 2024-01-08 22:11_

---

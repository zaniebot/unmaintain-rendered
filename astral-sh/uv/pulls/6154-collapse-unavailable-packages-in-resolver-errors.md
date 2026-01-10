```yaml
number: 6154
title: Collapse unavailable packages in resolver errors
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/error-unavailable
created_at: 2024-08-16T16:41:11Z
updated_at: 2024-08-19T14:02:04Z
url: https://github.com/astral-sh/uv/pull/6154
synced_at: 2026-01-10T13:09:50Z
```

# Collapse unavailable packages in resolver errors

---

_Pull request opened by @zanieb on 2024-08-16 16:41_

Uses my expanding tree reduction knowledge from #6092 to improve the long-standing issue of verbose messages for unavailable packages.

Implements https://github.com/pubgrub-rs/pubgrub/issues/232, but post-resolution instead of during resolution.

Partially addresses https://github.com/astral-sh/uv/issues/5046
Closes https://github.com/astral-sh/uv/issues/2519

---

_Label `error messages` added by @zanieb on 2024-08-16 16:41_

---

_@zanieb reviewed on 2024-08-16 16:41_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:230 on 2024-08-16 16:41_

Need to see the tree before and after reduction when doing complex transforms

---

_@zanieb reviewed on 2024-08-16 16:45_

---

_Review comment by @zanieb on `crates/uv/tests/cache_prune.rs`:244 on 2024-08-16 16:45_

Okay what this error was bad? Need to improve this separately

---

_@zanieb reviewed on 2024-08-16 16:47_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1933 on 2024-08-16 16:47_

Now we enumerate all the versions in a single clause instead of having a clause for each! I think now we can simplify this range with the known available versions and it'll be way better.

---

_Marked ready for review by @zanieb on 2024-08-16 16:52_

---

_@zanieb reviewed on 2024-08-16 16:57_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1933 on 2024-08-16 16:57_

See https://github.com/astral-sh/uv/pull/6155

---

_@zanieb reviewed on 2024-08-16 17:08_

---

_Review comment by @zanieb on `crates/uv/tests/cache_prune.rs`:244 on 2024-08-16 17:08_

See #6156 

---

_@zanieb reviewed on 2024-08-16 17:08_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1965 on 2024-08-16 17:08_

Needs to be plural form "have no usable wheels" — will require more changes separately.

---

_Review requested from @charliermarsh by @zanieb on 2024-08-16 18:51_

---

_Review requested from @BurntSushi by @zanieb on 2024-08-16 18:51_

---

_@charliermarsh reviewed on 2024-08-16 19:57_

---

_Review comment by @charliermarsh on `crates/uv/tests/cache_prune.rs`:251 on 2024-08-16 19:57_

I think this says "and any of network connectivity is disabled"

---

_@charliermarsh reviewed on 2024-08-16 19:57_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:1928 on 2024-08-16 19:57_

Should it be "all of"?

---

_@charliermarsh approved on 2024-08-16 19:58_

---

_@zanieb reviewed on 2024-08-16 20:18_

---

_Review comment by @zanieb on `crates/uv/tests/cache_prune.rs`:251 on 2024-08-16 20:18_

https://github.com/astral-sh/uv/pull/6154#discussion_r1720079162

---

_@zanieb reviewed on 2024-08-16 20:19_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1928 on 2024-08-16 20:19_

Yeah it's a little confusing. I think it's saying "any of a or b" instead of "all of a and b" — we don't use "and" or "or" so "all" and "any" do that. We can probably make this better somehow...

---

_Merged by @zanieb on 2024-08-16 20:19_

---

_Closed by @zanieb on 2024-08-16 20:19_

---

_Branch deleted on 2024-08-16 20:20_

---

_@BurntSushi reviewed on 2024-08-19 14:02_

Nice! Much better.

---

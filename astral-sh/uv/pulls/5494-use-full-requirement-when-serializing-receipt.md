```yaml
number: 5494
title: Use full requirement when serializing receipt
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/req-toml
created_at: 2024-07-26T21:01:59Z
updated_at: 2024-07-31T16:16:40Z
url: https://github.com/astral-sh/uv/pull/5494
synced_at: 2026-01-12T16:06:51Z
```

# Use full requirement when serializing receipt

---

_@charliermarsh_

## Summary

The current receipt doesn't capture quite enough information. For example, it doesn't differentiate between editable and non-editable requirements. This PR instead uses the full `Requirement` type. I think we should use a custom representation like we do in the lockfile, but I'm just using the default representation to demonstrate the idea.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-26 21:02_

---

_Label `preview` added by @charliermarsh on 2024-07-26 21:02_

---

_@charliermarsh reviewed on 2024-07-26 21:02_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:401 on 2024-07-26 21:02_

This was the motivating example.

---

_@charliermarsh reviewed on 2024-07-26 22:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:383 on 2024-07-26 22:18_

If we're comfortable with the general idea here, I will iron out the actual requirement schema.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-29 22:06_

---

_Comment by @charliermarsh on 2024-07-29 22:06_

Please just focus on the concept here ([example](https://github.com/astral-sh/uv/pull/5494/files#r1693648630)) and ignore the code and exact representation for now.

---

_Comment by @zanieb on 2024-07-30 13:33_

I think this makes sense to me. My only concern is that I want users to be able to declare tools declaratively eventually. I don't think that's in conflict though — I think the receipt can be considered "machine read / write" and "human inspectable" like the lock file. We can iron out a separate schema for users to declaratively request tools later.

---

_Comment by @BurntSushi on 2024-07-30 14:55_

I'm not terribly familiar with tool receipts yet. Can you say more at a high level about what problem this is trying to solve?

(Insomuch as I understand what this PR is doing, I think it LGTM.)

---

_Comment by @charliermarsh on 2024-07-30 15:00_

Copying my response from Discord:

```
Yes sorry
When a user installs a tool (like cargo install), we save a receipt alongside it enumerating the input requirements
When they run uv tool upgrade, we respect those input requirements
...
It was using PEP 508 so...
If a user did tool install -e ./black, we didn't capture that it was installed as editable, for example
Since that can't be expressed in PEP 508
But our requirement type is richer
```

---

_Comment by @charliermarsh on 2024-07-31 14:20_

Ok happy with how this turned out.

---

_Comment by @charliermarsh on 2024-07-31 14:26_

Ready for review @zanieb @BurntSushi at your convenience.

---

_Review comment by @BurntSushi on `crates/pypi-types/src/requirement.rs`:660 on 2024-07-31 15:17_

If we're using a Git URL here, perhaps we should in the lock file too? Then I could just close https://github.com/astral-sh/uv/pull/4631.

---

_@BurntSushi approved on 2024-07-31 15:17_

LGTM

---

_@charliermarsh reviewed on 2024-07-31 15:19_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:660 on 2024-07-31 15:19_

That seems ok to me.

---

_@charliermarsh reviewed on 2024-07-31 15:20_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:660 on 2024-07-31 15:20_

Let me re-review that quickly, I think I lost it.

---

_@charliermarsh reviewed on 2024-07-31 15:21_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:660 on 2024-07-31 15:21_

Honestly, I'm fine with either!

---

_Merged by @charliermarsh on 2024-07-31 16:16_

---

_Closed by @charliermarsh on 2024-07-31 16:16_

---

_Branch deleted on 2024-07-31 16:16_

---

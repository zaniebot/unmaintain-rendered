```yaml
number: 2062
title: Update linters pypi links to latest version
type: pull_request
state: merged
author: alonme
labels: []
assignees: []
merged: true
base: main
head: update-readme-linters-links
created_at: 2023-01-21T16:20:32Z
updated_at: 2023-01-22T18:10:33Z
url: https://github.com/astral-sh/ruff/pull/2062
synced_at: 2026-01-12T04:52:00Z
```

# Update linters pypi links to latest version

---

_Pull request opened by @alonme on 2023-01-21 16:20_

I saw that some of the links to the linters PYPI page are not pointing to the latest version - updated those.

BTW - any reason to link to a specific version and not to the latest (https://pypi.org/project/ruff/ instead of https://pypi.org/project/ruff/0.0.228/) ?

---

_Comment by @charliermarsh on 2023-01-21 18:05_

Yeah, the versions actually were intentional, since they represent the version at which the plugin was ported over (in the event that they continue to evolve). But, maybe that's just confusing, and not very helpful.

If we do want to change this, though, it needs to be done in Rust files themselves -- so, e.g., we'd need to change this:

```rust
//! Rules from [flake8-tidy-imports](https://pypi.org/project/flake8-tidy-imports/4.8.0/).
```

Then run `cargo dev generate-all` to regenerate the README :)

---

_Comment by @alonme on 2023-01-21 19:56_

Ok cool,
I found it confusing - but maybe it just needs to say that explicitly,
So whatever you think is best

---

_Comment by @charliermarsh on 2023-01-21 20:10_

I think it's fine to change it, if you don't mind modifying the references in those source files.

---

_Comment by @alonme on 2023-01-21 21:43_

by change do you mean update to latest version or delete the version?

---

_Comment by @charliermarsh on 2023-01-21 21:47_

Ah sorry, I mean remove the version, and point to latest.

---

_Comment by @not-my-profile on 2023-01-22 07:14_

I think it makes sense to remove the versions since if anything we'd have to track the upstream version for each rule individually ... it doesn't make sense to track it for a whole linter since we add and update rules on an individual basis.

---

_@not-my-profile requested changes on 2023-01-22 07:15_

The links should include the trailing `/` because otherwise there's an unnecessary redirect when following these links.

You'll also want to run `cargo dev regenerate-all` to update the links in the README.

---

_@sbrugman reviewed on 2023-01-22 12:40_

Please also update the `scripts/add_plugin.py` example:
https://github.com/charliermarsh/ruff/blob/main/scripts/add_plugin.py#L8

---

_Review requested from @sbrugman by @alonme on 2023-01-22 14:58_

---

_Review request for @sbrugman removed by @alonme on 2023-01-22 14:58_

---

_Review requested from @not-my-profile by @alonme on 2023-01-22 14:58_

---

_Review comment by @not-my-profile on `src/rules/pygrep_hooks/mod.rs`:1 on 2023-01-22 15:36_

nit: but the trailing slash isn't necessary for GitHub

---

_@not-my-profile reviewed on 2023-01-22 15:36_

---

_@not-my-profile reviewed on 2023-01-22 15:37_

---

_Review comment by @not-my-profile on `src/rules/flake8_import_conventions/mod.rs`:1 on 2023-01-22 15:37_

same here^^

---

_@sbrugman reviewed on 2023-01-22 16:16_

---

_Review comment by @sbrugman on `scripts/add_plugin.py`:8 on 2023-01-22 16:16_

There is one more reference on line 107

---

_Merged by @charliermarsh on 2023-01-22 18:10_

---

_Closed by @charliermarsh on 2023-01-22 18:10_

---

_Comment by @charliermarsh on 2023-01-22 18:10_

Thanks!

---

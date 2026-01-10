```yaml
number: 10833
title: Respect visitation order for proxy packages
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - resolver
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2025-01-21T23:15:37Z
updated_at: 2025-01-22T17:12:48Z
url: https://github.com/astral-sh/uv/pull/10833
synced_at: 2026-01-10T11:45:13Z
```

# Respect visitation order for proxy packages

---

_Pull request opened by @charliermarsh on 2025-01-21 23:15_

## Summary

This PR reverts https://github.com/astral-sh/uv/pull/10441 and applies a different fix for https://github.com/astral-sh/uv/issues/10425.

In #10441, I changed prioritization to visit proxies eagerly. I think this is actually wrong, since it means we prioritize proxy packages above _everything_ else. And while a proxy only depends on itself, it does mean we're selecting a _version_ for the proxy package earlier than anything else. So, if you look at #10828, we end up choosing a version for `async-timeout` before we choose a version for `langchain`, despite the latter being a first-party dependency. (`async-timeout` has a marker on it, so it has a proxy package, so we solve for it first.)

To fix #10425, we instead need to make sure we visit proxies in the order we see them. I think the virtual tiebreaker for proxies is reversed? We want to visit the package we see first, first.

So, in short: this reverts #10441, then corrects the ordering for visiting proxies.

Closes https://github.com/astral-sh/uv/issues/10828.


---

_Review requested from @konstin by @charliermarsh on 2025-01-21 23:15_

---

_Label `bug` added by @charliermarsh on 2025-01-21 23:15_

---

_Label `resolver` added by @charliermarsh on 2025-01-21 23:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2313 on 2025-01-22 11:14_

Where are the hash changes coming from?

---

_Review comment by @konstin on `crates/uv-resolver/src/dependency_provider.rs`:21 on 2025-01-22 12:09_

See #10853

---

_Comment by @konstin on 2025-01-22 12:14_

> To fix https://github.com/astral-sh/uv/issues/10425, we instead need to make sure we visit proxies in the order we see them. I think the virtual tiebreaker for proxies is reversed? We want to visit the package we see first, first.

Would it fix https://github.com/astral-sh/uv/issues/10425 if we visited the virtual packages inside the package-batch first?

Like:

* A{foo}
* A{bar}
* A <- This one is last
* B <- has no virtual packages
* C{python_version}
* C{extra}
* C <- this one is last

---

_@konstin approved on 2025-01-22 12:16_

See #10853 for a follow-up 

---

_Comment by @charliermarsh on 2025-01-22 16:57_

@konstin -- I actually don't think that would be sufficient. Imagine:

```
foo==1
bar==1
```

We resolve `foo`, which depends on `bar ; sys_platform == "darwin"`. We'd then prioritize the `bar ; sys_platform == "darwin"` proxy over the base package, and we'd select a version of `bar` that didn't meet the `bar==1` requirement.


---

_Comment by @charliermarsh on 2025-01-22 16:57_

I think a separate thing we could do, to make the order here less critical, is change these hash-mismatch errors to incompatibilities, so we backtrack rather than fail hard just for _trying_ an incompatible package.

---

_Merged by @charliermarsh on 2025-01-22 17:12_

---

_Closed by @charliermarsh on 2025-01-22 17:12_

---

_Branch deleted on 2025-01-22 17:12_

---

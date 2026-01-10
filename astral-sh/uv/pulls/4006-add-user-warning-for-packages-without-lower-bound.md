```yaml
number: 4006
title: "Add user warning for packages without lower bound with `--resolution=lowest` or `lowest-direct`"
type: pull_request
state: closed
author: ottaviohartman
labels: []
assignees: []
base: main
head: thartman/warning-for-resolution-lowest
created_at: 2024-06-04T01:46:25Z
updated_at: 2024-07-18T14:34:26Z
url: https://github.com/astral-sh/uv/pull/4006
synced_at: 2026-01-10T13:42:52Z
```

# Add user warning for packages without lower bound with `--resolution=lowest` or `lowest-direct`

---

_Pull request opened by @ottaviohartman on 2024-06-04 01:46_

## Summary

Fixes #2797.

Warns the user if a direct or transitive dependency is unpinned when using `--resolution=lowest` or `lowest-direct`.

## TODO
- [ ] Write unit tests

## Test Plan

- [ ] Awaiting confirmation that this implementation looks relatively OK before writing unit tests.

---

_Review requested from @konstin by @zanieb on 2024-06-04 14:43_

---

_Review requested from @konstin by @zanieb on 2024-06-04 14:43_

---

_@ottaviohartman reviewed on 2024-06-04 21:26_

---

_Review comment by @ottaviohartman on `_typos.toml`:14 on 2024-06-04 21:26_

Had to add this otherwise the pre-commit hook would think `Nd` should be `And` in `pip_install.rs`

---

_Comment by @konstin on 2024-06-05 10:44_

Thanks for creating this PR!

There's two parts to having warnings here: Warnings for direct dependencies, those that the user controls, and warnings for indirect dependencies, those that the user doesn't control.

We can warn for direct dependencies, the user can add dependency constraints (as implemented).

For indirect dependencies, the user can't add constraints on that other package. Whenever we encounter an unconstrained dependency in pubgrub, we should `debug!` it with the name and version package that contained the dependency (instead of `warn_user_once!`). This helps with debugging without warning about something that users can't change themselves.

In the resolver, we may encounter unconstrained dependencies that are constrained elsewhere, either by another dependency or by the user passing a constraint files. After the resolver is done, we should note unconstrained dependencies in two case: There was a build failure of a dependency lacking a lower bound. In this case, we should add a note about the missing lower bound to the context of the build error. The other case is that after we're done, we have picked the lowest version of a dependency without any constraint (that is most likely to fail when running the code), we should warn about that and ask the user to add a constraint.

For the tests, we should follow our own best practices and add lower bounds for the lowest(-direct) tests except for one or two tests that check for warnings.

I would suggest splitting this into two PRs: One that only adds the warnings for the lowest-direct mode and debug logging for missing constraint on indirect dependencies, and another one that adds the context on failed builds and warns after the resolution finished.

---

_Comment by @ottaviohartman on 2024-06-07 01:56_

Thanks for the guidance @konstin , it's very helpful.

I'll convert this PR into what you recommend as the first PR:

> only adds the warnings for the lowest-direct mode and debug logging for missing constraint on indirect dependencies

What's the best place in the code to put the warning? The resolver, as implemented, misses constraints (I think?) but more importantly doesn't correctly notice that the following is pinned:

```sh
anyio @ https://files.pythonhosted.org/packages/2d/b8/7333d87d5f03247215d86a86362fd3e324111788c6cdd8d2e6196a6ba833/anyio-4.2.0.tar.gz
```

I'll be looking around the code in the meantime to find an appropriate spot to check if a version is unpinned.

---

_Comment by @konstin on 2024-06-07 07:26_

You can check `PubGrubPackage` for `PubGrubPackageInner` -> `Package` -> `url: None`

---

_Comment by @zanieb on 2024-07-18 14:34_

Taken up in https://github.com/astral-sh/uv/pull/5142 now

---

_Closed by @zanieb on 2024-07-18 14:34_

---

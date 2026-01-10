```yaml
number: 3452
title: "Warn when missing minimal bounds when using `tool.uv.sources`"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/ensure-bound
created_at: 2024-05-08T07:41:18Z
updated_at: 2024-08-23T11:05:43Z
url: https://github.com/astral-sh/uv/pull/3452
synced_at: 2026-01-10T13:09:50Z
```

# Warn when missing minimal bounds when using `tool.uv.sources`

---

_Pull request opened by @konstin on 2024-05-08 07:41_

When using `tool.uv.sources`, we warn that requirements have a bound, i.e. at least a lower version constraint.

When using a library, the symbols you import were introduced in different versions, creating an implicit lower bound. This warning makes this explicit. This is crucial to prevent backtracking resolvers from selecting an ancient versions that is not compatible (or worse, doesn't build), and a performance optimization on top.

This feature is gated to `tool.uv.sources` (as it should have been to begin with for #3263/#3443) to not unnecessarily break legacy workflows. It is also helpful specifically when using a `tool.uv.sources` section that contains constraints that are not published to pypi, e.g. for workspace dependencies. We can adjust those later to e.g. not constrain workspace dependencies with `publish = false`, but i think it's the right setting to start with.

---

_Comment by @zanieb on 2024-05-08 13:31_

I think I have a strong preference for this being a warning? 

Maybe it'd be configurable in the future.. i.e. `unconstrainted-requirements = error | warn | allow`

A warning seems less likely to block adoption.

---

_Renamed from "Enforce minimal bounds when using `tool.uv.sources`" to "Warn when missing minimal bounds when using `tool.uv.sources`" by @konstin on 2024-05-08 13:52_

---

_Label `preview` added by @konstin on 2024-05-08 13:53_

---

_@zanieb reviewed on 2024-05-08 13:53_

---

_Review comment by @zanieb on `crates/uv-requirements/src/pyproject.rs`:410 on 2024-05-08 13:53_

Should we use `warn_user_once`?

---

_Comment by @konstin on 2024-05-08 13:54_

This is a correctness check (no lower bound is almost certainly incorrect), you should not be able to silence correctness checks; the correct way to get rid of the warning is to add the lower bound your project has, either implicitly or by policy.

---

_@zanieb reviewed on 2024-05-08 13:54_

---

_Review comment by @zanieb on `crates/uv-requirements/src/pyproject.rs`:497 on 2024-05-08 13:54_

Can we frame this without the "You" i.e. "No version constraint (e.g. ...) specified for {}"

---

_@zanieb reviewed on 2024-05-08 13:55_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:8658 on 2024-05-08 13:55_

Is there a test ensuring this warning is not raised without `tool.uv.sources` present?

---

_@zanieb approved on 2024-05-08 13:55_

---

_Comment by @charliermarsh on 2024-05-08 14:16_

I'm not totally convinced that I want this, but I guess it's fine for now if it's limited to `tool.uv.sources` and doesn't show up for non-sources users.

---

_@charliermarsh reviewed on 2024-05-08 14:16_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:497 on 2024-05-08 14:16_

Let's make this: "Missing version constraint (e.g., lower bound) for `{}`"

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:497 on 2024-05-08 14:16_

Note the backticks

---

_@charliermarsh reviewed on 2024-05-08 14:16_

---

_@charliermarsh reviewed on 2024-05-08 14:33_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:497 on 2024-05-08 14:33_

Please change "e.g. a lower bound" to "e.g., lower bound".

---

_Merged by @charliermarsh on 2024-05-08 17:16_

---

_Closed by @charliermarsh on 2024-05-08 17:16_

---

_Branch deleted on 2024-05-08 17:16_

---

_Comment by @denkasyanov on 2024-08-22 21:34_

Is there a way to add lower bounds automatically for installed dependencies that doesn't have them?

Not sure what exactly triggered the warning, update to 0.3.1 or (I guess more likely) adding a dependency from GitHub. 

---

_Comment by @konstin on 2024-08-23 08:13_

@denkasyanov Can you describe steps to reproduce your problem and the exact error you're seeing?

---

_Comment by @denkasyanov on 2024-08-23 11:05_

Sorry, should have added more context from the beginning.

I updated from uv 0.3.0 to 0.3.1 AND replaced the `spyne` package from pypi with `spyne` package from GitHub like that:
```shell
uv remove spyne
uv add git+https://github.com/arskom/spyne@f105ec2f41495485fef1211fe73394231b3f76e5
```

After that I tried to run `uv sync` and received warnings

```
warning: Missing version constraint (e.g., a lower bound) for `celery`
warning: Missing version constraint (e.g., a lower bound) for `django`
```

My dependencies at the moment:

```toml
[project]
dependncies = [
  "celery",
  "django",
 ]
```

This was expected, so no error (not even a real problem) here.

But since I had a ton of dependencies, I thought, maybe there is a way to "half-lock" them in the [project] table.

So that I could turn the example above into something like:

```toml
[project]
dependncies = [
  "celery>=5.4.0",
  "django>=5.1",
 ]
```

for all regular and dev dependencies.

---

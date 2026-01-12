```yaml
number: 6171
title: Respect release-only semantics of python_full_version when constructing markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - resolver
assignees: []
merged: true
base: main
head: charlie/pre-py
created_at: 2024-08-17T17:26:41Z
updated_at: 2024-08-17T19:29:58Z
url: https://github.com/astral-sh/uv/pull/6171
synced_at: 2026-01-12T16:07:15Z
```

# Respect release-only semantics of python_full_version when constructing markers

---

_@charliermarsh_

## Summary

In the resolver, we use release-only semantics to normalize `python_full_version`. So, if we see `python_full_version < '3.13'`, we treat that as `(Unbounded, Exclude(3.13))`. `3.13b0` evaluates as `true` to that range, so we were accepting pre-releases for these markers.

Instead, we need to exclude pre-release segments when performing these evaluations.

Closes https://github.com/astral-sh/uv/issues/6169.

## Test Plan

Hard to write a test for this because you need a pre-release Python locally... so:

`echo "sqlalchemy==2.0.32" | cargo run pip compile - --python 3.13 -n`


---

_Label `bug` added by @charliermarsh on 2024-08-17 17:29_

---

_Label `resolver` added by @charliermarsh on 2024-08-17 17:29_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-17 17:29_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/resolver_markers.rs`:26 on 2024-08-17 17:38_

Can we guarantee pre-releases are stripped in `MarkerEnvironment` instead?

---

_@ibraheemdev reviewed on 2024-08-17 17:38_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/resolver_markers.rs`:26 on 2024-08-17 17:39_

I don't think they should be... I think `MarkerEnvironment` should be capable of representing the true markers. Otherwise, we'll misreport (for example) when we send telemetry up to PyPI about the user's Python version.

---

_@charliermarsh reviewed on 2024-08-17 17:39_

---

_@charliermarsh reviewed on 2024-08-17 17:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/resolver_markers.rs`:26 on 2024-08-17 17:40_

(I did consider that.)

---

_@charliermarsh reviewed on 2024-08-17 17:46_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/resolver_markers.rs`:26 on 2024-08-17 17:46_

(Do you disagree?)

---

_@ibraheemdev reviewed on 2024-08-17 17:52_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/resolver_markers.rs`:26 on 2024-08-17 17:52_

Hmm okay that makes sense. I guess `ResolverMarkers` is the correct place to put this then.

---

_@ibraheemdev approved on 2024-08-17 17:52_

---

_@charliermarsh reviewed on 2024-08-17 19:17_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/resolver_markers.rs`:26 on 2024-08-17 19:17_

I'll add distinct type to enforce this.

---

_Merged by @charliermarsh on 2024-08-17 19:29_

---

_Closed by @charliermarsh on 2024-08-17 19:29_

---

_Branch deleted on 2024-08-17 19:29_

---

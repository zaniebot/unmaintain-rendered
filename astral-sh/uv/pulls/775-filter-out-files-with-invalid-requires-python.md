```yaml
number: 775
title: Filter out files with invalid requires python specifiers
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: filter-out-invalid-files
created_at: 2024-01-04T13:16:32Z
updated_at: 2024-01-09T02:46:29Z
url: https://github.com/astral-sh/uv/pull/775
synced_at: 2026-01-10T15:44:44Z
```

# Filter out files with invalid requires python specifiers

---

_Pull request opened by @konstin on 2024-01-04 13:16_

Instead of trying to fixup _all_ the invalid version specifiers on pypi and elsewhere, this filters out distributions with invalid `requires-python` version specifiers that even `LenientVersionSpecifiers` couldn't parse, as opposed to failing entirely, which we currently do.

I would be nicer to model through an invalid distribution pubgrub type, together with e.g. source dists with an unknown extension, so that the version itself still shows up in the error trace.

At the same time, we reduce the log level for fixups from warning to trace, as they are not actionable for the user.

---

_@charliermarsh reviewed on 2024-01-04 13:50_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-04 13:50_

Hmm, should we make this user-facing?

---

_@konstin reviewed on 2024-01-04 13:56_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-04 13:56_

I wouldn't because not only is there nothing the user can do (they don't control the package), but there's also nothing the package author can really do because this old metadata is immutable (unless deleting old versions, which i don't even know if it's possible). Yanking isn't an option either, the file will display as yanked but the invalid metadata will remain. Having a pubgrub tombstone would be nice though.

---

_@charliermarsh reviewed on 2024-01-04 13:57_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-04 13:57_

My concern is that we'll now never see these (unlike today when we're being forced to fix them).

---

_@charliermarsh reviewed on 2024-01-04 13:57_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-04 13:57_

I'm actually not sure that we should filter them out at all, or perhaps we keep them in debug or something?

---

_@konstin reviewed on 2024-01-04 14:04_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-04 14:04_

We could warn if the file is younger than let's say 6 months, so invalid requires python values are phased out.

> I'm actually not sure that we should filter them out at all, or perhaps we keep them in debug or something?

What would you use of requires python value for those?

I'd like a pubgrub tombstone, it just felt like a disproportionate amount of work.

---

_@charliermarsh reviewed on 2024-01-04 16:39_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-04 16:39_

I more mean: should we add debug assertions here?

---

_@konstin reviewed on 2024-01-04 17:01_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-04 17:01_

i don't think so

---

_@konstin reviewed on 2024-01-05 15:47_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-05 15:47_

I'd go even further and say we should silence the other warnings too:
```
$ target/profiling/puffin pip-compile --no-cache scripts/requirements/boto3.in
warning:  Fixing invalid requirement by removing trailing comma, removing stray quotes (before: `botocore>=1.3.0,<1.4.0',`; after: `botocore>=1.3.0,<1.4.0`)
⠼ boto3==1.2.0
```

This is not actionable on either side. I'd say it's only actionable if it's the latest version that has this kind of problem, because then releasing a new version is a fix.

---

_@charliermarsh reviewed on 2024-01-06 22:01_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-06 22:01_

I agree we should silence those

---

_@charliermarsh reviewed on 2024-01-06 22:01_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-06 22:01_

But we need a way to detect these when developing. If we didn't have these errors, we never would've found any of the existing fixups.

---

_@konstin reviewed on 2024-01-08 13:29_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:477 on 2024-01-08 13:29_

I've set the level to `warn` for `cfg(debug_assertion)` and `trace` otherwise. I've reduced the log level for successful fixups to trace. I'll look into passing these to pubgrub later.

---

_Review comment by @charliermarsh on `crates/pypi-types/src/lenient_requirement.rs`:65 on 2024-01-08 18:00_

I really think this should be `warn`, even if it's not user-facing.

---

_@charliermarsh reviewed on 2024-01-08 18:00_

---

_@charliermarsh reviewed on 2024-01-08 18:01_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:478 on 2024-01-08 18:01_

I still feel like this should be a debug assertion, so that we fail in debug builds here. What's the downside?

---

_@charliermarsh approved on 2024-01-09 02:40_

I made them tracing warnings (not user-facing by default) just to keep this moving, let me know if you disagree.

---

_Label `internal` added by @charliermarsh on 2024-01-09 02:40_

---

_Merged by @charliermarsh on 2024-01-09 02:46_

---

_Closed by @charliermarsh on 2024-01-09 02:46_

---

_Branch deleted on 2024-01-09 02:46_

---

```yaml
number: 8156
title: "Add `uv license` or similar to audit dependency licenses"
type: issue
state: open
author: autinerd
labels:
  - wish
assignees: []
created_at: 2024-10-13T08:41:18Z
updated_at: 2024-12-13T16:53:48Z
url: https://github.com/astral-sh/uv/issues/8156
synced_at: 2026-01-10T04:36:20Z
```

# Add `uv license` or similar to audit dependency licenses

---

_Issue opened by @autinerd on 2024-10-13 08:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

It would be cool to have a possibility to audit licenses of dependencies like `pip-license` does. (And including the new License-Expression in Metadata 2.4)

---

_Label `wish` added by @charliermarsh on 2024-10-14 13:43_

---

_Comment by @yayami3 on 2024-11-29 06:10_

I'm on board!

---

_Comment by @ryanleary on 2024-12-13 16:48_

@charliermarsh would an appropriate approach here to include license information in the `uv.lock` file? I am new to the `uv` codebase, but exploring the feasibility of adding this capability.

It seems that ideally given a set of dependencies pulled in from a Lockfile that we would then introspect the license and report back.  I spent an ~hour exploring the `Package` and `PackageMetadata` data models. It seems like most of what is discovered by the resolver is ultimately serialized into uv.lock and I'm not sure that would be appropriate here.

I'm exploring the codepath of `uv tree` as that seems most conceptually similar. Given a list of Packages, what would the most sensible way be to access either (a) classifiers, and/or (b) pyproject toml license data?

---

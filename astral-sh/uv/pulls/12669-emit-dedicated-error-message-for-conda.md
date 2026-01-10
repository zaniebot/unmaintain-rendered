```yaml
number: 12669
title: Emit dedicated error message for Conda environment.yml files
type: pull_request
state: merged
author: christeefy
labels:
  - error messages
assignees: []
merged: true
base: main
head: error-msg-for-environment-yml
created_at: 2025-04-04T12:25:06Z
updated_at: 2025-04-13T17:05:01Z
url: https://github.com/astral-sh/uv/pull/12669
synced_at: 2026-01-10T11:10:40Z
```

# Emit dedicated error message for Conda environment.yml files

---

_Pull request opened by @christeefy on 2025-04-04 12:25_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #12606.

Two options considered, thanks to @zanieb's guidance are:
1. Special-casing on parse error and encountering the `environment.yml` filename, possibly at `RequirementsTxt::parse`
2. Adding a new `RequirementsSource::EnvironmentYml` variant and erroring on `RequirementSpecification::from_source`

I went with the latter for the following reasons:
- This edge case is explicitly modelled within the type system. However, it changes the semantics of `RequirementsSource` to also model _unsupported_ sources.
- (**Separation of concerns**) The special-casing would occur in the `uv-requirements-txt` crate, which seems to be relatively deep in the guts of the codebase. In my opinion, maintainers working in `uv-requirements-txt` would reasonably assume the input file to be a `requirements.txt` file, instead of having to be concerned with it being another file format (`environment.yml`, `pyproject.toml`, etc.)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Manually tested as follows:
```sh
>>> cargo run -- pip install -r environment.yml
error: Conda environment file `environment.yml` is not supported

>>> cargo run -- add -r environment.yml
error: Conda environment file `environment.yml` is not supported
``` 

If you can point me to the appropriate test module, I can write up tests for these to use `insta`.

<!-- How was it tested? -->


---

_Renamed from "Produce dedicated error message on for Conda environment.yml files" to "Produce dedicated error message for Conda environment.yml files" by @christeefy on 2025-04-04 12:25_

---

_Renamed from "Produce dedicated error message for Conda environment.yml files" to "Emit dedicated error message for Conda environment.yml files" by @christeefy on 2025-04-04 12:29_

---

_Comment by @zanieb on 2025-04-04 16:31_

Nice, can you add a couple unit tests? e.g., in `it/edit.rs` and `it/pip_install.rs`?

---

_Label `error messages` added by @zanieb on 2025-04-04 16:31_

---

_Comment by @christeefy on 2025-04-04 20:45_

Done! Thanks for the help, Zanie 😊

---

_Comment by @christeefy on 2025-04-04 20:55_

By the way, I noticed a couple of things:
- In the tests, should the exit code be 1 (user input error) instead of 2 (unexpected error)?
- I think `RequirementsSource::from_requirements_txt` isn't used anywhere in the repo—should we consider removing it?

---

_@zanieb reviewed on 2025-04-04 23:14_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:3694 on 2025-04-04 23:14_

I'd probably say something like 

> Conda environment files (i.e., `{}`) are not support

---

_Comment by @zanieb on 2025-04-04 23:16_

> In the tests, should the exit code be 1 (user input error) instead of 2 (unexpected error)?

It'd probably be "better", but I think it's fine as is. We're not strict about this right now.

> I think RequirementsSource::from_requirements_txt isn't used anywhere in the repo—should we consider removing it?

Oh, interesting. I presume it's fine to remove. @charliermarsh?



---

_@zanieb approved on 2025-04-04 23:16_

---

_@christeefy reviewed on 2025-04-05 12:26_

---

_Review comment by @christeefy on `crates/uv/tests/it/edit.rs`:3694 on 2025-04-05 12:26_

Makes sense; updated

---

_Comment by @christeefy on 2025-04-06 22:38_

> It'd probably be "better", but I think it's fine as is. We're not strict about this right now.

Got it.

By the way, the `deadsnakes` CI timed out — seems to be transient. Appreciate if you can help to re-run it :) 

---

_Merged by @zanieb on 2025-04-07 23:11_

---

_Closed by @zanieb on 2025-04-07 23:11_

---

_Comment by @zanieb on 2025-04-07 23:16_

Thanks!

---

_Branch deleted on 2025-04-08 11:31_

---

_@Seagh0098 approved on 2025-04-13 17:04_

---

_@Seagh0098 approved on 2025-04-13 17:05_

---

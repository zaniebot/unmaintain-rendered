```yaml
number: 17188
title: "Fix dropped support of `-` in pip constraints/overrides/excludes/build_constraints"
type: pull_request
state: merged
author: EndPositive
labels: []
assignees: []
merged: true
base: main
head: fix/stdin-redirection-pip-install
created_at: 2025-12-19T10:34:44Z
updated_at: 2025-12-24T13:47:39Z
url: https://github.com/astral-sh/uv/pull/17188
synced_at: 2026-01-10T05:49:14Z
```

# Fix dropped support of `-` in pip constraints/overrides/excludes/build_constraints

---

_Pull request opened by @EndPositive on 2025-12-19 10:34_

Since #16923, `-` stdin paths are suddenly only supported on the `RequirementsSource::Extensionless`. However, parsing of cli arguments using `from_requirements_txt`, `from_constraints_txt` `from_overrides_txt` would always output a `RequirementsSource::RequirementsTxt`. Resulting in the error:

```
$ cat overrides.txt | cargo run --bin uv --profile dev --manifest-path ./uv/crates/uv/Cargo.toml pip install 'numpy' --overrides=-
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.22s
     Running `./uv/target/debug/uv pip install 'numpy' --overrides=-`
error: File not found: `-`
```

In this PR, I've added a small check in those for the `-` paths to use `RequirementsSource::ExtensionLess`.

I'm not too sure about this change though, as it would also implicitly start allowing PEP 723 scripts as input to overrides/constraints. I don't see the direct issue in that, but then maybe we should explicitly handle it so that an `--overrides=script.py` would also be supported. @zanieb what do you think?

Relates to #17227

---

_Review requested from @charliermarsh by @konstin on 2025-12-19 12:32_

---

_Comment by @charliermarsh on 2025-12-19 14:23_

Can you add some test coverage for this?

---

_@charliermarsh approved on 2025-12-24 13:47_

---

_Merged by @charliermarsh on 2025-12-24 13:47_

---

_Closed by @charliermarsh on 2025-12-24 13:47_

---

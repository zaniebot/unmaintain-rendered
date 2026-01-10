```yaml
number: 2554
title: Add UV_CUSTOM_COMPILE_COMMAND support to uv pip compile
type: pull_request
state: merged
author: BakerNet
labels:
  - configuration
assignees: []
merged: true
base: main
head: feature/custom-command-header
created_at: 2024-03-19T22:23:17Z
updated_at: 2024-03-21T09:23:27Z
url: https://github.com/astral-sh/uv/pull/2554
synced_at: 2026-01-10T14:49:08Z
```

# Add UV_CUSTOM_COMPILE_COMMAND support to uv pip compile

---

_Pull request opened by @BakerNet on 2024-03-19 22:23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This adds support for `CUSTOM_COMPILE_COMMAND` support to change the header comment in generated requirements files.

See Issue:  
- #1527 

From [pip-tools docs](https://pip-tools.readthedocs.io/en/latest/):

> You might be wrapping the pip-compile command in another script. To avoid confusing consumers of your custom script you can override the update command generated at the top of requirements files by setting the CUSTOM_COMPILE_COMMAND environment variable.

## Test Plan

<!-- How was it tested? -->

See unit test included

---

_@BakerNet reviewed on 2024-03-19 23:25_

---

_Review comment by @BakerNet on `crates/uv/src/commands/pip_compile.rs`:379 on 2024-03-19 23:25_

Alternatively, if you prefer to have it inline:
```suggestion
                custom_compile_command.unwrap_or(cmd(include_index_url, include_find_links))
```
and revert the changes in `cmd` function

---

_Review comment by @BakerNet on `crates/uv/src/commands/pip_compile.rs`:454 on 2024-03-19 23:26_

If above suggestion, revert
```suggestion
fn cmd(include_index_url: bool, include_find_links: bool) -> String {
```

---

_@BakerNet reviewed on 2024-03-19 23:26_

---

_@BakerNet reviewed on 2024-03-19 23:32_

---

_Review comment by @BakerNet on `crates/uv/src/main.rs`:328 on 2024-03-19 23:32_

Alternate env name:
```suggestion
    #[clap(long, env = "UV_CUSTOM_COMPILE_COMMAND")]
```

---

_Review comment by @BakerNet on `crates/uv/tests/pip_compile.rs`:3521 on 2024-03-19 23:32_

If alternate env name used:
```suggestion
            .env("UV_CUSTOM_COMPILE_COMMAND", "./custom-uv-compile.sh"), @r###"
```

---

_@BakerNet reviewed on 2024-03-19 23:32_

---

_Label `configuration` added by @zanieb on 2024-03-20 00:20_

---

_@konstin approved on 2024-03-20 11:13_

Thank you!

---

_Merged by @konstin on 2024-03-20 11:22_

---

_Closed by @konstin on 2024-03-20 11:22_

---

_Comment by @henryiii on 2024-03-21 06:22_

This adds `UV_CUSTOM_COMPILE_COMMAND`, not `CUSTOM_COMPILE_COMMAND`. Both the PR and the release notes state the `CUSTOM_COMPILE_COMMAND` one, which is the same as pip compile, but this was added with a `UV_` prefix.

---

_Comment by @henryiii on 2024-03-21 06:23_

(I read the release notes, then couldn't figure out why `CUSTOM_COMPILE_COMMAND` that was previously set for `pip-compile` wasn't now working for uv until I read the diff)

---

_Renamed from "Add CUSTOM_COMPILE_COMMAND support to uv pip compile" to "Add UV_CUSTOM_COMPILE_COMMAND support to uv pip compile" by @konstin on 2024-03-21 09:22_

---

_Comment by @konstin on 2024-03-21 09:23_

Thanks, i updated the release notes.

---

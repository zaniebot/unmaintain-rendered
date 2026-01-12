```yaml
number: 7895
title: "Ignore `UV_CACHE_DIR` in `help` tests"
type: pull_request
state: merged
author: sobolevn
labels:
  - internal
assignees: []
merged: true
base: main
head: issue-7542
created_at: 2024-10-03T13:02:31Z
updated_at: 2024-10-04T14:43:24Z
url: https://github.com/astral-sh/uv/pull/7895
synced_at: 2026-01-12T16:08:04Z
```

# Ignore `UV_CACHE_DIR` in `help` tests

---

_@sobolevn_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I added a new function to push an extra filter to ignore `\[env: UV_CACHE_DIR=.+\]` line and replace it with an empty value.

I also changed `.editorconfig` file, because you actually need empty lines with spaces. Other option: I can add a default filter to ignore empty lines with spaces and replace them with just empty lines.

Plus, I added `setup-uv` action instead of `curl -LsSf https://astral.sh/uv/install.sh | sh`, not sure if that is planned right now, though!

Closes https://github.com/astral-sh/uv/issues/7542

## Test Plan

I executed this locally, it works!

```
» UV_CACHE_DIR=/tmp cargo nextest run --no-fail-fast help                   
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.22s
------------
 Nextest run ID f8ee686c-2fb9-49da-a657-aeeaf4dbb534 with nextest profile: default
    Starting 13 tests across 89 binaries (1693 tests skipped)
        PASS [   1.289s] uv::help help_flag_subsubcommand
        PASS [   1.295s] uv::help help_with_help
        PASS [   1.301s] uv::help help_flag_subcommand
        PASS [   1.317s] uv::help help_short_flag
        PASS [   1.326s] uv::help help_flag
        PASS [   1.331s] uv::help help_with_no_pager
        PASS [   1.333s] uv::help help_subcommand
        PASS [   1.334s] uv::help help
        PASS [   1.334s] uv::help help_unknown_subsubcommand
        PASS [   1.346s] uv::help help_subsubcommand
        PASS [   1.351s] uv::help help_with_global_option
        PASS [   1.380s] uv::help help_unknown_subcommand
        PASS [   0.132s] uv::help help_with_version
------------
     Summary [   1.422s] 13 tests run: 13 passed, 1693 skipped
                                       
```


---

_Comment by @sobolevn on 2024-10-03 13:16_

I reverted the CI change, because I don't quite understand how this works ;)

---

_@zanieb reviewed on 2024-10-03 14:17_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:409 on 2024-10-03 14:17_

The current naming pattern for for this is `with_filtered_...`, e.g., `TestContext::with_filtered_exe_suffix`

---

_Comment by @zanieb on 2024-10-03 14:19_

In the CI change, it looks like you dropped the `uv python install` command so tests fail. I think you want something like this:

```yaml
      - uses: astral-sh/setup-uv@v3
        with:
          version: "latest"
          enable-cache: true
      - name: "Install required Python versions"
        run: uv python install
```

---

_Comment by @sobolevn on 2024-10-03 15:27_

Thanks!

---

_@konstin approved on 2024-10-04 14:03_

Thank you!

---

_Assigned to @zanieb by @zanieb on 2024-10-04 14:15_

---

_Merged by @zanieb on 2024-10-04 14:41_

---

_Closed by @zanieb on 2024-10-04 14:41_

---

_Label `internal` added by @zanieb on 2024-10-04 14:43_

---

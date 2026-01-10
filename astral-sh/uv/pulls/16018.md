```yaml
number: 16018
title: "Add `--outdated` to `uv python uninstall`"
type: pull_request
state: open
author: yumeminami
labels:
  - enhancement
assignees: []
base: main
head: feat/python-uninstall-outdated
created_at: 2025-09-24T18:21:19Z
updated_at: 2025-11-25T19:08:23Z
url: https://github.com/astral-sh/uv/pull/16018
synced_at: 2026-01-10T05:58:11Z
```

# Add `--outdated` to `uv python uninstall`

---

_Pull request opened by @yumeminami on 2025-09-24 18:21_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
add `--outdated` flag in `uv python uninstall` to remove old Python versions 

close https://github.com/astral-sh/uv/issues/15805


---

_Comment by @zanieb on 2025-09-29 18:35_

Hey! I took a look at the implementation and made some tweaks in https://github.com/astral-sh/uv/pull/16018/commits/b385cee2dccba38c1358ddd1a6aa007b873f8f95

I think we should continue to allow and respect requests, e.g., `uv python uninstall --outdated pypy` seems totally valid, so I adjusted things to handle that. I didn't add corresponding test coverage though.

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:200 on 2025-09-29 18:37_

I wonder if we should always return `Success` unless requests were made? I'll think about making a tracking issue. It's a little weird to have different behavior for `--outdated`.

---

_@zanieb reviewed on 2025-09-29 18:37_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:5563 on 2025-09-29 18:43_

I think you'll need to be a bit clearer here. Maybe "the latest patch version for each minor version"? and provide an example?

---

_@zanieb reviewed on 2025-09-29 18:43_

---

_@yumeminami reviewed on 2025-10-05 14:48_

---

_Review comment by @yumeminami on `crates/uv/src/commands/python/uninstall.rs`:200 on 2025-10-05 14:48_

@zanieb I think the return code is 0 when there are no matchings because it indicates no failure, similar to commands like `apt autoremove` and `docker system prune`, which return 0 even when there's nothing to clean up, nothing to uninstall.

---

_@yumeminami reviewed on 2025-10-05 15:06_

---

_Review comment by @yumeminami on `crates/uv-cli/src/lib.rs`:5563 on 2025-10-05 15:06_

I updated the description of the `outdated` param in https://github.com/astral-sh/uv/pull/16018/commits/6db50debbec87f9266a78b23ce8ae023841810f5. 

---

_Comment by @yumeminami on 2025-10-05 15:32_

> Hey! I took a look at the implementation and made some tweaks in [b385cee](https://github.com/astral-sh/uv/commit/b385cee2dccba38c1358ddd1a6aa007b873f8f95)
> 
> I think we should continue to allow and respect requests, e.g., `uv python uninstall --outdated pypy` seems totally valid, so I adjusted things to handle that. I didn't add corresponding test coverage though.

@zanieb I‘ve added corresponding test in https://github.com/astral-sh/uv/pull/16018/commits/59c090c5adb784a8254d28f11cc6ff3e6a2f32b5 `python_uninstall_outdated_pypy`

---

_Review comment by @yumeminami on `crates/uv/src/commands/python/uninstall.rs`:81 on 2025-10-05 15:32_

@zanieb The custom error message "No targets specified for uninstall" was causing
snapshot test failures in several python_install tests. https://github.com/astral-sh/uv/actions/runs/18260396431/job/51987532018

Should restore the previous approach or update all snapshots?

---

_@yumeminami reviewed on 2025-10-05 15:32_

---

_Renamed from "Feat/python uninstall outdated" to "Add `--outdated` to `uv python uninstall`" by @konstin on 2025-10-06 08:52_

---

_Label `enhancement` added by @konstin on 2025-10-06 08:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:81 on 2025-10-06 16:54_

I think it's okay to change this error message in the snapshots. Thanks for checking in!

---

_@zanieb reviewed on 2025-10-06 16:54_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:200 on 2025-10-06 16:54_

Yeah that makes sense to me. I just think we'll want to do something similar for the other case — separate from this pull request.

---

_@zanieb reviewed on 2025-10-06 16:54_

---

_Review comment by @yumeminami on `crates/uv/src/commands/python/uninstall.rs`:81 on 2025-10-09 09:50_

@zanieb Updated related error messages in test cases, https://github.com/astral-sh/uv/pull/16018/commits/c9ad625f4827783bab417846aad7981c20199957.

---

_@yumeminami reviewed on 2025-10-09 09:50_

---

_@yumeminami reviewed on 2025-10-09 09:50_

---

_Review comment by @yumeminami on `crates/uv/src/commands/python/uninstall.rs`:200 on 2025-10-09 09:50_

yeah I agree.

---

_@charliermarsh approved on 2025-10-27 02:10_

Defer to @zanieb to merge.

---

_Comment by @powercoconola on 2025-11-25 19:08_

Hi just checking on this - does anything more need to be done or is help needed in some way for this to be merged?
Thanks for all the work on this!

---

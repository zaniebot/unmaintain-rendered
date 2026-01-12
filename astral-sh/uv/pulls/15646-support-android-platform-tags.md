```yaml
number: 15646
title: Support Android platform tags
type: pull_request
state: merged
author: timrid
labels:
  - enhancement
assignees: []
merged: true
base: main
head: android-support
created_at: 2025-09-02T23:20:23Z
updated_at: 2025-09-03T14:24:34Z
url: https://github.com/astral-sh/uv/pull/15646
synced_at: 2026-01-12T16:11:52Z
```

# Support Android platform tags

---

_@timrid_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
This implements the Android part of https://github.com/astral-sh/uv/issues/8029

FYI: @freakboy3742 @mhsmith

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Create a venv with uv and run `cargo run pip install --python-platform aarch64-linux-android pybase64`. Then the Android binary of pybase64 should be installed inside the venv.

---

_@charliermarsh reviewed on 2025-09-03 01:33_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/tags.rs`:821 on 2025-09-03 01:33_

Should this return a structured error?

---

_@charliermarsh reviewed on 2025-09-03 01:35_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/tags.rs`:805 on 2025-09-03 01:35_

Is there a link we could include for the supported values here (e.g., to documentation)? I see `arm64_v8a`  and `x86_64` in the linked issue (https://github.com/astral-sh/uv/issues/8029), but not the other two.

---

_@timrid reviewed on 2025-09-03 07:48_

---

_Review comment by @timrid on `crates/uv-platform-tags/src/tags.rs`:821 on 2025-09-03 07:48_

Added an structured error.

---

_@timrid reviewed on 2025-09-03 07:49_

---

_Review comment by @timrid on `crates/uv-platform-tags/src/tags.rs`:805 on 2025-09-03 07:49_

This is described in [PEP738](https://peps.python.org/pep-0738/#architectures). I added a link to it.

---

_@mhsmith reviewed on 2025-09-03 09:12_

---

_Review comment by @mhsmith on `crates/uv-platform-tags/src/tags.rs`:805 on 2025-09-03 09:12_

The live specification is now in the [packaging user guide](https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#android), so I'll suggest a change to the link. 

The 32-bit architectures are not officially supported by Python, but it's possible people might use them unofficially.

---

_Review comment by @mhsmith on `crates/uv-platform-tags/src/tags.rs`:788 on 2025-09-03 09:13_

```suggestion
    // Source: https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#android
```

---

_Review comment by @mhsmith on `crates/uv-configuration/src/target_triple.rs`:847 on 2025-09-03 09:18_

```suggestion
/// Return the Android API level as parsed from the environment.
```

---

_Review comment by @mhsmith on `crates/uv-configuration/src/target_triple.rs`:488 on 2025-09-03 09:24_

In Python 3.14 the minimum API level has been [increased to 24](https://github.com/python/cpython/blob/main/Android/android-env.sh), and packages on PyPI will be tagged accordingly. So I'm not sure what's the best way to handle this in uv, but it would be preferable not to exclude all of those packages by default.

---

_@mhsmith reviewed on 2025-09-03 09:24_

---

_Review comment by @timrid on `crates/uv-platform-tags/src/tags.rs`:788 on 2025-09-03 09:31_

In https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#android the schema is described as `android_apilevel_abi`. So I should probably rename `AndroidArch` to `AndroidAbi`?

---

_@timrid reviewed on 2025-09-03 09:31_

---

_@mhsmith reviewed on 2025-09-03 09:38_

---

_Review comment by @mhsmith on `crates/uv-configuration/src/target_triple.rs`:488 on 2025-09-03 09:38_

Maybe the default in uv could just be increased to 24, and then it would accept wheels tagged as 24 and below on all Python versions. This would be similar to macOS, where uv defaults to a minimum version of 12.0, even though [Python itself](https://www.python.org/downloads/release/python-3137/) is still compatible with 10.13.

---

_@mhsmith reviewed on 2025-09-03 09:41_

---

_Review comment by @mhsmith on `crates/uv-platform-tags/src/tags.rs`:788 on 2025-09-03 09:41_

Yes, that would make sense, since that's the term used in all the Android documentation. They don't currently have multiple ABIs per architecture, but they have done in the past (e.g. `armeabi-v7a-hard`).

---

_@timrid reviewed on 2025-09-03 09:55_

---

_Review comment by @timrid on `crates/uv-configuration/src/target_triple.rs`:488 on 2025-09-03 09:55_

Ok, I will change that.

(Actually macOS defaults to a [minimum version of 13.0](https://github.com/astral-sh/uv/blob/ad35d120d66e43caf4073c27c06e432adadf9867/crates/uv-configuration/src/target_triple.rs#L260). The [uv docs are wrong](https://github.com/astral-sh/uv/blob/ad35d120d66e43caf4073c27c06e432adadf9867/crates/uv-cli/src/lib.rs#L1449). I fixed that in https://github.com/astral-sh/uv/pull/15640.)

---

_@timrid reviewed on 2025-09-03 10:03_

---

_Review comment by @timrid on `crates/uv-configuration/src/target_triple.rs`:488 on 2025-09-03 10:03_

Changed the default from 21 to 24.

---

_@timrid reviewed on 2025-09-03 10:03_

---

_Review comment by @timrid on `crates/uv-platform-tags/src/tags.rs`:788 on 2025-09-03 10:03_

Changed `AndroidArch` to `AndroidAbi`.

---

_@charliermarsh approved on 2025-09-03 14:24_

---

_Label `enhancement` added by @charliermarsh on 2025-09-03 14:24_

---

_Merged by @charliermarsh on 2025-09-03 14:24_

---

_Closed by @charliermarsh on 2025-09-03 14:24_

---

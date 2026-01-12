```yaml
number: 15640
title: Support iOS platform tags
type: pull_request
state: merged
author: timrid
labels: []
assignees: []
merged: true
base: main
head: ios-support
created_at: 2025-09-02T20:28:22Z
updated_at: 2025-09-03T22:24:49Z
url: https://github.com/astral-sh/uv/pull/15640
synced_at: 2026-01-12T16:11:52Z
```

# Support iOS platform tags

---

_@timrid_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
This implements the iOS part of https://github.com/astral-sh/uv/issues/8029

FYI: @freakboy3742

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Create a venv with uv and run `cargo run pip install --python-platform arm64-apple-ios pillow`. Then the iOS binary of pillow should be installed inside the venv.

---

_Review requested from @konstin by @zanieb on 2025-09-02 21:27_

---

_Comment by @charliermarsh on 2025-09-02 22:02_

Thanks for contributing!

---

_@timrid reviewed on 2025-09-03 07:59_

---

_Review comment by @timrid on `crates/uv-platform-tags/src/platform_tag.rs`:298 on 2025-09-03 07:59_

@freakboy3742 Is `multiarch` the correct name in `ios_{major}_{minor}_{multiarch}`? I took it from [here](https://peps.python.org/pep-0730/#platform-identification), but [here](https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#ios) it is called `arch_sdk`, so I am unsure what is a better name. If changing from `multiarch` to `arch_sdk` the Enum name `IosMultiarch` has also to be renamed to `IosArchSdk`.

---

_Comment by @charliermarsh on 2025-09-03 14:24_

@timrid -- Do you mind rebasing?

---

_Review comment by @freakboy3742 on `crates/uv-platform-tags/src/platform_tag.rs`:298 on 2025-09-03 21:56_

The second link you've provided has a clarification a little further down: 

> The combination of `arch_sdk` is referred to as the “multiarch”. 

That is "multi arch" is the combination of the arch, followed by the sdk. `arch` will be `arm64` or `x86_64`; `sdk` will be `iphoneos` or `iphonesimulator`; and multiarch will end up being one of `arm64_iphoneos`, `arm64_iphonesimulator` or `x86_64_iphonesimulator`. 

So - if you're defining a constant for the entire "content  after major and minor", it would be called "multiarch"; but if you were to break it down further, you could have independent constants for "arch" and "sdk"; but you wouldn't have an "arch_sdk".

In the context, I don't think there's a lot of use for splitting multiarch into its constituent parts; if only because you could accidentlly construct `x86_64_iphoneos`, which won't be (and is unlikely to ever be) legal.

So - my suggestion would be to keep the constant as currently defined.

---

_@freakboy3742 reviewed on 2025-09-03 21:56_

---

_@charliermarsh approved on 2025-09-03 22:24_

This is great, thanks.

---

_Merged by @charliermarsh on 2025-09-03 22:24_

---

_Closed by @charliermarsh on 2025-09-03 22:24_

---

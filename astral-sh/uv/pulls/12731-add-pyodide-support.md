```yaml
number: 12731
title: Add Pyodide support
type: pull_request
state: merged
author: hoodmane
labels:
  - enhancement
assignees: []
merged: true
base: main
head: pyodide-support
created_at: 2025-04-07T21:59:48Z
updated_at: 2025-06-24T01:32:41Z
url: https://github.com/astral-sh/uv/pull/12731
synced_at: 2026-01-10T11:10:40Z
```

# Add Pyodide support

---

_Pull request opened by @hoodmane on 2025-04-07 21:59_

This includes some initial work on adding Pyodide support (issue #12729). It is enough to get
```
uv pip compile -p /path/to/pyodide --extra-index-url file:/path/to/simple-index
```
to work which should already be quite useful.

## Test Plan

* added a unit test for `pyodide_platform`
* integration tested manually with:
```
cargo run pip install \
-p /home/rchatham/Documents/programming/tmp/pyodide-venv-test/.pyodide-xbuildenv-0.29.3/0.27.4/xbuildenv/pyodide-root/dist/python \
--extra-index-url file:/home/rchatham/Documents/programming/tmp/pyodide-venv-test/.pyodide-xbuildenv-0.29.3/0.27.4/xbuildenv/pyodide-root/package_index \
--index-strategy unsafe-best-match --target blah --no-build \
numpy pydantic
```


---

_Comment by @codspeed-hq[bot] on 2025-04-08 08:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/hoodmane%3Apyodide-support)

### Merging #12731 will **not alter performance**

<sub>Comparing <code>hoodmane:pyodide-support</code> (e9e37be) with <code>main</code> (37e22e4)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Comment by @hoodmane on 2025-04-08 08:58_

Uh oh tests are failing because of a pypi outage.

---

_Review requested from @konstin by @zanieb on 2025-04-08 17:41_

---

_Comment by @konstin on 2025-04-10 09:26_

The micro-benchmark regression is probably a fluke but is fine otherwise too, as long as the resolver benchmarks don't change

---

_Label `enhancement` added by @konstin on 2025-04-10 09:26_

---

_Comment by @hoodmane on 2025-04-10 10:26_

Thanks @konstin. I'll add an integration test and then mark this ready for review.

---

_Comment by @zanieb on 2025-04-10 16:00_

We keep seeing that microbenchmark false positive. I don't know what's up with it.

---

_Marked ready for review by @hoodmane on 2025-05-08 22:11_

---

_Comment by @hoodmane on 2025-05-08 22:12_

Okay I added an integration test and it is working:
https://github.com/astral-sh/uv/actions/runs/14916812495/job/41904275552?pr=12731

So this is ready for review.

---

_Review comment by @konstin on `.github/workflows/ci.yml`:1313 on 2025-05-09 08:49_

Should we add a version bound to `pyodide-build`?

---

_Review comment by @konstin on `.github/workflows/ci.yml`:1319 on 2025-05-09 08:50_

This has been merged

---

_Review comment by @konstin on `crates/uv-configuration/src/target_triple.rs`:229 on 2025-05-09 09:19_

```suggestion
    Aarch64Manylinux240,

    /// A wasm32 target using the the pyodide 2024 platform.
    #[cfg_attr(feature = "clap", value(name = "wasm32-pyodide2024"))]
```

---

_Comment by @konstin on 2025-05-09 09:36_

Where does the 2024 in pyodide 2024 come from? I couldn't find it in the docs or the GitHub page.

---

_Review comment by @konstin on `crates/uv-python/python/get_interpreter_info.py`:520 on 2025-05-09 09:41_

Can you add a dedicated error, something that says that emscripten is only support with pyodide?

---

_Review comment by @konstin on `crates/uv-configuration/src/target_triple.rs`:589 on 2025-05-09 09:43_

Is there a specific reason for this value?

---

_Review comment by @konstin on `crates/uv-configuration/src/target_triple.rs`:632 on 2025-05-09 09:43_

Can you add a comment that this is the emscripten version, so future reviewers know what is emscripten and what is pyodide?

---

_@konstin reviewed on 2025-05-09 09:44_

---

_Comment by @hoodmane on 2025-05-16 03:12_

Thanks for the review @konstin !

---

_@hoodmane reviewed on 2025-05-17 15:48_

---

_Review comment by @hoodmane on `crates/uv-configuration/src/target_triple.rs`:589 on 2025-05-17 15:48_

Not really but it's what Emscripten puts in this field. Added a comment and a link to Emscripten's libc.

---

_Review comment by @hoodmane on `crates/uv-configuration/src/target_triple.rs`:632 on 2025-05-17 15:53_

Added.

---

_@hoodmane reviewed on 2025-05-17 15:53_

---

_Comment by @hoodmane on 2025-05-17 16:06_

> Where does the 2024 in pyodide 2024 come from? I couldn't find it in the docs or the GitHub page.

It's vaguely documented here:
https://pyodide.org/en/stable/development/abi.html#pyodide-2024-0

Also the Pyodide packaging pep:
https://peps.python.org/pep-0783/


---

_@konstin approved on 2025-05-19 07:21_

Thanks!

---

_Comment by @hoodmane on 2025-06-03 15:37_

@zanieb would appreciate review on this when you have a chance.

---

_Comment by @zanieb on 2025-06-03 16:39_

🤦‍♀️ thanks for the poke

---

_@zanieb approved on 2025-06-03 16:40_

---

_Merged by @zanieb on 2025-06-03 17:01_

---

_Closed by @zanieb on 2025-06-03 17:01_

---

_Comment by @hoodmane on 2025-06-03 18:54_

Thanks @zanieb and @konstin!

---

_Branch deleted on 2025-06-03 18:54_

---

_@charliermarsh reviewed on 2025-06-24 00:54_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform_tag.rs`:268 on 2025-06-24 00:54_

@hoodmane -- Should this not be `emscripten_3_1_58_wasm32`, for `pyodide_2024_0`?

---

_@charliermarsh reviewed on 2025-06-24 01:21_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform_tag.rs`:268 on 2025-06-24 01:21_

I put up a PR here, I may be mistaken though: https://github.com/astral-sh/uv/pull/14226

---

_@charliermarsh reviewed on 2025-06-24 01:32_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform_tag.rs`:268 on 2025-06-24 01:32_

Ah okay, I'm seeing that the wheels in `pyodide config get package_index` _do_ use this pyodide tag. Should we support both?

---

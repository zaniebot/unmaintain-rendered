```yaml
number: 13437
title: Warn when two packages write to the same module
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/warn-on-module-conflicts
created_at: 2025-05-13T20:55:00Z
updated_at: 2025-08-18T07:53:05Z
url: https://github.com/astral-sh/uv/pull/13437
synced_at: 2026-01-12T16:10:41Z
```

# Warn when two packages write to the same module

---

_@konstin_

We regularly get confusing bug reports where a package sometimes works and sometimes doesn't and it's not clear to the user why. Ultimately, it turns out that two packages contain the same module and there is a race condition when installing the two packages. Usually, it's one of the opencv-python distributions, but recently it's been z3, too. These error are completely inscrutable to users.

* https://github.com/astral-sh/uv/issues/10708
* https://github.com/astral-sh/uv/issues/11806
* https://github.com/astral-sh/uv/issues/11659
* https://github.com/astral-sh/uv/issues/13435
* https://github.com/astral-sh/uv/issues/13550
* https://github.com/astral-sh/uv/issues/14030
* https://github.com/astral-sh/uv/issues/15238

We now warn for top-level modules (pattern: `<identifier>/__init__.py`) that collide in a single installation, naming the offending wheels. Checking for `__init__.py` excludes namespace packages.

Test script:

```
uv venv -q && cargo run -q --profile fast-build pip install --no-progress --link-mode clone opencv-python opencv-contrib-python --no-build --no-deps
uv venv -q && cargo run -q --profile fast-build pip install --no-progress --link-mode copy opencv-python opencv-contrib-python --no-build --no-deps
uv venv -q && cargo run -q --profile fast-build pip install --no-progress --link-mode hardlink opencv-python opencv-contrib-python --no-build --no-deps
uv venv -q && cargo run -q --profile fast-build pip install --no-progress --link-mode symlink opencv-python opencv-contrib-python --no-build --no-deps
```

We currently only catch conflicts in a single installation. Should we prime the lock database with the site-packages contents, and would that carry overhead?


---

_Review requested from @charliermarsh by @konstin on 2025-05-13 20:55_

---

_Label `bug` added by @konstin on 2025-05-13 20:55_

---

_@konstin reviewed on 2025-05-13 20:55_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/linker.rs`:33 on 2025-05-13 20:55_

This should get <1000 hits so I've implemented the naive approach.

---

_Comment by @cthoyt on 2025-05-14 06:14_

An example that's vexed me in the past:

1. https://pypi.org/project/Flask-Bootstrap/ (original, unmaintained project)
2. https://pypi.org/project/Bootstrap-Flask/ (maintained fork, uses same package name but 
different on PyPI)

both install to `flask_bootstrap`

---

_Comment by @charliermarsh on 2025-05-20 14:16_

I don't know that we should make this user-facing, to be honest. Isn't it going to trigger any time anyone installs Jupyter?

---

_Comment by @konstin on 2025-05-20 14:58_

I strongly think this should be user-facing, I linked some of the issues this would have prevented above (we had another one just today: #13550), especially since this is otherwise non-deterministic, silent and almost impossible to figure out if you don't know what you're looking for.

It doesn't warn for `uv pip install jupyter`, is there a reason we would expect it to? Note that we're checking for an `__init__.py`, so this does not affect namespace packages that are allowed to interleave.

---

_Comment by @atinary-bvollmer on 2025-05-21 06:34_

Hey @charliermarsh,

I'm the author of #13550. Thanks to @konstin I found the issue but I would strongly side with here that especially for less experienced users this can lead to very frustrating problems. A warning during the install would have most likely prevented it for me.

Thanks!

---

_Comment by @konstin on 2025-05-27 21:09_

Added a test case

---

_Assigned to @zanieb by @zanieb on 2025-05-28 18:39_

---

_Comment by @konstin on 2025-05-30 11:31_

One place where this could fail is the Python 2 `path = import("pkgutil").extend_path(__path__, name)` hack, but I haven't seen that one in a while, and we can special case it if necessary.

---

_Comment by @zanieb on 2025-05-30 12:02_

(I'll review this)

---

_Review requested from @zanieb by @konstin on 2025-06-05 10:08_

---

_@zanieb reviewed on 2025-06-06 19:01_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:12147 on 2025-06-06 19:01_

Why do we install directly from wheels instead of the source trees?

---

_@zanieb reviewed on 2025-06-06 19:01_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-06 19:01_

Why do we show the wheel filenames here? Is that an important detail?

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-06 19:06_

For debugging, I usually need to go open the wheel and look inside of it, so I consider this a valuable detail. We should show at least the version though, otherwise we can't find where this is from.

---

_@konstin reviewed on 2025-06-06 19:06_

---

_@zanieb reviewed on 2025-06-06 19:07_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/linker.rs`:42 on 2025-06-06 19:07_

I reworded this in https://github.com/astral-sh/uv/pull/13889, curious for your thoughts

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-06 19:08_

See also https://github.com/astral-sh/uv/pull/13437/files#r2132758628

---

_@zanieb reviewed on 2025-06-06 19:08_

---

_@zanieb reviewed on 2025-06-06 19:09_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-06 19:09_

I worry about this as a user-facing detail in the warning though, isn't the file name going to be in the verbose logs?

---

_@zanieb reviewed on 2025-06-06 19:09_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-06 19:09_

Also, aren't the versions included in the install summary?

---

_@zanieb reviewed on 2025-06-06 19:14_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/linker.rs`:557 on 2025-06-06 19:14_

What does this look like for modules like `foo.bar`? Are we only going to show `bar`? Or are you only checking for the top-level modules because of `relative.components.count() == 2`?

---

_@zanieb reviewed on 2025-06-06 19:15_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:12508 on 2025-06-06 19:15_

Can we also demonstrate that installing these wheels one at a time does not warn?

---

_@konstin reviewed on 2025-06-11 19:28_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/linker.rs`:557 on 2025-06-11 19:28_

That's the TODO above, we're only looking for `foo/__init__.py` rn and don't track `foo/bar/__init__.py`

---

_@konstin reviewed on 2025-06-11 19:33_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-11 19:33_

The error should by itself contain enough information to track down the problem, so I would like it to at least contain the versions.

---

_@zanieb reviewed on 2025-06-11 20:05_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-11 20:05_

I'm okay with the versions, styled as we do elsewhere, e.g., `foo (v1.0.0)`. I think the filenames sound like a developer-facing rather than user-facing detail.

---

_@charliermarsh reviewed on 2025-06-11 20:17_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-11 20:17_

The exclamation mark is a bit much, I don't think we do that anywhere else.

---

_@charliermarsh reviewed on 2025-06-11 20:19_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/linker.rs`:42 on 2025-06-11 20:19_

Will this trigger if two packages include _the same_ file?

---

_@konstin reviewed on 2025-06-11 20:27_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/linker.rs`:42 on 2025-06-11 20:27_

Yes, do we have a case where this is expected and valid to happen?

---

_@zanieb reviewed on 2025-06-11 20:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-11 20:31_

That's dropped in https://github.com/astral-sh/uv/pull/13889

---

_@konstin reviewed on 2025-06-11 20:40_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:11412 on 2025-06-11 20:40_

I updated the error message

---

_@charliermarsh reviewed on 2025-06-11 20:45_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/linker.rs`:42 on 2025-06-11 20:45_

Isn't it a _feature_ that two packages could contain the same shared module, and could just vendor it into their own wheel?

---

_Comment by @codspeed-hq[bot] on 2025-06-11 20:55_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fwarn-on-module-conflicts?runnerMode=WallTime)

### Merging #13437 will **not alter performance**

<sub>Comparing <code>konsti/warn-on-module-conflicts</code> (f18ca49) with <code>main</code> (8968d78)</sub>



### Summary

`✅ 3` untouched benchmarks  





---

_@zanieb reviewed on 2025-06-11 21:21_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/linker.rs`:42 on 2025-06-11 21:21_

Is it? That seems pretty problematic from a versioning perspective.

---

_@konstin reviewed on 2025-06-12 19:33_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/linker.rs`:42 on 2025-06-12 19:33_

I haven't seen any vendoring cases where the vendored modules would overlap.

---

_Comment by @MalteEbner on 2025-06-13 16:02_

Suggested solution: I think it should be possible for the user to define different packages under the same module name similar to the sources logic. Something similar to this:

```
[tool.uv.sources]
pillow = [
  { package = "pillow-simd>=9,<10", marker = "platform_machine == 'x86_64'"},
  { package = "pillow>=11,<12", marker = "platform_machine == 'aarch64'"},
]
```

---

_Comment by @konstin on 2025-07-24 10:04_

> Suggested solution: I think it should be possible for the user to define different packages under the same module name similar to the sources logic. Something similar to this:
> 
> ```
> [tool.uv.sources]
> pillow = [
>   { package = "pillow-simd>=9,<10", marker = "platform_machine == 'x86_64'"},
>   { package = "pillow>=11,<12", marker = "platform_machine == 'aarch64'"},
> ]
> ```

~~This is already supported and recommended, since this is a valid usage it's not affected by this PR.~~

Sorry is misread the comment.

---

_Review requested from @zanieb by @konstin on 2025-08-07 14:30_

---

_@zanieb approved on 2025-08-07 18:25_

---

_Merged by @konstin on 2025-08-08 09:01_

---

_Closed by @konstin on 2025-08-08 09:01_

---

_Branch deleted on 2025-08-08 09:01_

---

_Comment by @geofft on 2025-08-08 19:30_

I sort of thought I've seen pre-PEP-420 namespace packages recently enough. [Debian Code Search for `path:__init__.py extend_path`](https://codesearch.debian.net/search?q=path%3A__init__.py+extend_path&literal=1&perpkg=1) shows a good amount of hits, though several are in vendored and possibly old packages. Protobuf has a `google/__init__.py` in its sources, but it doesn't seem to actually ship it in the wheel. The latest PyQt6 wheel does have a `PyQt6/__init__.py`, but I'm not sure if anything else installs into that namespace.

[edit: meant to write "pre-PEP-420", not "PEP-420"]

---

_Comment by @konstin on 2025-08-08 19:33_

Valid PEP 420 packages that don't clash are not affected by this change.

---

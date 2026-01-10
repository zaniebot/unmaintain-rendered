```yaml
number: 16074
title: Error when built wheel is for the wrong platform
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/check-built-wheel-tags
created_at: 2025-09-30T12:44:19Z
updated_at: 2025-12-10T07:27:08Z
url: https://github.com/astral-sh/uv/pull/16074
synced_at: 2026-01-10T05:49:14Z
```

# Error when built wheel is for the wrong platform

---

_Pull request opened by @konstin on 2025-09-30 12:44_

Error when a built wheel is for the wrong platform. This can happen especially when using `--python-platform` or `--python-version` with `uv pip install`.

Fixes #16019


---

_Label `bug` added by @konstin on 2025-09-30 12:44_

---

_Marked ready for review by @konstin on 2025-10-08 08:14_

---

_Comment by @konstin on 2025-10-08 08:14_

There are no changes in the ecosystem tests, so this is ready to be reviewed and merged.

---

_Review requested from @zanieb by @konstin on 2025-10-16 20:56_

---

_@zanieb reviewed on 2025-12-03 13:02_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13314 on 2025-12-03 13:02_

fwiw the preferred pattern is now `context.with_filter`

---

_@zanieb reviewed on 2025-12-03 13:03_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13462 on 2025-12-03 13:03_

I might just say..
```suggestion
      ╰─▶ The built wheel `py313-0.1.0-py313-none-any.whl` is not compatible with the current Python 3.12 on [ARCH] [OS]
```

---

_@zanieb reviewed on 2025-12-03 13:05_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13462 on 2025-12-03 13:05_

Do we need to say something like...

> The build backend (setuptools) does not appear to support this cross build

I'm not sure if that's quite safe to say but something in that vein?

---

_Comment by @zanieb on 2025-12-03 13:06_

Is it possible that this can cause false positives or regressions?

What if we're just building the wheel for a resolve (i.e., `uv pip compile`) and don't care if it's not valid?

---

_Comment by @konstin on 2025-12-03 13:43_

> Is it possible that this can cause false positives or regressions?
> 
> What if we're just building the wheel for a resolve (i.e., `uv pip compile`) and don't care if it's not valid?

It's technically possible but for this too happen, we need to have a case where:
1. There is no static metadata
2. The build backend doesn't support `prepare_metadata_for_build_wheel`
3. The build backend always targets a specific platform
4. The user is running `uv pip compile` on a non-target platform. This excludes cases with `uv pip compile -p` pointing to a target Python that can be emulated on the host.

Of these, (3) is very unlikely, in my experience build backends only have limited cross-compiling support if any, and I've never seen one that defaults to a different platform than the current one. It would also be strange to me if a build backend wouldn't implement `prepare_metadata_for_build_wheel`, as the functionality is a subset of the `build_wheel` hook.

`--python-platform` is a bit fragile by design (PEP 517 has no concept of a target that can be given to build backends, you can only pass them a real Python interpreter as `sys.executable`), but I'm not aware of other cases where this would cause a regression.

---

_Comment by @zanieb on 2025-12-03 13:47_

I'm wary because I think `uv pip compile --python-platform` is far more common than `uv pip install --python-platform` and I think this error can only cause regressions in the former / is only helpful for the latter.

What do you mean by (3)? Yes I would only expect most build backends to target the current platform and have no conception of cross-builds. Why would "always targets a _specific_ platform" be needed for this to cause a regression?

---

_Comment by @konstin on 2025-12-03 16:43_

That's a very good point, I added a test specifically for this. The good news is it's already passing as the metadata-only branch in `DistributionDatabase` doesn't pass around tags nor does it check them.

The stronger checks uncovered a bug in how we generate tags for Pyston: https://github.com/astral-sh/uv/pull/16972. Merging this PR may uncover more such problems on non-PyPI platform that usually don't have built wheels.

---

_@zanieb approved on 2025-12-04 15:23_

I'd probably consider this an "enhancement" rather than a "bug"

---

_Label `bug` removed by @konstin on 2025-12-05 10:57_

---

_Label `enhancement` added by @konstin on 2025-12-05 10:57_

---

_Merged by @konstin on 2025-12-05 15:04_

---

_Closed by @konstin on 2025-12-05 15:04_

---

_Branch deleted on 2025-12-05 15:04_

---

_Comment by @mjamroz on 2025-12-09 09:47_

guys is there any way to ignore that error? With this upgrade we cannot install `kinto-11.2.1-cp3-none-any.whl` on alpine anymore

---

_Comment by @konstin on 2025-12-09 09:50_

@mjamroz Can you please file a new issue with a reproduction? `cp3-none-any` is a very odd tag, should it by `py3-none-any`?

---

_Comment by @mjamroz on 2025-12-10 07:27_

@konstin here you go https://github.com/astral-sh/uv/issues/17061

---

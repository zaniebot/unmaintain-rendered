---
number: 14244
title: Is --prerelease=allow supposed to cause packages to be upgraded?
type: issue
state: open
author: DavidQChuang
labels:
  - question
assignees: []
created_at: 2025-06-24T16:03:21Z
updated_at: 2025-07-09T20:56:25Z
url: https://github.com/astral-sh/uv/issues/14244
synced_at: 2026-01-10T01:25:43Z
---

# Is --prerelease=allow supposed to cause packages to be upgraded?

---

_Issue opened by @DavidQChuang on 2025-06-24 16:03_

### Question

I have a package, `library` which depends on `library-test-runner`. `library` specifies but does not constrain versions of dependencies `dep1 v1.0.0` and `dep2 v1.0.0` of `library-test-runner` in the uv.lock file. `library-test-runner` constrains versions of `dep1` and `dep2`. These constraints are in requirements.txt and pulled into pyproject.toml through `[tool.setuptools.dynamic] dependencies = {file = ["requirements.txt"]}` and can be adjusted by CI pipelines.

I have private pypi indexes which contain release and prerelease versions of all of these packages and can be published to by CI/CD pipelines.

My CI pipeline clones `library` and in its venv, runs `uv run --with library-test-runner={RUNNER_VERSION} run_library`, in which run_library is from library-test-runner. 

The usual behavior I've found of uv with this command is to keep `dep1` and `dep2` at the versions specified in `library`'s uv.lock (1.0.0), even though there are higher release versions available - unless the constraints from `library-test-runner` are incompatible. However, with `--prerelease=allow`, these dependencies get upgraded to the latest prerelease version all the time as if --upgrade-package was specified, which is an issue because sometimes these are experimental artifacts from random pipelines. To be clear, when I say upgraded, I mean within the ephemeral environment in uv run --with.

My question is, is this behavior expected, or is there a better way to adjust the constraints of library-test-runner that would stop this from happening?
Thanks.

### Platform

Ubuntu 20.04 amd64

### Version

uv 0.7.12

---

_Label `question` added by @DavidQChuang on 2025-06-24 16:03_

---

_Comment by @DavidQChuang on 2025-06-24 17:27_

Apparently, this is a feature, as it is specifically implemented in the code to invalidate the entire lockfile when prerelease mode changes. I'm not sure why this is the case - would it not make sense to mark the lockfile as preferred rather than invalidating it when changing from a more restrictive mode to a less restrictive mode?

Can someone explain why this is the case in general rather than just for certain cases?

---

_Comment by @charliermarsh on 2025-07-04 02:12_

I think the intent is that, without this behavior, we wouldn't upgrade a package from `0.1.0` to `1.0.0a0` upon adding `--prerelease=allow`, which would also be somewhat confusing.

---

_Comment by @DavidQChuang on 2025-07-09 05:39_

It's confusing to me that a package is upgraded from 0.1.0 to 1.0.0a0 upon allowing prerelease versions in `uv run` even though previously, the package would have stayed at 0.1.0 even if 1.0.0 was available unless updating the package is specified or needed. I'll also note that it causes all other packages in the lockfile to be upgraded to whatever version is latest as well, which has undesirable effects a la `pip install -r requirements.txt`, which is what uv is trying to avoid.

Maybe the way we're using it is an edge case, but in my experience the package versions in the lockfile are left alone by `uv run` even if upgrades are available, unless incompatible with the constraints. If this is intended, then my point is that --prerelease=* should be treated the same way as version constraints rather than a signal to throw out the lockfile and upgrade everything. --upgrade-package already exists if you want to upgrade packages to latest rather than using lockfile versions, after all.

---

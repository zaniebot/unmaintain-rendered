```yaml
number: 16828
title: uv build fails to retrieve correct version with setuptools-scm on a release tag
type: issue
state: closed
author: greatvovan
labels:
  - question
  - external
assignees: []
created_at: 2025-11-24T10:52:59Z
updated_at: 2025-11-29T07:09:17Z
url: https://github.com/astral-sh/uv/issues/16828
synced_at: 2026-01-12T16:02:38Z
```

# uv build fails to retrieve correct version with setuptools-scm on a release tag

---

_@greatvovan_

### Summary

The issue is affecting me in GitHub action, but it is reproducible locally as well.

So, for a [project](https://github.com/greatvovan/ios-backup-browser) with

```
[project]
...
dynamic = ["version"]

[build-system]
requires = ["setuptools>=80", "setuptools_scm>=8"]
build-backend = "setuptools.build_meta"
```

_setuptools-scm_ module returns correct version:

```
% python -m setuptools_scm
0.1.1
```

but result of the build is always one patch step ahead and generates "dev" suffix:

```
creating ios_backup_browser-0.1.2.dev0
...
```

even despite the fact of using `SETUPTOOLS_SCM_OVERRIDES_FOR_${DIST_NAME}` to [prevent](https://setuptools-scm.readthedocs.io/en/latest/integrations/#the-solution) dirty or dev versions.

I don't know if [this issue](https://github.com/astral-sh/uv/issues/6964) is related to mine, but the solution there was to use `reinstall-package = ["my-package"]` – setting I tried with ([v0.1.4](https://github.com/greatvovan/ios-backup-browser/actions/runs/19630933388/job/56210305304)) and without ([v0.1.5](https://github.com/greatvovan/ios-backup-browser/actions/runs/19631047444/job/56210669004)) to no avail.

To reproduce it locally, we can checkout the tag and try `python -m setuptools_scm` and `uv build` with the same outcome.

Please kindly assist with the issue.

### Platform

GitHub Actions (ubuntu-latest), astral-sh/setup-uv@v6

### Version

0.9.11

### Python version

3.14

---

_Label `bug` added by @greatvovan on 2025-11-24 10:53_

---

_Comment by @konstin on 2025-11-27 16:35_

I can reproduce this with `python -m build`, so this is something about `setuptools_scm`, rather than specific to `uv build`.

---

_Label `bug` removed by @konstin on 2025-11-27 16:35_

---

_Label `question` added by @konstin on 2025-11-27 16:35_

---

_Comment by @greatvovan on 2025-11-28 11:06_

Thanks @konstin, created an [issue](https://github.com/pypa/setuptools-scm/issues/1238) in their project, let's see if they suggest something.

---

_Label `external` added by @konstin on 2025-11-28 11:16_

---

_Closed by @greatvovan on 2025-11-29 05:41_

---

_Comment by @greatvovan on 2025-11-29 07:09_

The problem was combination of my error and counterintuitive behavior of the SCM module. Closing the issue.

---

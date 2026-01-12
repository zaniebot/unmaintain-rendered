```yaml
number: 7532
title: "uv info or metadata high level command (keyword: outdated)"
type: issue
state: open
author: inoa-jboliveira
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-09-19T03:06:42Z
updated_at: 2024-12-27T14:40:01Z
url: https://github.com/astral-sh/uv/issues/7532
synced_at: 2026-01-12T15:59:14Z
```

# uv info or metadata high level command (keyword: outdated)

---

_@inoa-jboliveira_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I was looking into library updates and #2150 would be helpful by displaying the same data pip generates. That is just partially what I need.

I decided to open a new thread since the [last comment](https://github.com/astral-sh/uv/issues/2150#issuecomment-2339057211) there was about people hijacking it for other requests and I don't want to disturb it further.

UV evolved so much since the `--outdated` flag was proposed, I believe it is best if there is both an "uv pip" interface (legacy) and a modern version for apps taking advantage of "uv sync" et al. Or just the modern version since uv proved it is not just a "fast pip". The legacy version could be just a byproduct or specialization of this for the sake of pip completeness.

#### My proposal is:


Provide a high level "info" or "metadata" command where it can output any known information about a package or the installed packages. It would be a superset of `uv pip show` and `uv pip list`

```bash
$ uv info
Package    Installed    Latest
--------   ---------    -------
django     5.0          5.1.1
foobar     1.1          2.3
``` 

One feature that is important to me is to know how far apart the versions are by checking the release date. Some project move from semver to calendar versioning so you jump from e.g. 2.0 to 24.8. Seeing release dates may seem less scary.

Another reason would be to find stale or dead projects. This would be an awesome use case. If you are up to date, but latest version is from 2017 it is time to replace the dependency.

```bash
$ uv info --released
Package    Installed          Latest
--------   ----------------   ----------------
django     5.0 (2023-12-04)   5.1 (2024-09-03)
foobar     1.1 (2022-02-23)   2.3 (2024-05-01)
``` 

Show the current installation (superset of `uv pip show`). Should also support `--released` next to version number
```bash
$ uv info django
Name: django
Installed Version: 5.0
Latest Version: 5.1.1
Location: ...
``` 

Show the actual package metadata so you don't need to browse pypi
```bash
$ uv info django==5.1.1 --metadata
Metadata-Version: 2.1
Name: Django
Version: 5.1.1
Summary: A high-level Python web framework that encourages rapid development and clean, pragmatic design.
Author-email: Django Software Foundation <foundation@djangoproject.com>
License: BSD-3-Clause
...
``` 

One last useful thing would be support for multiple formats, such as JSON for integrating UV into pipelines.

This command (specially the --released option) would pair quite well with the `--exclude-newer` global parameter.




---

_Label `enhancement` added by @charliermarsh on 2024-12-27 14:40_

---

_Label `wish` added by @charliermarsh on 2024-12-27 14:40_

---

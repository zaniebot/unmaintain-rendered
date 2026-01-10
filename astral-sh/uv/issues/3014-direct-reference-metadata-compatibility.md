```yaml
number: 3014
title: Direct reference metadata compatibility
type: issue
state: closed
author: ofek
labels:
  - bug
assignees: []
created_at: 2024-04-12T14:48:00Z
updated_at: 2025-01-22T17:16:50Z
url: https://github.com/astral-sh/uv/issues/3014
synced_at: 2026-01-10T04:27:57Z
```

# Direct reference metadata compatibility

---

_Issue opened by @ofek on 2024-04-12 14:48_

I'm in the process of adding support for UV in Hatch and I noticed that the test suite is failing because of a difference in how UV writes `direct_url.json` vs pip, specifically the `requested_revision` field. The reason this is read is to determine whether dependencies are in sync.

Given the following requirements:

```
requests @ git+https://github.com/psf/requests
httpx @ git+https://github.com/encode/httpx@master
```

The following command shows different results based on which installer was used:

```
python -c "import json,pprint;from importlib.metadata import distribution;pprint.pprint([json.loads(distribution(pkg).read_text('direct_url.json')) for pkg in ['requests', 'httpx']])"
```

UV:

```
[{'url': 'https://github.com/psf/requests',
  'vcs_info': {'commit_id': '31ebb8102c00f8cf8b396a6356743cca4362e07b',
               'requested_revision': '31ebb8102c00f8cf8b396a6356743cca4362e07b',
               'vcs': 'git'}},
 {'url': 'https://github.com/encode/httpx',
  'vcs_info': {'commit_id': '4b85e6c3898b94e686b427afd83138c87520b479',
               'requested_revision': '4b85e6c3898b94e686b427afd83138c87520b479',
               'vcs': 'git'}}]
```

pip:

```
[{'url': 'https://github.com/psf/requests',
  'vcs_info': {'commit_id': '31ebb8102c00f8cf8b396a6356743cca4362e07b',
               'vcs': 'git'}},
 {'url': 'https://github.com/encode/httpx',
  'vcs_info': {'commit_id': '4b85e6c3898b94e686b427afd83138c87520b479',
               'requested_revision': 'master',
               'vcs': 'git'}}]
```

---

_Comment by @charliermarsh on 2024-04-12 14:51_

Thanks, I can take a look.

---

_Comment by @pradyunsg on 2024-04-12 15:07_

FWIW, I think the spec doesn't make it clear that the expectation is for the mismatched field (requested revision) to match what was originally specified, rather than the final commit hash.

This clarification is worth a spec update PR on the packaging.python.org page. 

---

_Comment by @ofek on 2024-04-12 15:16_

I think the requested revision should match what the user specified (or did not) indeed. If you want to encode more data it looks like the [PEP](https://peps.python.org/pep-0610/#specification) has this text which was not ported over to the [living document spec](https://packaging.python.org/en/latest/specifications/direct-url-data-structure/#vcs-urls):

> * If the installer could discover additional information about the requested revision, it MAY add a `resolved_revision` and/or `resolved_revision_type` field. If no revision was provided in the requested URL, `resolved_revision` MAY contain the default branch that was installed, and `resolved_revision_type` will be `branch`. If the installer determines that `requested_revision` was a tag, it MAY add `resolved_revision_type` with value `tag`.

---

_Comment by @ofek on 2024-04-12 15:24_

> This clarification is worth a spec update PR on the packaging.python.org page.

https://github.com/pypa/packaging.python.org/pull/1531

---

_Label `bug` added by @konstin on 2024-04-12 17:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-12 19:06_

---

_Comment by @ofek on 2024-04-14 20:29_

The text has been updated: https://packaging.python.org/en/latest/specifications/direct-url-data-structure/#vcs-urls

---

_Comment by @charliermarsh on 2024-04-14 20:31_

Thanks. Probably not gonna fix it immediately, will require some internal refactors.

---

_Comment by @ofek on 2024-10-01 23:39_

Have the refactors been completed?

---

_Closed by @charliermarsh on 2025-01-22 17:16_

---

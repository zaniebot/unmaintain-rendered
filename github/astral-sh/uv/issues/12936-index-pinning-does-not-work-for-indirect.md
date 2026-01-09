---
number: 12936
title: Index pinning does not work for indirect dependencies
type: issue
state: closed
author: mzoll
labels:
  - bug
assignees: []
created_at: 2025-04-17T06:48:06Z
updated_at: 2025-04-17T12:53:46Z
url: https://github.com/astral-sh/uv/issues/12936
synced_at: 2026-01-07T13:12:18-06:00
---

# Index pinning does not work for indirect dependencies

---

_Issue opened by @mzoll on 2025-04-17 06:48_

### Summary

Consider package 'foo' depending on package 'bar'. Both reside on a private index. the following pyproject.toml does not resolve the package to the correct index:

```
[project]
name = "my_project"
...
dependencies = ["foo"]
]

[[tool.uv.index]]
name = "artifactory"
url = "https://pkgs.dev.azure.com/.../pypi/simple/"
explicit = true

[tool.uv.sources]
foo = { index = "artifactory" }
bar = { index = "artifactory" }
```

running this by `uv lock` results in Error (the index was successfully inspected though):
```
 [Information] [CredentialProvider]VstsCredentialProvider - Acquired bearer token using 'MSAL Silent'
[Information] [CredentialProvider]VstsCredentialProvider - Attempting to exchange the bearer token for an Azure DevOps session token.
  x No solution found when resolving dependencies for split (python_full_version >= '3.12'):
  `-> Because bar was not found in the package registry and foo==1.0.0 depends on bar, we can conclude that foo==1.0.0
      cannot be used.
      And because only foo<=1.0.0 is available and your project depends on foo>=1.0.0, we can conclude that your project's requirements are unsatisfiable.
```

However, adding 'bar' as an additional explicit dependency to the project will resolve the packages successfully.





### Platform

windows

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

### Python version

3.10

---

_Label `bug` added by @mzoll on 2025-04-17 06:48_

---

_Comment by @konstin on 2025-04-17 09:45_

I think the problem here is that `bar` is not a direct dependency of `my_project`, and sources currently only apply to direct dependencies.

---

_Renamed from "uv lock resolution does not heed explicit strict index" to "Index pinning does not work for indirect dependencies" by @konstin on 2025-04-17 09:45_

---

_Comment by @charliermarsh on 2025-04-17 12:53_

Looks like a duplicate of https://github.com/astral-sh/uv/issues/8253.

---

_Closed by @charliermarsh on 2025-04-17 12:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-17 12:53_

---

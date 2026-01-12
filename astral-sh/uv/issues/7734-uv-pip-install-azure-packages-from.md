```yaml
number: 7734
title: uv pip install - Azure packages from upstreamSources are never found
type: issue
state: closed
author: ErichMarx
labels:
  - needs-mre
assignees: []
created_at: 2024-09-27T10:15:04Z
updated_at: 2024-10-03T10:48:11Z
url: https://github.com/astral-sh/uv/issues/7734
synced_at: 2026-01-12T15:59:16Z
```

# uv pip install - Azure packages from upstreamSources are never found

---

_@ErichMarx_

Hi,
I don't think I can reproduce the error here, but here's the issue:

We have many azure artifacts feeds on my company. Some feeds are linked to others through Upstream Sources, which means that for instance, if feed ProjectB is an upstream source on ProjectA, I should be able to get packages from ProjectB using the index-url of ProjectA only.

In the example below, I try to install a 'test_package' using the index-url (extra-index-url also fails) of the feed 'Feed1', in which this package doesn't exist. Feed1 however, has as Upstream Source another feed in which test_package exists.
When using pip install, I can find and install test_package and any other packages on my Feed1.

With uv pip install, I can only find package that exist on Feed1, any other package that is coming from another feed (through Upstream Sources) can't be found.

The [Azure REST API (Get Feeds) ](https://learn.microsoft.com/en-us/rest/api/azure/devops/artifacts/feed-management?view=azure-devops-rest-7.1)returns a feed object with attribute upstreamSources. Those upstream sources should also be searched when installing a package (I believe that is what is missing).

Here's what I tried:
`uv pip install test_package --index-url="https://PAT@pkgs.dev.azure.com/<Organization>/<project>/_packaging/Feed1/pypi/simple/" --verbose`

```
DEBUG uv 0.4.16
DEBUG Searching for default Python interpreter in system path or `py` launcher
DEBUG Found `cpython-3.11.6-windows-x86_64-none` at `C:\Users\<user>\testt\venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.11.6 environment at venv\Scripts\python.exe
DEBUG Acquired lock for `venv`
DEBUG At least one requirement is not satisfied: test_package
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.6
DEBUG Solving with target Python version: >=3.11.6
DEBUG Adding direct dependency: test_package*
DEBUG No cache entry for: https://pkgs.dev.azure.com/<Organization>/<project>/_packaging/Feed1/pypi/simple/test_package/
DEBUG No cache entry for: https://pkgs.dev.azure.com/<Organization>/<project>/_packaging/Feed1/pypi/simple/test_package/
DEBUG Searching for a compatible version of test_package(*)
DEBUG No compatible version found for: test_package
  × No solution found when resolving dependencies:
  ╰─▶ Because test_package was not found in the package registry and you require test_package, we can conclude that your requirements are unsatisfiable.
DEBUG Released lock at `C:\Users\<user>\testt\venv\.lock`
```

---

_Comment by @charliermarsh on 2024-09-27 12:22_

I'd love to help but I think we really need a reproduction that we can test here since it likely depends on details on how the Upstream Sources work, and how pip treats the redirect.

---

_Label `needs-mre` added by @charliermarsh on 2024-09-27 12:22_

---

_Comment by @ErichMarx on 2024-10-03 06:19_

Hi @charliermarsh ,
Just a FYi.
This was actually an issue on my Azure Feed. UV works fine with upstream sources, just like pip.
Apologies for this 

---

_Closed by @ErichMarx on 2024-10-03 06:19_

---

_Comment by @charliermarsh on 2024-10-03 10:48_

Thank you for following up!

---

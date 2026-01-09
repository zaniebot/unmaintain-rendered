---
number: 16236
title: "credential issue with `uv publish` since 0.8.14"
type: issue
state: closed
author: gwennole-cournez
labels:
  - bug
assignees: []
created_at: 2025-10-10T17:08:28Z
updated_at: 2025-11-18T14:38:28Z
url: https://github.com/astral-sh/uv/issues/16236
synced_at: 2026-01-07T13:12:19-06:00
---

# credential issue with `uv publish` since 0.8.14

---

_Issue opened by @gwennole-cournez on 2025-10-10 17:08_

### Summary

### Context:

In order to publish from a gitlab-ci to a private gitlab package registry, I am using username: gitlab-ci-token and password $CI_JOB_TOKEN as defined [here](https://github.com/astral-sh/uv/issues/8352).

### Issue

Since uv 0.8.14, I have a `Missing credential error` when publishing. Nothing else changed.

### Logs uv 0.8.13

```
$ uv publish --index gitlab --username gitlab-[MASKED] --password ${CI_JOB_TOKEN} -v
DEBUG uv 0.8.13
DEBUG Publishing with index gitlab
DEBUG Not a distribution filename: `.gitignore`
Publishing 2 files {package_registry_url}
DEBUG Using request timeout of 900s
DEBUG Checking for {my_project}-4.1.0-py3-none-any.whl in the registry
DEBUG No cache entry for: {package_registry_url}/simple/{project_name}/
DEBUG No netrc file found
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
WARN Package not found in the registry; skipping upload check for {my_project}-4.1.0-py3-none-any.whl
Uploading {my_project}-4.1.0-py3-none-any.whl (31.7KiB)
DEBUG Hashing dist/{my_project}-4.1.0.py3-none-any.whl
DEBUG Using HTTP Basic authentication
DEBUG Response code for https://{package_registry_url}: 201 Created
INFO Upload succeeded
DEBUG Checking for {my_project}-4.1.0.tar.gz in the registry
DEBUG No cache entry for: {package_registry_url}/simple/{project_name}/
Uploading {my_project}-4.1.0.tar.gz (93.0KiB)
DEBUG Hashing dist/{my_project}-4.1.0.tar.gz
DEBUG Using HTTP Basic authentication
DEBUG Response code for https://{package_registry_url}/pypi: 201 Created
INFO Upload succeeded
```

### Logs uv 0.8.14
```
$ uv publish --index gitlab --username gitlab-[MASKED] --password ${CI_JOB_TOKEN} -v
DEBUG uv 0.8.14
DEBUG Publishing with index gitlab
DEBUG Not a distribution filename: `.gitignore`
Publishing 2 files {package_registry_url}
DEBUG Using request timeout of 900s
DEBUG Checking for {my_project}-4.1.0-py3-none-any.whl in the registry
DEBUG No cache entry for: {package_registry_url}/simple/{project_name}/
DEBUG No netrc file found
error: Failed to query check URL
  Caused by: Failed to fetch: `https://{package_registry_url}/pypi/simple/{project_name}/`
  Caused by: Missing credentials for https://{package_registry_url}/pypi/simple/{project_name}
```

### Platform

Ubuntu 22.04

### Version

0.8.14 and latests

### Python version

3.10

---

_Label `bug` added by @gwennole-cournez on 2025-10-10 17:08_

---

_Renamed from "credential issue when `uv publish` since 0.8.14" to "credential issue with `uv publish` since 0.8.14" by @gwennole-cournez on 2025-10-10 17:16_

---

_Comment by @charliermarsh on 2025-10-11 01:21_

It looks like your credentials don't work for _reading_ from the index (is that expected?), and we made the `check-url` behavior more strict such that it errors if we fail that request.

---

_Comment by @gwennole-cournez on 2025-10-14 20:42_

> It looks like your credentials don't work for _reading_ from the index (is that expected?), and we made the `check-url` behavior more strict such that it errors if we fail that request.

No it's not expected, the token should be able to read from the package-registry.
Are those two lines liked or not ?
```
DEBUG No netrc file found
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
```
The request should take credentials from the token, not from the netrc file

---

_Comment by @zanieb on 2025-10-14 22:20_

The password you're providing is only used for writing to the registry. I think you'd also need to provide the password for reading? e.g., via `UV_INDEX_GITLAB_PASSWORD` and `UV_INDEX_GITLAB_USERNAME`? in 0.8.13, the check failed, but we ignored that failure.

I presume we should use the publish credentials for read if we can't find any other credentials, but that might be sort of hard to implement.

cc @konstin 

---

_Comment by @gwennole-cournez on 2025-10-15 06:53_

My project didn't have any dependency from my gitlab package-registry, so indeed `UV_INDEX_GITLAB_USERNAME`  `UV_INDEX_GITLAB_PASSWORD` were not defined.

It works fine now !

My only recommendation would be to make it clear in the `uv publish` documentation that both reading and writing tokens are required. `UV_INDEX_GITLAB_PASSWORD` only provides reading and `UV_PUBLISH_PASSWORD` only provides writing

Thank you @charliermarsh  and @zanieb.

---

_Closed by @konstin on 2025-11-18 14:38_

---

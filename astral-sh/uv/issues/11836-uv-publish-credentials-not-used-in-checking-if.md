---
number: 11836
title: "`uv publish`: Credentials not used in checking if package exists"
type: issue
state: closed
author: doraut
labels:
  - documentation
assignees: []
created_at: 2025-02-27T17:12:07Z
updated_at: 2025-08-27T16:19:11Z
url: https://github.com/astral-sh/uv/issues/11836
synced_at: 2026-01-10T01:25:11Z
---

# `uv publish`: Credentials not used in checking if package exists

---

_Issue opened by @doraut on 2025-02-27 17:12_

### Summary

When publishing to an index and passing a `check-url`, the provided credentials are not used when checking if the file exists. 

For example:
```sh
uv publish \ 
  --publish-url=https://$INDEX_URL \
  --check-url=https://$INDEX_URL/simple \
  --password=$PASSWORD \
  --username=$USERNAME \
  *.whl
```
will error if one of the wheels exists and the index does not allow uploading the same file again.

As a workaround, the password and username can be provided in the check-url (`--check-url=https://$USERNAME:$PASSWORD@$INDEX_URL/simple`). 


 

### Platform

Ubuntu 24.04.2 x86_64

### Version

uv 0.6.3

### Python version

Python 3.11.11

---

_Label `bug` added by @doraut on 2025-02-27 17:12_

---

_Comment by @konstin on 2025-02-27 17:16_

The username and password are only used for publish URL, not for the check URL, which uses regular index authentication (all the usual way for setting username/password for it work the same as with installation). We've split it this way since usually, upload credentials and fetch credentials are different.

---

_Label `bug` removed by @konstin on 2025-02-27 17:16_

---

_Label `documentation` added by @konstin on 2025-02-27 17:16_

---

_Referenced in [astral-sh/uv#12652](../../astral-sh/uv/issues/12652.md) on 2025-04-03 14:09_

---

_Comment by @kwaegel on 2025-06-20 17:31_

@konstin I'm not entirely sure if this is a documentation bug. I was about to file a new bug report that is effectively this, but I have the regular index authentication provided via `UV_INDEX_MYCOMPANY_{USERNAME,PASSWORD}` and the re-upload check still doesn't work (fails with a 401 Forbidden error). All other commands are able to pull from our private repository (set as the default) just fine, but not `uv publish --index`.

Using uv 0.7.13

---

_Comment by @konstin on 2025-06-20 17:38_

Is it the reupload check that fails, or the upload itself that fails? Can you share the commands you use and the log?

---

_Comment by @asturnus on 2025-07-01 08:46_

@konstin Can confirm that `uv` does not read credentials from the environment when publishing with `--check-url`, so the reupload check fails with `401`. It does not matter if `check-url` is specified in the config or as an argument.

<details>
<summary>Click to see the log of publishing to a GitLab registry.</summary>

```bash
$ uv publish --check-url $INDEX_URL -vv
DEBUG uv 0.7.17
Publishing 1 file to https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/
DEBUG Using request timeout of 900s
DEBUG Checking for <redacted>.whl in the registry
TRACE Fetching metadata for <redacted> from https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/
TRACE No cache entry exists for /root/.cache/uv/simple-v16/index/840f9e7cec2b1188/<redacted>.rkyv
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/
TRACE Sending fresh GET request for https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/
TRACE Handling request for https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/ with authentication policy auto
TRACE Request for https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/
TRACE Attempting unauthenticated request for https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/
TRACE Request for https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for URL https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple
TRACE No credentials in cache for realm https://gitlab.com
DEBUG No netrc file found
TRACE Considering retry of response HTTP 401 Unauthorized for https://gitlab.com/api/v4/projects/<redacted>/packages/pypi/simple/<redacted>/
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
WARN Package not found in the registry; skipping upload check for <redacted>.whl
```

</details>

Using the same workaround as above: embedding the credentials into `check-url`.

---

_Referenced in [astral-sh/uv#14419](../../astral-sh/uv/pulls/14419.md) on 2025-07-02 13:12_

---

_Assigned to @konstin by @konstin on 2025-08-26 15:35_

---

_Referenced in [astral-sh/uv#15545](../../astral-sh/uv/pulls/15545.md) on 2025-08-27 08:44_

---

_Referenced in [astral-sh/uv#15546](../../astral-sh/uv/pulls/15546.md) on 2025-08-27 09:10_

---

_Closed by @zanieb on 2025-08-27 16:19_

---

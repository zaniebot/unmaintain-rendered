```yaml
number: 13208
title: Failure to download packages from Azure Artifacts starting version 0.7.0
type: issue
state: closed
author: ycheng517
labels:
  - bug
assignees: []
created_at: 2025-04-30T02:03:03Z
updated_at: 2025-06-16T12:45:52Z
url: https://github.com/astral-sh/uv/issues/13208
synced_at: 2026-01-10T03:32:45Z
```

# Failure to download packages from Azure Artifacts starting version 0.7.0

---

_Issue opened by @ycheng517 on 2025-04-30 02:03_

### Summary

Starting with UV 0.7.0 just today, I can no longer download packages from Azure Artifacts. Error:

```
uv sync
Resolved 382 packages in 0.74ms
  × Failed to download `my-private-pkg==4.0.0`
  ├─▶ Failed to extract archive: my_private_pkg-4.0.0-py3-none-any.whl
  ├─▶ an upstream reader returned an error: unexpected end of file
  ╰─▶ unexpected end of file
```

In the verbose logs I see the following which might be relevant:

```
TRACE Cached response https://pkgs.dev.azure.com/XXXXXX/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/my-private-pkg/4/my_private_pkg-4.0.0-py3-none-any.whl is not storable because it does not meet any of the necessary criteria (e.g., it doesn't have an 'Expires' header set or a 'max-age' cache-control directive)
TRACE Considering retry of error: Extract("my_private_pkg-4.0.0-py3-none-any.whl", AsyncZip(UpstreamReadError(Kind(UnexpectedEof))))
TRACE Retrying error: `ConnectionReset` or `UnexpectedEof`
DEBUG Transient failure while handling response from https://pkgs.dev.azure.com/XXXXXX/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/my-private-pkg/4/my_private_pkg-4.0.0-py3-none-any.whl; retrying...
```

These are the relevant portions of my `pyproject.toml` file:

```
dependencies = [
  "my-private-pkg>=4.0",
  ...
]

[tool.uv.sources]
my-private-pkg = { index = "azure" }
...

[[tool.uv.index]]
name = "azure"
url = "https://pkgs.dev.azure.com/XXXXXX/_packaging/XXXXXX/pypi/simple"
```

### Platform

Ubuntu 24.04 amd64

### Version

uv 0.7.0

### Python version

Python 3.12.3

---

_Label `bug` added by @ycheng517 on 2025-04-30 02:03_

---

_Comment by @zanieb on 2025-04-30 02:08_

Thanks for the report. You can confirm this works on 0.6.17?

---

_Comment by @zanieb on 2025-04-30 02:09_

Does this occur every time, or intermittently?

---

_Comment by @zanieb on 2025-04-30 02:10_

Can you share full verbose logs? Perhaps in a [Gist](https://gist.github.com)?

---

_Comment by @ycheng517 on 2025-04-30 02:11_

I can confirm this works if I revert back to 0.6.16. It happens every time. I will try to send a Gist shortly, just need some time to redact  some names.

---

_Comment by @zanieb on 2025-04-30 02:13_

Wonderful, thank you!

cc @jtfmumm 

---

_Comment by @zanieb on 2025-04-30 02:16_

Just to make sure, because it narrows down where we're looking, 0.6.17 works or fails? You referenced 0.6.16.

---

_Comment by @ycheng517 on 2025-04-30 02:29_

I just tried 0.6.17, it also works. Here's [verbose logs](https://gist.github.com/ycheng517/63437cccf5279c15018356cf9ca8b971) of the failure on 0.7.0. Note that for this log, I'm simply adding a private package, as it's a simpler manifestation of the issue.

Note that the package I'm adding was built using `uv build --wheel` on UV version 0.6.16.

---

_Comment by @raiderrobert on 2025-04-30 02:36_

I also have this issue with uv starting on version 0.7.0. I also use Azure Artifacts.

---

_Comment by @charliermarsh on 2025-04-30 02:43_

Thanks for the detailed logs -- we'll take a look. Unfortunately I don't see anything in the changelog that's obviously relevant here...

---

_Comment by @zanieb on 2025-04-30 03:53_

A log of the success for comparison would be helpful too.

---

_Comment by @zanieb on 2025-04-30 03:59_

The key seems to be around

```
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/My-Project-Name/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/rr-config/4/rr_config-4.0.0-py3-none-any.whl
TRACE Request for https://pkgs.dev.azure.com/My-Project-Name/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/rr-config/4/rr_config-4.0.0-py3-none-any.whl failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://pkgs.dev.azure.com
TRACE Retrying request for https://pkgs.dev.azure.com/My-Project-Name/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/rr-config/4/rr_config-4.0.0-py3-none-any.whl with credentials from cache Basic { username: Username(Some("My-Username")), password: Some(****) }
TRACE Cached response https://pkgs.dev.azure.com/My-Project-Name/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/rr-config/4/rr_config-4.0.0-py3-none-any.whl is not storable because it does not meet any of the necessary criteria (e.g., it doesn't have an 'Expires' header set or a 'max-age' cache-control directive)
TRACE Considering retry of error: Metadata("https://pkgs.dev.azure.com/My-Project-Name/_packaging/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX/pypi/download/rr-config/4/rr_config-4.0.0-py3-none-any.whl", AsyncZip(UpstreamReadError(Kind(UnexpectedEof))))
TRACE Retrying error: `ConnectionReset` or `UnexpectedEof`
TRACE Received built distribution metadata for: rr-config==4.0.0
WARN rr-config==4.0.0 has an invalid package format: Failed to read from zip file
WARN rr-config has an invalid package format: Failed to read from zip file
```

How are you providing credentials?

---

_Comment by @paulstaab on 2025-04-30 07:35_

Happens for us as well using Azure Devops Artifacts Feed.

~/.config/uv/uv.toml:
```
[[index]]
name = "internal"
url = "https://***@pkgs.dev.azure.com/DDC-DAAI/_packaging/***/pypi/simple"
default = true
```

Here is the log with all sensible values replaced with `***` for versions 0.7.0 and 0.6.17:

```
uv pip install -vvv -U "pip==25.1" 2> uv-0.7.0.log
```

[uv-0.7.0.log](https://github.com/user-attachments/files/19973028/uv-0.7.0.log)
[uv-0.6.17.log](https://github.com/user-attachments/files/19973029/uv-0.6.17.log)

Providing the credentials via `UV_INDEX_INTERNAL_PASSWORD` produces the same problem.

---

_Comment by @jenshnielsen on 2025-04-30 07:35_

I am seeing the same issue. I have my feed configured via `UV_INDEX_URL` env variable as `https://VssSessionToken@myorg.pkgs.visualstudio.com/_packaging/myfeed/pypi/simple` and artifacts-keyring installed as a uv tool



---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-30 07:40_

---

_Comment by @lskbr on 2025-04-30 07:40_

I confirm the problem with 0.7 on Azure pipelines, but I can also reproduce it in my command line.

```
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.dev.azure.com")), port: None, path: "/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(405), url: "https://pkgs.dev.azure.com/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl" }))) }
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for boto3-1.38.5-py3-none-any.whl; streaming wheel
TRACE connection.state=Open
TRACE poll
TRACE poll_complete
TRACE schedule_pending_open
TRACE flushing buffer
TRACE No cache entry exists for /tmp/.tmpBl974A/wheels-v5/index/4acd0df98cde8196/boto3/1.38.5-py3-none-any.msgpack
DEBUG No cache entry for: https://pkgs.dev.azure.com/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl
TRACE Handling request for https://pkgs.dev.azure.com/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl with authentication policy auto
TRACE Request for https://pkgs.dev.azure.com/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/COMPANY/UUID/_packaging/01523230-7280-468a-a936-0704848f8ed3/pypi/download/boto3/1.38.5/boto3-1.38.5-py3-none-any.whl
TRACE take? ("https", pkgs.dev.azure.com): expiration = Some(90s)
```

with `uv pip install boto3 --no-cache --force-reinstall`, it tries to download all versions, from 1.8.5, invalidating each one and downgrading to the next version of the package (it errors until I interrupt). When I read the TRACE LOG, some parts got my attention:
1. The error 405 in the request. Is it related to the range request?
2. The authentication is missing.

In the local environment, I set my azure password using a personal access token (PAT), so it is passed as the UV_EXTRA_INDEX_URL=https://{$PAT}@pkgs.dev.azure... as in all previous versions. I tested the same setup with  uv 0.6.4 and it works fine. The same mechanism is used in azure pipelines, but the token is created using a specific task Pip1@Authenticate.

PS: I replaced COMPANY and UUID to anonymize the trace.

---

_Comment by @jtfmumm on 2025-04-30 07:41_

I'm able to replicate this issue and verified that it occurs with 0.7.0 and not 0.6.17. Investigating now.

---

_Comment by @thomasaarholt on 2025-04-30 07:51_

A quick guess would be that the regression was introduced in this [commit](https://github.com/astral-sh/uv/commit/de1479c4ef41106d5e87f5ad6ba27b9f94c46b0c) 

"Use index URL instead of package URL for keyring credential lookups (https://github.com/astral-sh/uv/pull/12651)"

(I was notified of it because that commit was listed as fixing my Azure DevOps issue at #11507)

---

_Comment by @jtfmumm on 2025-04-30 08:05_

I'm pretty certain this is because of changes to redirect handling. I'm going to revert those changes to address this problem while investigating.

---

_Comment by @jenshnielsen on 2025-04-30 08:14_

Thanks @jtfmumm If you would benefit from some additional testing, I am happy to try your pr(s) against my feed. Please feel free to ping me for that

---

_Comment by @jtfmumm on 2025-04-30 08:15_

@jenshnielsen That would be helpful! The PR is #13215

---

_Comment by @brynmathias on 2025-04-30 08:21_

Done some testing on our jfrog repo that doesn't mirror the global pypi
If we configure with out the ignore `ignore-error-codes = [401]` flag it fails as it doesn't go on to the next index url (ie pypi.org)

however if we explicitly add the `ignore-error-codes` it works fine

I would propose that for the env var `UV_EXTRA_INDEX_URL` that the ignore codes are set to sensible defaults

---

_Comment by @jtfmumm on 2025-04-30 08:24_

@brynmathias Please open a separate issue and I can respond there.

---

_Comment by @brynmathias on 2025-04-30 08:25_

@jtfmumm will do

---

_Comment by @dmkulazhenko on 2025-04-30 08:43_

+1, with self-hosted pypi + hatchling as build system it leads to:
```
> [7/7] RUN hatch env create dev:
0.394 Creating environment: dev
0.406 Installing project in development mode
0.555   × Failed to build `----- @ file:///app`
0.555   ├─▶ Failed to resolve requirements from `build-system.requires`
0.555   ├─▶ No solution found when resolving: `hatchling`
0.[555](https://github.com/-----/actions/runs/----/job/---#step:4:556)   ╰─▶ Because there are no versions of hatchling and you require hatchling, we
0.555       can conclude that your requirements are unsatisfiable.
------
```

For anyone googling by trace above: `pip install uv==0.6.17` until fix will be released.

---

_Comment by @jtfmumm on 2025-04-30 08:54_

This issue should have been addressed for now by #13215. I'm going to leave it open to track the underlying problem.

---

_Comment by @jtfmumm on 2025-04-30 11:04_

`0.7.1` is now available with this fix.

---

_Comment by @josteinl on 2025-04-30 11:55_

Fix in `0.7.1` works for me! 

Version `0.6.17` worked fine. Version `0.7.0` failed before lunch, and `0.7.1` works after lunch! Super thanks for the fast fix @jtfmumm !

---

_Comment by @zanieb on 2025-05-01 18:28_

We'll track rolling this change back out (with a fix for this case) in https://github.com/astral-sh/uv/issues/13255

---

_Closed by @zanieb on 2025-05-01 18:28_

---

_Comment by @jtfmumm on 2025-06-16 12:11_

We have a new fix for authentication on redirects on #13754 that avoids this problem. It would be very helpful if anyone would be willing to test that PR on their own private index (whether Azure or something else). You could build that PR or you could also use:

```
uvx -v --from "uv @ git+https://github.com/astral-sh/uv@jtfm/update-redirect-handling" uv add <pkg-from-private-index> --default-index https://<username>:<password>@<private-index-url>
```

filling in `<pkg-from-private-index>`, `<username>`, `<password>`, and `<private-index-url>` with your own values.

---

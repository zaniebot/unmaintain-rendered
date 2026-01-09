---
number: 8510
title: Private repository cloudrepo.io 401 Unauthorized
type: issue
state: closed
author: imatw4r
labels:
  - needs-mre
assignees: []
created_at: 2024-10-23T22:37:44Z
updated_at: 2024-10-24T18:59:58Z
url: https://github.com/astral-sh/uv/issues/8510
synced_at: 2026-01-07T13:12:17-06:00
---

# Private repository cloudrepo.io 401 Unauthorized

---

_Issue opened by @imatw4r on 2024-10-23 22:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hey folks! I am running to a problem installing dependencies with private registry hosted on [cloudrepo](https://www.cloudrepo.io/).
The command I execute is:
```
uv pip install -r requirements.txt --extra-index-url https://<USERNAME>:<PASSWORD>@[redacted].mycloudrepo.io/repositories/[redacted]
```
That causes:
```
Failed to download `[redacted-package-name]==0.0.13`
  Caused by: HTTP status client error (401 Unauthorized) for url (https://[redacted].mycloudrepo.io/repositories/[redacted]/[redacted-package-name]/[redacted-package-name]-0.0.13-py2.py3-none-any.whl)
```

Executing pip command:
```
pip install -r requirements.txt
```
With ENV variable `PIP_EXTRA_INDEX_URL=` set to the same value I pass via `--extra-index-url` to UV works, and the dependency is found and installed.

Note: I pass password & username directly as a string, not trying to interpolate (I am aware of #5734 discussion). I also tried setting an environment variable UV_PIP_EXTRA_INDEX_URL but it doesn't work.

I know there was an issue reported (#1709) that seems to be closed already, but I wonder if it was supposed to be fixed for all platforms (azure/aws, etc) or if it's needed to be added separately for `mycloudrepo.io`?

Some more details:

Platform:
MacOS, Sonoma 14.4
M1 Apple Silicon

```
uv --version
> uv 0.4.22 (Homebrew 2024-10-15)
```


---

_Renamed from "Cloudrepo.io 401 Unauthorized" to "Private repository cloudrepo.io 401 Unauthorized" by @imatw4r on 2024-10-23 22:38_

---

_Comment by @charliermarsh on 2024-10-23 23:38_

The environment variable you're looking for is `UV_EXTRA_INDEX_URL` (no `_PIP_`), though passing on the command-line should be working too. It's hard to say more without being able to debug the index response itself...

---

_Label `needs-mre` added by @charliermarsh on 2024-10-23 23:38_

---

_Comment by @imatw4r on 2024-10-24 08:47_

@charliermarsh for pip it's `PIP_`, for `uv` I was using `UV_EXTRA_INDEX_URL`. 

One, more thing that I've noticed is that installing from a file `uv pip install -r requirements.txt --extra-index-url ...` fails with 401 Unauthorized and a package:
```
error: Failed to download `debonair==1.8.0`
  Caused by: HTTP status client error (401 Unauthorized) for url (https://[REDACTED].mycloudrepo.io/repositories/[REDACTED]/debonair/debonair-1.8.0-py3-none-any.whl)
```

While installing package directly: `uv pip install debonair==1.8.0 --extra-index-url ...` works just fine.

I am attaching an output of `uv pip install -vv -r requirements.txt --extra-index-url ...` - maybe it will help:
[out.txt](https://github.com/user-attachments/files/17504114/out.txt)


---

_Comment by @zanieb on 2024-10-24 12:38_

Can you share redacted logs with `RUST_LOG=uv=trace`? That'll give us more information about the authentication flow.

---

_Comment by @imatw4r on 2024-10-24 15:58_

@zanieb thanks for the info about `RUST_LOG=trace`. It helped me solved the issue.

TLDR in my `requirements.txt` I had specified:

```
# Add our private package repo, with read only credentials.
--extra-index-url https://${CLOUDREPO_USERNAME}:${CLOUDREPO_PASSWORD}@[REDACTED].mycloudrepo.io/repositories/goodrx

package1==version
package2==version
```

And seems like index specified in `requirements.txt` has higher priority over the one passed explicitly via `--extra-index-url` when calling `uv pip install`, which caused an issue for me as I didn't have these env variables specified.

It works now, sorry for the inconvenience @zanieb @charliermarsh 

---

_Closed by @imatw4r on 2024-10-24 18:59_

---

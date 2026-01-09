---
number: 7809
title: "`uv publish` times out when uploading large packages (~85MiB)"
type: issue
state: closed
author: benniekiss
labels:
  - bug
assignees: []
created_at: 2024-09-30T13:19:55Z
updated_at: 2024-10-04T13:52:24Z
url: https://github.com/astral-sh/uv/issues/7809
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv publish` times out when uploading large packages (~85MiB)

---

_Issue opened by @benniekiss on 2024-09-30 13:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
When using `uv publish` to upload a package to a personal forgejo instance or at https://code.forgejo.org, I'm experiencing timeouts at the authentication stage. I'm seeing this when using either `--username` and `--password` or `--token`. Using `uvx twine upload` works.

EDIT:
I've also tested this against https://test.pypi.org/legacy/ and experience the same issue. See https://github.com/astral-sh/uv/issues/7809#issuecomment-2392235235

---

_Comment by @zanieb on 2024-09-30 13:49_

Can you share verbose logs with `RUST_LOG=trace uv publish -v`? Make sure to redact any credentials.

---

_Label `bug` added by @zanieb on 2024-09-30 13:49_

---

_Assigned to @konstin by @konstin on 2024-09-30 14:01_

---

_Comment by @benniekiss on 2024-09-30 14:03_

Here are logs when trying to upload to code.forgejo.org - 

```
(build) 📦[bennie@micro-dev deps]$ RUST_LOG=trace uv publish -v --publish-url https://code.forgejo.org/api/packages/benniekiss/pypi --username benniekiss --password MYPASSWORD dist/exllamav2-0.2.3-cp312-cp312-linux_x86_64.whl 
DEBUG uv 0.4.17
warning: `uv publish` is experimental and may change without warning
Publishing 1 file to https://code.forgejo.org/api/packages/benniekiss/pypi
DEBUG Using request timeout of 30s
Uploading exllamav2-0.2.3-cp312-cp312-linux_x86_64.whl (130.9MiB)
DEBUG Using username/password basic auth
TRACE Handling request for https://benniekiss:****@code.forgejo.org/api/packages/benniekiss/pypi
TRACE Request for https://benniekiss:****@code.forgejo.org/api/packages/benniekiss/pypi is already fully authenticated
TRACE checkout waiting for idle connection: ("https", code.forgejo.org)
DEBUG starting new connection: https://code.forgejo.org/    
TRACE Http::connect; scheme=Some("https"), host=Some("code.forgejo.org"), port=None
DEBUG resolving host="code.forgejo.org"
DEBUG connecting to [2a01:4f9:3081:51ec::102]:443
DEBUG connected to [2a01:4f9:3081:51ec::102]:443
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", code.forgejo.org)
DEBUG Transient request failure for https://code.forgejo.org/api/packages/benniekiss/pypi, retrying: error sending request for url (https://code.forgejo.org/api/packages/benniekiss/pypi)
  Caused by: operation timed out
WARN Transient request failure for https://code.forgejo.org/api/packages/benniekiss/pypi, retrying
DEBUG Using username/password basic auth
TRACE Handling request for https://benniekiss:****@code.forgejo.org/api/packages/benniekiss/pypi
TRACE Request for https://benniekiss:****@code.forgejo.org/api/packages/benniekiss/pypi is already fully authenticated
TRACE checkout waiting for idle connection: ("https", code.forgejo.org)
DEBUG starting new connection: https://code.forgejo.org/    
TRACE Http::connect; scheme=Some("https"), host=Some("code.forgejo.org"), port=None
DEBUG resolving host="code.forgejo.org"
DEBUG connecting to [2a01:4f9:3081:51ec::102]:443
DEBUG connected to [2a01:4f9:3081:51ec::102]:443
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", code.forgejo.org)
```

---

_Comment by @zanieb on 2024-09-30 14:14_

Hm weird. It's not clear to me what's going on there. 

cc @konstin 

---

_Comment by @benniekiss on 2024-09-30 14:21_

I'm getting this in my forgejo instance logs when I upload, but I'm not getting anything more meaningful - 

`2024/09/30 10:20:13 ...eb/routing/logger.go:102:func1() [I] router: completed POST /api/packages/gts/pypi for 100.64.0.13:0, 400 Bad Request in 31283.3ms @ pypi/pypi.go:108(pypi.UploadPackageFile)`

forgejo code - https://codeberg.org/forgejo/forgejo/src/commit/e3eaa284bb86a3767bff7c601f74e8350b66f6f4/routers/api/packages/pypi/pypi.go#L107-L190

---

_Comment by @konstin on 2024-10-01 12:50_

I unfortunately can't reproduce, uploading succeeds for me: https://code.forgejo.org/konstin/-/packages/pypi/black/0.1.0

---

_Label `needs-mre` added by @konstin on 2024-10-01 12:50_

---

_Label `bug` removed by @konstin on 2024-10-01 12:50_

---

_Comment by @benniekiss on 2024-10-01 13:42_

> I unfortunately can't reproduce, uploading succeeds for me: https://code.forgejo.org/konstin/-/packages/pypi/black/0.1.0

Are you using a PAT as password or a regular password? I did not mention this earlier, but I was using a PAT

---

_Comment by @konstin on 2024-10-01 13:43_

I generated a token and used that for the upload

---

_Comment by @benniekiss on 2024-10-01 14:09_

I'll see if I can't get it working again on my end and give a better example

---

_Comment by @benniekiss on 2024-10-01 14:27_

So I am also able to publish the same `black` wheel you published (I just downloaded it from the link you posted); however, I still cannot publish the `exllama` wheel, and I think it may be related to the size of the file that's causing it to timeout. Here's what my console looks like when trying to publish the wheel:

<img width="824" alt="Screenshot 2024-10-01 at 10 26 37" src="https://github.com/user-attachments/assets/318b11c4-c8a2-4c93-b735-016fc363c267">

It typically restarts/timeouts after about 75mb have been uploaded, but sometimes it'll upload as much as 90% before timing out


---

_Comment by @benniekiss on 2024-10-03 16:03_

Here is a pre-built wheel that I cannot upload using `uv publish`, but that does upload using `uvx twine upload` - https://pypi.org/project/vllm/#files

code to run:
```
uv publish --username $MY_USER --password $MY_PASSWORD --publish-url https://code.forgejo.org/api/packages/$MY_USER/pypi vllm-0.6.2-cp38-abi3-manylinux1_x86_64.whl
```
This fails for me in the same way as above. I can successfully upload the wheel using twine:
```
uvx twine upload --username $MY_USER --password $MY_PASSWORD --repository-url https://code.forgejo.org/api/packages/$MY_USER/pypi vllm-0.6.2-cp38-abi3-manylinux1_x86_64.whl
```

I also tried testing by uploading to https://test.pypi.org/legacy to see if it was specific to forgejo, but I need to rebuild the wheel with a different name because of namespace issues. Ill update once I get to rebuilding and if I encounter the same timeout.

---

_Comment by @benniekiss on 2024-10-03 20:05_

I was able to reproduce this problem when uploading a package to https://test.pypi.org/legacy/ . Since pypi has namespace and size limits, creating an MRE is a little involved -

```shell
export MY_PYPI_TOKEN=

mkdir -p test_uv_publish_size/src/my_package
cd test_uv_publish_size

## make pyproject.toml
cat <<EOF > pyproject.toml
[project]
name = "test_uv_publish_size_$(openssl rand -hex 4)"
version = "0.0.1"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/my_package"]
EOF

## make our program
cat <<EOF > src/my_package/__init__.py
print("Hello World")
EOF

## fake our package size
dd if=/dev/urandom of=src/my_package/test_data bs=90MB count=1

## build with uv
uv build --wheel .

## upload with uv
uv publish --publish-url https://test.pypi.org/legacy/ --token $MY_PYPI_TOKEN dist/*
```

Here is what my console looks like during the failure:

<img width="855" alt="Screenshot 2024-10-03 at 15 57 54" src="https://github.com/user-attachments/assets/33e08da5-aec4-4134-b307-21000cb19c83">

<img width="855" alt="Screenshot 2024-10-03 at 15 58 27" src="https://github.com/user-attachments/assets/33e628c7-5bd4-43e6-85bd-2cc2804750f0">





---

_Renamed from "`uv publish` times out when uploading package to forgejo registry" to "`uv publish` times out when uploading large packages (~85MiB)" by @benniekiss on 2024-10-03 20:11_

---

_Referenced in [astral-sh/uv#7923](../../astral-sh/uv/pulls/7923.md) on 2024-10-04 11:17_

---

_Comment by @konstin on 2024-10-04 11:19_

Thank you for the great reproduction!

Our default timeouts are too low and we weren't showing the retry warnings, fixed in #7921 and we'll increase timeouts to 15min in #7923.

---

_Label `needs-mre` removed by @konstin on 2024-10-04 11:19_

---

_Label `bug` added by @konstin on 2024-10-04 11:19_

---

_Closed by @konstin on 2024-10-04 13:52_

---

_Referenced in [astral-sh/uv#7839](../../astral-sh/uv/issues/7839.md) on 2024-11-03 18:09_

---

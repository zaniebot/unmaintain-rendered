---
number: 9361
title: UV on gitlab CICD has some issue
type: issue
state: closed
author: MaKaNu
labels:
  - question
assignees: []
created_at: 2024-11-22T18:18:00Z
updated_at: 2024-11-22T22:18:58Z
url: https://github.com/astral-sh/uv/issues/9361
synced_at: 2026-01-07T13:12:18-06:00
---

# UV on gitlab CICD has some issue

---

_Issue opened by @MaKaNu on 2024-11-22 18:18_

I am not sure at the moment, if this is an issue with the uv image or a [bug on gitlab](https://gitlab.com/gitlab-org/charts/gitlab-runner/-/issues/477), but I stumbled over a different issue when testing my pipelines:

I have tested the following scenarios with different small changes based on the presented errors:

**Gitlab runner on Kubernetes executor:**

.gitlab-ci.yml:
```yml
variables:
  UV_VERSION: 0.5
  PYTHON_VERSION: 3.12
  BASE_LAYER: bookworm-slim

test_ndp_cli:
  stage: test
  image: ghcr.io/astral-sh/uv:latest
  variables:
    UV_CACHE_DIR: .uv-cache
  cache:
    - key:
        files:
          - uv.lock
      paths:
        - $UV_CACHE_DIR
  script:
    - uv python install
    - uv sync --all-extras --dev
    - uv run pytest
    - uv cache prune --ci
```
result:

```bash
ERROR: Job failed (system failure): prepare environment: setting up trapping scripts on emptyDir: unable to upgrade connection: container not found ("build"). Check https://docs.gitlab.com/runner/shells/index.html#shell-profile-loading for more information
```

I have diagnosed a few issues with our kubernetes system, so I have run it multiple times and got a different result:

```bash
WARNING: Event retrieved from the cluster: Error: failed to create containerd task: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "sh": executable file not found in $PATH: unknown
ERROR: Job failed (system failure): prepare environment: waiting for pod running: timed out waiting for pod to start. Check https://docs.gitlab.com/runner/shells/index.html#shell-profile-loading for more information
```

So I thought maybe still something wrong on Kubernetes, let me test on a standalone-docker executor:

result:

```bash
Using docker image sha256:b4f508bf6cb1eda0e534b608a2a4dec80a8d01cfa2c4f3f8953537b60734d729 for ghcr.io/astral-sh/uv:latest with digest ghcr.io/astral-sh/uv@sha256:5436c72d52c9c0d011010ce68f4c399702b3b0764adcf282fe0e546f20ebaef6 ...
error: unrecognized subcommand 'sh'
Usage: uv [OPTIONS] <COMMAND>
For more information, try '--help'.
Cleaning up project directory and file based variables 00:01
ERROR: Job failed: exit code 2
```

Something again with 'sh' so I thought lets test with empty entrypoint:

```diff
diff --git a/.gitlab-ci.yml b/.gitlab-ci.yml
index 909efe2..f30df03 100644
--- a/.gitlab-ci.yml
+++ b/.gitlab-ci.yml
@@ -16,8 +16,12 @@ variables:
   BASE_LAYER: bookworm-slim

 test_ndp_cli:
+  tags: [docker-standalone]
+
   stage: test
-  image: ghcr.io/astral-sh/uv:latest
+  image:
+    name: ghcr.io/astral-sh/uv:latest
+    entrypoint: [""]
   variables:
     UV_CACHE_DIR: .uv-cache
   cache:
@@ -33,8 +37,11 @@ test_ndp_cli:
     - uv cache prune --ci

 build_linux:
+  tags: [docker-standalone]
   stage: build
-  image: ghcr.io/astral-sh/uv:latest
+  image:
+    name: ghcr.io/astral-sh/uv:latest
+    entrypoint: [""]
   variables:
     UV_CACHE_DIR: .uv-cache
   cache:
```

result:

```bash
ERROR: Job failed (system failure): Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "sh": executable file not found in $PATH: unknown (exec.go:77:0s)
```


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @zanieb on 2024-11-22 18:31_

Have you tried using an image as described in our GitLab integration guide? https://docs.astral.sh/uv/guides/integration/gitlab/

For GitLab, you'll want an image that isn't distroless: https://docs.astral.sh/uv/guides/integration/docker/#available-images

---

_Label `question` added by @zanieb on 2024-11-22 18:31_

---

_Comment by @MaKaNu on 2024-11-22 22:18_

I was blind and deaf the hole time. As you can see I already had the variables ready but for what ever reason I missed entering the image correctly.

I always tinker at the wrong position... Thanks for pointing out. Kube now works as expected...

---

_Closed by @MaKaNu on 2024-11-22 22:18_

---

_Referenced in [astral-sh/uv-docker-example#31](../../astral-sh/uv-docker-example/issues/31.md) on 2024-11-27 08:24_

---

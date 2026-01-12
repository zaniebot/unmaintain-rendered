```yaml
number: 6208
title: "Install `ca-certificates` in docker and use pipefail"
type: pull_request
state: merged
author: konstin
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: konsti/install-recommends
created_at: 2024-08-19T14:45:59Z
updated_at: 2024-08-19T17:14:25Z
url: https://github.com/astral-sh/uv/pull/6208
synced_at: 2026-01-12T16:07:16Z
```

# Install `ca-certificates` in docker and use pipefail

---

_@konstin_

A dockerfile using `ubuntu` instead of `python` as base image currently silently fails to install.

```dockerfile
FROM ubuntu
RUN apt-get update && apt-get install -y curl --no-install-recommends
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
RUN uv --version
```

```console
$ docker buildx build --progress plain --no-cache .
[...]
#6 [3/4] RUN curl -LsSf https://astral.sh/uv/install.sh | sh
#6 0.144 curl: (77) error setting certificate file: /etc/ssl/certs/ca-certificates.crt
#6 DONE 0.2s

#7 [4/4] RUN uv --version
#7 0.113 /bin/sh: 1: uv: not found
#7 ERROR: process "/bin/sh -c uv --version" did not complete successfully: exit code: 127
```

There's two underlying problems: Pipefail, and missing `ca-certificates`.

In most shells, the source of a pipe erroring doesn't fail the entire command, so `curl -LsSf https://astral.sh/uv/install.sh | sh` passes even if the curl part fails. In bash, you can prefix the command with `set -o pipefail &&` to change this behavior. But in the `ubuntu` docker container, dash is the default shell, not bash. dash doesn't have a pipefail option (in the version in ubuntu), so the [best practice](https://docs.docker.com/build/building/best-practices/#using-pipes) is `RUN ["/bin/bash", "-c", "set -o pipefail && curl -LsSf https://astral.sh/uv/install.sh | sh"]`. That's not very readable, so i'm going for `RUN curl -LsSf https://astral.sh/uv/install.sh > /tmp/uv-installer.sh && sh /tmp/uv-installer.sh && rm /tmp/uv-installer.sh` instead.

```dockerfile
FROM ubuntu
RUN apt-get update && apt-get install -y curl --no-install-recommends
RUN curl -LsSf https://astral.sh/uv/install.sh > /tmp/uv-installer.sh && sh /tmp/uv-installer.sh && rm /tmp/uv-installer.sh \
RUN uv --version
```

```console
$ docker buildx build --progress plain --no-cache .
[...]
#6 [3/3] RUN curl -LsSf https://astral.sh/uv/install.sh > /tmp/uv-installer.sh && sh /tmp/uv-installer.sh && rm /tmp/uv-installer.sh RUN uv --version
#6 0.179 curl: (77) error setting certificate file: /etc/ssl/certs/ca-certificates.crt
#6 ERROR: process "/bin/sh -c curl -LsSf https://astral.sh/uv/install.sh > /tmp/uv-installer.sh && sh /tmp/uv-installer.sh && rm /tmp/uv-installer.sh RUN uv --version" did not complete successfully: exit code: 77
```

The source for this error is `ca-certificates` missing, which is a recommended package. We need to drop `--no-install-recommends` and the installation passes again.

---

_Label `documentation` added by @konstin on 2024-08-19 14:45_

---

_Comment by @zanieb on 2024-08-19 14:52_

Should we just actually request the packages we need alongside `curl` instead of installing all recommended packages?

---

_Comment by @charliermarsh on 2024-08-19 14:53_

Yeah I think `--no-install-recommends` is a best practice in Docker.

---

_@zanieb reviewed on 2024-08-19 14:54_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:26 on 2024-08-19 14:54_

Can we just `ADD` the file from the remote URL if we're doing this? Does that even require `curl`?

---

_@konstin reviewed on 2024-08-19 14:56_

---

_Review comment by @konstin on `docs/guides/integration/docker.md`:26 on 2024-08-19 14:56_

I tried that, but cargo-dist requires curl or wget to download the uv archive.

---

_Comment by @konstin on 2024-08-19 14:57_

Do we know what the packages we're unselecting and do we know that unlike `ca-certificates`, we don't need them? I've also checked with https://docs.docker.com/build/building/best-practices/#apt-get and they don't feature `--no-install-recommends` (but do add `&& rm -rf /var/lib/apt/lists/*` which we're missing).

---

_Comment by @zanieb on 2024-08-19 14:59_

I mean... as described the image works fine — you changed the base image and it failed. I agree we could do better though.

`--no-install-recommends` is included in Hadolint: https://github.com/hadolint/hadolint/wiki/DL3015

---

_Review requested from @zanieb by @konstin on 2024-08-19 16:31_

---

_Label `preview` added by @zanieb on 2024-08-19 17:14_

---

_Merged by @zanieb on 2024-08-19 17:14_

---

_Closed by @zanieb on 2024-08-19 17:14_

---

_Branch deleted on 2024-08-19 17:14_

---

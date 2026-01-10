```yaml
number: 238
title: Add docker builder
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: docker-builder
created_at: 2023-10-30T19:16:52Z
updated_at: 2023-11-02T11:03:57Z
url: https://github.com/astral-sh/uv/pull/238
synced_at: 2026-01-10T15:50:28Z
```

# Add docker builder

---

_Pull request opened by @konstin on 2023-10-30 19:16_

This docker container provides isolation of source distribution builds, whether [intended to be helpful](https://pypi.org/project/nvidia-pyindex/) or other more or less malicious forms of host system modification. 

Fixes #194

---

_@charliermarsh reviewed on 2023-10-30 19:54_

---

_Review comment by @charliermarsh on `.dockerignore`:2 on 2023-10-30 19:54_

Nit: mind adding newlines to all of these files? Otherwise they'll get changed when they're next edited.

---

_@charliermarsh reviewed on 2023-10-30 19:55_

How / when is this intended to be used? 

---

_@zanieb reviewed on 2023-10-30 20:10_

---

_Review comment by @zanieb on `builder.dockerfile`:12 on 2023-10-30 20:10_

Since we don't require any of the `ADD` magic we should use `COPY`
```suggestion
COPY rust-toolchain.toml rust-toolchain.toml
```

---

_Review comment by @zanieb on `builder.dockerfile`:17 on 2023-10-30 20:10_

Should we set pipefail here?

---

_@zanieb reviewed on 2023-10-30 20:10_

---

_@zanieb reviewed on 2023-10-30 20:15_

---

_Review comment by @zanieb on `builder.dockerfile`:6 on 2023-10-30 20:15_

General apt-get best practice in Dockerfiles; I don't feel strongly about the formatting although I generally like to separate dependencies on new lines
```suggestion
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3 \
        python3-pip \
        python3-venv \
        build-essential \
        make \
        autoconf \
        curl \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
```

---

_@zanieb reviewed on 2023-10-30 20:46_

---

_Review comment by @zanieb on `builder.dockerfile`:18 on 2023-10-30 20:46_

Just wondering, why modify `HOME`?

Should we use `WORKDIR`?

---

_@konstin reviewed on 2023-10-31 11:27_

---

_Review comment by @konstin on `builder.dockerfile`:18 on 2023-10-31 11:27_

I wanted absolute paths in `PATH` and needed a prefix; I can also set `WORKDIR` but i think we don't need to?

---

_@konstin reviewed on 2023-10-31 11:28_

---

_Review comment by @konstin on `.dockerignore`:2 on 2023-10-31 11:28_

Good idea, found the setting

![image](https://github.com/astral-sh/puffin/assets/6826232/a377374d-02ca-4855-af7d-973a21d8485d)


---

_Review comment by @konstin on `builder.dockerfile`:17 on 2023-10-31 11:31_

I wouldn't bother for a development container

---

_@konstin reviewed on 2023-10-31 11:31_

---

_Comment by @konstin on 2023-10-31 11:36_

> How / when is this intended to be used?

The primary usage is when building random packages to avoid them modifying your system (https://github.com/astral-sh/puffin/issues/194#issue-1963338852), the secondary usage is that we have a shared build environment to make e.g. failures reproducible and get the same compiler versions.

My personal usage is by integrating this into my IDE:

![image](https://github.com/astral-sh/puffin/assets/6826232/73288f2f-b8ee-4658-ac1e-a1a287ba61b7)

![image](https://github.com/astral-sh/puffin/assets/6826232/ea5b97d6-0d86-4048-9612-fb0d6365b57b)

---

_Comment by @zanieb on 2023-10-31 20:43_

Is there a way we can codify usage of this in a CONTRIBUTING guide? Perhaps in a simpler way than IDE integrations, like just a `docker run` command? Otherwise adding this to the repository doesn't quite make sense to me.

---

_Comment by @konstin on 2023-11-01 12:44_

Good idea, done

---

_Review comment by @konstin on `crates/puffin-dev/src/resolve_many.rs`:29 on 2023-11-01 12:44_



This is to have the example makes sense


---

_@konstin reviewed on 2023-11-01 12:44_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:5 on 2023-11-01 15:46_

```suggestion
Source distributions can run arbitrary code on build and can make unwanted modifications to your system (https://moyix.blogspot.com/2022/09/someones-been-messing-with-my-subnormals.html, https://pypi.org/project/nvidia-pyindex/), which can even occur when just resolving requirements. To prevent this, there's a Docker container you can run commands in:
```

---

_@zanieb reviewed on 2023-11-01 15:46_

---

_@zanieb approved on 2023-11-01 15:47_

Thanks!

---

_Merged by @konstin on 2023-11-02 11:03_

---

_Closed by @konstin on 2023-11-02 11:03_

---

_Branch deleted on 2023-11-02 11:03_

---

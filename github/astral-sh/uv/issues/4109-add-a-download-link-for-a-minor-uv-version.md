---
number: 4109
title: Add a download link for a minor uv version
type: issue
state: open
author: danielhollas
labels:
  - enhancement
  - installer
assignees: []
created_at: 2024-06-06T19:01:01Z
updated_at: 2025-08-30T07:35:21Z
url: https://github.com/astral-sh/uv/issues/4109
synced_at: 2026-01-07T13:12:17-06:00
---

# Add a download link for a minor uv version

---

_Issue opened by @danielhollas on 2024-06-06 19:01_

Right now when installing uv via URL, one can either install the latest version

```console
curl -LsSf https://astral.sh/uv/install.sh | sh
```

or a specific version

```console
curl -LsSf https://astral.sh/uv/0.2.9/install.sh | sh
```

Given that uv now has official version policy, it would be nice to be able to specify a major and minor version, without pinning the patch version. Something like:

```
curl -LsSf https://astral.sh/uv/0.2/install.sh | sh
```

---

_Label `installer` added by @charliermarsh on 2024-06-06 19:30_

---

_Comment by @zanieb on 2024-08-19 22:23_

(We should do this)

---

_Comment by @samypr100 on 2024-08-19 23:39_

This would be arguably nice to extend to docker tags too

---

_Comment by @zanieb on 2024-08-20 00:07_

Good point! I had some fun with this at my last job... https://github.com/PrefectHQ/prefect/blob/4da5da207b126639998917cb1e246f7b15484405/.github/workflows/docker-images.yaml#L110-L116

---

_Referenced in [astral-sh/uv#7042](../../astral-sh/uv/issues/7042.md) on 2024-09-04 18:46_

---

_Comment by @nikpivkin on 2024-12-12 06:27_

I found this command in the release `curl --proto ‘=https’ --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.5.8/uv-installer.sh | sh`. Why is it not documented [here](https://docs.astral.sh/uv/getting-started/installation/#standalone-installer)?

---

_Comment by @zanieb on 2024-12-12 14:09_

@nikpivkin that's documented at https://docs.astral.sh/uv/getting-started/installation/#github-releases

---

_Label `enhancement` added by @zanieb on 2025-01-08 18:38_

---

_Referenced in [astral-sh/uv#10404](../../astral-sh/uv/issues/10404.md) on 2025-01-08 18:38_

---

_Comment by @mynewestgitaccount on 2025-08-30 07:27_

Until this is added, does anyone know a way to install the latest patch in a minor version without going through pip?

Context: I am installing uv in an Azure Pipeline like `pip install uv~=0.x.0` right now, but I would like to avoid using pip if possible. Searching for alternatives is how I found this issue, actually. The proposed download link one-liner seems very natural.

---

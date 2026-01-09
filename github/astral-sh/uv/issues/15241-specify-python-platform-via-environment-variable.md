---
number: 15241
title: Specify Python platform via environment variable
type: issue
state: open
author: tlinhart
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2025-08-12T15:44:13Z
updated_at: 2025-08-12T17:24:27Z
url: https://github.com/astral-sh/uv/issues/15241
synced_at: 2026-01-07T13:12:19-06:00
---

# Specify Python platform via environment variable

---

_Issue opened by @tlinhart on 2025-08-12 15:44_

### Question

Hi,
is it possible to specify Python platform for `uv sync` via an environment variable? A lot of options is supported but I couldn't find a way to specify this one [in the documentation](https://docs.astral.sh/uv/reference/cli/#uv-sync--python-platform).

Use case: I use Pulumi IaC where the stack runs on ARM64 architecture on AWS in production. However, sometimes I need to provision the stack from local computer (x86_64 architecture) before deploying via GitHub Actions CI. I use `uv` toolchain with Pulumi but need it to install dependencies for the target architecture, e.g. when creating Lambda deployment packages or Docker images. AFAIK there's no way how I can supply command line options for `uv` when running Pulumi so env variable seems like the only option.

### Platform

Ubuntu 24.04.3 x86_64

### Version

uv 0.8.0

---

_Label `question` added by @tlinhart on 2025-08-12 15:44_

---

_Comment by @zanieb on 2025-08-12 17:10_

This seems okay to me. The use-case makes sense!

---

_Label `question` removed by @zanieb on 2025-08-12 17:10_

---

_Label `configuration` added by @zanieb on 2025-08-12 17:10_

---

_Label `good first issue` added by @zanieb on 2025-08-12 17:10_

---

_Comment by @zanieb on 2025-08-12 17:11_

We might want to bikeshed the name a little / if this is scoped to the top-level interface or affects `uv pip`?

You might want to also ask Pulumi to just expose this? It's a little dangerous for us to expose an option like this as a persistent setting.

---

_Comment by @tlinhart on 2025-08-12 17:21_

I'm not sure how `uv pip` is implemented, but if it's just a wrapper for `pip` then it already allows specifying the platform via `--platform` option. Or, using `platform` key under `install` section in `pip.conf` which is the way I did it when I used a `pip` toolchain with Pulumi.

When it comes to Pulumi, there might be a way to add another runtime option similar to e.g. `nodeargs` (see https://www.pulumi.com/docs/iac/concepts/projects/project-file/#runtime-options). However, I still find it useful to be able to specify it via an env variable directly to `uv`.

---

_Comment by @zanieb on 2025-08-12 17:24_

It's not a wrapper for `pip`.

We do support it via a flag https://docs.astral.sh/uv/reference/cli/#uv-pip-install--python-platform

---

_Referenced in [pulumi/pulumi#20277](../../pulumi/pulumi/issues/20277.md) on 2025-08-12 17:30_

---

_Referenced in [astral-sh/uv#15308](../../astral-sh/uv/pulls/15308.md) on 2025-08-15 13:26_

---

```yaml
number: 1977
title: "User Agent includes uv's version"
type: issue
state: closed
author: samypr100
labels:
  - enhancement
assignees: []
created_at: 2024-02-26T03:03:13Z
updated_at: 2024-03-04T19:48:42Z
url: https://github.com/astral-sh/uv/issues/1977
synced_at: 2026-01-10T05:40:32Z
```

# User Agent includes uv's version

---

_Issue opened by @samypr100 on 2024-02-26 03:03_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Currently when uv makes requests the User Agent is set to just `uv`.

It would be nice that it includes the version (e.g. `uv/{version}`) for improved visibility and statistics for the registry provider on the client being used.

For example, pip usually has `pip/{version} {other metadata}`, curl has `curl/{version}`, httpx has `httpx/{version}`, requests has `python-requests/{version}`, and so forth. Hence `uv/{version}` seems like the correct way to go.

---

_Comment by @samypr100 on 2024-02-26 03:04_

Upon looking at `uv-client`, I see that's where the User Agent seems to be set for all request across all packages, so there's likely a couple solutions to this depending on the approach since that's where the user agent is being set.

* Option 1: Expose a module in `uv-client` that allows to globally/static set the UA version (e.g. via OnceCell with getter/setter) and set the version once at the beginning in UV's main entrypoint function.

* Option 2: Create a new crate (e.g. `uv-version` or `uv-codex`) that has becomes the single source of truth for this type of data given cargo dependencies are unidirectional.

* Option 3: Change RegistryClientBuilder to take a UA version and modify all related interfaces (among all dependencies) to pass down uv's version across the codebase to RegistryClientBuilder. Not entirely sure of the complexity here (e.g. if there's only one client or multiple standalone clients across dependencies).

* Option 4: Maybe we could use cargo feature flags to hack around it, but that'd seems burdensome to maintain.

Option 1 seems the most straightforward, but if we are thinking of adding more data to be shared by other modules maybe Option 2 is best. If the type of data in the User Agent (or other request headers) end up being more dynamic over time, then Option 3 might be needed instead.

Thoughts here?

Maybe related note: pip itself seems to also include [other metadata](https://github.com/pypa/pip/blob/main/src/pip/_internal/network/session.py#L109) in JSON form in the User Agent header. Seems like the data described in https://github.com/astral-sh/uv/issues/1958?

---

_Label `enhancement` added by @charliermarsh on 2024-02-26 03:35_

---

_Comment by @charliermarsh on 2024-02-26 03:38_

I think if we _just_ wanted to include the version, something like (1) would be fine. Although, it's possible we can just use `env!("CARGO_PKG_VERSION")` to inline the version in `uv-client`, and not make _any_ other code changes?

If we want to support https://github.com/astral-sh/uv/issues/1958 though, we'll need a way to set the user agent based on the Python interpreter. We have that information in-memory already, but threading it through to the client will require some work.

---

_Comment by @samypr100 on 2024-02-26 03:45_

> Although, it's possible we can just use env!("CARGO_PKG_VERSION") to inline the version in uv-client, and not make any other code changes?

I was intitially thinking it would be ideal to be uv's version and not uv-client's version otherwise I think the header would be `uv/0.0.1` versus `uv/0.1.10` unless you're thinking of aligning the uv-* package versions the same as uv, which would also be a valid option here.

---

_Comment by @charliermarsh on 2024-02-26 20:46_

Oh yes, sorry, it should definitely be the `uv` version. (I was confused because we use `env!("CARGO_PKG_VERSION")` for the virtualenv, but that code is actually in the `uv` crate, not a layer below.)

---

_Closed by @charliermarsh on 2024-03-04 19:48_

---

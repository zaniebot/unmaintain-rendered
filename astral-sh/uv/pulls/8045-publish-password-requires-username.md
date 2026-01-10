```yaml
number: 8045
title: "Publish: Password requires username"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/password-requires-username
created_at: 2024-10-09T12:20:41Z
updated_at: 2024-10-15T12:18:10Z
url: https://github.com/astral-sh/uv/pull/8045
synced_at: 2026-01-10T12:54:01Z
```

# Publish: Password requires username

---

_Pull request opened by @konstin on 2024-10-09 12:20_

You can't use a password without a username in `uv publish`, which we can catch in clap directly.

New error when using only a password:
```console
$ uv publish --password dummy
error: the following required arguments were not provided:
  --username <USERNAME>

Usage: uv.exe publish --username <USERNAME> --password <PASSWORD> [FILES]...

For more information, try '--help'.
```

Fixes #8023

---

_Label `bug` added by @konstin on 2024-10-09 12:20_

---

_@charliermarsh approved on 2024-10-09 12:46_

This looks like a good change, but I'm still wondering if we can guide people to success here. Can we add a custom hint about tokens?

---

_Comment by @konstin on 2024-10-09 12:52_

What about adding it in the docstring of `--password`?

---

_Comment by @zanieb on 2024-10-09 13:34_

It would probably be best to include the hint in the error message, which would mean checking later manually instead of using Clap.

The short help won't show it if it's in the docstring.

---

_Comment by @charliermarsh on 2024-10-09 17:42_

Yeah can we just add a hint when we see `Failed to publish...` with a password but no username? You can even use Miette if you want to make it a bit easier to render the hint. See `crates/uv/src/commands/diagnostics.rs` for an example.

---

_Comment by @zanieb on 2024-10-09 18:31_

I think actually some indexes don't even require a username, just a password — this was an annoyance with `twine` in `packse` where I was forced to provide a dummy username — so we probably shouldn't enforce this until there's a failure?

---

_Comment by @konstin on 2024-10-10 07:07_

Do you have an example for such an index?

---

_Comment by @charliermarsh on 2024-10-10 09:07_

@konsti -- Is there any objection to adding a real hint to the error trace? Can we just do that?

---

_Merged by @konstin on 2024-10-15 08:01_

---

_Closed by @konstin on 2024-10-15 08:01_

---

_Branch deleted on 2024-10-15 08:01_

---

_Comment by @charliermarsh on 2024-10-15 11:53_

@konstin -- What about Zanie's concern above? I thought the conversation was veering towards only showing this when we hit a failure.

---

_@charliermarsh reviewed on 2024-10-15 11:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/publish.rs`:79 on 2024-10-15 11:56_

I think we should suggest `--user` and `--token`, and note the env vars since it's common in GHA:

> Attempted to publish with a password, but no username. Either provide a username with `--user` (`UV_PUBLISH_USERNAME`), or use `--token` (`UV_PUBLISH_TOKEN`) in lieu of a password.


---

_Comment by @konstin on 2024-10-15 11:56_

I thought the concern was that we should tell users about tokens?

We definitely can't support password-only without an example of an index and how that index would encode this in the HTTP message, the current serialization always requires a username.

---

_Comment by @charliermarsh on 2024-10-15 12:02_

Maybe @zanieb can provide an example then. If it's true then this could break users as-is.

---

_Comment by @konstin on 2024-10-15 12:13_

There may be other use cases we need to support, but it is not a regression due to: https://github.com/astral-sh/uv/blob/01c44af3c3ffc9f5e78ef4b415305350c029d1e0/crates/uv-publish/src/lib.rs#L541

---

_@konstin reviewed on 2024-10-15 12:18_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:79 on 2024-10-15 12:18_

This case only appears when you use only a password and not a username, despite token vs username/password are clearly documented in the pypi docs and our docs, and other indexes that i have tested providing clear guidance on username and password values. This is a fringe case and I'd rather spend our efforts on more impactful UX aspects, such as showing good messages after 403 errors, guiding users to the correct github actions trusted publishing settings, skip-existing/retries, etc. 

---

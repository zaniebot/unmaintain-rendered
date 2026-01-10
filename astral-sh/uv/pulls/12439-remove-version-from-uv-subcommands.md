```yaml
number: 12439
title: Remove --version from uv subcommands
type: pull_request
state: closed
author: jtfmumm
labels:
  - breaking
assignees: []
base: release/070
head: jtfm/subcommand-version
created_at: 2025-03-24T16:38:04Z
updated_at: 2025-06-01T11:19:19Z
url: https://github.com/astral-sh/uv/pull/12439
synced_at: 2026-01-10T11:10:40Z
```

# Remove --version from uv subcommands

---

_Pull request opened by @jtfmumm on 2025-03-24 16:38_

Currently, uv will return the uv version when `--version` is passed into subcommands, though it is displayed as follows:

```
$ uv run --version
uv-run 0.6.7 (029b9e1fc 2025-03-17)
```

We don't actually version subcommands separately and we may want to use the `--version` flag for other purposes (e.g., #12376). This PR removes this default version from all subcommands. `uv --version` still works.

Closes #12431

---

_Label `breaking` added by @zanieb on 2025-03-24 16:39_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-03-24 16:39_

---

_Comment by @inoa-jboliveira on 2025-03-25 21:31_

Hi, I think `--version` is quite important for uvx since it lacks `uvx version` and it can be installed standalone without uv.
Is it preserved here?

---

_Comment by @jtfmumm on 2025-03-25 22:03_

@inoa-jboliveira that’s a good point. I’ll update to cover that. 

---

_Comment by @zanieb on 2025-03-27 19:42_

Can we add a `uvx --version` to the smoke tests or something? (done in https://github.com/astral-sh/uv/pull/12516)

---

_Comment by @jtfmumm on 2025-03-28 14:31_

This change invalidates a couple of our old tests that checked that passing `--version` before the final command (e.g., `uv run --version python`) would output uv's version and not Python's. I've updated these tests to use `--help`, but this is testing something slightly different. `uv run --help python` outputs `uv run` help, not uv help. 

---

_Marked ready for review by @jtfmumm on 2025-04-01 06:46_

---

_@zanieb reviewed on 2025-04-15 21:58_

---

_Review comment by @zanieb on `crates/uv/tests/it/help.rs`:119 on 2025-04-15 21:58_

I feel like the old description was better? This feels like a minor regression

---

_Review comment by @jtfmumm on `crates/uv/tests/it/help.rs`:119 on 2025-04-25 15:28_

Unfortunately, it seems like you can't update the default version help message when using the clap macro approach. Do you think this regression is a reason to move this out of the release? 

---

_@jtfmumm reviewed on 2025-04-25 15:28_

---

_@zanieb reviewed on 2025-04-25 15:56_

---

_Review comment by @zanieb on `crates/uv/tests/it/help.rs`:119 on 2025-04-25 15:56_

Why do we need to use the clap macro? Can't we just make this flag only applicable at the root instead of global?

---

_Review comment by @zanieb on `crates/uv/tests/it/help.rs`:119 on 2025-04-26 00:40_

See https://github.com/astral-sh/uv/pull/13108

---

_@zanieb reviewed on 2025-04-26 00:40_

---

_Comment by @jtfmumm on 2025-04-28 07:07_

Closing in favor of #13108

---

_Closed by @jtfmumm on 2025-04-28 07:07_

---

_Branch deleted on 2025-06-01 11:19_

---

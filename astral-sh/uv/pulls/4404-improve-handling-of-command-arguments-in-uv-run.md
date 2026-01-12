```yaml
number: 4404
title: "Improve handling of command arguments in `uv run` and `uv tool run`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/run-command
created_at: 2024-06-18T20:55:20Z
updated_at: 2024-06-19T15:28:11Z
url: https://github.com/astral-sh/uv/pull/4404
synced_at: 2026-01-12T16:06:12Z
```

# Improve handling of command arguments in `uv run` and `uv tool run`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4390

We no longer require `--` to disambiguate child command options that overlap with uv options.

---

_Label `preview` added by @zanieb on 2024-06-18 20:55_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-18 20:59_

---

_Review requested from @konstin by @zanieb on 2024-06-18 21:01_

---

_@zanieb reviewed on 2024-06-18 21:02_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1465 on 2024-06-18 21:02_

Lifted from Rye.

---

_@zanieb reviewed on 2024-06-18 21:02_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1484 on 2024-06-18 21:02_

Makes life easier in the implementations, we special case the first argument.

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:8 on 2024-06-18 21:02_

Our first tool run tests!

---

_@zanieb reviewed on 2024-06-18 21:02_

---

_@zanieb reviewed on 2024-06-18 21:03_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:47 on 2024-06-18 21:03_

We output `uv-run ...` for `uv run --version`. Kind of annoying tbh but it's Clap's behavior I think so just going to filter it.

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:29 on 2024-06-18 21:04_

We split this into `target` and `args` late so we can avoid borrowing, cloning, or otherwise painfully popping the first item from the vector.

---

_@zanieb reviewed on 2024-06-18 21:04_

---

_Review comment by @BurntSushi on `crates/uv/src/cli.rs`:1479 on 2024-06-19 13:46_

It is odd to need to use `.deref()` explicitly. Does just `self.as_slice()` work?

---

_@BurntSushi reviewed on 2024-06-19 13:50_

Can you say at a high level how this works? Does this introduce any ambiguities?

---

_Comment by @zanieb on 2024-06-19 14:43_

> Can you say at a high level how this works? Does this introduce any ambiguities?

Honestly I'm not sure about the details, just that this is the standard flag for this purpose. My understanding is as follows though:

- Once we start parsing the external command we'll treat all the flags we see as options for that command
- If we add a `uv run` subcommand e.g. `uv run list` then `list` will be treated as a uv subcommand and we won't parse anything as an external flag
- Now, the only reason to use `--` is to disambiguate between an *external* subcommand and a *uv subcommand* but we don't have any right now

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1479 on 2024-06-19 14:43_

Probably just me being dumb, thanks!

---

_@zanieb reviewed on 2024-06-19 14:43_

---

_@BurntSushi approved on 2024-06-19 14:49_

---

_Review comment by @konstin on `crates/uv/src/cli.rs`:1484 on 2024-06-19 14:50_

That's implemented in std as `split_first`

---

_Merged by @zanieb on 2024-06-19 14:55_

---

_Closed by @zanieb on 2024-06-19 14:55_

---

_Branch deleted on 2024-06-19 14:55_

---

_Review comment by @konstin on `crates/uv/src/commands/tool/run.rs`:43 on 2024-06-19 15:00_

This can be `bail!`

---

_Review comment by @konstin on `crates/uv/src/commands/tool/run.rs`:51 on 2024-06-19 15:04_

This can be `.context("...")`, anyhow conveniently implements it on option too

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:450 on 2024-06-19 15:06_

We probably need to abstract those away soon, we're having them in a lot of subcommand test instantiators now

---

_@konstin approved on 2024-06-19 15:07_

---

_@zanieb reviewed on 2024-06-19 15:28_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:450 on 2024-06-19 15:28_

Agreed

---

_@zanieb reviewed on 2024-06-19 15:28_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1484 on 2024-06-19 15:28_

Ah thanks!

---

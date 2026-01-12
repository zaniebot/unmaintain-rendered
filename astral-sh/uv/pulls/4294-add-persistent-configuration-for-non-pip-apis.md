```yaml
number: 4294
title: "Add persistent configuration for non-`pip` APIs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: charlie/configuration
created_at: 2024-06-13T01:04:17Z
updated_at: 2024-06-14T00:56:39Z
url: https://github.com/astral-sh/uv/pull/4294
synced_at: 2026-01-12T16:06:08Z
```

# Add persistent configuration for non-`pip` APIs

---

_@charliermarsh_

## Summary

This PR introduces top-level configuration for uv, such that you can do:

```toml
[tool.uv]
index-url = "https://test.pypi.org/simple"
```

And `uv pip compile`, `uv run`, `uv tool run`, etc., will all respect that configuration.

The settings that were escalated to the top-level remain on `tool.uv.pip` too, but they're only respected in `uv pip` commands. If they're specified in both places, then the `pip` settings win out.

While making this change, I also wired up some of the global options, like `connectivity` and `native_tls`, through to all the relevant places.

Closes #4250.


---

_Label `configuration` added by @charliermarsh on 2024-06-13 01:04_

---

_Label `preview` added by @charliermarsh on 2024-06-13 01:04_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-13 01:11_

---

_@charliermarsh reviewed on 2024-06-13 01:17_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 01:17_

I used the following terminology:

- "Compile" or "Compiler" for something that _just_ does resolution.
- "Sync" or "Syncer" for something that _just_ does installation (from an existing resolution).
- "Install" or "Installer" for something that does both resolution and installer.

In `cli.r`, the `CompilerArgs` are used for (e.g.) `uv pip compile` and `uv lock`. The `SyncerArgs` are used for (e.g.) `uv pip sync` and `uv sync`. The `InstallerArgs` are used for `uv pip install` and `uv run` and `uv tool run`.

Internally, the Project and Tool APIs just take `InstallerSettings`, so the commands get some settings they don't need. But it was a bit easier that way.


---

_@charliermarsh reviewed on 2024-06-13 01:26_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:9471 on 2024-06-13 01:26_

Not really ideal, I think.


---

_@zanieb reviewed on 2024-06-13 13:58_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 13:58_

I find it really weird that "Installer" isn't the thing that does _just_ installation. What about:

- "Resolver": _just_ does resolution
- "Installer": _just_ does installation
- "Complete": for both

Really not sure about the third name but the other two are way more obvious?

As mentioned offline I also kind of like "Resolution" and "Installation".

---

_@zanieb reviewed on 2024-06-13 13:59_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 13:59_

I see the benefit of matching the command names, but I'm not sure it's quite worth it i.e. it falls apart with `uv run`.

---

_@charliermarsh reviewed on 2024-06-13 14:00_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 14:00_

I don't disagree but that's inconsistent with the `pip` API... The third name is what makes things really hard. I also considered `Operation` or something.


---

_@zanieb reviewed on 2024-06-13 14:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:287 on 2024-06-13 14:03_

Separate, but I wonder if we should have a `Settings::as_client_builder` or `BaseClientBuilder::from_settings`

---

_@zanieb reviewed on 2024-06-13 14:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:333 on 2024-06-13 14:04_

Just so it's on your mind as well as mine, I feel like `RegistryClientBuilder` should take a `BaseClientBuilder` instead of duplicating the global options? I found this awkward in some places where I needed to construct both types.

---

_@zanieb reviewed on 2024-06-13 14:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:05_

Are we going to use some of these in the future?

---

_@zanieb reviewed on 2024-06-13 14:08_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:1424 on 2024-06-13 14:08_

It feels a little weird that we do this conversion. Like... what if the method consuming `InstallerOptions` intends to respect what the user specified for a non-syncer option?

---

_@charliermarsh reviewed on 2024-06-13 14:09_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:09_

No, but I think we should be forced to think about it if we add new settings.

---

_@charliermarsh reviewed on 2024-06-13 14:09_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:1424 on 2024-06-13 14:09_

This is fixable but will require a few hundred more lines of boilerplate, probably.

---

_@zanieb reviewed on 2024-06-13 14:10_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:9471 on 2024-06-13 14:10_

It's a little awkward but at least it's consistent, maybe we should warn in that case? Are we going to have more complex merging rules for the pip namespace from the user-level config?

---

_@zanieb reviewed on 2024-06-13 14:11_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 14:11_

I'd rather focus on internal consistency than consistency with the `pip` command names, I think.

Aren't you often taking the resolution and installation options and constructing the third "full" options from them anyway?

---

_@charliermarsh reviewed on 2024-06-13 14:12_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:1424 on 2024-06-13 14:12_

I will fix it. It requires that we change (e.g.) `pip sync` to take a new `SyncerSettings` instead of `InstallerSettings`, so we need corresponding structs for `Syncer` and `Compiler` (I know, we'll rename those). And then we need conversion methods.


---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:12_

I feel like a lot of these actually apply though? Like don't we care about the index locations and strategy here? e.g. if we need to install a requested package

---

_@zanieb reviewed on 2024-06-13 14:12_

---

_@zanieb reviewed on 2024-06-13 14:14_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:1424 on 2024-06-13 14:14_

Okay. I'm not saying it _has_ to change but it does seem like a possible footgun.

---

_@charliermarsh reviewed on 2024-06-13 14:14_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:14_

Where? This is just for `Toolchain::find_or_fetch`.

---

_@charliermarsh reviewed on 2024-06-13 14:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:15_

We could omit `keyring_provider` entirely if you want, since we know the URLs that will be fetched here.

---

_@charliermarsh reviewed on 2024-06-13 14:16_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 14:16_

> Aren't you often taking the resolution and installation options and constructing the third "full" options from them anyway?

I think that's going to disappear though based on https://github.com/astral-sh/uv/pull/4294#discussion_r1638293555.

---

_@zanieb reviewed on 2024-06-13 14:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:16_

Oh sorry I didn't realize that was the context. I wouldn't use the keyring for `find_or_fetch`.

---

_@zanieb reviewed on 2024-06-13 14:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:17_

Thanks for clarifying.

---

_@charliermarsh reviewed on 2024-06-13 14:17_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 14:17_

I'll do whatever. I can just run with "Complete" for now.


---

_@charliermarsh reviewed on 2024-06-13 14:18_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:124 on 2024-06-13 14:18_

Kk will remove!

---

_@charliermarsh reviewed on 2024-06-13 14:18_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:1424 on 2024-06-13 14:18_

I think it's "correct" to split them out into dedicated types, it's just so much boilerplate. But it is correct.

---

_@zanieb reviewed on 2024-06-13 14:18_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 14:18_

I'm guessing there's a lot of overlap between installation and resolution and we shouldn't just do `Third(InstallationOptions, ResolutionOptions)`?

---

_@zanieb reviewed on 2024-06-13 14:19_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 14:19_

If only there was an obvious third name.

---

_@charliermarsh reviewed on 2024-06-13 14:22_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 14:22_

Yeah they mostly overlap but critically there are a few options that don't make sense for one vs. the other and I think exposing them on the CLI would be bad (like `uv lock` shouldn't have `--compile-bytecode`).

---

_Comment by @charliermarsh on 2024-06-13 14:59_

@zanieb - Are you cool with this modulo (1) renaming, and (2) removing the `From<SyncerArgs> for InstallerOptions` impls?

---

_Comment by @zanieb on 2024-06-13 15:25_

> @zanieb - Are you cool with this modulo (1) renaming, and (2) removing the `From<SyncerArgs> for InstallerOptions` impls?

I think so! It seems pretty reasonable.

---

_@charliermarsh reviewed on 2024-06-13 18:51_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:68 on 2024-06-13 18:51_

Ok, I changed it to `Installer`, `Resolver`, and `Complete`.

---

_Comment by @charliermarsh on 2024-06-13 18:58_

Okay, I believe I addressed the biggest feedback. I'm saddened by how much boilerplate will be required to add a new setting in the future. Perhaps we can DRY a lot of this up with macros in the future, but it's not my strength.

---

_@charliermarsh reviewed on 2024-06-13 22:56_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:333 on 2024-06-13 22:56_

Makes sense, I can look at it at some point.

---

_@zanieb approved on 2024-06-14 00:31_

Thank you!

---

_Merged by @charliermarsh on 2024-06-14 00:56_

---

_Closed by @charliermarsh on 2024-06-14 00:56_

---

_Branch deleted on 2024-06-14 00:56_

---

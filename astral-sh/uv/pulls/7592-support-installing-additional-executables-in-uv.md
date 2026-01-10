```yaml
number: 7592
title: "Support installing additional executables in `uv tool install`"
type: pull_request
state: closed
author: lucab
labels:
  - enhancement
  - uv tool
assignees: []
base: main
head: ups/tool-install-additional-exes
created_at: 2024-09-20T17:04:46Z
updated_at: 2025-07-30T19:50:46Z
url: https://github.com/astral-sh/uv/pull/7592
synced_at: 2026-01-10T06:53:01Z
```

# Support installing additional executables in `uv tool install`

---

_Pull request opened by @lucab on 2024-09-20 17:04_

This enhances `uv tool install` in order to allow installing executables from multiple packages in a single call.

Closes https://github.com/astral-sh/uv/issues/6314

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3276 on 2024-09-20 17:13_

Nit: Should be `Vec<PackageName>`

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3086 on 2024-09-20 17:19_

Funny, suggests we probably want to switch to a list display at some point?

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3086 on 2024-09-20 17:19_

Why is it 24 uninstalled but 11 + 1 + 1 installed?

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3058 on 2024-09-20 17:20_

We'll probably want to collapse these and/or include the "from" package

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3029 on 2024-09-20 17:21_

Minor note: This is probably an expensive test because the package is big. We might want to find a smaller package to keep the test case minimal.

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:880 on 2024-09-20 17:21_

```suggestion
            // Synthesize extra dependencies by merging all additionally
```

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3060 on 2024-09-20 17:23_

We'll definitely want to snapshot the receipt here.

---

_@zanieb reviewed on 2024-09-20 17:23_

Nice! This seems like a great start.

---

_@lucab reviewed on 2024-09-20 17:40_

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:3086 on 2024-09-20 17:40_

I think it's a bug due to double counting the `ansible-core` executables.
Not exactly sure where that is coming from, but I think it may be due to the fact that the package appears both as an implicit dependency and as a manually specified package.

---

_Label `enhancement` added by @zanieb on 2024-09-20 17:50_

---

_Label `uv tool` added by @zanieb on 2024-09-20 17:50_

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:3086 on 2024-09-24 10:58_

Nevermind, this was a bug in my new code, fixed now.

---

_@lucab reviewed on 2024-09-24 10:58_

---

_@lucab reviewed on 2024-09-24 11:00_

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:3058 on 2024-09-24 11:00_

Do you have a preference on how this should look like?
For the moment I've updated it to:
```
Installed 11 additional executables from `ansible-core`: ansible, ansible-config, ansible-connection, ansible-console, ansible-doc, ansible-galaxy, ansible-inventory, ansible-playbook, ansible-pull, ansible-test, ansible-vault
Installed 1 additional executable from `ansible-lint`: ansible-lint
Installed 1 executable: ansible-community
```

I'm absolutely not sold on this being the final form.

---

_@zanieb reviewed on 2024-09-24 13:09_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3058 on 2024-09-24 13:09_

The part that stands out to me there is that it's a bit weird to say "additional" _before_ we show the main group of executables we installed.

I think I'd go with whatever is decent and simple here, then revisit the output entirely. For example, I think this should probably match the style of our install commands? Like:

```
Installed 13 executables
+ ansible (from ansible-core)
+ ansible-config (from ansible-core)
+ ansible-lint
+ ansible-community
...
```

If the package name matches the executable name I'd probably omit the "from" portion.

Anyway, I think that is separate from the goal here and we should avoid going down that road in this pull request.

---

_@lucab reviewed on 2024-09-24 13:21_

---

_Review comment by @lucab on `crates/uv-cli/src/lib.rs`:3276 on 2024-09-24 13:21_

Just double-checking: this field is the CLI args, other CLI fields also seems to be simpler type. Were you maybe suggesting to make this change on the `ToolInstallSettings` field instead? I think it makes sense to turn that one into a `BTreeSet<PackageName>` maybe, so that it is both de-duplicated and strongly typed.

---

_@zanieb reviewed on 2024-09-24 13:29_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3276 on 2024-09-24 13:29_

We use `PackageName` in the CLI where we can e.g. https://github.com/astral-sh/uv/blob/63b60bc0c8425f4475adbd0ba207159694946a08/crates/uv-cli/src/lib.rs#L1878

I'm all for converting it to a `BTreeSet` later too.

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:3056 on 2024-09-25 10:48_

There is a funny issue here, it seems that on Windows only there is a different sorting for the strings `ansible` and `ansible-<whatever>`:
```diff
- Installed 11 executables from `ansible-core`: ansible, ansible-config, ansible-connection, ansible-console, ansible-doc, ansible-galaxy, ansible-inventory, ansible-playbook, ansible-pull, ansible-test, ansible-vault
+ Installed 11 executables from `ansible-core`: ansible-config, ansible-connection, ansible-console, ansible-doc, ansible-galaxy, ansible-inventory, ansible-playbook, ansible-pull, ansible-test, ansible-vault, ansible
```

---

_@lucab reviewed on 2024-09-25 10:48_

---

_@lucab reviewed on 2024-09-25 12:35_

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:3029 on 2024-09-25 12:35_

Many dependencies were coming from `ansible-lint`, which is an extra package providing an additional entrypoint. I replaced that with `black`, as it covers the same scenario with fewer transitive deps.

If you have an idea about a better combo than `ansible` and `ansible-core`, I'll happily replace them here.

---

_@zanieb reviewed on 2024-09-27 15:33_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3056 on 2024-09-27 15:33_

That's horrible :(

---

_@lucab reviewed on 2024-09-27 15:47_

---

_Review comment by @lucab on `crates/uv/tests/tool_install.rs`:3056 on 2024-09-27 15:47_

I had to abuse CI a bit to get to the bottom of this (sorry if I flooded you with notifications).
It turns out there is a `.exe` suffix (filtered out in test output) and thus `ansible < ansible-config(.exe) < ansible.exe` (due to the ASCII order of `-` vs `.`).
The same suffix is also present in the receipt, making its ordering OS-dependant.

---

_@zanieb reviewed on 2024-09-27 16:02_

---

_Review comment by @zanieb on `crates/uv/tests/tool_install.rs`:3056 on 2024-09-27 16:02_

I'm used to Charlie force-pushing 1000 times per pull request so no worries.

Ah that's really interesting. Good sleuthing. I was about to suggest trimming the suffix for sorts but it looks like you're on it in https://github.com/astral-sh/uv/pull/7592/commits/0c9f450e1ccde874130e5614b410661787581044 — should you use the `std::env::consts::EXE_SUFFIX` const there?

The filtering can be quite misleading like that sometimes. I wonder if we can output an unfiltered snapshot on failure too? cc @konstin the snapshot macro master.

---

_Renamed from "[WIP] Support installing additional executables in `uv tool install`" to "Support installing additional executables in `uv tool install`" by @lucab on 2024-09-30 14:04_

---

_Comment by @lucab on 2024-09-30 14:13_

I think this should be in a complete state now, and there are functional tests for install/upgrade/uninstall flows.

The main remaining thing to decide is the name of the flag, in order to finalize the PR.
My personal suggestion would be `--with-commands-from`, as it blends with existing nomenclature.
But I'm happy to go with whatever @zanieb or @charliermarsh actually prefer, just let me know here.

---

_Assigned to @zanieb by @zanieb on 2024-09-30 14:15_

---

_Marked ready for review by @lucab on 2024-10-03 09:23_

---

_Comment by @zanieb on 2024-10-03 23:45_

Thanks! I'll try to review this tomorrow.

edit: I had some time to give it a quick look now

---

_@zanieb reviewed on 2024-10-03 23:59_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3356 on 2024-10-03 23:59_

I noticed you're following "install commands" from the other docstrings but we say "executables" elsehwere in the documentation and I think I prefer that i.e.

```suggestion
    /// Install executables from an additional package.
    ///
    /// May be provided multiple times.
    #[arg(long)]
    pub with_executables_from: Vec<PackageName>,
```

I can change the other option documentation to match.

---

_@zanieb reviewed on 2024-10-04 00:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:413 on 2024-10-04 00:01_

Why do we force install if there are additional packages?

---

_@zanieb reviewed on 2024-10-04 00:02_

---

_Review comment by @zanieb on `crates/uv-tool/src/tool.rs`:87 on 2024-10-04 00:02_

Will adding this break old tool installs, i.e., if it's missing?

---

_Comment by @gaborbernat on 2025-03-26 20:01_

Anything is left here to do? from the latest review seems to be implied that it's ready.

---

_Comment by @zanieb on 2025-03-26 20:07_

Yeah there's still work to do here. i.e., https://github.com/astral-sh/uv/pull/7592#discussion_r1786955527 is important. I'd have to go deeper on the topic again to see if there's more. Are you interested in picking it up?

---

_@aaron-ang reviewed on 2025-06-13 06:07_

---

_Review comment by @aaron-ang on `crates/uv-tool/src/tool.rs`:87 on 2025-06-13 06:07_

The only other location `from` is used is in `upgrade.rs` to collect dependency packages (excluding the main tool) that have entry points. If `from` is `None`, then only entry points from site packages are installed in `install_executables`.

---

_Comment by @mykelalvis on 2025-07-16 12:50_

Bumping this as "very nice to have".  If there is testing to perform, I'm willing to help out, but my knowledge of this codebase is low and my available time is limited.

---

_Closed by @zanieb on 2025-07-30 19:50_

---

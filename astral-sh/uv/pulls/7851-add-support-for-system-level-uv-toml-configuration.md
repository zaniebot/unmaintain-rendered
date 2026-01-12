```yaml
number: 7851
title: "Add support for system-level `uv.toml` configuration"
type: pull_request
state: merged
author: amjith
labels:
  - configuration
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-01T17:40:14Z
updated_at: 2024-10-30T12:43:56Z
url: https://github.com/astral-sh/uv/pull/7851
synced_at: 2026-01-12T16:08:02Z
```

# Add support for system-level `uv.toml` configuration

---

_@amjith_

## Summary

Look for a system level uv.toml config file under `/etc/uv/uv.toml` or `C:\ProgramData`. 

This PR is to address #6742 and start a conversation. 

## Test Plan

This was tested locally manually on MacOS. I am happy to contribute tests once we settle on the approach. 

cc @thatch 


---

_Comment by @zanieb on 2024-10-01 17:48_

Thanks for contributing :) Why not use the XDG defaults?

I'm not sure if we'll need to address https://github.com/astral-sh/uv/issues/6742#issuecomment-2383897018

---

_Comment by @thatch on 2024-10-01 18:21_

The code change to follow XDG isn't all that complex (split on colon, take first matching one), but I'm a fan of simpler docs as a metric of measuring whether you're writing your best code.  The use of `.config` on mac instead of `Application Support` seemed to be an indicator in this direction, which is why the first attempt is this way.  If this needs a try 2 because XDG gives value, would like some advice on what the `Err` ought to look like when a dir (not the last one) doesn't exist, or if that promotion to a more specific `Err` in `user()` isn't really necessary here.

---

_@charliermarsh approved on 2024-10-02 11:19_

The code looks good to me, assuming we're happy with the paths! I'll defer to @zanieb on the choice of path.

---

_Label `configuration` added by @charliermarsh on 2024-10-02 11:19_

---

_Comment by @charliermarsh on 2024-10-02 11:19_

\cc @pradyunsg

---

_Comment by @amjith on 2024-10-02 13:27_

> Why not use the XDG defaults?

From reading the XDG docs it seemed like `/etc/xdg/uv/uv.toml` is the path in linux and MacOS doesn't really have an equivalent but we could use `~/Library/Preferences /Library/Application Support /Library/Preferences`. 

I decided to go with a stable location that is shorter and the same between Linux and MacOS. I'm happy to check for the env var and use that if it exists and fallback to `/etc/uv/uv.toml` if it doesn't exist. 

Let me know your preference and I can adjust this accordingly. 

Testing:
I'm new to rust hence not too familiar with the testing story. Are there relevant tests I should update or add? 

---

_Assigned to @zanieb by @zanieb on 2024-10-02 15:18_

---

_Comment by @charliermarsh on 2024-10-12 15:11_

> Why not use the XDG defaults?

What _are_ the XDG defaults for this?

---

_Comment by @zanieb on 2024-10-12 15:21_

> $XDG_CONFIG_DIRS defines the preference-ordered set of base directories to search for configuration files in addition to the $XDG_CONFIG_HOME base directory. The directories in $XDG_CONFIG_DIRS should be separated with a colon ':'.
>
> If $XDG_CONFIG_DIRS is either not set or empty, a value equal to `/etc/xdg` should be used.

---

_Comment by @zanieb on 2024-10-12 15:24_

Basically, at the very least, we need to respect `XDG_CONFIG_DIRS` and look in all of the directories given there. If not set, we should also look in `/etc/xdg`. I'm down to read `/etc/uv` in addition to all of the XDG paths, but we need to decide if it's lower or higher priority than the XDG ones. I think we should do the same thing on macOS as we do on Linux.

---

_Comment by @charliermarsh on 2024-10-12 21:09_

Okay cool, makes sense. How's this for a proposal:

1. If `XDG_CONFIG_DIRS` is defined and non-empty, check each `${DIR}/uv/uv.toml`.
2. If it's not defined or empty, check `/etc/xdg/uv/uv.toml`.
3. Check `/etc/uv/uv.toml`.

For rationale, we store in `/etc/uv/uv.toml` rather than `/etc/uv.toml` based on ([ref](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03s07.html)):

> It is recommended that files be stored in subdirectories of /etc rather than directly in /etc.

(This is actually counter to what pip and npm do, but oh well.)

Additionally, we check `/etc/uv/uv.toml` _last_ (rather than first) so that users can override configuration by adding to `XDG_CONFIG_DIRS`, rather than `/etc/uv/uv.toml` always taking precedent (rare, but we have to decide one way or the other).

---

How does this sound? @amjith -- are you interested in implementing these changes?


---

_Comment by @amjith on 2024-10-16 20:57_

Yes. I'm happy to do that. I'll take a stab at it this Friday. 

---

_Comment by @zanieb on 2024-10-16 20:59_

And we combine each config we find?

---

_Comment by @amjith on 2024-10-16 21:51_

Should it combine them all or should it stop at the first one it finds?

---

_Comment by @amjith on 2024-10-16 21:52_

The current behavior is to combine project + system + user. If we choose to combine config from `XDG_CONFIG_DIRS` and `/etc/uv/uv.toml` and call that the system config that could get confusing. Especially since `XDG_CONFIG_DIRS` can have multiple folders and each one could have a uv.toml file. 

---

_Comment by @charliermarsh on 2024-10-16 22:07_

I'd probably assume that we only take the first one.

---

_Comment by @zanieb on 2024-10-16 22:08_

XDG says you can do either :)

> A specification that refers to $XDG_DATA_DIRS or $XDG_CONFIG_DIRS should define what the behaviour must be when a file is located under multiple base directories. It could, for example, define that only the file under the most important base directory should be used or, as another example, it could define rules for merging the information from the different files.

We should probably choose one. Not merging seems simpler, I agree. Are there other tools that respect these that we can refer to though? 

---

_@thatch reviewed on 2024-10-18 19:29_

---

_Review comment by @thatch on `crates/uv-settings/src/lib.rs`:204 on 2024-10-18 19:29_

I went to re-read the spec, which says

> If $XDG_CONFIG_DIRS is either not set or empty, a value equal to /etc/xdg should be used.

I think this means don't fall back to /etc/xdg if the env var contains even just `:`

If that's true you should handle the "/etc/xdg" default here and not in `system_config_file()`.  To make things easier you could pass in the env var (so you don't have to set it in the test), since this is only used internally.


---

_@thatch reviewed on 2024-10-18 19:40_

---

_Review comment by @thatch on `crates/uv-settings/src/lib.rs`:63 on 2024-10-18 19:40_

You can use the same pattern here as from line 37, to save some indent by putting the None up top.

---

_Review comment by @thatch on `crates/uv-settings/src/lib.rs`:64 on 2024-10-18 19:42_

Now that the 3rd case (not a dir) is removed, you can just `let options = read_file(&file)?` I think.

See https://doc.rust-lang.org/beta/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator

---

_@thatch reviewed on 2024-10-18 19:42_

---

_Review comment by @thatch on `crates/uv/src/lib.rs`:131 on 2024-10-18 19:46_

Can you double-check the order of the `combine` calls?  It looks like `project.combine(user)` is in decreasing order, so I'd expect `system` to come last?

---

_@thatch reviewed on 2024-10-18 19:46_

---

_@thatch reviewed on 2024-10-18 19:54_

---

_Review comment by @thatch on `crates/uv-settings/src/lib.rs`:272 on 2024-10-18 19:54_

fixtureS

---

_Comment by @amjith on 2024-10-18 22:50_

I'm happy with the code changes in the PR. Let me know your feedback. 

Unfortunately the compilation fails in windows and I could use a hint on how to resolve that. 

---

_Comment by @amjith on 2024-10-19 03:52_

I'm stuck on the Windows test failing. I don't have a windows laptop to try this out and somehow the env vars in Windows aren't working as I assumed it would. 

If you you folks have any suggestions, I'm all ears. 

---

_Comment by @charliermarsh on 2024-10-19 14:26_

No worries — I can take a look tomorrow (when I’m back home) on my Windows machine. 

---

_@Dominik50111 approved on 2024-10-19 22:47_

---

_@charliermarsh reviewed on 2024-10-20 23:00_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:204 on 2024-10-20 23:00_

Does `env::split_paths` work here?

---

_@charliermarsh reviewed on 2024-10-20 23:30_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:204 on 2024-10-20 23:30_

Empirically, appears not!

---

_Comment by @charliermarsh on 2024-10-20 23:50_

I think the Windows test is failing because you're expecting a `ProgramData` child that doesn't exist in the fixture. I'm gonna refactor the fixtures slightly to write the files within the tests themselves.

---

_@zanieb reviewed on 2024-10-20 23:59_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:204 on 2024-10-20 23:59_

Really? That's pretty surprising.

---

_@charliermarsh reviewed on 2024-10-21 00:04_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:204 on 2024-10-21 00:04_

Yeah. It returns `""` in the iterator, which then resolves to the current working directory.

---

_@charliermarsh reviewed on 2024-10-21 00:06_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:204 on 2024-10-21 00:06_

(We could filter empty string I guess.)

---

_Comment by @charliermarsh on 2024-10-21 00:11_

Should we be using `%ProgramData%` rather than `${SYSTEMDRIVE}/ProgramData`?

---

_Comment by @charliermarsh on 2024-10-21 00:34_

This looks good to me -- tests are passing, and I added some docs. I'll give @zanieb some time to review too though before merging.

---

_Comment by @charliermarsh on 2024-10-21 00:35_

I sort of think we should use `SHGetKnownFolderPath(FOLDERID_ProgramData)` instead of the current approach on Windows, but I may do it as a separate change and it's not blocking.

---

_Review requested from @zanieb by @charliermarsh on 2024-10-21 00:35_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:204 on 2024-10-21 13:37_

Oh interesting. Yeah I guess it seems more correct to handle the empty string separately. I don't feel strongly since it's already platform-specific code though.

---

_@zanieb reviewed on 2024-10-21 13:37_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:217 on 2024-10-21 14:39_

It seems like this could use a little touchup for readability.

---

_@zanieb reviewed on 2024-10-21 14:39_

---

_@zanieb reviewed on 2024-10-21 14:41_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:291 on 2024-10-21 14:41_

Why is this in a block?

---

_@zanieb reviewed on 2024-10-21 14:42_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:304 on 2024-10-21 14:42_

This isn't thread-safe and our tests run concurrently.

Unfortunately this isn't easy to solve. In the Python discovery tests, we use this pattern:

https://github.com/astral-sh/uv/blob/01c44af3c3ffc9f5e78ef4b415305350c029d1e0/crates/uv-python/src/lib.rs#L44-L55

https://github.com/astral-sh/uv/blob/01c44af3c3ffc9f5e78ef4b415305350c029d1e0/crates/uv-python/src/tests.rs#L78-L105

---

_@zanieb reviewed on 2024-10-21 14:42_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:348 on 2024-10-21 14:42_

Same as https://github.com/astral-sh/uv/pull/7851/files#r1808963874

---

_@zanieb reviewed on 2024-10-21 14:44_

---

_Review comment by @zanieb on `docs/configuration/files.md`:44 on 2024-10-21 14:44_

We should clarify that we'll take the first system-level configuration file we find, not merge them all.

---

_@zanieb reviewed on 2024-10-21 14:44_

---

_Review comment by @zanieb on `docs/configuration/files.md`:44 on 2024-10-21 14:44_

Maybe below in the merging paragraph?

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:304 on 2024-10-21 14:44_

Why do we even need this? The tests pass without changing the directory.

---

_@charliermarsh reviewed on 2024-10-21 14:44_

---

_@charliermarsh reviewed on 2024-10-21 14:45_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:291 on 2024-10-21 14:45_

To clarify the scope.

---

_@charliermarsh reviewed on 2024-10-21 14:45_

---

_Review comment by @charliermarsh on `docs/configuration/files.md`:44 on 2024-10-21 14:45_

Feel free to push edits if you'd like, otherwise I can get to them later.

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:348 on 2024-10-21 14:46_

This is different than the above. The above is changing the current directory. This is changing an env var. (The same critiques may apply, I just wanted to point it out.)

---

_@charliermarsh reviewed on 2024-10-21 14:46_

---

_@zanieb reviewed on 2024-10-21 14:48_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:304 on 2024-10-21 14:48_

I'm not sure. Seems like we don't then? I don't know why we'd read the current directory for this? Maybe if you have `XDG_CONFIG_PATHS=foo::bar` and we parse something relative to the working directory? Seems weird.

---

_@zanieb reviewed on 2024-10-21 14:49_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:348 on 2024-10-21 14:49_

Yeah thanks for clarifying, the same problem is true unfortunately. If there's another test that reads `SYSTEMDRIVE` we can encounter a race condition and the tests will be flaky. The solution is to use the `run_vars` utility, you don't need the `current_dir` from `PWD` hack.

---

_@charliermarsh reviewed on 2024-10-21 16:03_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/lib.rs`:348 on 2024-10-21 16:03_

Okay cool, got it!

---

_@zanieb reviewed on 2024-10-21 16:17_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:348 on 2024-10-21 16:17_

Unfortunately I think this may still be a problem even if you use `with_vars` because [temp-env](https://crates.io/crates/temp-env) creates a mutex to prevent concurrent conflicts between variables but if we're not using `temp-env` elsewhere we can still end up mutating their state. This isn't a big problem now because there aren't tests that rely on reading the environment that we're mutating in the Python discovery tests. The same is true of this test now, but it may not be true forever.

---

_@zanieb reviewed on 2024-10-21 16:18_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:291 on 2024-10-21 16:18_

The scope of changing the current directory? I think it's misleading because it's not like `set_current_dir` reverts when the scope exits.

---

_Comment by @charliermarsh on 2024-10-21 16:20_

I'll tweak and merge this today.

---

_Renamed from "Add system level uv.toml config." to "Add support for system-level `uv.toml` configuration" by @charliermarsh on 2024-10-21 17:22_

---

_Merged by @charliermarsh on 2024-10-21 17:33_

---

_Closed by @charliermarsh on 2024-10-21 17:33_

---

_Comment by @charliermarsh on 2024-10-21 17:33_

Thanks @amjith and @thatch!

---

_Comment by @gaby on 2024-10-22 14:22_

@charliermarsh Is this in the latest release or the next one?

Thanks for this, looking forward to automating `uv` using ansible

---

_Comment by @charliermarsh on 2024-10-22 14:25_

It's not released yet, maybe today :)

---

_Comment by @wimglenn on 2024-10-30 03:32_

sysadmin note - if you arrived here looking for system-level uv config in /etc, the minimum uv version needed is [0.4.26](https://github.com/astral-sh/uv/releases/tag/0.4.26)

---

_Comment by @gaby on 2024-10-30 11:35_

@wimglenn Great start but its missing all the ENV fields from uv. There's no way to setup a proxy via uv.toml either

---

_Comment by @charliermarsh on 2024-10-30 12:43_

@gaby -- Can I kindly request that you refrain from reposting that comment across multiple issues / PRs? You've mentioned it in a few places already, and it's best tracked elsewhere. Thanks.

---

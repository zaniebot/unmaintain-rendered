```yaml
number: 7959
title: "Warn when patch version in `requires-python` is implicitly `0`"
type: pull_request
state: merged
author: ianpaul10
labels: []
assignees: []
merged: true
base: main
head: feat/warn-on-pin-py-ver
created_at: 2024-10-07T06:35:01Z
updated_at: 2024-10-16T04:21:26Z
url: https://github.com/astral-sh/uv/pull/7959
synced_at: 2026-01-12T16:08:06Z
```

# Warn when patch version in `requires-python` is implicitly `0`

---

_@ianpaul10_

When patch version isn't specified and a matching version is referenced, it will default patch to 0 which could be unclear/confusing. This PR warns the user of that default.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The first part of this issue https://github.com/astral-sh/uv/issues/7426. Will tackle the second part mentioned (`~=`) in a separate PR once I know this is the correct way to warn users.

## Test Plan

<!-- How was it tested? -->

Unit tests were added

---

_Assigned to @zanieb by @zanieb on 2024-10-08 18:14_

---

_Comment by @zanieb on 2024-10-08 19:00_

I'll need to take a closer look, but I was originally thinking we'd only do this for `requires-python` and that we'd enforce it when we read that value. It'd be nice to add it for other requirements too, but I think we could add those incrementally. If you implement the warning here, there's no way to describe the associated package / field we've read the version from which seems important for having a good message.

You'll also want to use `warn_user_once!`.  

Does that make sense?

cc @BurntSushi would you be interested in being a primary reviewer for this one?

---

_Comment by @ianpaul10 on 2024-10-09 09:22_

Ah yes that makes sense, my mistake. So by "enforce it when we read that value", do you mean check it somewhere in [requires_python.rs](https://github.com/ianpaul10/uv/blob/feat/warn-on-pin-py-ver/crates/uv-resolver/src/requires_python.rs)? I couldn't see exactly where it's being read for a given project.

---

_Review requested from @BurntSushi by @BurntSushi on 2024-10-09 13:36_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-10-09 13:36_

---

_Unassigned @zanieb by @BurntSushi on 2024-10-09 13:36_

---

_Comment by @BurntSushi on 2024-10-10 16:44_

@ianpaul10 I don't think we want to do it at a super low level like on `RequiresPython` itself. It's hard to say exactly when that will get used and whether a warning in all such cases would be appropriate. It's probably better to go higher level. I can see two different spots for this. One is every time we load a pyproject.toml:

https://github.com/astral-sh/uv/blob/7b80b1816693f7d30b0614206ac9447fafbbea35/crates/uv-workspace/src/pyproject.rs#L56-L70

Another spot is only when the user runs `uv lock`, which is also where we have some other warnings related to `requires-python`:

https://github.com/astral-sh/uv/blob/7b80b1816693f7d30b0614206ac9447fafbbea35/crates/uv/src/commands/project/lock.rs#L332-L346

I think I would go the latter option. And if you add the warning there, it should show up when running `uv sync` as well, automatically (among any other commands that try and do a lock). Otherwise, we'd be issuing this warning for almost all `uv` commands, and I'm not sure we want to go that route.

---

_Comment by @ianpaul10 on 2024-10-11 08:40_

Thanks a ton for the in-depth comment. That all makes sense, I didn't really know the best place to put it so that colour is helpful. I took your advice and added it into the lock.rs file.

Let me know if that's different than what you had in mind, and if the tests I included are appropriate.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:804 on 2024-10-11 12:16_

Can you move this to the group of non-std crate-external imports below? With `uv_pep440` and `uv_distribution_filename`.

The general pattern in the ecosystem is to put imports in three groups, in this order: std imports, non-`crate` imports, `crate` imports. (I also usually separate `core` and `alloc` imports too, but that's only for `no_std` libraries.)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:924 on 2024-10-11 12:20_

I know we're using this macro in at least one other place too, but I'd skip on this and just write out the code instead.

```rust
#[test]
fn is_matching_without_patch() {
    let test_cases = [
        ("==3.12", true),
        ("==2.7", true),
        ("3.12.1", false),
        // ...
    ];
    for (version, expected) in test_cases {
        let version_specifiers = VersionSpecifiers::from_str(version).unwrap();
        let requires_python = RequiresPython::from_specifiers(&version_specifiers).unwrap();
        assert_eq!(requires_python.is_matching_without_patch(), expected, "version: {version}");
    }
}
```

The main reason for this is that it would be great to reduce our dependence on proc macros in service of helping compile times.

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:340 on 2024-10-11 12:23_

The phrasing seems a touch off here. Maybe this instead? Not totally sure.

```suggestion
            warn_user_once!("The workspace `requires-python` value does not have a patch version: `{requires_python}`. It will be interpreted as `{requires_python}.0`. Did you mean `{requires_python}.*`?");
```

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:923 on 2024-10-11 12:26_

Perhaps an odd case, but this makes me think of something like, `==3.10, <3.11`. I think that should return `true`?

---

_@BurntSushi had review dismissed on 2024-10-11 12:37_

---

_Comment by @ianpaul10 on 2024-10-14 12:47_

Ok thanks for all the comments, I believe I addressed them all, let me know if there's anything I missed.

Also, I'm not sure what's happening with the windows tests and mkdocs, but it appears to be happening on other PRs 🤔 Are they normally flaky or should I investigate further?

---

_Comment by @zanieb on 2024-10-14 13:21_

I'll look into that, thanks!

---

_Review requested from @BurntSushi by @ianpaul10 on 2024-10-16 02:51_

---

_@zanieb approved on 2024-10-16 04:07_

Thank you and nice work!

I made some minor tweaks, let me know if you have any questions or concerns.

---

_Renamed from "Warn when patch isn't specified" to "Warn when patch version in `requires-python` is implicitly `0`" by @zanieb on 2024-10-16 04:08_

---

_Comment by @ianpaul10 on 2024-10-16 04:13_

Thanks for reviewing and improving the language! Those all make sense to me 👍 

---

_Merged by @zanieb on 2024-10-16 04:21_

---

_Closed by @zanieb on 2024-10-16 04:21_

---

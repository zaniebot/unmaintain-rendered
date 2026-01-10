```yaml
number: 11783
title: Add MACOSX_DEPLOYMENT_TARGET hint for macos platform wheels
type: pull_request
state: open
author: dcarrier
labels:
  - error messages
assignees: []
base: main
head: issue-10699
created_at: 2025-02-25T22:57:31Z
updated_at: 2025-11-28T18:54:23Z
url: https://github.com/astral-sh/uv/pull/11783
synced_at: 2026-01-10T05:49:14Z
```

# Add MACOSX_DEPLOYMENT_TARGET hint for macos platform wheels

---

_Pull request opened by @dcarrier on 2025-02-25 22:57_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Addresses: https://github.com/astral-sh/uv/issues/10699

Adds a hint that the user may utilize `MACOSX_DEPLOYMENT_TARGET` to change the deployment target for macos platform wheels.

## Test Plan

<!-- How was it tested? -->
```shell
uv git:(issue-10699) echo "mlx" | cargo run -- pip compile - -p 3.12 --python-platform=macos
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of mlx are available:
          mlx==0.0.2
          mlx==0.0.3
          mlx==0.0.4
          mlx==0.0.5
          mlx==0.0.6
          mlx==0.0.7
          mlx==0.0.9
          mlx==0.0.10
          mlx==0.0.11
          mlx==0.1.0
          mlx==0.2.0
          mlx==0.3.0
          mlx==0.4.0
          mlx==0.5.1
          mlx==0.6.0
          mlx==0.7.0
          mlx==0.8.1
          mlx==0.9.1
          mlx==0.10.0
          mlx==0.11.1
          mlx==0.12.2
          mlx==0.13.0
          mlx==0.13.1
          mlx==0.14.0
          mlx==0.14.1
          mlx==0.15.0
          mlx==0.15.1
          mlx==0.15.2
          mlx==0.16.0
          mlx==0.16.1
          mlx==0.16.2
          mlx==0.16.3
          mlx==0.17.1
          mlx==0.17.2
          mlx==0.17.3
          mlx==0.18.0
          mlx==0.18.1
          mlx==0.19.0
          mlx==0.19.3
          mlx==0.20.0
          mlx==0.21.0
          mlx==0.21.1
          mlx==0.22.0
          mlx==0.22.1
          mlx==0.23.1
      and mlx<=0.0.3 has no wheels with a matching Python ABI tag (e.g., `cp312`), we can conclude that mlx<=0.0.3 cannot be used.
      And because mlx>=0.0.4 has no wheels with a matching platform tag (e.g., `macosx_12_0_arm64`) and you require mlx, we can conclude that your requirements are unsatisfiable.

      hint: You require CPython 3.12 (`cp312`), but we only found wheels for `mlx` (v0.0.3) with the following Python ABI tags: `cp38`, `cp39`, `cp310`, `cp311`

      hint: Wheels are available for `mlx` (v0.23.1) on the following platforms: `manylinux_2_31_x86_64`, `macosx_13_0_arm64`, `macosx_14_0_arm64`

      hint: Use `MACOSX_DEPLOYMENT_TARGET` to set the deployment target on macos platform
``` 

---

_Review comment by @j178 on `crates/uv-resolver/src/pubgrub/report.rs`:1667 on 2025-02-26 01:26_

nit: this can be `EnvVars::MACOSX_
_DEPLOYMENT_TARGET`

---

_@j178 reviewed on 2025-02-26 01:26_

---

_@zanieb reviewed on 2025-02-26 03:32_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1660 on 2025-02-26 03:32_

Presumably this will show the hint even if you're not targeting macOS? Is that what we want? e.g., in the changed test `invalid_platform` the target is `linux`. Showing the hint there seems confusing, right?

---

_@zanieb reviewed on 2025-02-26 03:33_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:14795 on 2025-02-26 03:33_

I think we'll also need to explain the current deployment target. e.g., "The current macOS deployment target is <>, set `...` to change the deployment target to a different version"

Do we need to talk about how the deployment target is a minimum or a maximum? Can we use one of the target versions from the available wheels to show an example?

---

_Comment by @zanieb on 2025-02-26 03:33_

Cool! Thanks for contributing. I have a few questions.

---

_Assigned to @zanieb by @zanieb on 2025-02-26 03:33_

---

_Label `error messages` added by @zanieb on 2025-02-26 03:34_

---

_@dcarrier reviewed on 2025-02-26 22:17_

---

_Review comment by @dcarrier on `crates/uv-resolver/src/pubgrub/report.rs`:1667 on 2025-02-26 22:17_

Thank you, I will fix this one.

---

_@dcarrier reviewed on 2025-02-27 00:10_

---

_Review comment by @dcarrier on `crates/uv-resolver/src/pubgrub/report.rs`:1660 on 2025-02-27 00:10_

Great point, confusing indeed! I have been poking around this afternoon to find a way to filter on platform and passing in `ResolverEnvironment` looks promising.

Is this also what you had in mind?

---

_@dcarrier reviewed on 2025-02-27 00:16_

---

_Review comment by @dcarrier on `crates/uv/tests/it/pip_compile.rs`:14795 on 2025-02-27 00:16_

This is good advice thank you. I have found using a `TargetTriple` instance is a good way to grab the current deployment target.

I will add that, talk about minimum and maximum, and utilize one of the `PlatformTags` to provide an example.

---

_@zanieb reviewed on 2025-03-01 17:52_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1660 on 2025-03-01 17:52_

That sounds reasonable — looks like we use that in other hints too.

---

_Comment by @dcarrier on 2025-03-05 19:09_

Alright, in addition to the requested changes I made a few modifications. 

Most notably:
- Made the macOS deployment target its own hint as it felt more ergonomic.
- Modified `tag_hint` -> `tag_hints` so we can return multiple `PubgrubHints` in the Platform scenario.
- Added a snapshot test based on the linked issue.

Hint now looks like this:
```
hint: The current minimum macOS deployment target is 12.0, set environment variable MACOSX_DEPLOYMENT_TARGET to change the minimum deployment target (e.g. MACOSX_DEPLOYMENT_TARGET=13.0)
```

---

_Review comment by @zanieb on `crates/uv-configuration/src/target_triple.rs`:732 on 2025-03-05 23:37_

Why did you decide to change this call signature instead of unwrapping to something like `DEFAULT_MACOS_DEPLOYMENT_TARGET` at callsites?

This might be better, just curious if there was anything in particular that motivated it.

cc @charliermarsh as the owner of this 

---

_@zanieb reviewed on 2025-03-05 23:37_

---

_@zanieb reviewed on 2025-03-05 23:41_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1701 on 2025-03-05 23:41_

Here you pull the current target from the environment which I presume motivates the change in https://github.com/astral-sh/uv/pull/11783/files#r1982335893 but shouldn't we be pulling this value from the failing resolution's platform target triple instead?

---

_Comment by @zanieb on 2025-03-05 23:42_

Thank you for following up on my feedback!

---

_@zanieb reviewed on 2025-03-05 23:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:14825 on 2025-03-05 23:43_

I still find it a bit surprising these are separate hints, as the latter is only relevant if the first is shown, correct? Can you say a bit more about why you found separate hints more ergonomic?

---

_@zanieb reviewed on 2025-03-05 23:45_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1701 on 2025-03-05 23:45_

(I'm not entirely sure this matters in practice, because the only way the deployment target can be set is via environment variable right now? But I think it could be an easy cause of a future bug)

---

_@zanieb reviewed on 2025-03-05 23:45_

---

_Review comment by @zanieb on `crates/uv-configuration/src/target_triple.rs`:732 on 2025-03-05 23:45_

(Noticed this was related to https://github.com/astral-sh/uv/pull/11783#discussion_r1982338343)

---

_@dcarrier reviewed on 2025-03-06 00:07_

---

_Review comment by @dcarrier on `crates/uv/tests/it/pip_compile.rs`:14825 on 2025-03-06 00:07_

Yep you are correct, the macOS deployment target hint is dependent on the platform hint. I did mess around with breaking out the logic of the macOS deployment target into a separate function instead of a new PubgrubHint variant, but it didn't feel as clean in the codebase as following the variant pattern in the fmt function. It also feels nice to compose the hints at the `tag_hints` level.

Happy to switch it up if necessary!

---

_@dcarrier reviewed on 2025-03-06 00:14_

---

_Review comment by @dcarrier on `crates/uv-configuration/src/target_triple.rs`:732 on 2025-03-06 00:14_

I liked centralizing the logic and moving the default version unwrap out of the platform constructor. It had the added benefit that it assisted with https://github.com/astral-sh/uv/pull/11783#discussion_r1982338343 as you pointed out 😄.

Your suggestion is clean and would work the same. I would just change to `DEFAULT_MACOS_DEPLOYMENT_TARGET` in `platform()` unwrap.

---

_@dcarrier reviewed on 2025-03-06 00:57_

---

_Review comment by @dcarrier on `crates/uv-resolver/src/pubgrub/report.rs`:1701 on 2025-03-06 00:57_

As far as I know the environment variable is the only way to control the deployment target, correct.

If you don’t mind, I may need some advice on the best path forward. The major / minor fields in the platform object are not included in the resolution marker env object and the originally constructed target triple is only used to create the marker. 

---

_@zanieb reviewed on 2025-03-06 04:31_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1701 on 2025-03-06 04:31_

I see — yeah I wasn't sure if it was necessarily easy to get at. I can take a look. Charlie may also have an idea.

---

_@charliermarsh reviewed on 2025-03-06 16:39_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1701 on 2025-03-06 16:39_

I think what you want to do is... Iterate over `self.tags`, and find the minimum macOS major-minor on any platform tag. Then use _that_ as the inferred deployment target.

---

_@charliermarsh reviewed on 2025-03-06 16:39_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/target_triple.rs`:732 on 2025-03-06 16:39_

I would probably keep this as-is and unwrap at each site. I think each site should be "forced" to consider the value it uses here.

---

_@charliermarsh reviewed on 2025-03-06 16:40_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:14825 on 2025-03-06 16:40_

I don't know that this hint actually works today... Has anyone tried it? I think we only look at `MACOSX_DEPLOYMENT_TARGET` when `--python-platform` is specified. I wouldn't expect it to have any affect on `uv lock`, for example. We might need to change our tag inference logic to respect it first. (I could be wrong.)

---

_@zanieb reviewed on 2025-03-06 16:46_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:14825 on 2025-03-06 16:46_

I haven't tried it, no. Interesting.

@dcarrier don't worry about looking into that (unless you're interested), we can explore it separately from this change.

---

_@dcarrier reviewed on 2025-03-07 20:04_

---

_Review comment by @dcarrier on `crates/uv-resolver/src/pubgrub/report.rs`:1701 on 2025-03-07 20:04_

Thanks Charlie! I was looking into iterating over `self.tags` with one caveat: Should we actually find the max to infer the macosx deployment target?

It looks like we use the `major` field of Os as the top end to generate the list of platform_tag versions for anything >= 11. (ex: https://github.com/astral-sh/uv/blob/main/crates/uv-platform-tags/src/tags.rs#L497)

This feels counterintuitive to me because MACOSX_DEPLOYMENT_TARGET specifies the minimum to be supported, but for the purposes of the hint it would make the most sense as feedback to the user.

Btw thanks to you and Zanie for the feedback, I feel like I am making this complicated but I want to be sure I understand what I am changing.

---

_@dcarrier reviewed on 2025-03-07 20:06_

---

_Review comment by @dcarrier on `crates/uv-configuration/src/target_triple.rs`:732 on 2025-03-07 20:06_

Ack, I will revert these changes in favor of the call site unwrap. Thank you!

---

_Comment by @zanieb on 2025-05-27 16:31_

Are you interested in continuing this work? Otherwise, we can try to find someone to pick it up.

---

_Comment by @dcarrier on 2025-05-28 18:36_

> Are you interested in continuing this work? Otherwise, we can try to find someone to pick it up.

Hey @zanieb, thanks for checking in. Unfortunately I no longer have the cycles to work on this.

---

_Comment by @TaKO8Ki on 2025-11-28 18:54_

Hi, @zanieb. I have created a pull request for taking over this pr. Could you review it when you have time? https://github.com/astral-sh/uv/pull/16553

---

```yaml
number: 10067
title: "Allow users to mark platforms as \"required\" for wheel coverage"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/required-platforms
created_at: 2024-12-20T22:00:55Z
updated_at: 2025-02-18T13:44:04Z
url: https://github.com/astral-sh/uv/pull/10067
synced_at: 2026-01-10T11:10:34Z
```

# Allow users to mark platforms as "required" for wheel coverage

---

_Pull request opened by @charliermarsh on 2024-12-20 22:00_

## Summary

This PR revives https://github.com/astral-sh/uv/pull/10017, which might be viable now that we _don't_ enforce any platforms by default.

The basic idea here is that users can mark certain platforms as required (empty, by default). When resolving, we ensure that the specified platforms have wheel coverage, backtracking if not.

For example, to require that we include a version of PyTorch that supports Intel macOS:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = ["torch>1.13"]

[tool.uv]
required-platforms = [
    "sys_platform == 'darwin' and platform_machine == 'x86_64'"
]
```

Other than that, the forking is identical to past iterations of this PR.

This would give users a way to resolve the tail of issues in #9711, but with manual opt-in to supporting specific platforms.


---

_Marked ready for review by @charliermarsh on 2024-12-21 04:14_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-21 04:14_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-21 04:14_

---

_Review requested from @konstin by @charliermarsh on 2024-12-21 04:14_

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/prioritized_distribution.rs`:243 on 2024-12-21 13:04_

Nice

---

_Review comment by @BurntSushi on `docs/reference/settings.md`:272 on 2024-12-21 13:22_

I think it might help if the docs specifically "compared and contrasted" `required-platforms` with `environments`. They do different things of course, but they take the same inputs and I could see end users being confused by them.

(What I tend to do in docs is to mention the other in the docs and contrast them. e.g., mention `environments` in this section and `required-platforms` in the `environments` section. That way, users see the contrasting doc regardless of where they look.)

---

_@BurntSushi approved on 2024-12-21 13:22_

I like this!

---

_Comment by @charliermarsh on 2024-12-21 23:59_

Another case that this could solve (but would require user intervention): https://github.com/astral-sh/uv/issues/10085. Ideally, I think we should be able to give users a hint when the sync fails -- like "There are no available wheels for this platform, add `required-platforms`".

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:535 on 2025-01-10 13:17_

Should this be `required_environments` or `required_markers`? The other markers list is `environments`, `platforms` lacks symmetry

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:3677 on 2025-01-10 13:25_

Don't we add additional incompatibilities that target `id` when we find a new package `foo` with `foo --[marker]--> id`? Could that lead to packages missing platform support because we could have a case like the following:

Required platform linux

Choose A 2, with `A -> B; windows`, `A -> C`
Choose B 2. B 2 does have only a mac wheel but that's fine because it's only required on windows, so our required linux is coverd.
Choose C 2, with `C -> B`

Try to install on linux. We try to install A 2, C 2 and B 2, but B 2 fails since it has no wheel.

---

_@konstin reviewed on 2025-01-10 13:27_

The feature is definitely needed and i like the approach, though the `find_environments` if i haven't missed something fails to uphold the required platforms in certain conditions.

---

_@charliermarsh reviewed on 2025-01-10 14:36_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:3677 on 2025-01-10 14:36_

@konstin -- I think this would actually work as-is, though for "bad" reasons. Basically, when we visit `B`, we won't know that it's "only required for Windows", because we don't do "aggressive forking" today, so we'd still try to solve for Linux. It might get filtered out later? I can't quite remember.


---

_@charliermarsh reviewed on 2025-01-10 14:36_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:535 on 2025-01-10 14:36_

It was somewhat intentional but I can change it.

---

_@zanieb reviewed on 2025-01-10 17:47_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:535 on 2025-01-10 17:47_

We might want to change the other one? I feel like `environments` is a bit overloaded in the longterm.

---

_Comment by @zanieb on 2025-01-10 17:50_

Since this is specific to _wheels_ should we name this differently? Like, if I encounter a build failure why would it be obvious that setting a `required_platform` would solve that? Is this sort of `no_build_platforms`?

---

_Comment by @charliermarsh on 2025-01-11 14:13_

@zanieb -- That's true, although I'd read that setting as "platforms on which building should be disabled", which isn't quite the same. We're not enforcing that all packages have wheels on these platforms. We're enforcing that all packages have wheels _or_ source distributions on these platforms (and so, building is considered ok).

---

_Comment by @charliermarsh on 2025-01-11 14:13_

I sort of prefer something that doesn't get into the source vs. binary distinction, and instead gets at the intent of the flag: to ensure that the lockfile supports the platforms you care about.

---

_Comment by @zanieb on 2025-01-11 19:53_

Oh I guess I'm getting confused by the pull request title and body which say...

>  When resolving, we ensure that the specified platforms have wheel coverage

This sounds like we require wheels for the platforms?

---

_Comment by @charliermarsh on 2025-01-11 19:54_

We require that there are compatible distributions for the platform. That could be a source distribution, or (for packages that lack source distributions) a wheel.

---

_Comment by @zanieb on 2025-01-11 20:26_

That makes sense, it'd help to change the way this is presented in the title / body then. With this context, `no_build_platforms` definitely does not make sense.

---

_@zanieb reviewed on 2025-01-11 20:27_

---

_Review comment by @zanieb on `docs/reference/settings.md`:272 on 2025-01-11 20:27_

I think doing this would be helpful for naming. (as well as very helpful for users)

---

_@konstin reviewed on 2025-01-13 10:17_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:3677 on 2025-01-13 10:17_

Could you add a test case covering this scenario? So it doesn't fail with future resolver changes.

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:525 on 2025-02-11 02:47_

I re-did the docs here. Is it any better, @zanieb @BurntSushi?

---

_@charliermarsh reviewed on 2025-02-11 02:47_

---

_Review comment by @konstin on `docs/reference/settings.md`:283 on 2025-02-11 11:06_

I would add something about using older versions here, like "If a version of a package does not contain the required source distribution or wheels, uv will try older versions until it finds a version with sufficient support".

---

_Review comment by @konstin on `docs/reference/settings.md`:292 on 2025-02-11 11:06_

```suggestion
order to be installable, while still including Linux and Windows wheels in the lockfile.
```

---

_@konstin approved on 2025-02-11 11:07_

I like the updated documentation, i left some nits.

---

_@BurntSushi reviewed on 2025-02-11 13:54_

---

_Review comment by @BurntSushi on `crates/uv-workspace/src/pyproject.rs`:525 on 2025-02-11 13:54_

Beautiful. Thank you!

---

_@BurntSushi approved on 2025-02-11 13:55_

Docs are much improved!

---

_@zanieb approved on 2025-02-11 22:27_

The docs are great. Not sure if we want to consider another name. This seems fine for now.

---

_Merged by @charliermarsh on 2025-02-14 20:11_

---

_Closed by @charliermarsh on 2025-02-14 20:11_

---

_Branch deleted on 2025-02-14 20:11_

---

_Comment by @johnthagen on 2025-02-17 19:43_

I've wanted this for so long I almost can't believe my eyes that someone has implemented it. ❤️ ❤️ ❤️ 

---

_Comment by @charliermarsh on 2025-02-17 19:43_

Please let me know what you use it for and if it works for you (or not)!

---

_Comment by @johnthagen on 2025-02-17 19:50_

> Please let me know what you use it for and if it works for you (or not)!

My use case is that I work with dependencies that will sometimes (suddenly) drop publishing wheels for certain platforms (often something like ARM64 Linux) and it can become very challenging to detect this. What happens is that we'll `poetry update` or similar on, say, macOS ARM and that will resolve to the latest version that is missing wheels for _other_ platforms that we need.

A couple real-life examples;

- (Missing Linux ARM wheels): https://pypi.org/project/ai-edge-litert-nightly/1.1.2.dev20250211/#files
- (Now they're back!): https://pypi.org/project/ai-edge-litert-nightly/1.1.2.dev20250216/#files

We wouldn't want to accidentally lock to the version that is missing Linux ARM wheels while locking from macOS.

Another example:

- 0.18 has Linux ARM wheels: https://pypi.org/project/open3d/0.18.0/
- 0.19 does not have Linux ARM wheels: https://pypi.org/project/open3d/0.19.0/#files

Our current workaround is to pin versions in `pyproject.toml` after we either hit issues later (for trying to `poetry sync`  in a Linux ARM64 container on an ARM64 macOS host).

Being able to express our platform needs so that the resolver can make this work properly would be great.

I hope this feedback helps!

---

_Comment by @uwu-420 on 2025-02-18 09:18_

@charliermarsh Love to see this making it into the latest release, thank you ❤️
This also helps me massively at work because I can at least ensure that my team only adds packages that have wheels for `{x86_64, aarch64} x {linux, macos}`. The only thing that's missing for us is to ensure that there are wheels for macos 10.15 (for x86_64) and 11.0 (for aarch64) because these are the minimum macos versions we have to support. Related to my issue https://github.com/astral-sh/uv/issues/9837

---

_Comment by @trim21 on 2025-02-18 13:44_

the option in PR description is not updated, it should be `required-environments` instead of `required-platforms` ...

---

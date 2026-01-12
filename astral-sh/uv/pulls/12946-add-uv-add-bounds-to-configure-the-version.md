```yaml
number: 12946
title: "Add `uv add --bounds` to configure the version constraint"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - great writeup
assignees: []
merged: true
base: main
head: konsti/configurable-operator
created_at: 2025-04-17T14:26:52Z
updated_at: 2025-05-28T13:11:32Z
url: https://github.com/astral-sh/uv/pull/12946
synced_at: 2026-01-12T16:10:28Z
```

# Add `uv add --bounds` to configure the version constraint

---

_@konstin_

By default, uv uses only a lower bound in `uv add`, which avoids dependency conflicts due to upper bounds. With this PR, this cna be changed by setting a different bound kind. The bound kind can be configured in `uv.toml`, as a user preference, in `pyproject.toml`, as a project preference, or on the CLI, when adding a specific project.

We add two options that add an upper bound on the constraint, one for SemVer (`>=1.2.3,<2.0.0`, dubbed "major", modeled after the SemVer caret) and another one for dependencies that make breaking changes in minor version (`>=1.2.3,<1.3.0`, dubbed "minor", modeled after the SemVer tilde). Intuitively, the major option bumps the most significant version component, while the minor option bumps the second most significant version component. There is also an exact bounds option (`==1.2.3`), though generally we recommend setting a wider bound and using the lockfile for pinning.

Versions can have leading zeroes, such as `0.1` or `0.0.1`. For a single leading 0, we shift the the meaning of major and minor similar to cargo. For two or more leading zeroes, the difference between major and minor becomes inapplicable, instead both bump the most significant component:
- major: `0.1` -> `>=0.1,<0.2`
- major: `0.0.1` -> `>=0.0.1,<0.0.2`
- major: `0.0.1.1` -> `>=0.0.1.1,<0.0.2.0`
- major: `0.0.0.1` -> `>=0.0.0.1,<0.0.0.2`
- minor: `0.1` -> `>=0.1,<0.1.1`
- minor: `0.0.1` -> `>=0.0.1,<0.0.2`
- minor: `0.0.1.1` -> `>=0.0.1.1,<0.0.2.0`
- minor: `0.0.0.1` -> `>=0.0.0.1,<0.0.0.2`

For a consistent appearance, we try to preserve the number of components in the upper bound. For example, adding a version `2.17` with the major option is stored as `>=2.17,<3.0`. If a version uses three components and is greater than 0, both bounds will also use three components (SemVer versions always have three components). Of the top 100 PyPI packages, 8 use a non-three-component version (docutils, idna, pycparser and soupsieve with two components, packaging, pytz and tzdata with two component, CalVer and trove-classifiers with four component CalVer). Example `pyproject.toml` files with the top 100 packages: [`--bounds major`](https://gist.github.com/konstin/0aaffa9ea53c4834c22759e8865409f4) and [`--bounds minor`](https://gist.github.com/konstin/e77f5e990a7efe8a3c8a97c5c5b76964). While many projects follow version scheme that roughly or directly matches the major or minor options, these compatibility ranges are usually not applicable for the also popular CalVer versioning.

For pre-release versions, there are two framings we could take: One is that pre-releases generally make no guarantees about compatibility between them and are used to introduce breaking changes, so we should pin them exactly. In many cases however, pre-release specifiers are used because a project needs a bugfix or a feature that hasn't made it into a stable release, or because a project is compatible with the next version before a final version for that release is published. In those cases, compatibility with other packages that depend on the same library is more important, so the desired bound is the same as it would be for the stable release, except with the lower bound lowered to include pre-release. 

The names of the bounds and the name of the flag is up for bikeshedding. Currently, the option is call `tool.uv.bounds`, but we could also move it under `tool.uv.edit.bounds`, where it would be the first/only entry.

Fixes #6783

---

_Label `enhancement` added by @konstin on 2025-04-17 14:26_

---

_Marked ready for review by @konstin on 2025-04-21 14:12_

---

_@zanieb reviewed on 2025-04-21 14:16_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:89 on 2025-04-21 14:16_

Should this comment say when happens if the assumption is broken?

---

_@zanieb reviewed on 2025-04-21 14:16_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:104 on 2025-04-21 14:16_

For consistency with the others
```suggestion
    /// Allow the same minor version, similar to the semver tilde, e.g., `>=1.2.3,<1.3.0`.
```

---

_@zanieb reviewed on 2025-04-21 14:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:11244 on 2025-04-21 14:21_

What happens if I change my bounds setting then add an existing package again?

---

_@zanieb reviewed on 2025-04-21 14:23_

---

_Review comment by @zanieb on `docs/reference/settings.md`:645 on 2025-04-21 14:23_

I worry this feels like we'll use these bounds in other operations. I wonder if it should be `default-bounds`? or `add.bounds`? Do you think we'll read this setting during `uv upgrade` eventually? Would we want a separate option for that?

---

_Comment by @zanieb on 2025-04-21 14:25_

We'll need a small documentation section in https://docs.astral.sh/uv/concepts/projects/dependencies/ and https://docs.astral.sh/uv/concepts/projects/config/ (not sure which should link to the other yet)

---

_@zanieb reviewed on 2025-04-21 14:26_

---

_Review comment by @zanieb on `docs/reference/settings.md`:645 on 2025-04-21 14:26_

(`edit.bounds` is okay, but I worry it won't be obvious to users and it _only_ applies to `add` edits, right?)

---

_Review comment by @konstin on `crates/uv/tests/it/edit.rs`:11244 on 2025-04-21 15:09_

Currently, we only ever set a bound if no bound is provided on the CLI and no bound is already present. If you e.g. `uv add numpy` and then `uv add numpy` again after a new release, we don't update the lower bound. Similarly, if you manually edit the `pyproject.toml` to `numpy==2.0.0`, `uv add numpy` does not change that bound. I've retained that behavior with the bounds options. I expect actually editing of bounds to shift to `uv upgrade`, so we can avoid the complexity here.

---

_@konstin reviewed on 2025-04-21 15:09_

---

_@zanieb reviewed on 2025-04-21 15:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:11244 on 2025-04-21 15:28_

I think `uv add foo --bound major` then `uv add foo --bound minor` should mutate the bounds. Similarly, I think `uv add numpy --upgrade` should update the bounds. I could be okay with these changes happening separately, as you're preserving some existing behavior here, but I find the existing behavior weird.

---

_@konstin reviewed on 2025-04-21 15:45_

---

_Review comment by @konstin on `docs/reference/settings.md`:645 on 2025-04-21 15:45_

`add.bounds` sounds good. I have to do the systematic research for that still, but I expect upgrade to have more complex options, such as https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference#versioning-strategy--.

---

_@konstin reviewed on 2025-04-23 13:24_

---

_Review comment by @konstin on `docs/reference/settings.md`:645 on 2025-04-23 13:24_

I've gone with `add-bounds` for now to avoid `add.bounds` being the only nested option besides the pip namespace.

---

_Review requested from @zanieb by @konstin on 2025-04-25 11:37_

---

_Comment by @konstin on 2025-04-25 11:38_

This should be ready now

---

_@zanieb reviewed on 2025-05-02 15:05_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3491 on 2025-05-02 15:05_

```suggestion
    /// The kind of version specifier to use when adding dependencies.
```

---

_@zanieb reviewed on 2025-05-02 15:08_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3495 on 2025-05-02 15:08_

```suggestion
    /// When adding a dependency to the project, if no constraint or URL is provided, a constraint is
    /// added based on the latest compatible version of the package. By default, a lower bound constraint
    /// is used, e.g., `>=1.2.3`.
    ///
    /// When `--frozen` is provided, no resolution is performed, and dependencies are always added without
    /// constraints.
```

---

_@zanieb reviewed on 2025-05-02 15:08_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3495 on 2025-05-02 15:08_

(Sorry, hacky wrapping)

---

_@zanieb reviewed on 2025-05-02 15:09_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:2079 on 2025-05-02 15:09_

See https://github.com/astral-sh/uv/pull/12946/files#r2071754289

---

_@zanieb reviewed on 2025-05-02 15:11_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:112 on 2025-05-02 15:11_

Nit, very minor preference
```suggestion
    /// This option is not recommended, as versions are already pinned in the uv lockfile.
```

---

_@zanieb reviewed on 2025-05-02 15:14_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:127 on 2025-05-02 15:14_

Do I need to review this closely? Should we leverage another reviewer like @BurntSushi so I can focus on the ux?

---

_@zanieb reviewed on 2025-05-02 15:14_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:747 on 2025-05-02 15:14_

Needs update

---

_@zanieb reviewed on 2025-05-02 15:15_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:747 on 2025-05-02 15:15_

(several other occurrences too)

---

_@zanieb reviewed on 2025-05-02 15:15_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1670 on 2025-05-02 15:15_

<3

---

_@zanieb reviewed on 2025-05-02 15:16_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1721 on 2025-05-02 15:16_

Here, there are spaces, but in the documentation, there wasn't. Should the docs have a space here?

---

_@zanieb reviewed on 2025-05-02 15:16_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1728 on 2025-05-02 15:16_

Really into this behavior.

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1720 on 2025-05-02 15:18_

Isn't this equivalent to the major case above? `("0.0.1.1", ">=0.0.1.1, <0.0.2.0"),` a little confused.

---

_@zanieb reviewed on 2025-05-02 15:18_

---

_@zanieb reviewed on 2025-05-02 15:19_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1727 on 2025-05-02 15:19_

Should this have a trailing zero? I guess we match the original length?

---

_@zanieb reviewed on 2025-05-02 15:19_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1720 on 2025-05-02 15:19_

Also unsure why this lacks the trailing zero when we do for `major`?

---

_@zanieb reviewed on 2025-05-02 15:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:99 on 2025-05-02 15:20_

```suggestion
            "The bounds option is in preview and may change in any future release."
```

---

_@konstin reviewed on 2025-05-02 15:20_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:1727 on 2025-05-02 15:20_

yes, the design is to try to match the length in both constraints.

---

_@zanieb reviewed on 2025-05-02 15:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:99 on 2025-05-02 15:21_

or

```suggestion
            "The bounds option is in preview and may change without warning."
```

---

_@zanieb reviewed on 2025-05-02 15:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:99 on 2025-05-02 15:21_

I guess `This option is in preview and may change in any future release.` is used elsewhere. I don't love that messaging. We can revisit separately though.

---

_@zanieb reviewed on 2025-05-02 15:23_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:11863 on 2025-05-02 15:23_

Can you add tests for repeated adds with different bound requests? (even if not updated, we should have coverage)

Can you also add tests for a subsequent add with explicit bounds? And use of `--bounds` with explicit bounds?

---

_@zanieb reviewed on 2025-05-02 15:25_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1727 on 2025-05-02 15:25_

I find it a bit surprising with `major` that we include the trailing zeros, honestly.

---

_@zanieb reviewed on 2025-05-02 15:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:11863 on 2025-05-02 15:28_

We should probably still pin to an exact version if you do like... `uv add foo>0.1 --bounds exact`? Otherwise, you'd have to use a constraint to do that? I guess that means I generally think we should apply `--bounds` when a constraint is provided unless `--raw-sources` is used? (we ought to rename that option to make it more generic). I imagine companies that need exact pinning in their `pyproject.toml` want to apply even when a constraint is given unless there's some additional opt-out.

---

_@BurntSushi reviewed on 2025-05-02 15:34_

LGTM

---

_Comment by @BurntSushi on 2025-05-02 15:34_

Also, great PR description! It was super helpful during review.

---

_Label `great writeup` added by @BurntSushi on 2025-05-02 15:34_

---

_@zanieb reviewed on 2025-05-02 15:36_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject_mut.rs`:1727 on 2025-05-02 15:36_

I do find the example from the description compelling though

> For example, adding a version 2.17 with the major option is stored as >=2.17,<3.0

(Sorry for not reading that detail at first)

---

_@konstin reviewed on 2025-05-02 16:19_

---

_Review comment by @konstin on `crates/uv/src/commands/project/add.rs`:99 on 2025-05-02 16:19_

I consider the option itself rather stable, and the only major change I'd expect would be whether we change existing bounds as discussed, so I'm aiming for some messaging that's like "it works, but we might have to toggle the behavior a bit even before the next breaking release"  

---

_Comment by @T-256 on 2025-05-02 16:46_

> While many projects follow version scheme that roughly or directly matches the major or minor options, these compatibility ranges are usually not applicable for the also popular CalVer versioning.

For fixing that versioning, I recommend a per package configuration option in user/workspace `uv.toml`:
```toml
add-bounds = "major"

[per-package-add-bounds]
tzdata = "lower"
```

(Yes, it also could be separate work as follow-up...)

---

_@konstin reviewed on 2025-05-04 16:22_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:1720 on 2025-05-04 16:22_

fixed by adding the missing zero

---

_@konstin reviewed on 2025-05-04 18:03_

---

_Review comment by @konstin on `crates/uv/tests/it/edit.rs`:11863 on 2025-05-04 18:03_

I would still apply explicit bounds over the bounds preference, this way you can set `bounds = "major"` in the configuration and run `uv add numpy pandas torch==2.6.0`, applying the defaults except for deps for which you have more specific knowledge. It otherwise gets hard to reason about e.g. `uv add numpy>=1 --bounds major`, which unintuitively would add `numpy>=2,<3`. That may require more work from the user in specific cases (such as when `foo>0.1` is possible but wouldn't be picked without the constraint due to preferences but you want an exact bound as your use case can't rely on `uv.lock`).

Given that you usually run `uv add` once, I prefer the simpler rules (any explicit bound always beats the bounds preference) over complex rules trying to infer better bounds from between existing bounds, CLI bounds and default/explicit bounds preference from settings/CLI that require a complex mental model on the user's end to get their desired behavior. On the other hand, I do expect some of that complexity to be part of `uv upgrade`, where we always have an existing bound and want to compute a new bound from the old bound and a latest compatible version (but without a CLI-provided constraint).

---

_Comment by @konstin on 2025-05-04 18:03_

@T-256 I would expect something like that to rather go into `uv upgrade`, as `uv add` usually only happens once per package.

---

_@zanieb reviewed on 2025-05-07 16:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:11863 on 2025-05-07 16:47_

>  That may require more work from the user in specific cases (such as when foo>0.1 is possible but wouldn't be picked without the constraint due to preferences but you want an exact bound as your use case can't rely on uv.lock).

Yeah, this is the use-case I'm worried about. I think people will be surprised by this. Would it be reasonable to implement _only_ when the constraints are trivially less exact than the bounds? I see how it could be confusing in other contexts, but `uv add numpy>=1 --bounds minor` and `uv add numpy>=1 --bounds exact` seem easy for users to reason about. As I said before, I would expect to have to use a `--raw` flag to retain my exact constraint in this context.

Reflecting on your example...

>  `uv add numpy>=1 --bounds major`, which unintuitively would add `numpy>=2,<3` more

I think this not not actually unintuitive to me? It's a bit surprising that we chose the lower bound of `>=2`, but it'd be far _more_ surprising to me that we'd omit the upper bound entirely after I asked for it?

> Given that you usually run uv add once

I don't think this is strictly true. I think this is the only logical place in our interface to change bounds on a package without doing so manually, or doing an upgrade.

If the alternative wasn't that they had to literally write their constraints to a file, I might be more willing to just go with this as-is, but it's a painful workflow

I'm really on the fence here, I hear that you want the rules to be simple but I'm worry it's not handling some key use-cases. Perhaps returning to https://github.com/astral-sh/uv/pull/12946#discussion_r2071852834 ... since it's in preview perhaps we can try the simple thing and see how it goes? I just think we'll want to be clear that this behavior may evolve.

---

_@zanieb reviewed on 2025-05-07 17:00_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:11863 on 2025-05-07 17:00_

At the very least, if we're going to ignore `--bounds` / `add-bounds`, we should warn when the user provides a specifier?

---

_@konstin reviewed on 2025-05-12 18:37_

---

_Review comment by @konstin on `crates/uv/tests/it/edit.rs`:11863 on 2025-05-12 18:37_

> I'm really on the fence here, I hear that you want the rules to be simple but I'm worry it's not handling some key use-cases. Perhaps returning to #12946 (comment) ... since it's in preview perhaps we can try the simple thing and see how it goes? I just think we'll want to be clear that this behavior may evolve.

I'm pushing back on adding the complexity as I'm not having enough motivation that we need good constraints support here. Specifically, I haven't encountered a situation yet where I was adding a new package and it was resolved to a too-low version that could have been fixed with constraints that the user knows about without knowing the exact bounds. If we change it, we should do it consistently, e.g., change all bounds in the `uv add` CLI to constraints unless `--raw` is given. We also need to consider that the bounds preference setting can be a user level default, a project level default or CLI parameter.

When providing bounds, a user knows that they want a not-too-old version, without knowing the exact bounds they want. We cloud add an option to force adding the latest version with the preferred bounds (`--latest` or `--force-latest`). This would similarly highlight any dependencies conflicting with the new version while overriding lockfile preferences, serving a user who knows that they want the latest version but doesn't know the exact range yet. More generally, we could also always hint the user when we inferred an older version, e.g., telling them that for `uv add foo` with `add-bounds = "major"`, we add `foo>=5,<6` instead of `foo>=6,<7`, caused by `bar==1.2.3` in the lockfile requiring `foo<6`.

> At the very least, if we're going to ignore `--bounds` / `add-bounds`, we should warn when the user provides a specifier?

I've added a note when we're using the CLI bounds over a bounds preference.

---

_Comment by @zanieb on 2025-05-23 15:13_

@konstin I'm okay with this, but I'd like to change this message

https://github.com/astral-sh/uv/pull/12946/files#r2071770491

so that it's not scoped to saying "its configuration" may change. We have more leeway with the behavior that way.

---

_@zanieb reviewed on 2025-05-23 15:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3572 on 2025-05-23 15:14_

Should we add one line at the end that says

> This option is in preview and may change in any future release.

---

_@zanieb reviewed on 2025-05-23 15:14_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:2082 on 2025-05-23 15:14_

Same as https://github.com/astral-sh/uv/pull/12946/files#r2104809243

---

_@zanieb approved on 2025-05-23 15:16_

I have some minor requests to ensure the preview messaging is clear and allows us to change behavior here. I'm not sure the behavior when the user provides constraints makes sense to me still, but agree it seems hard to "get right" and that changes should be deferred until we have more signal from users.

---

_Merged by @konstin on 2025-05-28 13:11_

---

_Closed by @konstin on 2025-05-28 13:11_

---

_Branch deleted on 2025-05-28 13:11_

---

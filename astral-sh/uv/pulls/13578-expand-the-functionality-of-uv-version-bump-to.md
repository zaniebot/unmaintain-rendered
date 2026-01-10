```yaml
number: 13578
title: "Expand the functionality of `uv version --bump` to support pre-releases"
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
assignees: []
merged: true
base: main
head: gankra/ver-advanced
created_at: 2025-05-21T14:51:38Z
updated_at: 2025-07-10T15:02:15Z
url: https://github.com/astral-sh/uv/pull/13578
synced_at: 2026-01-10T06:53:01Z
```

# Expand the functionality of `uv version --bump` to support pre-releases

---

_Pull request opened by @Gankra on 2025-05-21 14:51_

This adds `alpha`, `beta`, `rc`, `stable`, `post`, and `dev` modes to `uv version --bump`.

The components that `--bump` accepts are ordered as follows:

    major > minor > patch > stable > alpha > beta > rc > post > dev
    
Bumping a component "clears" all lesser component (`alpha`, `beta`, and `rc` all overwrite each other): 

* `--bump minor` on `1.2.3a4.post5.dev6` => `1.3.0`
* `--bump alpha` on `1.2.3a4.post5.dev6` => `1.2.3a5` 
* `--bump dev  ` on `1.2.3a4.post5.dev6` => `1.2.3a4.post5.dev7`

In addition, `--bump` can now be repeated. The primary motivation of this is "bump stable version and also enter a prerelease", but it technically lets you express other things if you want them:

* `--bump patch --bump alpha` on `1.2.3` => `1.2.4a1` ("bump patch version and go to alpha 1")
* `--bump minor --bump patch` on `1.2.3` => `1.3.1` ("bump minor version and got to patch 1")
* `--bump minor --bump minor` on `1.2.3` => `1.4.0` ("bump minor version twice")

The `--bump` flags are sorted by their priority, so that you don't need to remember the priority yourself. This ordering is the only "useful" one that preserves every `--bump` you passed, so there's no concern about loss of expressiveness. For instance `--bump minor --bump major` would just be `--bump major` if we didn't sort, as the major bump clears the minor version. The ordering of `beta` after `alpha` means `--bump alpha --bump beta` will just result in beta 1; this is the one case where a bump request will effectively get overwritten.

The `stable` mode "bumps to the next stable release", clearing the pre (`alpha`, `beta`, `rc`), `dev`, and `post` components from a version (`1.2.3a4.post5.dev6` => `1.2.3`). The choice to clear `post` here is a bit odd, in that `1.2.3.post4` => `1.2.3` is actually a version decrease, but I think this gives a more intuitive model (as preserving `post5` in the previous example is definitely wrong), and also post-releases are extremely obscure so probably no one will notice. In the cases where this behaviour isn't useful, you probably wanted to pass `--bump patch` or something anyway which *should* definitely clear the `post5` (putting it another way: the only cases where `--bump stable` has dubious behaviour is when you wanted it to do a noop, which, is a command you could have just not written at all).

In all cases we preserve the "epoch" and "local" components of a version, so the `7!` and `+local` in `7!1.2.3+local` will never be modified by `--bump` (you can use the raw version set mode if you want to touch those). The preservation of `local` is another slightly odd choice, but it's a really obscure feature (so again it mostly won't come up) and when it's used it seems to mostly be used for referring to variant releases, in which case preserving it tends to be correct.

Fixes #13223


---

_Label `enhancement` added by @Gankra on 2025-05-21 14:51_

---

_@Gankra reviewed on 2025-05-21 14:55_

---

_Review comment by @Gankra on `crates/uv-pep440/src/version.rs`:4189 on 2025-05-21 14:55_

These chunks of the PR should ideally cover any corner case you might want to ask about.

---

_Comment by @Gankra on 2025-05-21 15:02_

"Did you consider a `--bump pre` mode" -- yes I don't think it's worth the API space, it seems reasonable to ask a user to know what flavour of prerelease they're using (and if I'm wrong we can add it later).

---

_Review requested from @BurntSushi by @zanieb on 2025-05-21 15:05_

---

_Review requested from @konstin by @zanieb on 2025-05-21 15:05_

---

_Assigned to @konstin by @zanieb on 2025-05-21 15:05_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:621 on 2025-05-21 15:18_

I'd keep the user-facing docs at basic version numbers without nested suffixes

---

_Comment by @Gankra on 2025-05-21 15:18_

For the sake of getting an intrusive thought out of my head: there are several times where I said "maybe you should be able to express `--bump minor --bump alpha` in a way where the alpha doesn't get reset" (e.g. `1.2.3a4` => `1.3.0a5`) before reminding myself that this makes absolutely no semantic sense (so no we're not supporting that).

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:625 on 2025-05-21 15:19_

> The choice to clear post here is a bit odd, in that 1.2.3.post4 => 1.2.3 is actually a version decrease, but I think this gives a more intuitive model (as preserving post5 in the previous example is definitely wrong), and also post-releases are extremely obscure so probably no one will notice. 

Clearing post would be a negative bump, imo using stable with a post release should error.

---

_Comment by @konstin on 2025-05-21 15:28_

Currently, there can be negative version bumps, where bumping `0.1.0b1` with `uv version --bump alpha` lowers the version instead of increasing it, as this isn't the next alpha but the previous alpha before the beta.

---

_Review comment by @konstin on `docs/reference/cli.md`:1503 on 2025-05-21 15:32_

Can you add the lifecycle workflow to the docs? Telling users about the prerelease workflow starting with `uv version --bump <patch|minor|major> --bump <alpha|beta|rc>` to go to the next prerelease, then using `uv version --bump stable` to release the final version.

---

_Review comment by @konstin on `docs/reference/cli.md`:1500 on 2025-05-21 15:32_

Can you document that the values get sorted?

---

_Review comment by @konstin on `crates/uv-pep440/src/version.rs`:681 on 2025-05-21 15:38_

typo

---

_@konstin reviewed on 2025-05-21 15:42_

The code looks good, but I'm concerned by how easy it is to decrease the version accidentally, the version is decreased when using a prerelease bump on stable version by default.

---

_Comment by @Gankra on 2025-05-21 16:39_

I've pushed up a separate commit that adds an `--allow-decreases` flag, and made the code error if a series of bumps fails to increase the version, as well as tests for the 3 cases I know of where this can happen.

---

_Comment by @Gankra on 2025-05-21 16:59_

Updated cli docs a bit

---

_@Gankra reviewed on 2025-05-21 17:01_

---

_Review comment by @Gankra on `crates/uv-pep440/src/version.rs`:681 on 2025-05-21 17:01_

...I keep staring at this and I can't find the typo

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 19:38_

nit: This is rendered as markdown, the whitespace alignment will be lost. 

---

_Review comment by @konstin on `crates/uv-pep440/src/version.rs`:681 on 2025-05-21 19:39_

i'm confused by the `1` just after the `kind`

---

_@konstin approved on 2025-05-21 19:39_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 19:47_

Ah yeah I wanted it to render helpfully in the CLI but I think I need to move these docs off `bump` and onto the command itself...


---

_@Gankra reviewed on 2025-05-21 19:47_

---

_@Gankra reviewed on 2025-05-21 19:52_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 19:52_

...no we supress verbose help so much it's web docs or nothing, oh well!

---

_@konstin reviewed on 2025-05-21 19:57_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 19:57_

we also fix some markdown parsing in https://github.com/astral-sh/uv/pull/13166

---

_@zanieb reviewed on 2025-05-21 20:01_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 20:01_

We do have verbose help.. `uv help version` — please make sure it formats nicely everywhere.

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 20:28_

![Screenshot 2025-05-21 at 4 28 37 PM](https://github.com/user-attachments/assets/6b301e0d-14a8-4ebc-80fc-2271ef261a51)

i'm being pranked by pseudo markdown 😭 

---

_@Gankra reviewed on 2025-05-21 20:28_

---

_@konstin reviewed on 2025-05-21 21:25_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 21:25_

:/ another case of https://github.com/clap-rs/clap/issues/5900

---

_@zanieb reviewed on 2025-05-21 21:57_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:547 on 2025-05-21 21:57_

I would just move this kind of "workflow example" documentation into the actual docs instead of the CLI reference instead. I'd stick to syntax and semantics here, mostly.

---

_Review comment by @zanieb on `docs/guides/projects.md`:166 on 2025-05-22 14:15_

I think we'd usually say "publishing guide" here 

---

_Review comment by @zanieb on `docs/guides/projects.md`:166 on 2025-05-22 14:15_

(and have less of it be a link)

---

_Review comment by @zanieb on `docs/guides/projects.md`:168 on 2025-05-22 14:16_

I don't know if you need to say "with no other arguments", I don't think we do that elsewhere 

---

_Review comment by @zanieb on `docs/guides/projects.md`:175 on 2025-05-22 14:16_

Arbitrary package or workspace member?

---

_Review comment by @zanieb on `docs/guides/projects.md`:182 on 2025-05-22 14:17_

Instead of "no other output" maybe "without the package name"?

---

_Review comment by @zanieb on `docs/guides/projects.md`:189 on 2025-05-22 14:18_

We should probably stylize as "JSON" in prose

---

_Review comment by @zanieb on `docs/guides/projects.md`:188 on 2025-05-22 14:18_

Just wondering... will we ever populate this for project versions? Did we just include this for compatibility with the old command? It seems a bit weird. 

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:559 on 2025-05-22 14:20_

Is there a use case for this flag? If so, it might be nice to mention it in the docs here. I'm kind of having trouble envisioning one... If there isn't a use case or if it's really obscure, would it make sense to jettison this extra option for now?

---

_Review comment by @zanieb on `docs/guides/package.md`:64 on 2025-05-22 14:20_

"To update to an exact version, provide it as a positional argument:"

Maybe?

---

_Review comment by @zanieb on `docs/guides/package.md`:71 on 2025-05-22 14:21_

"To preview the change without updating the `pyproject.toml`, ..."

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:634 on 2025-05-22 14:21_

I might mention here that this could decrease the version number, and therefore, it's common to use this in conjunction with another `--bump` flag like `--bump patch`.

---

_Review comment by @zanieb on `docs/guides/package.md`:95 on 2025-05-22 14:22_

I don't know if users will be able to reason about this clobbering thing. I wonder if we should just say they're applied in this order?

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/version.rs`:201 on 2025-05-22 14:24_

Perhaps a bit pedantic, but I wonder if this would be better as `allow_non_increasing` with `new_version <= old_version`. That is, if my `--bump` _doesn't_ increase the version number in some way, then I think I'd want to be bleated at. So if the version stays equal, for example, that would error here.

(`allow_non_increasing` is kind of a mouthful, so it might make sense to keep the `allow_decreases` name and just have it be slightly imprecise.)

---

_Review comment by @zanieb on `docs/guides/package.md`:95 on 2025-05-22 14:25_

Interesting. I presume there was discussion on this, but I'm on my phone so just want to flag it. Why is the patch bump needed too? We fail without it?

(I'll also just read the rest of the pull request when I'm home)

---

_Review comment by @zanieb on `docs/guides/package.md`:113 on 2025-05-22 14:25_

Oh interesting. Why do we want to allow this at all? Seems like added complexity. Is someone asking for it?

---

_@BurntSushi approved on 2025-05-22 14:25_

Nice work!!! I wish this was in Cargo. :-)

---

_Review comment by @zanieb on `docs/guides/package.md`:111 on 2025-05-22 14:26_

I would say "it would decrease the current version" not "it will" if you're going to error.

---

_@zanieb reviewed on 2025-05-22 14:32_

---

_Comment by @Xemnas0 on 2025-05-25 21:08_

Would it be possible to include a functionality of bumping the version in certain files, like [commitizen does](https://github.com/commitizen-tools/commitizen/blob/master/docs/commands/bump.md#version_files-)? It also allows to look for what lines to change according to a per-file regex

---

_Review comment by @konstin on `docs/concepts/projects/config.md`:163 on 2025-05-28 12:49_

Empty heading?

---

_Review comment by @konstin on `docs/guides/package.md`:98 on 2025-05-28 12:52_

This sounds too complex for the guide documentation, I'd guide users towards using either a single `--bump` or a stable and an unstable bump and leave the rest to the concept/reference documentation.

---

_@konstin reviewed on 2025-05-28 12:53_

---

_Comment by @Gankra on 2025-05-28 14:15_

> Would it be possible to include a functionality of bumping the version in certain files, like [commitizen does](https://github.com/commitizen-tools/commitizen/blob/master/docs/commands/bump.md#version_files-)? It also allows to look for what lines to change according to a per-file regex

I've certainly always found those kinds of features useful in similar commands, although it would require non-trivial design work we're definitely not doing in this PR.

---

_@zanieb reviewed on 2025-05-28 14:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:976 on 2025-05-28 14:42_

It's weird this ends in a question mark. We should be assertive about what they should do, or say "Did you mean ...?".

Here, it seems like


```suggestion
    error: 2.3.4 => 2.3.4a1 didn't increase the version; when bumping to a prerelease you also need to specify a release segment to increase, e.g., by providing `--bump patch`
```

---

_@zanieb reviewed on 2025-05-28 14:44_

---

_Review comment by @zanieb on `crates/uv/tests/it/version.rs`:949 on 2025-05-28 14:44_

Is there not a suggestion to provide here?

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:243 on 2025-05-28 15:06_

This is a bit confusing, it sounds like bump doesn't accept other values!

```suggestion
                "Only one pre-release kind can be provided to `--bump`, got: {...}"
```

---

_@zanieb reviewed on 2025-05-28 15:06_

---

_@zanieb reviewed on 2025-05-28 15:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:240 on 2025-05-28 15:06_

fyi we stylize as `pre-releases` everywhere outside code

---

_@zanieb reviewed on 2025-05-28 15:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:232 on 2025-05-28 15:06_

same as https://github.com/astral-sh/uv/pull/13578/files#r2112148090

---

_@zanieb reviewed on 2025-05-28 15:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:222 on 2025-05-28 15:07_

I guess?

```suggestion
                "`--bump post` cannot be combined with any other `--bump` kind"
```

---

_@zanieb reviewed on 2025-05-28 15:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:210 on 2025-05-28 15:08_

I find this kind of confusing. I might just prefer the later error?

---

_@zanieb reviewed on 2025-05-28 15:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:214 on 2025-05-28 15:09_

As with https://github.com/astral-sh/uv/pull/13578/files#r2112148090 it seems best to enumerate the values
```suggestion
                "`--bump stable` cannot be combined with any other `--bump` kind, got: {...}"
```

---

_@zanieb reviewed on 2025-05-28 15:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:222 on 2025-05-28 15:09_

Same as https://github.com/astral-sh/uv/pull/13578/files#r2112155380 really

---

_@zanieb reviewed on 2025-05-28 15:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:626 on 2025-05-28 15:10_

I would say like.. 
```suggestion
    /// Move from a pre-release to stable version (1.2.3b4.post5.dev6 => 1.2.3)
```

---

_@zanieb reviewed on 2025-05-28 15:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:620 on 2025-05-28 15:10_

We might want...
```suggestion
    /// Increase the major version (e.g., 1.2.3 => 2.0.0)
```
for all of these

---

_@zanieb reviewed on 2025-05-28 15:11_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:628 on 2025-05-28 15:11_

Why say "intentionally" here? Can you explain why it does this?

---

_@Gankra reviewed on 2025-06-02 18:03_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:628 on 2025-06-02 18:03_

The mention of `post` here can probably be thrown out as the one case where it's sus is now an error (`1.2.3.postN` => `1.2.3`). The `+local` preservation is "this is kinda orthogonal and opaque and we can't meaningfully ever manipulate it so we never touch it, even though there's an argument that it's kinda prereleasey metadata".

---

_@Gankra reviewed on 2025-06-02 18:04_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:626 on 2025-06-02 18:04_

Pedantically `1.2.3.dev6` isn't a prerelease but I think that eliding that nuance might be fine.

---

_@zanieb reviewed on 2025-06-02 18:38_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:626 on 2025-06-02 18:38_

I think that's probably fine 🙃 

---

_@konstin reviewed on 2025-06-03 09:01_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:626 on 2025-06-03 09:01_

good news, they are now: https://discuss.python.org/t/are-developmental-releases-a-type-of-pre-release/81666/13

---

_Comment by @VedankPurohit on 2025-06-05 11:40_

When will this be merged?


---

_Comment by @zanieb on 2025-06-05 13:25_

When it's done — we're a small team balancing a lot of work!

---

_Comment by @97k on 2025-07-08 09:32_

Hey @zanieb @Gankra , just checking in - do we have any ETA on this getting merged?
We've been managing dev versions manually for now, and this feature would really streamline our workflow. Totally understand you're a small team and appreciate all the work you're putting into this!

---

_@zanieb approved on 2025-07-10 13:30_

---

_Renamed from "Expand the functionality of `uv version --bump` " to "Expand the functionality of `uv version --bump` to support pre-releases" by @zanieb on 2025-07-10 13:31_

---

_Merged by @zanieb on 2025-07-10 13:45_

---

_Closed by @zanieb on 2025-07-10 13:45_

---

_Branch deleted on 2025-07-10 13:45_

---

_Comment by @tmke8 on 2025-07-10 14:25_

I wonder if it wouldn't have been better to introduce a new argument instead of allowing repeated `--bump`. It seems that the only realistic use case here combines one "stable" bump and one "unstable" bump. So, one could have a `--pre-bump` flag and write the example in the PR description like this:

* `--bump patch --pre-bump alpha` on `1.2.3` => `1.2.4a1` ("bump patch version and go to alpha 1")

And this would result in an error:

* `--bump minor --bump patch`

This would remove confusing behavior such as

> The ordering of `beta` after `alpha` means `--bump alpha --bump beta` will just result in beta 1; this is the one case where a bump request will effectively get overwritten.

`--pre-bump` would allow `alpha`, `beta`, `rc`, `dev`, and `post` as argument.

Instead of `--bump stable`, I think something like `--clear-suffixes` would be clearer, which then also makes the `--allow-decreases` flag unnecessary, because the name `--clear-suffixes` doesn't imply an increase in the version, so the user won't be surprised when `--clear-suffixes` decreases the version (which it always does, I think). ~~`--clear-suffixes` would be permitted to be used in combination with `--bump` but not with `--pre-bump` (because that makes no sense.~~ EDIT: actually, `--clear-suffixes` should not be combined with any bumps; the stable bumps imply this already.

EDIT2: one problem with the name "pre-bump" is that `.post` is not actually a pre-release. Another option might be `--bump-suffix`.

---

_Comment by @zanieb on 2025-07-10 14:41_

> The ordering of beta after alpha means --bump alpha --bump beta will just result in beta 1; this is the one case where a bump request will effectively get overwritten.

We just ban this now.

>  which then also makes the --allow-decreases flag unnecessary

We removed this, it's not worth the complexity.

---

_Comment by @tmke8 on 2025-07-10 14:52_

Okay, that doesn't sound too bad then. It just seems to me that this commandline interface is "too weakly typed" in the sense that the "signature" seems to allow many things that will actually give a runtime error.

---

_Comment by @zanieb on 2025-07-10 14:55_

I think the things that will give a runtime error are pretty weird for a user to do though, wouldn't you think?

---

_Comment by @tmke8 on 2025-07-10 15:02_

That's a good point, yes.

---

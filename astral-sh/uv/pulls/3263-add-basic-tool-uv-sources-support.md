```yaml
number: 3263
title: " Add basic `tool.uv.sources` support "
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/tool-uv-sources-analysis
created_at: 2024-04-25T11:34:05Z
updated_at: 2024-05-06T11:20:16Z
url: https://github.com/astral-sh/uv/pull/3263
synced_at: 2026-01-10T14:37:54Z
```

#  Add basic `tool.uv.sources` support 

---

_Pull request opened by @konstin on 2024-04-25 11:34_

## Introduction

PEP 621 is limited. Specifically, it lacks
* Relative path support
* Editable support
* Workspace support
* Index pinning or any sort of index specification

The semantics of urls are a custom extension, PEP 440 does not specify how to use git references or subdirectories, instead pip has a custom stringly format. We need to somehow support these while still stying compatible with PEP 621. 

## `tool.uv.source`

Drawing inspiration from cargo, poetry and rye, we add `tool.uv.sources` or (for now stub only) `tool.uv.workspace`:

```toml
[project]
name = "albatross"
version = "0.1.0"
dependencies = [
  "tqdm >=4.66.2,<5",
  "torch ==2.2.2",
  "transformers[torch] >=4.39.3,<5",
  "importlib_metadata >=7.1.0,<8; python_version < '3.10'",
  "mollymawk ==0.1.0"
]

[tool.uv.sources]
tqdm = { git = "https://github.com/tqdm/tqdm", rev = "cc372d09dcd5a5eabdc6ed4cf365bdb0be004d44" }
importlib_metadata = { url = "https://github.com/python/importlib_metadata/archive/refs/tags/v7.1.0.zip" }
torch = { index = "torch-cu118" }
mollymawk = { workspace = true }

[tool.uv.workspace]
include = [
  "packages/mollymawk"
]

[tool.uv.indexes]
torch-cu118 = "https://download.pytorch.org/whl/cu118"
```

See `docs/specifying_dependencies.md` for a detailed explanation of the format. The basic gist is that `project.dependencies` is what ends up on pypi, while `tool.uv.sources` are your non-published additions. We do support the full range or PEP 508, we just hide it in the docs and prefer the exploded table for easier readability and less confusing with actual url parts.

This format should eventually be able to subsume requirements.txt's current use cases. While we will continue to support the legacy `uv pip` interface, this is a piece of the uv's own top level interface. Together with `uv run` and a lockfile format, you should only need to write `pyproject.toml` and do `uv run`, which generates/uses/updates your lockfile behind the scenes, no more pip-style requirements involved. It also lays the groundwork for implementing index pinning.

## Changes

This PR implements:
* Reading and lowering `project.dependencies`, `project.optional-dependencies` and `tool.uv.sources` into a new requirements format, including:
  * Git dependencies
  * Url dependencies
  * Path dependencies, including relative and editable
* `pip install` integration
* Error reporting for invalid `tool.uv.sources`
* Json schema integration (works in pycharm, see below)
* Draft user-level docs (see `docs/specifying_dependencies.md`)

It does not implement:
* No `pip compile` testing, deprioritizing towards our own lockfile
* Index pinning (stub definitions only)
* Development dependencies
* Workspace support (stub definitions only)
* Overrides in pyproject.toml
* Patching/replacing dependencies

One technically breaking change is that we now require user provided pyproject.toml to be valid wrt to PEP 621. Included files still fall back to PEP 517. That means `pip install -r requirements.txt` requires it to be valid while `pip install -r requirements.txt` with `-e .` as content falls back to PEP 517 as before.

## Implementation

The `pep508` requirement is replaced by a new `UvRequirement` (name up for bikeshedding, not particularly attached to the uv prefix). The still existing `pep508_rs::Requirement` type is a url format copied from pip's requirements.txt and doesn't appropriately capture all features we want/need to support. The bulk of the diff is changing the requirement type throughout the codebase.

We still use `VerbatimUrl` in many places, where we would expect a parsed/decomposed url type, specifically:
* Reading core metadata except top level pyproject.toml files, we fail a step later instead if the url isn't supported.
* Allowed `Urls`.
* `PackageId` with a custom `CanonicalUrl` comparison, instead of canonicalizing urls eagerly.
* `PubGrubPackage`: We eventually convert the `VerbatimUrl` back to a `Dist` (`Dist::from_url`), instead of remembering the url.
* Source dist types: We use verbatim url even though we know and require that these are supported urls we can and have parsed.

I tried to make improve the situation be replacing `VerbatimUrl`, but these changes would require massive invasive changes (see e.g. https://github.com/astral-sh/uv/pull/3253). A main problem is the ref `VersionOrUrl` and applying overrides, which assume the same requirement/url type everywhere. In its current form, this PR increases this tech debt.

I've tried to split off PRs and commits, but the main refactoring is still a single monolith commit to make it compile and the tests pass.

## Demo

Adding https://gist.githubusercontent.com/konstin/68f3e5ea5e8f1a30a0c9096ae72925c1/raw/d1ae3b85d5a0f3c4bb562c7dfd38c3dafc04ec08/pyproject.json as json schema (v7) to pycharm for `pyproject.toml`, you can try the IDE support already:

![pycharm](https://github.com/astral-sh/uv/assets/6826232/599082c7-6be5-41c1-a3cd-516092382f8d)

[dove.webm](https://github.com/astral-sh/uv/assets/6826232/c293c272-c80b-459d-8c95-8c46a8d198a1)


---

_Label `enhancement` added by @konstin on 2024-04-25 11:34_

---

_Comment by @konstin on 2024-04-26 12:35_

* **#3277** <a href="https://app.graphite.dev/github/pr/astral-sh/uv/3277?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>
* **#3263** <a href="https://app.graphite.dev/github/pr/astral-sh/uv/3263?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 👈
* `main`


---

_Label `preview` added by @konstin on 2024-04-26 14:49_

---

_Review requested from @charliermarsh by @konstin on 2024-04-28 19:53_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/error.rs`:28 on 2024-04-30 04:21_

I'd probably just remove this TODO.

---

_Review comment by @charliermarsh on `crates/distribution-types/src/installed.rs`:36 on 2024-04-30 04:21_

Why boxed?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:496 on 2024-04-30 04:29_

I think all the changes in this file could've been done as a tiny standalone PR, to reduce the size of the diff.

---

_Review comment by @charliermarsh on `crates/distribution-types/src/parsed_url.rs`:80 on 2024-04-30 04:30_

Is `MissingUrlPrefix` still used?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/traits.rs`:172 on 2024-04-30 04:31_

This seems off to me... Why is `git+` not tracked in `version_or_url`?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolution.rs`:126 on 2024-04-30 04:32_

I think we should just name this `Requirement`, and use `pep508::Requirement` everywhere else, personally. I'd like to avoid `Uv` in the name of abstractions.

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolution.rs`:75 on 2024-04-30 04:32_

Should this be `impl From<&ResolvedDist> for Requirement`?

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/unnamed.rs`:80 on 2024-04-30 04:34_

Seems like the changes in this file could be done in a separate commit, even merged directly to `main`.

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:314 on 2024-04-30 04:35_

Shouldn't this be a `pep508::Requirement`?

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:480 on 2024-04-30 04:35_

Why is this changing to support Uv requirements?

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/requirement.rs`:19 on 2024-04-30 04:36_

I feel like this should still be returning `Pep508(Requirement)`? I wouldn't have expected changes to this crate given that it doesn't support uv-style requirements. 

---

_Review comment by @charliermarsh on `crates/uv-dev/src/resolve_many.rs`:145 on 2024-04-30 04:37_

We should resolve `.expect("TODO(konsti)")`.

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:1563 on 2024-04-30 04:37_

Could commit this to `main` if you want to reduce the PR.

---

_Review comment by @charliermarsh on `crates/uv-dev/src/generate_json_schema.rs`:24 on 2024-04-30 04:38_

Hmm -- why isn't this change part of `uv_workspace` directly?

---

_Review comment by @charliermarsh on `crates/uv-git/src/lib.rs`:17 on 2024-04-30 04:38_

Nit: Oxford comma after "fragments"

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:275 on 2024-04-30 04:39_

These `Unreachable` seem like a sign that something is off in the data model. How could we refactor to avoid them?

---

_Review comment by @charliermarsh on `crates/uv-installer/src/satisfies.rs`:155 on 2024-04-30 04:40_

Do you think this whole refactor could be its own, separate PR?

---

_Review comment by @charliermarsh on `crates/uv-installer/src/satisfies.rs`:20 on 2024-04-30 04:41_

Can we make this an associated method on the enum instead, for discoverability?

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/lookahead.rs`:130 on 2024-04-30 04:41_

This is blocking, no?

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/specification.rs`:207 on 2024-04-30 04:43_

Need to fix the macro formatting here.

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/specification.rs`:167 on 2024-04-30 04:43_

What does it mean to be "strict about PEP 621" here? Can you expand on this comment? E.g., we're still falling back to treating it as a source tree below.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/error.rs`:101 on 2024-04-30 04:44_

This TODO should be made more specific or addressed, IMO.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:465 on 2024-04-30 04:45_

This is blocking.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/locals.rs`:198 on 2024-04-30 04:45_

This is blocking.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/urls.rs`:47 on 2024-04-30 04:47_

Is each branch not the same here? Do we need to split this up? `UvSource::Url { url, .. } => {` and `UvSource::Path { url, .. } => {` at least look indentical.

---

_@charliermarsh reviewed on 2024-04-30 04:47_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/uv_requirement.rs`:155 on 2024-04-30 04:49_

Can you please add Rustdoc for each variant here?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/uv_requirement.rs`:124 on 2024-04-30 04:49_

What's the work involved here? (What's the goal?)

---

_Review comment by @charliermarsh on `crates/distribution-types/src/uv_requirement.rs`:122 on 2024-04-30 04:49_

Should this be `IndexUrl`?

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:201 on 2024-04-30 04:50_

Can you add Rustdoc for each variant?

---

_@charliermarsh reviewed on 2024-04-30 04:50_

---

_@charliermarsh reviewed on 2024-04-30 04:52_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolution.rs`:126 on 2024-04-30 04:52_

(I'd apply this same judgment to all the other `Uv`-prefixed types. What do you think?)

---

_@konstin reviewed on 2024-04-30 09:45_

---

_Review comment by @konstin on `crates/distribution-types/src/installed.rs`:36 on 2024-04-30 09:45_

Clippy complains that the `InstalledDist` enum has a large variant size difference otherwise due to the large `DirectUrl` type, and boxing here is minimally intrusive.

```
error: large size difference between variants
  --> crates/distribution-types/src/installed.rs:18:1
   |
18 | / pub enum InstalledDist {
19 | |     /// The distribution was derived from a registry, like `PyPI`.
20 | |     Registry(InstalledRegistryDist),
   | |     ------------------------------- the second-largest variant contains at least 56 bytes
21 | |     /// The distribution was derived from an arbitrary URL.
22 | |     Url(InstalledDirectUrlDist),
   | |     --------------------------- the largest variant contains at least 272 bytes
23 | | }
   | |_^ the entire enum is at least 272 bytes
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#large_enum_variant
   = note: `-D clippy::large-enum-variant` implied by `-D warnings`
   = help: to override `-D warnings` add `#[allow(clippy::large_enum_variant)]`
help: consider boxing the large fields to reduce the total size of the enum
   |
22 |     Url(Box<InstalledDirectUrlDist>),
   |         ~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

---

_@konstin reviewed on 2024-04-30 09:51_

---

_Review comment by @konstin on `crates/distribution-types/src/lib.rs`:496 on 2024-04-30 09:51_

Good idea: #3320

---

_@konstin reviewed on 2024-04-30 09:52_

---

_Review comment by @konstin on `crates/distribution-types/src/parsed_url.rs`:80 on 2024-04-30 09:52_

No, it's already removed: https://github.com/astral-sh/uv/pull/3263/files#diff-1a2282604ae94f9266d311e67fd6e0c365c2a76a7ef0fbd707bfc19e6dde48e5L18-L19

---

_@konstin reviewed on 2024-04-30 10:10_

---

_Review comment by @konstin on `crates/distribution-types/src/uv_requirement.rs`:124 on 2024-04-30 10:10_

You can do things like `flask==3.0.0` and `flask = { git = "https://github.com/pallets/flask", tag = "v2.0.0" }`. We should error or warn when that happens, it's another consistency validation check.

---

_@konstin reviewed on 2024-04-30 10:12_

---

_Review comment by @konstin on `crates/distribution-types/src/uv_requirement.rs`:122 on 2024-04-30 10:12_

This is still mostly to-be-designed and mainly a placeholder, but i'd expect this to become the name of an index, e.g. `torch`, which is then defined elsewhere, either in this file or in a global configuration. This doesn't have any usages yet to be easy to change once we have the design

---

_@konstin reviewed on 2024-04-30 10:18_

---

_Review comment by @konstin on `crates/distribution-types/src/traits.rs`:172 on 2024-04-30 10:18_

In `foo = { url = "https://github.com/foo/foo" }` we don't have a git prefix, while in `foo @ git+https://github.com/foo/foo`, we do. I went with the option to normalize both to no `git+` prefix, causing this oddity. (Adding the `git+` just for `.version_or_url()` doesn't work because it returns a `&'a VerbatimUrl`)

---

_@konstin reviewed on 2024-04-30 10:19_

---

_Review comment by @konstin on `crates/distribution-types/src/resolution.rs`:75 on 2024-04-30 10:19_

done

---

_@konstin reviewed on 2024-04-30 10:26_

---

_Review comment by @konstin on `crates/pep508-rs/src/unnamed.rs`:80 on 2024-04-30 10:26_

Rebase fallout from https://github.com/astral-sh/uv/pull/3186, i'll revert the changes in this file

---

_Review comment by @konstin on `crates/requirements-txt/src/requirement.rs`:19 on 2024-04-30 11:08_

I tried, but `RequirementsSpecification.requirements` is `RequirementEntry`, which has `RequirementsTxtRequirement`. Should i change `RequirementEntry` to something else?

---

_@konstin reviewed on 2024-04-30 11:08_

---

_@konstin reviewed on 2024-04-30 11:10_

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:480 on 2024-04-30 11:10_

This trickled down from `RequirementsTxtRequirement` being entangled with `RequirementsSpecification.requirements` (see the other comment), should we change this to use separate types?

---

_@konstin reviewed on 2024-04-30 11:34_

---

_Review comment by @konstin on `crates/uv-dev/src/resolve_many.rs`:145 on 2024-04-30 11:34_

thx

---

_Review comment by @konstin on `crates/uv-dev/src/generate_json_schema.rs`:24 on 2024-04-30 11:36_

I was thinking that pure configuration files shouldn't have dependency information, so keeping them separate

---

_@konstin reviewed on 2024-04-30 11:36_

---

_@konstin reviewed on 2024-04-30 11:42_

---

_Review comment by @konstin on `crates/uv-requirements/src/lookahead.rs`:130 on 2024-04-30 11:42_

Outdated comment, the condition is already fixed

---

_@konstin reviewed on 2024-04-30 11:49_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/urls.rs`:47 on 2024-04-30 11:49_

Merged the url and path branch

---

_@konstin reviewed on 2024-04-30 12:13_

---

_Review comment by @konstin on `crates/uv-requirements/src/pyproject.rs`:201 on 2024-04-30 12:13_

I wrote those targeted at users since they are exported through json schema descriptions.

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:314 on 2024-04-30 14:14_

The problem is the entanglement with `RequirementsTxtRequirement`

---

_@konstin reviewed on 2024-04-30 14:14_

---

_@konstin reviewed on 2024-04-30 14:20_

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:1563 on 2024-04-30 14:20_

https://github.com/astral-sh/uv/pull/3323

---

_@konstin reviewed on 2024-04-30 14:30_

---

_Review comment by @konstin on `crates/distribution-types/src/traits.rs`:172 on 2024-04-30 14:30_

Changed this back due to a different git fix

---

_@charliermarsh reviewed on 2024-04-30 14:44_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:314 on 2024-04-30 14:44_

Can you say more? I still wouldn’t expect that this crate (which parses requirements files) needs to know about our requirement type (which doesn’t exist in requirements files). Couldn’t we convert back in the calling crate?

---

_@konstin reviewed on 2024-04-30 15:24_

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:314 on 2024-04-30 15:24_

(this comment is sorted above the one with the more detail, sorry) I tried not changing constraints but ran into two problems, one is that we parse the requirements and then push them as constraints (<https://github.com/astral-sh/uv/blob/b18610fe78a754ef8b394e0c0eb8cd0bc2d51068/crates/requirements-txt/src/lib.rs#L478-L482>). The other is that with `Constraints::apply` taking references to requirements and constraints that must be of the same type means that we either have to change the constraints to the new requirement too or we to rework (split?) a section of the requirements.txt parsing code/types.

---

_Review comment by @konstin on `crates/distribution-types/src/resolution.rs`:126 on 2024-04-30 16:30_

Done, was surprisingly ok to change

---

_@konstin reviewed on 2024-04-30 16:30_

---

_@charliermarsh reviewed on 2024-05-01 05:08_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolution.rs`:68 on 2024-05-01 05:08_

Nit: use `::from` please.

---

_@charliermarsh reviewed on 2024-05-01 05:09_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/uv_requirement.rs`:66 on 2024-05-01 05:09_

Can you write these in the style of a docstring please? E.g.:

```
/// Display the [`Requirement`], with the intention of being shown directly to a user, rather than for inclusion in a `requirements.txt` file.
```

---

_@charliermarsh reviewed on 2024-05-01 05:11_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/requirement.rs`:40 on 2024-05-01 05:11_

Why is this not implementing display?

---

_Review comment by @charliermarsh on `crates/uv-dev/src/generate_json_schema.rs`:16 on 2024-05-01 05:13_

What does this comment mean? I think in general we should work on including fewer of these comment fragments in the PR. Comments need to stand alone and carry meaning well into the future.

---

_@charliermarsh reviewed on 2024-05-01 05:13_

---

_@charliermarsh reviewed on 2024-05-01 05:15_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/lookahead.rs`:173 on 2024-05-01 05:15_

How can we avoid having to do this conversion here? This seems like the wrong place for this logic. Why should the lookahead resolver need to know how to translate from a URL and subdirectory to a `Dist`?

---

_@charliermarsh reviewed on 2024-05-01 05:16_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/specification.rs`:131 on 2024-05-01 05:16_

What does this comment mean? It needs more context, if I can't understand what it's referring to now I definitely won't be able to in the future!

---

_@charliermarsh reviewed on 2024-05-01 05:20_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:518 on 2024-05-01 05:20_

I think you should be using `VerbatimUrl::parse_path` here? Why not? Why parse the URL out-of-band like this?

---

_@charliermarsh reviewed on 2024-05-01 05:20_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:45 on 2024-05-01 05:20_

Can we just call this `LoweringError`? I want to avoid having `Uv` in so many struct and abstraction names.

---

_@charliermarsh reviewed on 2024-05-01 05:21_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:214 on 2024-05-01 05:21_

Why did this get renamed? Because the requirement type changed? `Pep621Metadata` still better expresses (to me) what this is. `UvMetadata` could be one of many different things, IMO.

---

_@charliermarsh reviewed on 2024-05-01 05:22_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:373 on 2024-05-01 05:22_

In the sources design, what if we need to support multiple entries for a single package? Is it possible? What if I want to use a different index URL depending on whether I'm on mac or Linux?

---

_@charliermarsh reviewed on 2024-05-01 05:23_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:193 on 2024-05-01 05:23_

What do this mean? We need to include way more context in these comments please!

---

_@charliermarsh reviewed on 2024-05-01 05:24_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:233 on 2024-05-01 05:24_

Hmm, I'm not a big fan of what's happening in this branch... I don't like that we're having to recreate these URLs all over the place. Also don't like how much `VerbatimUrl` conversion and URL cloning and deref and such that we're needing to do. What do you see as the better / correct solution here?

---

_@charliermarsh reviewed on 2024-05-01 05:25_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:458 on 2024-05-01 05:25_

Can we call this `requirements`? Again, I'd like to avoid encoding `uv_` in various places.

---

_@charliermarsh reviewed on 2024-05-01 05:27_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/uv_requirement.rs`:52 on 2024-05-01 05:27_

Why does this take a reference and not an owned value? Can't we use the value directly for `ParsedLocalFileUrl` at least?

---

_@charliermarsh reviewed on 2024-05-01 05:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1012 on 2024-05-01 05:29_

Any idea how we could avoid having to do this conversion in this loop...? 

---

_@charliermarsh reviewed on 2024-05-01 05:30_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:79 on 2024-05-01 05:30_

I think this should now be renamed to `requirement`, since `UvRequirement` was removed?

---

_@charliermarsh reviewed on 2024-05-01 05:31_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/uv_requirement.rs`:124 on 2024-05-01 05:31_

I see, thanks.

---

_Comment by @charliermarsh on 2024-05-01 05:33_

Another round of comments. I don't really expect to ask for any major changes, but I still have two _slightly_ larger topics to resolve:

1. I still kinda feel like `requirements-txt` should _not_ rely on the uv-specific requirement types.
2. I'm trying to understand what a better setup would look like to allow us to avoid all the URL reconstruction and deconstruction throughout the codebase.

---

_@konstin reviewed on 2024-05-02 11:38_

---

_Review comment by @konstin on `crates/requirements-txt/src/requirement.rs`:40 on 2024-05-02 11:38_

Which part? There's a `impl Display for RequirementsTxtRequirement`

---

_@konstin reviewed on 2024-05-02 11:48_

---

_Review comment by @konstin on `crates/uv-requirements/src/pyproject.rs`:373 on 2024-05-02 11:48_

toml does not allow redefining keys, so we can't use the METADATA email method of repeating keys. In the future, we can allow making `Source` an array (through an untagged enum):
```toml
foo = [
    { url = "https://linux.example.org/foo-2.0.0-cp39-cp39-linux_x86_64.whl", os = "linux" },
    { url = "https://linux.example.org/foo-2.0.0-cp39-cp39-win_amd64.whl", os = "windows" }
]
```
I'd leave this as future work.

---

_@konstin reviewed on 2024-05-02 12:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/dependencies.rs`:233 on 2024-05-02 12:35_

I've front-loaded this by putting a `VerbatimUrl` with the joined url on the url variants of `RequirementSource`.

---

_@konstin reviewed on 2024-05-02 13:05_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1012 on 2024-05-02 13:05_

Only by changing the type of `requires_dist`, if we want that. Currently the type mirrors the spec in that all urls are allowed, and we fail here if it's a url we don't support.

---

_Review comment by @konstin on `crates/uv-requirements/src/lookahead.rs`:173 on 2024-05-02 13:11_

I've front-loaded this by putting a `VerbatimUrl` with the joined url on the url variants of `RequirementSource`.

---

_@konstin reviewed on 2024-05-02 13:11_

---

_Review comment by @konstin on `crates/uv-requirements/src/pyproject.rs`:518 on 2024-05-02 13:19_

Thanks, didn't realize that method exists

---

_@konstin reviewed on 2024-05-02 13:19_

---

_Comment by @konstin on 2024-05-03 10:22_

> I still kinda feel like requirements-txt should not rely on the uv-specific requirement types.

Fixed by introducing `UnresolvedRequirementSpecification` and `UnresolvedRequirement` in `distribution-types`. It's a bit odd at first because `requirements-txt` depends on `distribution-types`, but it avoid cyclical crate dependencies and does the split.

---

_@konstin reviewed on 2024-05-03 10:42_

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:187 on 2024-05-03 10:42_

This is technically a breaking change, if we consider `pip compile`/`pip install` on a PEP 621 violating pyproject.toml a supported use case. Note that we still fall back for indirect pyproject.toml files (see test below), so e.g. a path dep on a hatch project with relative paths still works.

---

_@charliermarsh reviewed on 2024-05-03 20:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:5227 on 2024-05-03 20:20_

I feel like this error message got worse, can we restore it?

---

_@charliermarsh reviewed on 2024-05-03 20:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:5227 on 2024-05-03 20:20_

At least the part at the end.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:238 on 2024-05-03 20:55_

Why lose the verbatim here?

---

_@charliermarsh reviewed on 2024-05-03 20:56_

---

_@charliermarsh approved on 2024-05-03 21:02_

---

_Merged by @charliermarsh on 2024-05-03 21:10_

---

_Closed by @charliermarsh on 2024-05-03 21:10_

---

_Branch deleted on 2024-05-03 21:10_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:429 on 2024-05-04 02:48_

This is incorrect. Don't we need the actual string here?

---

_@charliermarsh reviewed on 2024-05-04 02:48_

---

_@charliermarsh reviewed on 2024-05-04 02:48_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:428 on 2024-05-04 02:48_

The docs on `RequirementSource::Git` say it's the URL _without_ the `git+` prefix. Is this correct?

---

_@charliermarsh reviewed on 2024-05-04 02:49_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/satisfies.rs`:64 on 2024-05-04 02:49_

This seems off. Shouldn't we instead parse the URL, and use `CanonicalUrl` to compare these? Why are we comparing as strings?

---

_@charliermarsh reviewed on 2024-05-04 02:54_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:429 on 2024-05-04 02:54_

@konstin - I guess we need to find a way to preserve the exact string from the TOML, right? Otherwise, how will we preserve environment variables?

---

_@charliermarsh reviewed on 2024-05-04 02:54_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:428 on 2024-05-04 02:54_

Fixed here: https://github.com/astral-sh/uv/pull/3365

---

_@konstin reviewed on 2024-05-06 10:56_

---

_Review comment by @konstin on `crates/uv-requirements/src/pyproject.rs`:429 on 2024-05-06 10:56_

imho toml shouldn't support env vars

---

_@konstin reviewed on 2024-05-06 10:57_

---

_Review comment by @konstin on `crates/uv-requirements/src/pyproject.rs`:428 on 2024-05-06 10:57_

Thanks

---

_@konstin reviewed on 2024-05-06 10:57_

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:64 on 2024-05-06 10:57_

(Fixed in https://github.com/astral-sh/uv/pull/3373)

---

_@konstin reviewed on 2024-05-06 11:20_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:5227 on 2024-05-06 11:20_

https://github.com/astral-sh/uv/pull/3403

I'd also like to track the origin of the invalid url, but that's a larger task.

---

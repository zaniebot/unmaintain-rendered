```yaml
number: 16523
title: Add SBOM export support
type: pull_request
state: merged
author: thomasschafer
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: sbom-export-support
created_at: 2025-10-30T19:26:41Z
updated_at: 2025-11-20T21:27:19Z
url: https://github.com/astral-sh/uv/pull/16523
synced_at: 2026-01-10T05:58:11Z
```

# Add SBOM export support

---

_Pull request opened by @thomasschafer on 2025-10-30 19:26_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds a new SBOM format (CycloneDX v1.5 JSON) to the `uv export` command.

One notable point about the implementation is the use of a synthetic root when using the `--all-packages` flag. This has been discussed separately in more detail, but on a high level, it is possible for workspace packages to be disconnected from the workspace root, so if we had the workspace root as the root component in the SBOM then in such cases there would be unreachable components, which causes issues with some SBOM tooling. By having a synthetic root we ensure that all components can be reached by traversing from the root of the SBOM.

<img width="1346" height="1210" alt="Screenshot 2025-10-30 at 17 38 49" src="https://github.com/user-attachments/assets/98439273-91de-4874-b5c9-cae1bc4a1d62" />

Resolves https://github.com/astral-sh/uv/issues/6012

## Test Plan

We've tested manually using a variety of uv projects locally, and have added a variety of tests to `crates/uv/tests/it/export.rs`.


---

_Review requested from @woodruffw by @konstin on 2025-10-31 09:37_

---

_Marked ready for review by @thomasschafer on 2025-10-31 09:49_

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:174 on 2025-10-31 19:07_

Hmm, is this from `PackageType`? If so I think we can remove this and `derive(Clone)` on `PackageType`, since it's a trivially clonable enum (all variants are bare or references).

```suggestion
```

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:413 on 2025-10-31 19:07_

Per above, this should work 🙂 

```suggestion
#[derive(Copy, Clone, Debug, Eq, PartialEq)]
```

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:185 on 2025-10-31 19:10_

Q: why not `Component::new` here? That would save a lot of lines of `None`s:

https://docs.rs/cyclonedx-bom/0.8.0/cyclonedx_bom/models/component/struct.Component.html#method.new

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:249 on 2025-10-31 19:16_

Same question about `Component::new`, although maybe it's not a good fit in this case because of `properties` 🙂 

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:295 on 2025-10-31 19:17_

Nit: I think this `or_else` could be placed directly inside the match body, unless I'm missing something -- it could be the evaluation of the fallthrough `_` pattern, rather than a separate call afterwards.

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:340 on 2025-10-31 19:19_

Flagging: I don't know uv workspaces well enough to know if this is right 🙂 -- @zanieb or @konstin should confirm this!

---

_Review comment by @woodruffw on `crates/uv/src/commands/project/export.rs`:372 on 2025-10-31 19:25_

Q: instead of building an intermediate buffer + string, why not send `export.output_as_json_v1_5` directly to `writer`? If I'm following right, that should avoid rebuffering and allocations here.

Ref: https://docs.rs/cyclonedx-bom/0.8.0/cyclonedx_bom/models/bom/struct.Bom.html#method.output_as_json_v1_5

---

_@woodruffw reviewed on 2025-10-31 19:26_

Thanks! Some questions and nitpicks 🙂 

---

_@thomasschafer reviewed on 2025-10-31 22:59_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:185 on 2025-10-31 22:59_

Unfortunately `Component::new` requires us to pass a value for `version`, but in this case we want it to be `None` - there are also no other constructor methods we could use instead (as far as I could see)

---

_@thomasschafer reviewed on 2025-10-31 23:08_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:249 on 2025-10-31 23:08_

We could make it mutable and manually set `properties` and `purl` which can't be passed in via `Component::new`, but also `package.id.version` is an option while `Component::new` requires us to pass in a `&str`, so we'd need to do something like

```rust
let mut component = Component::new(Classification::Library, name, "", Some(bom_ref));
component.version = version.as_deref().map(NormalizedString::new);
component.purl = purl;
component.properties = if !properties.is_empty() {
    Some(Properties(properties))
} else {
    None
};
component
```

and just having the verbosity of the manual construction felt a bit cleaner! Open to changing this though if you prefer

---

_@thomasschafer reviewed on 2025-10-31 23:12_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:295 on 2025-10-31 23:12_

The branch above i.e.

```rust
        [single_root] => nodes
            .iter()
            .find(|node| &node.package.id.name == *single_root)
            .map(|node| node.package),
```

returns an `Option<&Package>` so the fallback applies to that too - thought it was cleaner to have the fallback in one place rather than duplicated in both arms

---

_Review comment by @thomasschafer on `crates/uv/src/commands/project/export.rs`:372 on 2025-10-31 23:24_

Had a go at this but seeing the error below - I think it's because `output_as_json_v1_5` has a bound of `std::io::Write` which `OutputWriter` doesn't implement, it just has `write_fmt` defined

```shell
$ cargo check
    Checking uv v0.9.7 (/Users/thomasschafer/Development/uv/crates/uv)
error[E0277]: the trait bound `OutputWriter<'_>: std::io::Write` is not satisfied
   --> crates/uv/src/commands/project/export.rs:367:40
    |
367 |             export.output_as_json_v1_5(&mut writer)?;
    |                    ------------------- ^^^^^^^^^^^ the trait `std::io::Write` is not implemented for `OutputWriter<'_>`
    |                    |
    |                    required by a bound introduced by this call
    |
note: required by a bound in `cyclonedx_bom::models::bom::Bom::output_as_json_v1_5`
   --> /Users/thomasschafer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/cyclonedx-bom-0.8.0/src/models/bom.rs:315:35
    |
315 |     pub fn output_as_json_v1_5<W: std::io::Write>(
    |                                   ^^^^^^^^^^^^^^ required by this bound in `Bom::output_as_json_v1_5`

For more information about this error, try `rustc --explain E0277`.
error: could not compile `uv` (lib) due to 1 previous error
```

I could implement `Write` for `OutputWriter` if you think it's worth it? (Or I could be missing something else, let me know!)

---

_@thomasschafer reviewed on 2025-10-31 23:24_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:413 on 2025-10-31 23:26_

Done!

---

_@thomasschafer reviewed on 2025-10-31 23:26_

---

_@thomasschafer reviewed on 2025-10-31 23:27_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:174 on 2025-10-31 23:27_

Yes it is from `PackageType` - good point, derived `Copy` as mentioned below

---

_@thomasschafer reviewed on 2025-10-31 23:37_

---

_Review comment by @thomasschafer on `crates/uv/src/commands/project/export.rs`:372 on 2025-10-31 23:37_

Was fairly straightforward to implement `Write` for `OutputWriter` and I could then remove the `write_fmt` method so pushed that up, let me know what you think

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:249 on 2025-11-03 17:09_

Nope, makes sense! A single initialization is definitely a lot better than a `mut` + setters.

---

_@woodruffw reviewed on 2025-11-03 17:09_

---

_@woodruffw reviewed on 2025-11-03 17:10_

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:295 on 2025-11-03 17:10_

Oh, makes sense! Yeah, that's fine by me then 🙂 

---

_Review comment by @woodruffw on `crates/uv/src/commands/project/export.rs`:372 on 2025-11-03 17:15_

Ah yeah, I missed that this was our own `OutputWriter` type. I have no objection to doing this all through a `Write` impl., but @konstin or @zanieb might have context on why this was done with a explicit `write_fmt` implementation before 🙂 

---

_@woodruffw reviewed on 2025-11-03 17:15_

---

_Label `enhancement` added by @woodruffw on 2025-11-03 17:18_

---

_Label `cli` added by @woodruffw on 2025-11-03 17:18_

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:4895 on 2025-11-04 19:44_

Thinking out loud: these snapshots are pretty large, in part because `anyio==3.7.0` has a nontrivial number of dependencies.

We definitely want to exercise the dependency paths here, but maybe we could pick a slimmer package to use for testcases here? Some candidates:

* `urllib3` has fewer dependencies (particularly when no extras are used)
* `cryptography` has very few dependencies by default, but has extras we could use to exercise dependency cases. It has relatively large wheels, however.

---

_@woodruffw reviewed on 2025-11-04 19:44_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:340 on 2025-11-05 19:39_

That's correct, but can we rewrite this code so it can't panic? If there's a bug elsewhere, we shouldn't panic (at least outside `debug_assert`), but raise a useful error.

---

_@konstin reviewed on 2025-11-05 19:39_

---

_@konstin reviewed on 2025-11-05 20:33_

---

_Review comment by @konstin on `crates/uv/src/commands/project/export.rs`:372 on 2025-11-05 20:33_

I like the new version better, the old one was kinda hacky.

---

_@thomasschafer reviewed on 2025-11-05 22:52_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:340 on 2025-11-05 22:52_

Sure, done!

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:4895 on 2025-11-06 09:38_

Makes sense, I've replaced instances of `anyio` with `urllib3` in the cyclonedx tests

---

_@thomasschafer reviewed on 2025-11-06 09:38_

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:5057 on 2025-11-10 19:56_

Can we pick a smaller dep tree here too?

`urllib3` has tagged releases on GitHub that would probably suffice 🙂 

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:5294 on 2025-11-10 19:56_

Same dep tree question as above!

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:6773 on 2025-11-10 20:03_

Maybe `cryptography`? That should allow us to exercise wheel markers but it has fewer dependencies.

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:7004 on 2025-11-10 20:04_

`cryptograhy[ssh]` perhaps?

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:7204 on 2025-11-10 20:05_

Nitpick: I think we can remove this, assuming it was here for cross-referencing when writing the test originally 🙂 

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:7188 on 2025-11-10 20:06_

Should this be pinned to an exact version? I imagine the snapshot will go stale here over time unless we pin.

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:7602 on 2025-11-10 20:09_

I'm fine with this if there isn't another easy example, but it'd be great to pick a pair with a smaller dependency closure here (to keep the snapshot minimal).

---

_@woodruffw reviewed on 2025-11-10 20:15_

Thanks @thomasschafer! I flagged a couple of additional places in the snapshots that IMO would benefit from smaller trees/more hermeticity, but otherwise this looks pretty good to me.

---

_@woodruffw reviewed on 2025-11-10 20:24_

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-10 20:24_

I saw there was some discussion of this in https://github.com/astral-sh/uv/issues/6012, but I'm still murky on why this should be a `uv:` prefixed property instead of something more standard/uniform for all Python packaging -- the markers in question here are PEP 508 markers, which uv doesn't define the semantics for.

(That's not a "no" so much as an "I'd like to understand this" 🙂)


---

_@zanieb reviewed on 2025-11-10 20:26_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-10 20:26_

ref https://github.com/CycloneDX/cyclonedx-property-taxonomy/pull/142

---

_@zanieb reviewed on 2025-11-10 20:28_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-10 20:28_

I don't understand the points made there. I don't think there's a clear understanding upstream about what these markers mean?

---

_@woodruffw reviewed on 2025-11-10 20:37_

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-10 20:37_

Yeah, I don't understand those either -- there seems to be existing precedent in their own markers for things that describe resolution as it occurred (like `cdx:python:package:required-extra`), so I don't think I understand what makes PEP 508 markers unique.

---

_@thomasschafer reviewed on 2025-11-13 12:42_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:7204 on 2025-11-13 12:42_

I pretty much copied the test [here](https://github.com/astral-sh/uv/blob/c167146f8/crates/uv/tests/it/export.rs#L364-L373) (and updated to export an SBOM) which included that comment - removed now

---

_@thomasschafer reviewed on 2025-11-13 12:57_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-13 12:57_

Sorry for the slow reply here - yes, I'm still not sure why they are so reluctant to have it under `cdx:python`. I think their point is that an SBOM should be specific to the final, shippable artifact, and not contain any sort of conditional inclusion. This might work for e.g. `sys_platform` markers, as we could just generate the SBOM for the relevant platform and then exclude the markers, but there are lots of other markers and I'm not sure how feasible or desirable it is to resolve all of them when generating an SBOM - what do you think? Also I'm guessing that many uv users would rather have a more complete SBOM containing the markers. It just seems like the cyclonedx-property-taxonomy maintainers are completely opposed to having markers though and I'm not sure if I'll be able to convince them otherwise 😅 We can restart that conversation in https://github.com/CycloneDX/cyclonedx-property-taxonomy/pull/142 though if you think it would be worth it!

---

_@thomasschafer reviewed on 2025-11-13 13:01_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:7188 on 2025-11-13 13:01_

Good point, pinned (also updated other tests where a pinned version was missing). However, there are a bunch of tests that I didn't add which also don't have pinned versions e.g. [here](https://github.com/astral-sh/uv/blob/e28dc6235/crates/uv/tests/it/export.rs#L123) - want me to update and pin those in this PR too?

---

_@woodruffw reviewed on 2025-11-13 16:39_

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:7188 on 2025-11-13 16:39_

Oh hm, didn't realize we had others too. I think it's probably fine to leave the unrelated ones as-is, I'd rather keep the diff in this PR minimal 🙂 



---

_@thomasschafer reviewed on 2025-11-13 16:52_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:7188 on 2025-11-13 16:52_

Okay makes sense

---

_@thomasschafer reviewed on 2025-11-13 17:57_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:5057 on 2025-11-13 17:57_

Done!

---

_@thomasschafer reviewed on 2025-11-13 17:58_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:5294 on 2025-11-13 17:58_

Updated

---

_@thomasschafer reviewed on 2025-11-13 17:58_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:6773 on 2025-11-13 17:58_

Done

---

_@thomasschafer reviewed on 2025-11-13 17:58_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:7004 on 2025-11-13 17:58_

Done

---

_@thomasschafer reviewed on 2025-11-13 18:01_

---

_Review comment by @thomasschafer on `crates/uv/tests/it/export.rs`:7602 on 2025-11-13 18:01_

I couldn't find a pair that would result in a smaller snapshot from a bit of testing. If you know of any examples let me know!

---

_Review requested from @woodruffw by @thomasschafer on 2025-11-13 18:03_

---

_@woodruffw reviewed on 2025-11-14 15:28_

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-14 15:28_

We discussed this internally, and I think it's okay to keep the `uv:` namespace for now -- it's arguably pretty confusing given that markers are a fully standard aspect of Python packaging, but I think we'd rather ship this as a preview feature sooner rather than later and not depend on reaching a consensus 🙂 

(We could also always revisit this, if users notice and are confused/complain about the incorrect taxonomy here.)

---

_@woodruffw reviewed on 2025-11-14 15:28_

---

_Review comment by @woodruffw on `crates/uv/tests/it/export.rs`:7602 on 2025-11-14 15:28_

No worries, thanks for checking!

---

_@woodruffw approved on 2025-11-14 15:45_

Thanks @thomasschafer! This looks good to me; I also ran it locally and confirmed that the output and preview warning render as expected.

I'd like @zanieb or @konstin to do a last review as well for idioms if either has time 🙂 

---

_@thomasschafer reviewed on 2025-11-14 16:24_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-14 16:24_

Okay sounds good! If we're going with the `uv` namespace, I can optionally create a PR in https://github.com/CycloneDX/cyclonedx-property-taxonomy to add this to the registered top-level namespaces with a link to some uv docs containing a taxonomy, something like [this](https://github.com/CycloneDX/cyclonedx-property-taxonomy/pull/27) - what do you think? This isn't a hard requirement, but some users might look for it. If you do think it would be worth adding, where do you think would be best to add this taxonomy - I could add a page to the docs (in this or another PR) and then link to that?

---

_Review comment by @woodruffw on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-17 16:45_

> This isn't a hard requirement, but some users might look for it. If you do think it would be worth adding, where do you think would be best to add this taxonomy - I could add a page to the docs (in this or another PR) and then link to that?

My (weak) 0.02c would be for not adding it -- I don't think we want to establish a formal API contract/expectation around this namespace, since it would ideally be part of the `cdx:python:` taxonomy in the future.

---

_@woodruffw reviewed on 2025-11-17 16:45_

---

_Review comment by @konstin on `docs/guides/export.md`:84 on 2025-11-17 20:55_

```suggestion
    Support for exporting to CycloneDX is in [preview](../concepts/preview.md), and may change in any future release.
```



---

_Review comment by @konstin on `docs/guides/export.md`:62 on 2025-11-17 21:03_

There's a number of cases where users need to export `requirements.txt`, this is something that's totally fine to do if you need to interact with a tool that only understand `requirements.txt`.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:4 on 2025-11-17 21:07_

We don't nest imports in uv

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:62 on 2025-11-17 21:10_

The format in the implementation had id and name flipped

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:93 on 2025-11-17 21:14_

This can use `unwrap_or_default()`, or some form of matching, I find this one a bit hard to follow.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:96 on 2025-11-17 21:15_

Why is this type always pypi, even if we're donwloading from a direct URL, can you add a comment?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:322 on 2025-11-17 21:25_

We should use a sorted collection here or sort later to ensure the order is deterministic

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:303 on 2025-11-17 21:25_

Do we need this `is_local` check?

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4548 on 2025-11-17 21:31_

nit: if we put CycloneDX in a code block, it looks like it's the flag value, while for this case, name and flag value are different.

---

_@konstin reviewed on 2025-11-17 21:38_

Only minor things. Overall, really good work for a first uv PR!

---

_@thomasschafer reviewed on 2025-11-18 10:14_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:215 on 2025-11-18 10:14_

Okay this makes sense!

---

_Review comment by @thomasschafer on `docs/guides/export.md`:62 on 2025-11-18 11:21_

This is just copied across from [here](https://github.com/astral-sh/uv/blob/512c0ca5edc34f4eb932d5dd59cc86f842a8817c/docs/concepts/projects/sync.md?plain=1#L193-L194), originally added [here](https://github.com/astral-sh/uv/pull/6778/files#diff-bdfe85a7e0c476def2c69fd9e8de130db9d68a453c7412f97e0809b1a8695697) - happy to remove it though if you think it's no longer needed! 

---

_@thomasschafer reviewed on 2025-11-18 11:21_

---

_@thomasschafer reviewed on 2025-11-18 12:06_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:96 on 2025-11-18 12:06_

Added a comment to explain! I also updated the logic slightly to add a `repository_url` qualifier for `Registry` sources where the URL is not `https://pypi.org/...`

---

_@thomasschafer reviewed on 2025-11-18 12:07_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:62 on 2025-11-18 12:07_

Ah good spot! Fixed

---

_@thomasschafer reviewed on 2025-11-18 12:07_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:4 on 2025-11-18 12:07_

Ah I missed that - updated

---

_@thomasschafer reviewed on 2025-11-18 12:08_

---

_Review comment by @thomasschafer on `docs/guides/export.md`:84 on 2025-11-18 12:08_

Updated

---

_@thomasschafer reviewed on 2025-11-18 12:08_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:93 on 2025-11-18 12:08_

Updated to

```rust
        let version = Self::get_version_string(package)
            .map(|v| format!("@{}", percent_encode(v.as_bytes(), PURL_ENCODE_SET)))
            .unwrap_or_default();
```

Let me know if that's any better, happy to change if not!

---

_@thomasschafer reviewed on 2025-11-18 12:09_

---

_Review comment by @thomasschafer on `crates/uv-cli/src/lib.rs`:4548 on 2025-11-18 12:09_

Fixed - had to add an `#[allow(clippy::doc_markdown)]` as clippy was complaining otherwise

---

_@thomasschafer reviewed on 2025-11-18 12:19_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:322 on 2025-11-18 12:19_

Added a sort later in the `if all_packages {` branch, which is the only place we are actually adding `workspace_member_ids` to the SBOM - elsewhere we are just performing lookups

---

_@thomasschafer reviewed on 2025-11-18 12:23_

---

_Review comment by @thomasschafer on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:303 on 2025-11-18 12:23_

Good point, I guess actually we don't as all workspace members will be local. Removed!

---

_Review comment by @konstin on `docs/guides/export.md`:62 on 2025-11-18 13:15_

oh ok, in this case we don't need to change it in this PR and can keep it copied verbatim.

---

_@konstin reviewed on 2025-11-18 13:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/cyclonedx_json.rs`:96 on 2025-11-18 13:16_

great comment!

---

_@konstin reviewed on 2025-11-18 13:16_

---

_Comment by @thomasschafer on 2025-11-20 15:33_

Thank you both for the comments @konstin and @woodruffw ! Anything else for me to do here?

---

_@woodruffw approved on 2025-11-20 17:52_

Thanks a ton @thomasschafer! We really appreciate your hard work on this.

---

_Merged by @woodruffw on 2025-11-20 17:52_

---

_Closed by @woodruffw on 2025-11-20 17:52_

---

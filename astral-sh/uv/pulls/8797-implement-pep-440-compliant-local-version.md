```yaml
number: 8797
title: Implement PEP 440-compliant local version semantics
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: tracking/050
head: local-version-semantics
created_at: 2024-11-04T06:07:46Z
updated_at: 2024-11-10T08:27:07Z
url: https://github.com/astral-sh/uv/pull/8797
synced_at: 2026-01-10T12:00:00Z
```

# Implement PEP 440-compliant local version semantics

---

_Pull request opened by @ericmarkmartin on 2024-11-04 06:07_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Implement a full working version of local version semantics. The (AFAIA) major move towards this was implemented in #2430. This added support such that the version specifier `torch==2.1.0+cpu` would install `torch@2.1.0+cpu` and consider `torch@2.1.0+cpu` a valid way to satisfy the requirement `torch==2.1.0` in further dependency resolution.

In this feature, we more fully support local version semantics. Namely, we now allow `torch==2.1.0` to install `torch@2.1.0+cpu` regardless of whether `torch@2.1.0` (no local tag) actually exists.

We do this by adding an internal-only `Max` value to local versions that compare greater to all other local versions. Then we can translate `torch==2.1.0` into bounds: greater than 2.1.0 with no local tag and less than 2.1.0 with the `Max` local tag.

## Test Plan

Depends on https://github.com/astral-sh/packse/pull/227.


---

_Converted to draft by @ericmarkmartin on 2024-11-04 06:07_

---

_Comment by @ericmarkmartin on 2024-11-04 06:11_

@charliermarsh I took a stab at this. I didn't spend a ton of time testing but I think it's maybe right? If it is, the main issue here might be performance---after this change, `==` bounds get translated into a bounded range rather than a singleton, and the upper bound of the range has a local version (the `Max` sentinel value) so it has to be a `VersionFull`. This is maybe a bit sad for the number of slow comparisons it introduces. Maybe there's a way to encode the local version sentinel in `VersionSmall` such that we don't have to suffer this?

---

_Comment by @charliermarsh on 2024-11-04 13:43_

On first glance I think this approach makes a lot of sense? Clever! I need to review it more closely though, and we should indeed add something to `VersionSmall` to capture the sentinel, I think.

---

_Comment by @charliermarsh on 2024-11-04 13:48_

I will poke around at this, if that's cool -- see if I can get it passing.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-04 13:51_

---

_Comment by @ericmarkmartin on 2024-11-04 14:33_

Poke away! I took a whirl at trying to detect the weird synthesized bounds with the local version max and rewriting it back to a normal-looking equality in report.rs but didn't have much luck

---

_Comment by @charliermarsh on 2024-11-04 15:00_

👍 I think I know how to solve it, or at least have some ideas.

---

_Comment by @ericmarkmartin on 2024-11-04 15:06_

Another thing to think about--do we still want the #2430 mechanism? I don't think we need it after this change, but it's also clearly not hurting us. I could imagine it's still a good perf optimization but I'm not familiar enough with the resolver to make a claim either way

---

_Comment by @charliermarsh on 2024-11-04 15:09_

Did you link to the wrong PR? Heh

---

_Comment by @charliermarsh on 2024-11-04 15:11_

Maybe you're thinking of the version selection mechanism where we check if we're requesting an exact-equals version?

---

_Comment by @ericmarkmartin on 2024-11-04 15:21_

> Did you link to the wrong PR? Heh

Indeed! I mixed up #2340 and #2430--whoops! I edited it in the comment above

---

_Comment by @charliermarsh on 2024-11-04 20:31_

I almost have tests passing.

---

_Comment by @ericmarkmartin on 2024-11-04 20:42_

Nice! I realize that I used `LocalVersion::Max` but `LocalVersionSlice::Sentinel`---do you have a preference?

---

_Comment by @charliermarsh on 2024-11-04 21:07_

Probably a minor preference for `Max` because it's similar to the "max" we use elsewhere in `Version`.

---

_Comment by @charliermarsh on 2024-11-04 21:13_

Next I will try tearing out all the special-casing around local versions.

---

_Comment by @charliermarsh on 2024-11-04 21:23_

(I'll stop force-pushing to this branch and merge from now on so we can collab.)

---

_Comment by @charliermarsh on 2024-11-04 21:33_

I need to update Packse to change some of the auto-generated scenario expectations.

---

_Comment by @charliermarsh on 2024-11-04 23:11_

I think the main thing missing here is a few more Packse tests (I can write) plus a solution to the `VersionFull` thing.

---

_Comment by @charliermarsh on 2024-11-04 23:14_

@ericmarkmartin -- Do you think it's possible for us to somehow reuse the `max` / `SUFFIX_MAX` concept that already exists on `VersionSmall`? It's already "the version after all local versions".

---

_Comment by @charliermarsh on 2024-11-04 23:36_

> Do you think it's possible for us to somehow reuse the `max` / `SUFFIX_MAX` concept that already exists on `VersionSmall`? It's already "the version after all local versions".

I think _not_, because we don't want `foo==1.0.0` to match `1.0.0.post1`.

---

_Marked ready for review by @charliermarsh on 2024-11-05 01:47_

---

_Renamed from "WIP: local version semantics" to "Implement PEP 440-compliant local version semantics" by @charliermarsh on 2024-11-05 01:47_

---

_Label `enhancement` added by @charliermarsh on 2024-11-05 01:47_

---

_Label `compatibility` added by @charliermarsh on 2024-11-05 01:47_

---

_Comment by @charliermarsh on 2024-11-05 01:55_

We can probably just ship this without changes to `VersionSmall`, and handle those later.

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-11-05 01:55_

---

_Comment by @ericmarkmartin on 2024-11-05 02:39_

Are we sure there's no serious perf regressions?

---

_Comment by @ericmarkmartin on 2024-11-05 02:41_

> > Do you think it's possible for us to somehow reuse the `max` / `SUFFIX_MAX` concept that already exists on `VersionSmall`? It's already "the version after all local versions".
> 
> I think _not_, because we don't want `foo==1.0.0` to match `1.0.0.post1`.

Can we do `[1.0.0, 1.0.0.post1)`?

---

_Comment by @ericmarkmartin on 2024-11-05 02:42_

> Another thing to think about--do we still want the #2430 mechanism? I don't think we need it after this change, but it's also clearly not hurting us. I could imagine it's still a good perf optimization but I'm not familiar enough with the resolver to make a claim either way

Also did we settle on something here?

---

_Comment by @charliermarsh on 2024-11-05 02:42_

We roughly want to do this (it's a little trickier than past changes, e.g., the change to add `min` or `max`, because we're out of "space" going from 3 bits to 4 bits):

```diff
diff --git a/crates/uv-pep440/src/version.rs b/crates/uv-pep440/src/version.rs
index 26b3831ec..f7448137f 100644
--- a/crates/uv-pep440/src/version.rs
+++ b/crates/uv-pep440/src/version.rs
@@ -835,16 +835,16 @@ impl FromStr for Version {
 /// * Bytes 5, 4 and 3 correspond to the second, third and fourth release
 ///   segments, respectively.
 /// * Bytes 2, 1 and 0 represent *one* of the following:
-///   `min, .devN, aN, bN, rcN, <no suffix>, .postN, max`.
+///   `min, .devN, aN, bN, rcN, <no suffix>, local, .postN, max`.
 ///   Its representation is thus:
-///   * The most significant 3 bits of Byte 2 corresponds to a value in
-///     the range 0-6 inclusive, corresponding to min, dev, pre-a, pre-b, pre-rc,
+///   * The most significant 4 bits of Byte 2 corresponds to a value in
+///     the range 0-8 inclusive, corresponding to min, dev, pre-a, pre-b, pre-rc,
 ///     no-suffix or post releases, respectively. `min` is a special version that
 ///     does not exist in PEP 440, but is used here to represent the smallest
 ///     possible version, preceding any `dev`, `pre`, `post` or releases. `max` is
 ///     an analogous concept for the largest possible version, following any `post`
 ///     or local releases.
-///   * The low 5 bits combined with the bits in bytes 1 and 0 correspond
+///   * The low 4 bits combined with the bits in bytes 1 and 0 correspond
 ///     to the release number of the suffix, if one exists. If there is no
 ///     suffix, then these bits are always 0.
 ///
```

---

_Comment by @charliermarsh on 2024-11-05 02:42_

> Also did we settle on something here?

Yeah I did it here: https://github.com/astral-sh/uv/pull/8818

---

_Comment by @charliermarsh on 2024-11-05 02:43_

> Can we do `[1.0.0, 1.0.0.post1)`?

Hmm... I think that _could_ work? We can give it a shot. It might be a little more tedious to detect and remove those versions in the error reporter though.


---

_Comment by @charliermarsh on 2024-11-05 02:45_

(I think changing `VersionSmall` isn't too hard, I just either need time to focus on re-learning the representation / constants or @BurntSushi help.)

---

_Comment by @ericmarkmartin on 2024-11-05 04:02_

> > Can we do `[1.0.0, 1.0.0.post1)`?
> 
> Hmm... I think that _could_ work? We can give it a shot. It might be a little more tedious to detect and remove those versions in the error reporter though.

If making the `SmallVersion` work is possible, than is it worth trying the above? I'd assume the only advantage is saving you some room in the `SmallVersion` if that's particularly valuable

---

_Comment by @charliermarsh on 2024-11-05 04:10_

Yeah I think we should just try to modify SmallVersion — seems like the best path forward.

---

_Comment by @ericmarkmartin on 2024-11-05 04:35_

I guess there's also a possibility some actual local versions can be encoded in `VersionSmall`, though I suppose it's pretty unlikely anything useful could be? Maybe we can come up with special case representations for `+cpu` and `gpu` 🤣. I was genuinely concerned about the perf impact of releasing this without a `VersionSmall` representation due to introducing slow comparisons for any `==` specifier, but if you don't think that's an issue I'm happy to pick up the `VersionSmall` changes in another PR

---

_Comment by @BurntSushi on 2024-11-05 12:27_

For context on `VersionSmall`, its _purpose_ is to compactly represent `Version` values that are the most frequently occurring. Versions with local segments are specifically infrequent relative to the universe of all versions used (on PyPI at least), and so using `VersionSmall` to represent them is probably not that impactful.

---

_Comment by @ericmarkmartin on 2024-11-05 14:28_

> For context on `VersionSmall`, its _purpose_ is to compactly represent `Version` values that are the most frequently occurring.

Right, but if you change `VersionSmall` like Charlie describes above, then you have 2.5 bytes to encode a local version tag, so you may as well use it for something right? I guess there's some way you could make a specific encoding for `LocalVersion::Max` without following the existing scheme?

> Versions with local segments are specifically infrequent relative to the universe of all versions used (on PyPI at least)

Yeah, the spec even says

> Local version identifiers SHOULD NOT be used when publishing upstream projects to a public index server

---

_Comment by @charliermarsh on 2024-11-05 14:31_

Yeah I messaged Andrew about this -- I think we're all aligned on reclaiming some of the "version suffix" bits so that we can encode this special-case local tag.

---

_Comment by @BurntSushi on 2024-11-05 14:59_

@ericmarkmartin I think we need just one bit that we can pull from the version suffix. We definitely don't want to take more than that because the version suffix is where things like `10` in `.dev10` get stored. If we devoted all of the suffix to local segments, we'd be giving up a "small" representation for something that is much more common than things with a local segment.

I'm going to put up a PR with a slight refactor that I think should make this change easier.

---

_Comment by @charliermarsh on 2024-11-05 21:24_

Doing the `VersionSmall` changes now but it'll be a separate PR.

---

_Comment by @ericmarkmartin on 2024-11-05 23:24_

@BurntSushi I was mistakenly thinking we were reclaiming the extra bit so we could have a 4 bit discriminant. Then we'd have "local" as a possible value for said discriminant, hence my thinking we had 2.5 bytes to work with for the local segment in that case.

I now realize we're talking about just stealing the bit as an orthogonal indicator of the local sentinel, which makes way more sense. Sorry for my confusion 

---

_Comment by @charliermarsh on 2024-11-06 03:04_

(Starting the process of merging these into the v0.5 branch.)

---

_Merged by @charliermarsh on 2024-11-06 03:18_

---

_Closed by @charliermarsh on 2024-11-06 03:18_

---

_Branch deleted on 2024-11-08 04:35_

---

_Comment by @pradyunsg on 2024-11-10 08:27_

Coming here from the 0.5.0 changelog entry: this is quite smart!



---

```yaml
number: 367
title: "distribution-filename: speed up is_compatible"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/improve-wheel-tag-matching
created_at: 2023-11-08T19:36:23Z
updated_at: 2023-11-09T14:01:05Z
url: https://github.com/astral-sh/uv/pull/367
synced_at: 2026-01-10T15:50:28Z
```

# distribution-filename: speed up is_compatible

---

_Pull request opened by @BurntSushi on 2023-11-08 19:36_

This PR tweaks the representation of `Tags` in order to offer a
faster implementation of `WheelFilename::is_compatible`. We now use a
nested map of tags that lets us avoid looping over every supported
platform tag. As the code comments suggest, that is the essential gain.
We still do not mind looping over the tags in each wheel name since they
tend to be quite small. And pushing our thumb on that side of things can
make things worse overall since it would likely slow down WheelFilename
construction itself.

For micro-benchmarks, we improve considerably for compatibility
checking:

    $ critcmp base test3
    group                                                   base                                    test3
    -----                                                   ----                                    -----
    build_platform_tags/burntsushi-archlinux                1.00     46.2±0.28µs        ? ?/sec     2.48    114.8±0.45µs        ? ?/sec
    wheelname_parsing/flyte-long-compatible                 1.00    624.8±3.31ns   174.0 MB/sec     1.01    629.4±4.30ns   172.7 MB/sec
    wheelname_parsing/flyte-long-incompatible               1.00    743.6±4.23ns   165.4 MB/sec     1.00    746.9±4.62ns   164.7 MB/sec
    wheelname_parsing/flyte-short-compatible                1.00    526.7±4.76ns    54.3 MB/sec     1.01    530.2±5.81ns    54.0 MB/sec
    wheelname_parsing/flyte-short-incompatible              1.00    540.4±4.93ns    60.0 MB/sec     1.01    545.7±5.31ns    59.4 MB/sec
    wheelname_parsing_failure/flyte-long-extension          1.00     13.6±0.13ns     3.2 GB/sec     1.01     13.7±0.14ns     3.2 GB/sec
    wheelname_parsing_failure/flyte-short-extension         1.00     14.0±0.20ns  1160.4 MB/sec     1.01     14.1±0.14ns  1146.5 MB/sec
    wheelname_tag_compatibility/flyte-long-compatible       11.33   159.8±2.79ns   680.5 MB/sec     1.00     14.1±0.23ns     7.5 GB/sec
    wheelname_tag_compatibility/flyte-long-incompatible     237.60 1671.8±37.99ns    73.6 MB/sec    1.00      7.0±0.08ns    17.1 GB/sec
    wheelname_tag_compatibility/flyte-short-compatible      16.07   223.5±8.60ns   128.0 MB/sec     1.00     13.9±0.30ns     2.0 GB/sec
    wheelname_tag_compatibility/flyte-short-incompatible    149.83   628.3±2.13ns    51.6 MB/sec    1.00      4.2±0.10ns     7.6 GB/sec

We do regress slightly on the time it takes for `Tags::new` to run, but
this is somewhat expected. And in absolute terms, 114us is perfectly
acceptable given that it's only executed ~once for each `puffin`
invocation.

Ad hoc benchmarks indicate an overall 25% perf improvement in `puffin
pip-compile` times. This roughly corresponds with how much time
`is_compatible` was taking. Indeed, profiling confirms that it has
virtually disappeared from the profile.

Fixes #157

(It might be easiest to review this PR commit-by-commit.)

---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-08 19:36_

---

_Review requested from @konstin by @BurntSushi on 2023-11-08 19:36_

---

_Comment by @BurntSushi on 2023-11-08 19:39_

Fun fact: the last commit in this PR represents my second attempt at optimization. My first attempt ended up being bunk. I had attempted to build one regex that included all platform tags that could be matched against the wheel filename itself directly. It worked and `is_compatible` was implemented simply via `re.is_match(..)`. While it did indeed fix `is_compatible`, it made `Tags::new` slow enough that it took about ~10% of resolution time. The regex could likely be shrunk due to the amount of redundancy in the tags, but at that point, I figured it got too complex. The place I ended up was much simpler.

---

_Review comment by @charliermarsh on `crates/platform-tags/src/lib.rs`:12 on 2023-11-08 19:49_

I've also idly wondered if we could encode the set of possible python_tag, abi_tag, and platform_tag values statically (e.g., as an enum or similar), rather than allocating. But probably not a big deal given that this happens once at startup.


---

_@charliermarsh approved on 2023-11-08 19:51_

Excellent work!

---

_@charliermarsh reviewed on 2023-11-08 19:51_

---

_Review comment by @charliermarsh on `crates/bench/benches/distribution_filename.rs`:162 on 2023-11-08 19:51_

The thoroughness of this file is great.

---

_@charliermarsh reviewed on 2023-11-08 19:51_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/wheel.rs`:252 on 2023-11-08 19:51_

For small values, you could also configure Insta to put the expectations inline, if you prefer.

---

_@BurntSushi reviewed on 2023-11-08 19:55_

---

_Review comment by @BurntSushi on `crates/platform-tags/src/lib.rs`:12 on 2023-11-08 19:55_

Yeah, I did briefly consider something along those lines, or more generically, some kind of string interning. In theory that would make the `is_compatible` check faster by avoiding string hashing/comparison. I ended up not pursuing that path because I perceived that `WheelFilename::from_str` and `WheelFilename::is_compatible` are called about the same number of times. So if you're doing interning (of some kind), then ideally that happens when you build a `WheelFilename`. And that _could_ potentially be a big win if `is_compatible` is called many times. But it's only called once. So you still wind up needing to pay the cost of interning itself and I don't think it really ever amortizes.

(In the worst cases, interning is perhaps cheaper than the hashmap lookup approach I used here because you only need to hash each tag exactly once. So there is still a potential for a win here, but likely only with wheels that have a large number of tags. I'm not sure if those exist. And if they did, you would probably need a lot of them for it to matter.)

Very hand wavy, but that was my thought process anyway!

---

_@BurntSushi reviewed on 2023-11-08 19:57_

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/wheel.rs`:252 on 2023-11-08 19:57_

Can Insta do that automatically? I didn't see that if so. That would be great. Because otherwise, the workflow of having them in separate files is really just so nice. Which is unfortunate, because I agree it hampers readability.

---

_@charliermarsh reviewed on 2023-11-08 20:04_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/wheel.rs`:252 on 2023-11-08 20:04_

I believe so, let me check...

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/wheel.rs`:252 on 2023-11-08 20:06_

Check out "Inline Snapshots" here: https://insta.rs/docs/snapshot-types/

I think you just add that second argument to `assert_debug_snapshot`

---

_@charliermarsh reviewed on 2023-11-08 20:06_

---

_Review comment by @konstin on `crates/bench/benches/distribution_filename.rs`:7 on 2023-11-08 21:16_

Should these be doc comments (`///`)?

---

_Review comment by @konstin on `crates/bench/benches/distribution_filename.rs`:22 on 2023-11-08 21:16_

Good test case selection!

---

_Review comment by @konstin on `crates/bench/benches/distribution_filename.rs`:111 on 2023-11-08 21:18_

good catch!

---

_Review comment by @konstin on `crates/distribution-filename/src/wheel.rs`:39 on 2023-11-08 21:22_

I dropped it for the initial version to add it later when a case comes up but so far i didn't hit any case where there the build tag mattered (i haven't actually seen any build tag in the wild yet)

---

_Review comment by @konstin on `crates/distribution-filename/src/wheel.rs`:181 on 2023-11-08 21:23_

Could the error cases be inline display tests (either insta snapshots or just raw `assert_eq!`), so we check the error as it is shown to the user?

---

_Review comment by @konstin on `crates/distribution-filename/src/wheel.rs`:252 on 2023-11-08 21:25_

Inserting a second macro argument `@""` and `cargo test; cargo insta accept` should do

---

_@konstin approved on 2023-11-08 21:27_

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/wheel.rs`:252 on 2023-11-09 13:02_

Ah okay, yeah I knew about adding it to the macro. I got excited because it sounded like Insta could do that for you.

I'll switch these over to inline assertions.

---

_@BurntSushi reviewed on 2023-11-09 13:02_

---

_@BurntSushi reviewed on 2023-11-09 13:04_

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/wheel.rs`:252 on 2023-11-09 13:04_

Oh! @konstin I get it now. It does work automatically. Sweet.

---

_@BurntSushi reviewed on 2023-11-09 13:16_

---

_Review comment by @BurntSushi on `crates/bench/benches/distribution_filename.rs`:7 on 2023-11-09 13:16_

They aren't exported APIs and are in a `bin` crate so I don't think it matters too much, but I believe you can ask rustdoc to show unexported APIs. So it doesn't hurt to just switch them over. That's done. Thanks!

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/wheel.rs`:39 on 2023-11-09 13:20_

There are lots in the `flyte.in` benchmark. For example, `Pillow-9.4.0-2-pp39-pypy39_pp73-macosx_10_10_x86_64` has a build tag of `2`.

---

_@BurntSushi reviewed on 2023-11-09 13:20_

---

_@BurntSushi reviewed on 2023-11-09 13:27_

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/wheel.rs`:181 on 2023-11-09 13:27_

Done. I think this works in this case since the error type is simple, but I wouldn't normally expect the `Display` impl on its own to emit the full user visible error message. (Just a part of it.)

---

_Merged by @BurntSushi on 2023-11-09 14:01_

---

_Closed by @BurntSushi on 2023-11-09 14:01_

---

_Branch deleted on 2023-11-09 14:01_

---

```yaml
number: 1804
title: "Cargo.toml: try to make release binaries smaller"
type: pull_request
state: closed
author: vrmiguel
labels: []
assignees: []
base: master
head: smaller-binary-size
created_at: 2021-02-14T01:35:05Z
updated_at: 2021-05-30T13:24:17Z
url: https://github.com/BurntSushi/ripgrep/pull/1804
synced_at: 2026-01-12T18:23:14Z
```

# Cargo.toml: try to make release binaries smaller

---

_@vrmiguel_

Hello Andrew (and other contributors)! First off, congrats. on the great project ;)

This PR consists in
* Activating "fat" link-time optimization;
* Compiling on a single code generation unit;
* Setting `opt-level` to 3.

The most obvious (and possibly the only) downside I can see are slightly longer compilation times.

Compiling the latest (at the time of writing) `ripgrep` (master): 

```
    Finished release [optimized + debuginfo] target(s) in 1m 23s
```

Compiling `ripgrep` with this PR's `Cargo.toml`:

```
    Finished release [optimized + debuginfo] target(s) in 1m 29s
```

Both running on an AMD 4x 3.8GHz CPU, Linux 5.10.15 and a similarly undisturbed OS.

This is, of course, not the most scientific of tests but shows that the build time delta is not big.

The result in binary sizes are

```bash
$ du -sh rg
27M     rg           # Current Cargo.toml 
```

```bash
$ du -sh rg
18M     rg           # This PR's proposal
```

This is only anecdotal, but a 7.22% increase in build time gave us a 33.3% decrease in binary size, which seems (to me) a worthwhile investment.







---

_Renamed from "Cargo.toml: try to maker release binaries smaller" to "Cargo.toml: try to make release binaries smaller" by @vrmiguel on 2021-02-14 01:35_

---

_Comment by @BurntSushi on 2021-05-30 13:24_

OK, so people have tried to do this before. (There have been at least a few attempts.) I've rebuffed them every time, and unfortunately, this time is no different. I'm always willing to give it another shot to see if anything has changed, but it doesn't appear to be the case.

So on my laptop, without these options, it takes about 57s to compile ripgrep from scratch. With this change, it takes about 97s. That's a much bigger difference than what you measured.

But that actually isn't the biggest problem. The biggest problem is the increase in iteration time. My sniff test is, after a clean build, run `touch crates/core/main.rs` and then try a rebuild. Without these options, it takes a painful 13s. But with these options, it takes an excruciating 68s. This isn't something that I can really live with. 13s is already painful enough.

There are a couple higher level points here:

* Why do I care about release build times? Because I do most of my work with release builds, e.g., profiling.
* In theory, what we probably should do is patch `Cargo.toml`'s release configuration when generating the _actual_ release binaries in CI. But at that point, the size differences aren't as pronounced since CI already strips the binaries. So doing this is more about squeezing the binary as much as possible while perhaps enabling the best possible class of optimizations. But in some simple tests, I can't see a measurable difference.
* Patching the release configuration in CI means `cargo install` users don't get the benefits. (But I wouldn't consider this a hard blocker.)
* Finally, patching the release config in CI means that I'm not dog-fooding it.

So with that, I'm going to close this for now, although I appreciate efforts to decrease binary size. I don't think I've ever done a `cargo bloat` on ripgrep before, so it's possible there are some lower hanging fruits there.

---

_Closed by @BurntSushi on 2021-05-30 13:24_

---

```yaml
number: 1542
title: No git tags for recent releases.
type: issue
state: closed
author: nathan-at-least
labels: []
assignees: []
created_at: 2019-09-03T18:44:22Z
updated_at: 2020-02-01T12:38:17Z
url: https://github.com/clap-rs/clap/issues/1542
synced_at: 2026-01-12T16:14:11Z
```

# No git tags for recent releases.

---

_@nathan-at-least_


### Rust Version

```
$ rustc --version
rustc 1.36.0 (a53f9df32 2019-07-03)
```

### Affected Version of clap

git repository issue affecting versioning itself.

### Bug or Feature Request Summary

I can't figure out which git revision corresponds to the crates.io `2.33.0` release.

I wanted to understand the lifetime parameters of `clap::App` better while viewing the `2.33.0` docs for my project (which were generated from `cargo doc`). So, I cloned the source of `clap` and did `git tag --list`. I don't see `2.33.0`. The latest tags I see are `2.30.0` and `2.31.2`.

I skimmed the `README.md` to see details about release policy and how releases are made, but couldn't find a resolution to my mystery.

### Expected Behavior Summary

I expect `git tag --list` (as well as the [Github releases](https://github.com/clap-rs/clap/releases)) to include a tag for the release I've installed from crates.io so I have some indication that I'm viewing the correct revision of a git checkout.

If for some reason the release policy cannot or does not use git tags (and github releases) anymore, the docs need to describe how I can find the exact source revisions I'm relying on from crates.io.

### Actual Behavior Summary

When I run `git tag --list` I see recent releases up to `2.31.2` but no more recent release tags.

### Steps to Reproduce the issue

```
$ git clone 'https://github.com/clap-rs/clap'
$ cd clap
$ git tag --list
```

Visually verify the list does not contain `2.33.0`.

### Sample Code or Link to Sample Code

(See above steps.)

### Debug output

Not relevant.


---

_Comment by @Dylan-DPC-zz on 2019-09-03 18:49_

Yes we didn't push a tag for the last few releases. These are the commits that correspond to 2.33.0 release: https://github.com/clap-rs/clap/commit/784524f7eb193e35f81082cc69454c8c21b948f7

---

_Comment by @nathan-at-least on 2019-09-04 15:24_

Was this a change in process or an oversight? If the former, can you add some documentation about the release process? If the latter, can you retroactively add tags?

---

_Comment by @svenstaro on 2019-09-09 15:19_

I think you should definitely have a tag per release. It just makes everything more transparent and easier to reference cargo releases to git commits.

---

_Comment by @Dylan-DPC-zz on 2019-09-09 16:21_

Yes I agree tags are helpful, it is just that it got overlooked when we did the last few releases.

---

_Comment by @remram44 on 2019-10-28 17:12_

Stumbled on this today, was very surprised. Can you create tags now? It would be very helpful.

* 2.32.0 is eafe29d8be24193126f66c7872c91e95c35f9661
* 2.33.0 is c50deae6bcec44714368f739dfbe2919d740d7b6

---

_Comment by @CreepySkeleton on 2020-02-01 12:38_

Fixed.

---

_Closed by @CreepySkeleton on 2020-02-01 12:38_

---

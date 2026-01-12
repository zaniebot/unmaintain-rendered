```yaml
number: 412
title: Removed debug info from release builds
type: pull_request
state: closed
author: XAMPPRocky
labels: []
assignees: []
base: master
head: master
created_at: 2017-03-19T13:44:07Z
updated_at: 2017-08-23T22:08:23Z
url: https://github.com/BurntSushi/ripgrep/pull/412
synced_at: 2026-01-12T18:23:13Z
```

# Removed debug info from release builds

---

_@XAMPPRocky_

Having debug info enabled on release causes bloated binaries on Unix based systems. This affects people who use `cargo install ripgrep`.

---

_Comment by @BurntSushi on 2017-03-19 13:53_

What is the actual problem here? I have debug enabled on release binaries purposefully.

---

_Comment by @XAMPPRocky on 2017-03-19 13:57_

From IRC:
> ripgrep's executable is 26MB, 24MB of which is debug information.

This seems a bit much, what is the reasoning for leaving it on in release, profiling?

---

_Comment by @BurntSushi on 2017-03-19 14:00_

I understand that it makes the binary big. What i would like to know is *why* that's a problem.

---

_Comment by @XAMPPRocky on 2017-03-19 14:22_

Other than the inclination to not waste disk space if you don't need to. I think the main problem is the inconsistency as if I was to use one of the package managers using the GitHub releases I would have a binary that is ~1-5MB, whereas with cargo I get 20+MB.

---

_Comment by @BurntSushi on 2017-03-19 14:49_

@Aaronepower I've moved this discussion to #413 because it really shouldn't be tied to a particular implementation strategy.

---

_Comment by @kpp on 2017-04-03 19:31_

Well, you can manually strip debug info using `strip`.

---

_Comment by @BurntSushi on 2017-08-23 22:08_

I decided to just apply `strip` in CI, which I did in https://github.com/BurntSushi/ripgrep/commit/30ca3ecca65d06ebc1d0794f3968397709aff9a8

---

_Closed by @BurntSushi on 2017-08-23 22:08_

---

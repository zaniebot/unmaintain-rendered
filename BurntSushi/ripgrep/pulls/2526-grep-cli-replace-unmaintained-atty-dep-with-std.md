```yaml
number: 2526
title: "grep-cli: Replace unmaintained `atty` dep with `std::io::IsTerminal`"
type: pull_request
state: merged
author: Enselic
labels: []
assignees: []
merged: true
base: master
head: remove-atty
created_at: 2023-06-05T17:12:59Z
updated_at: 2023-07-12T17:40:19Z
url: https://github.com/BurntSushi/ripgrep/pull/2526
synced_at: 2026-01-12T18:23:14Z
```

# grep-cli: Replace unmaintained `atty` dep with `std::io::IsTerminal`

---

_@Enselic_

The `atty` crate is [unmaintained][1] and `std::io::IsTerminal` was stabilized in Rust 1.70.

For reference, please see previous PRs related to this:
* https://github.com/BurntSushi/ripgrep/pull/2481
* https://github.com/BurntSushi/ripgrep/pull/2515

[1]: https://rustsec.org/advisories/RUSTSEC-2021-0145.html

---

_@BurntSushi approved on 2023-06-05 17:18_

Thanks!

---

_Merged by @BurntSushi on 2023-06-05 18:00_

---

_Closed by @BurntSushi on 2023-06-05 18:00_

---

_Branch deleted on 2023-06-05 18:07_

---

_Comment by @decathorpe on 2023-07-12 17:32_

This PR resulted in a quite big MSRV bump (to 1.70.0) - given that this crate is still on the 2018 edition, was this intentional? 1.70.0 is *very* recent (it's the current stable Rust).

---

_Comment by @BurntSushi on 2023-07-12 17:35_

ripgrep tracks the latest stable release. It's documented in the README and this policy was decided based on feedback from distro maintainers: https://github.com/BurntSushi/ripgrep/issues/1019

The specific edition is mostly irrelevant. It's a detail that will eventually get changed, but it doesn't necessarily change in concert with everything else unless it needs to be. I maintain dozens of crates. I don't always keep MSRV and edition updated in synchronous lockstep. It's just too much to keep track of.

---

_Comment by @BurntSushi on 2023-07-12 17:39_

And honestly, the security warnings from github about `atty` are just absurd. It would have been great if @softprops could have just released a new version of `atty` to squash those, but that isn't what happened. There's the `is-terminal` crate which fixes the issues in `atty`, but its dependency tree is sprawling so there's no way I'm taking a dependency on it. So my only options left are to just depend on what's in std (which is compatible with ripgrep's MSRV policy) or fork `atty`, fix its issues and maintain yet another crate. I'm not keen on the latter.

---

_Comment by @decathorpe on 2023-07-12 17:40_

Thanks for the clarification! Makes sense to me. I was just surprised by the big bump, that's all, so I wanted to ask.

---

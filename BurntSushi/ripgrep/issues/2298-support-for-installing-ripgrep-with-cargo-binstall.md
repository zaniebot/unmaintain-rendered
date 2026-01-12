```yaml
number: 2298
title: Support for installing ripgrep with cargo-binstall
type: issue
state: closed
author: tomkarw
labels:
  - enhancement
  - help wanted
  - rollup
assignees: []
created_at: 2022-09-01T22:17:43Z
updated_at: 2023-11-25T20:03:59Z
url: https://github.com/BurntSushi/ripgrep/issues/2298
synced_at: 2026-01-12T16:13:24Z
```

# Support for installing ripgrep with cargo-binstall

---

_@tomkarw_

#### Describe your feature request

[cargo-binstall](https://github.com/cargo-bins/cargo-binstall) is a way to fetch precompiled binaries without the need to build from source. It is meant to speed up installing new cargo tools.

By default, `cargo-binstall` will look for GitHub releases artifacts, and `ripgrep` has these. Unfortunately, `cargo-generate` requires the version component to conform to `v.*` pattern, and `ripgrep` doesn't include the `v`.

The easiest way to achieve the desired result would be to override this default by adding this to `Cargo.toml`:
```
[package.metadata.binstall]
pkg-url = "{ repo }/releases/download/{ version }/{ name }-{ target }-v{ version }.{ archive-format }"
```
(notice the lack of `v` in the version component)

If support for `cargo-binstall` is deemed preferable, I'd be happy to land such PR and test that this solution works.

#### Alternative

An alternative would be to change the naming conventions for the releases, but I think the above solution is more surgical and less interfering.

---

_Comment by @BurntSushi on 2022-09-01 22:41_

I'm not sure I want to encourage more people installing ripgrep through Cargo, but the Cargo.toml lines here look very unobtrusive. So I would be happy to accept a PR for it. And would probably make sense to add `cargo binstall` instructions to the README.

---

_Label `enhancement` added by @BurntSushi on 2022-09-01 22:41_

---

_Label `help wanted` added by @BurntSushi on 2022-09-01 22:41_

---

_Comment by @tomkarw on 2022-09-02 06:58_

I figured out it would be easier for `cargo-binstall` to support versions without leading `v`, rather than go repo by repo and override the default paths. They added it straight away: https://github.com/cargo-bins/cargo-binstall/issues/328!

We can wait till this improvement in `cargo-binstall` gets released and mention it in the README, I'll prepare a PR for that soon.

---

_Comment by @BurntSushi on 2022-09-02 11:32_

Perfect, thanks!

And yeah in retrospect, I do kind of wish I used tags starting with `v`. But all of my Rust repos follow the same convention and it isn't bad enough to be worth me switching. And switching only some of them would probably drive me nuts. :)

---

_Comment by @lesleyrs on 2023-04-16 14:28_

> I'm not sure I want to encourage more people installing ripgrep through Cargo

Huh what's wrong with it?

---

_Comment by @BurntSushi on 2023-04-16 14:42_

`cargo` isn't a package manager..... I don't really have the time to go into detail about all of the things Cargo doesn't do that a package manager does. But it's _a lot_.

---

_Label `rollup` added by @BurntSushi on 2023-11-24 19:17_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---

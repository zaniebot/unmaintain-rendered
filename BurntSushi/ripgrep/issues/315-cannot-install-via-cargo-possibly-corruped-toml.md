```yaml
number: 315
title: Cannot install via cargo, possibly corruped TOML
type: issue
state: closed
author: deathbeam
labels: []
assignees: []
created_at: 2017-01-12T12:34:17Z
updated_at: 2017-01-12T12:50:43Z
url: https://github.com/BurntSushi/ripgrep/issues/315
synced_at: 2026-01-12T16:13:21Z
```

# Cannot install via cargo, possibly corruped TOML

---

_@deathbeam_

So I was trying to install ripgrep via cargo on Ubuntu 16.04 (since it is not in repos), and this is error that I am facing:

```
 Updating registry `https://github.com/rust-lang/crates.io-index`
failed to parse manifest at `/home/deathbeam/.cargo/registry/src/github.com-88ac128001ac3a9a/ripgrep-0.3.2/Cargo.toml`

Caused by:
  could not parse input as TOML
/home/deathbeam/.cargo/registry/src/github.com-88ac128001ac3a9a/ripgrep-0.3.2/Cargo.toml:43:9 expected a key but found an empty string
/home/deathbeam/.cargo/registry/src/github.com-88ac128001ac3a9a/ripgrep-0.3.2/Cargo.toml:43:9-43:10 expected `.`, but found `'`
```

I am not sure what is causing it, I installed cargo from repo (`apt install cargo`) and when I opened mentioned TOML file, it seemed okay.

---

_Comment by @BurntSushi on 2017-01-12 12:35_

What version of Rust? You need at least 1.11.

On Jan 12, 2017 7:34 AM, "Tomas Slusny" <notifications@github.com> wrote:

> So I was trying to install ripgrep via cargo on Ubuntu 16.04 (since it is
> not in repos), and this is error that I am facing:
>
>  Updating registry `https://github.com/rust-lang/crates.io-index` <https://github.com/rust-lang/crates.io-index>
> failed to parse manifest at `/home/deathbeam/.cargo/registry/src/github.com-88ac128001ac3a9a/ripgrep-0.3.2/Cargo.toml`
>
> Caused by:
>   could not parse input as TOML
> /home/deathbeam/.cargo/registry/src/github.com-88ac128001ac3a9a/ripgrep-0.3.2/Cargo.toml:43:9 expected a key but found an empty string
> /home/deathbeam/.cargo/registry/src/github.com-88ac128001ac3a9a/ripgrep-0.3.2/Cargo.toml:43:9-43:10 expected `.`, but found `'`
>
> I am not sure what is causing it, I installed cargo from repo (apt
> install cargo) and when I opened mentioned TOML file, it seemed okay.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/315>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34oo8L5zHggSrVgENXRRWwC0rErI8ks5rRh3KgaJpZM4Lhs5G>
> .
>


---

_Comment by @deathbeam on 2017-01-12 12:42_

Thank you for fast reply. I am not really sure, because I installed cargo solely to install ripgrep, and it seems that I do not even have rustc in PATH, but `cargo -V` outputs `cargo 0.8.0 (built 2016-03-22)`, so I guess it is outdated then.

---

_Comment by @BurntSushi on 2017-01-12 12:44_

Yup. Definitely too old. You can download a binary release though (which i
realize is not ideal).

On Jan 12, 2017 7:42 AM, "Tomas Slusny" <notifications@github.com> wrote:

> Thank you for fast reply. I am not really sure, because I installed cargo
> solely to install ripgrep, and it seems that I do not even have rustc in
> PATH, but cargo -V outputs cargo 0.8.0 (built 2016-03-22), so I guess it
> is outdated then.
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/315#issuecomment-272153858>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34qaS5Ee_nz4So0Ts4OmORqF1x1iJks5rRh-wgaJpZM4Lhs5G>
> .
>


---

_Comment by @deathbeam on 2017-01-12 12:50_

I installed up-to-date version of cargo, so now it is working. And yeah, downloading binary release is not ideal, but at least there is a binary release, many popular projects do not even bother with them. Anyway, my problem is solved, thank you for your help, closing this issue.

---

_Closed by @deathbeam on 2017-01-12 12:50_

---

```yaml
number: 722
title: "wincolor: migrate to winapi 0.3"
type: pull_request
state: closed
author: steffengy
labels: []
assignees: []
base: master
head: master
created_at: 2017-12-29T22:54:31Z
updated_at: 2018-01-17T15:09:20Z
url: https://github.com/BurntSushi/ripgrep/pull/722
synced_at: 2026-01-12T18:23:13Z
```

# wincolor: migrate to winapi 0.3

---

_@steffengy_

_No description provided._

---

_Comment by @BurntSushi on 2017-12-30 13:50_

@steffengy Thanks for doing this! Some of the extra dependencies that have been added because of this are concerning to me. Why do we now depend on fuschia stuff and the `glob` crate for example? 

---

_Comment by @steffengy on 2017-12-30 13:58_

`glob` is a dev-dependency of `globset`.
The fuchsia stuff seems to be pulled in through `rand <- tempdir <- globset`,
which is weird, since `target_os` shouldn't be fuchsia.

I honestly just did a `cargo build` after changing the winapi stuff in `cargo.toml`,

I'll try a `cargo build --release` and check if that reduces the pulled-in dependencies.

---

_Comment by @steffengy on 2017-12-30 14:03_

@BurntSushi 
I get the same `Cargo.lock` after reverting `Cargo.lock` and just doing a `cargo build --release`.
Not sure what's going on there.

---

_Comment by @BurntSushi on 2017-12-30 14:09_

@steffengy Strange. I'll take a look.

---

_Comment by @BurntSushi on 2017-12-30 21:02_

Thanks for doing this! I've rolled this PR into #724.

I don't know why `Cargo.lock` was getting updated on a `cargo build`, but it happened for me too. I took that opportunity to update lots of other dependencies as well, which I just combined into one big PR.

---

_Closed by @BurntSushi on 2017-12-30 21:02_

---

_Comment by @pravic on 2018-01-17 14:35_

@BurntSushi It would be nice to publish a version with winapi 0.3, since now it looks like a mess when some crates import 0.2 while others have migrated to 0.3.

And I was very confused that [wincolor 0.1.4](https://crates.io/crates/wincolor/0.1.4) requires winapi 0.2 (according to crates.io) while the version [in repository](https://github.com/BurntSushi/ripgrep/blob/master/wincolor/Cargo.toml) (which is also *0.1.4*) clearly uses winapi 0.3.

---

_Comment by @BurntSushi on 2018-01-17 15:07_

> It would be nice to publish a version with winapi 0.3, since now it looks like a mess when some crates import 0.2 while others have migrated to 0.3.

I'll do it when I can.

> And I was very confused that wincolor 0.1.4 requires winapi 0.2 (according to crates.io) while the version in repository (which is also 0.1.4) clearly uses winapi 0.3.

Versions don't get bumped until they get released. You should be [looking at tags instead](https://github.com/BurntSushi/ripgrep/tree/wincolor-0.1.4).

---

_Comment by @pravic on 2018-01-17 15:09_

I see. Thanks.

---

```yaml
number: 595
title: "ripgrep binary doesn't appear to be `strip`ed on Unix like expected"
type: issue
state: closed
author: joshsleeper
labels:
  - bug
  - doc
assignees: []
created_at: 2017-09-05T23:19:46Z
updated_at: 2022-04-15T15:35:23Z
url: https://github.com/BurntSushi/ripgrep/issues/595
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep binary doesn't appear to be `strip`ed on Unix like expected

---

_@joshsleeper_

rustc 1.20.0 (f3d6973f4 2017-08-27)
cargo 0.21.0 (5b4b8b2ae 2017-08-12)
ripgrep 0.6.0 -AVX -SIMD

Despite the discussion in #413 and the resultant commit 30ca3ecca65d06ebc1d0794f3968397709aff9a8, I'm still seeing rather large binaries after a fresh `cargo install ripgrep` on my CentOS 7 box (~27 MiB)

Running `strip ~/.cargo/bin/rg` gives me a binary that's a magnitude of size smaller, like expressed in #413.

Did I misunderstand the effect of that commit, or is this a bug?

---

_Comment by @BurntSushi on 2017-09-05 23:26_

> I'm still seeing rather large binaries after a fresh cargo install ripgrep on my CentOS 7 box (~27 MiB)

Only the [release tarballs are stripped](https://github.com/BurntSushi/ripgrep/releases). The only way to make `cargo install ripgrep` produce a smaller binary is to modify the `Cargo.toml` to disable debug symbols, and I'm not particularly interested in doing that.

I hate to say things like this, but `cargo install ripgrep` is pretty much the install option of last resort. I provide it because people want it, but it's really not "supposed" to be the way you install ripgrep.

I'd be happy to add a note to the README near the `cargo install` option with respect to its downsides.

---

_Label `bug` added by @BurntSushi on 2017-09-05 23:26_

---

_Label `doc` added by @BurntSushi on 2017-09-05 23:26_

---

_Comment by @joshsleeper on 2017-09-05 23:34_

Ahh, that makes more sense!
I think a doc update around the implications of `cargo install ripgrep` would be excellent!
Thanks so much for the quick reply @BurntSushi.

I didn't realize I could install it from `copr` so I'll go ahead and do that.

Curious, what's the estimated turnaround time for updating the package on `copr`?
It's currently `0.5.2` fyi.
https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/packages/


---

_Comment by @BurntSushi on 2017-09-06 00:43_

I don't know. I don't maintain that. The only things i personally maintain are:

* Github releases (which you can use)
* Crates.io
* Homebrew

I mean, you can continue using cargo install if you want and don't mind waiting a couple MB.

---

_Comment by @dvergeylen on 2017-10-03 11:00_

Hi @BurntSushi ,

Thank you for ripgrep, it's truly awesome.

Installing via `cargo install ripgrep` could be "fixed" if debug symbols were removed from the toml, as they are implied at compile time. For instance:

```bash
# From a fresh git clone of ripgrep, with debug=true removed from toml
$ cargo build
[...]
Finished dev [unoptimized + debuginfo] target(s) in 70.58 secs
$ ls -l target/debug/rg 
-rwxr-xr-x 2 dv dv 29M oct  3 12:43 target/debug/rg
```
outputs a binary with debug symbols in `target/debug` folder (as expected).


```bash
# From a fresh git clone of ripgrep, with debug= true removed from toml
$ cargo build --release
[...]
Finished release [optimized] target(s) in 227.42 secs
$ ls -l target/release/rg 
-rwxr-xr-x 2 dv dv 6,0M oct  3 12:49 target/release/rg
```
outputs an optimized binary (only 6Mb).

All users installing ripgrep from `cargo install ripgrep` would benefit from it :smiley:*.  What are you thoughts?

*I think about [Ubuntu/Debian](https://github.com/BurntSushi/ripgrep/issues/10) users like me.

---

_Comment by @dieggsy on 2017-10-03 11:15_

@dvergeylen earlier in this thread @BurntSushi said:

>The only way to make cargo install ripgrep produce a smaller binary is to modify the `Cargo.toml` to disable debug symbols, **and I'm not particularly interested in doing that.**

---

_Comment by @dvergeylen on 2017-10-03 11:17_

@dieggsy Yes I read that, my comment was actually a request to have more explanation on this :wink:. 

---

_Comment by @BurntSushi on 2017-10-03 11:34_

What other explanation would you like? I want debug symbols in the binary. I don't want to have to keep fiddling with the Cargo.toml. `cargo install` is provided on a best effort basis, and I'm not going to go through contortions to make it perfect.

I mean really, why do you care this much? Even if you're one of the few people that cares about a few MB of disk space, then you are of course free to run strip on the binary.

---

_Comment by @dvergeylen on 2017-10-03 14:26_

I didn't mean to upset you. Reducing the size of the executable by 80% might interest others:

```shell
$ git clone https://github.com/BurntSushi/ripgrep
$ pushd ripgrep
$ sed -i -e '/\[profile.release\]\|debug = true/d' Cargo.toml
$ cargo build --release
$ mv target/release/rg /usr/sbin/ # requires privileges
$ popd; rm -Rf ripgrep
```

Nevermind ¯\_(ツ)_/¯

---

_Comment by @BurntSushi on 2017-10-03 14:35_

@dvergeylen I'm not upset, but I already addressed this in this issue and I don't understand what it is you're looking for.

Instead of going through `Cargo.toml` contortions, just `strip` the binary:

```
$ cargo install ripgrep
$ strip $(which rg)
```

---

_Comment by @dvergeylen on 2017-10-03 15:05_

:+1:

 [#622](https://github.com/BurntSushi/ripgrep/pull/622)

---

_Closed by @BurntSushi on 2017-10-08 12:01_

---

_Comment by @SUPERCILEX on 2022-04-15 15:35_

Can a new profile be created instead? So release defaults to the proper settings and then you can use a profile called "debuggable_release" or something (maybe just "dr").

---

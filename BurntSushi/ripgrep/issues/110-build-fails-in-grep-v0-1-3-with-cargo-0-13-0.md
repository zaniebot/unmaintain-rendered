```yaml
number: 110
title: "build fails in 'grep v0.1.3' with cargo 0.13.0: match arms have incompatible types"
type: issue
state: closed
author: nikias
labels: []
assignees: []
created_at: 2016-09-26T22:53:18Z
updated_at: 2016-09-27T06:43:09Z
url: https://github.com/BurntSushi/ripgrep/issues/110
synced_at: 2026-01-12T18:23:11Z
```

# build fails in 'grep v0.1.3' with cargo 0.13.0: match arms have incompatible types

---

_@nikias_

Building with cargo 0.13.0 I got the following error when it compiles grep v0.1.3:

```
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:53:19: 56:10 error: match arms have incompatible types:
 expected `&[_; 0]`,
    found `&[u8]`
(expected array of 0 elements,
    found slice) [E0308]
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:53         let req = match req_lits.iter().max_by_key(|lit| lit.len()) {
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:54             None => &[],
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:55             Some(req) => &***req,
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:56         };
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:53:19: 56:10 help: run `rustc --explain E0308` to see a detailed explanation
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:55:26: 55:33 note: match arm with an incompatible type
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:55             Some(req) => &***req,
                                                                                                                     ^~~~~~~
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:68:35: 68:44 error: the type of this value must be known in this context
/Users/nikias/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.3/src/literals.rs:68         if req_lits.len() == 1 && req.len() > lit.len() {
                                                                                                                              ^~~~~~~~~
```


---

_Comment by @BurntSushi on 2016-09-26 22:59_

Can you show the output of `cargo --version` and `rustc --version`? e.g.,

```
[andrew@Cheetah ripgrep] cargo --version
cargo 0.13.0-nightly (9399229 2016-09-14)
[andrew@Cheetah ripgrep] rustc --version
rustc 1.13.0-nightly (4f9812a59 2016-09-21)
```


---

_Comment by @nikias on 2016-09-26 23:01_

```
$ cargo --version
cargo 0.13.0 (dd91250 2016-09-26)
$ rustc --version
rustc 1.7.0-dev
```


---

_Comment by @BurntSushi on 2016-09-26 23:01_

@nikias Ah, your Rust is old. Unfortunately, `ripgrep` requires 1.9 or newer!


---

_Closed by @BurntSushi on 2016-09-26 23:01_

---

_Comment by @BurntSushi on 2016-09-26 23:02_

I strongly recommend [rustup](https://rustup.rs/) for installing Rust!


---

_Comment by @nikias on 2016-09-27 06:43_

Thanks!


---

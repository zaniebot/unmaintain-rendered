```yaml
number: 964
title: Compatibility policy vs crate graph resolution in Cargo
type: issue
state: closed
author: alexcrichton
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2017-05-17T15:16:35Z
updated_at: 2018-08-02T03:30:07Z
url: https://github.com/clap-rs/clap/issues/964
synced_at: 2026-01-12T16:14:10Z
```

# Compatibility policy vs crate graph resolution in Cargo

---

_@alexcrichton_

Hello! I've noticed that in the [compatibility policy section of the README](https://github.com/kbknapp/clap-rs/blob/2923515a0aec80a7ebd4b1fc3430aad859bcda8a/README.md#compatibility-policy) you recommend a tilde dependency such as:

```toml
[dependencies]
clap = "~2.19.0"
```

First off it's awesome the level of detail given here to version compatibility, hats off to that! Unfortunately though this is an over-constrained version dependency in terms of API versioning in the sense that if you've always got an up-to-date compiler you may wish to depend on `clap = "2.19.0"` instead. Normally this is ok but there's a few other bugs with Cargo at play.

Right now Cargo's version resolution is pretty naive, it's just a brute-force search of the solution space, returning the first resolvable graph. This also means that it currently [won't terminate](https://github.com/rust-lang/cargo/issues/4066) until it proves there is not possible resolvable graph. This [leads to situations](https://github.com/rust-lang/rust/pull/41639#issuecomment-302120978) where workspaces with multiple binaries, for example, have two different dependencies such as:

```toml
# In one Cargo.toml
[dependencies]
clap = "~2.19.0"

# In another Cargo.toml
[dependencies]
clap = "2.22"
```

This is inherently an unresolvable crate graph in Cargo right now. Cargo requires there's only one major version of `clap`, and being in the same workspace these two crates must share a version. This is impossible in this location, though, as these version constraints cannot be met.

I wonder if maybe the README could be updated with a gotcha such as this? I definitely think Cargo needs better error messages here, but in general `~` are somewhat hazardous because they can be over-constraining the dependency graph. 

Does that all make sense? Not sure if it's just a bit rambling, but I'm curious to hear what you think!

---

_Comment by @kbknapp on 2017-05-19 15:07_

Sorry for the late reply! That makes perfect sense, I'll add a small gotcha/warning to the README for this case. Is there an official stance or policy on how updating minimum required versions of Rust affects version or Cargo's version resolution, or are there any RFCs to make cargo aware of these issues? I've heard it discussed a few times, but I think this issue (requiring a more recent version of Rust "breaking" downstream builds) is use case for `~` since I don't really want to bump major versions just to use new features of Rust...although in practice that's how's it's turning out 😜 since the original issues surfaced.

Thanks for the detailed write up too! 😄 

---

_Label `C: docs` added by @kbknapp on 2017-05-19 15:15_

---

_Label `D: easy` added by @kbknapp on 2017-05-19 15:15_

---

_Label `P2: need to have` added by @kbknapp on 2017-05-19 15:15_

---

_Label `T: enhancement` added by @kbknapp on 2017-05-19 15:15_

---

_Comment by @alexcrichton on 2017-05-19 16:55_

Nah I think using `~` to denote rust compatibility versions is pretty nifty actually! We don't have a great policy around supporting historical versions of Rust right now, though, unfortunately. The libs team generally has a rule of thumb that:

* The latest version of a crate always supports at least 2 versions of rust back
* Otherwise bumping the version of rust required does not constitute a breaking change, but should be done intentionally (e.g. updating a CI configuration)

---

_Comment by @BurntSushi on 2017-05-19 16:56_

@alexcrichton While that's our current policy, in practice I think we're quite a bit more conservative than that. :-)

(I am of the opinion that there's just no good choice...)

---

_Closed by @kbknapp on 2017-05-29 17:10_

---

_Referenced in [colored-rs/colored#85](../../colored-rs/colored/issues/85.md) on 2020-09-24 16:27_

---

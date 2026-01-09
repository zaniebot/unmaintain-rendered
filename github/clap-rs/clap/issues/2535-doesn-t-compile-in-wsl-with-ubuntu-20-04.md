---
number: 2535
title: "Doesn't compile in WSL with ubuntu 20.04"
type: issue
state: closed
author: IceSentry
labels:
  - C-bug
assignees: []
created_at: 2021-06-13T17:32:10Z
updated_at: 2021-06-13T22:59:34Z
url: https://github.com/clap-rs/clap/issues/2535
synced_at: 2026-01-07T13:12:19-06:00
---

# Doesn't compile in WSL with ubuntu 20.04

---

_Issue opened by @IceSentry on 2021-06-13 17:32_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.1 (9bc8c42bb 2021-05-09)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
fn main() {
	println!("Hello, world!");
}
```

in Cargo.toml
```toml
[package]
name = "clap-test"
version = "0.1.0"
authors = ["author"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
clap = "3.0.0-beta.2"
```

### Steps to reproduce the bug with the above code

run `cargo build` in wsl with ubuntu 20.04

### Actual Behaviour

```bash
 > cargo build
   Compiling clap v3.0.0-beta.2
error[E0107]: this struct takes 3 type arguments but only 2 type arguments were supplied
  --> /home/<username>/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-beta.2/src/parse/matches/arg_matches.rs:77:22
   |
77 |     pub(crate) args: IndexMap<Id, MatchedArg>,
   |                      ^^^^^^^^ --  ---------- supplied 2 type arguments
   |                      |
   |                      expected 3 type arguments
   |
note: struct defined here, with 3 type parameters: `K`, `V`, `S`
  --> /home/<username>/.cargo/registry/src/github.com-1ecc6299db9ec823/indexmap-1.6.2/src/map.rs:76:12
   |
76 | pub struct IndexMap<K, V, S> {
   |            ^^^^^^^^ -  -  -
help: add missing type argument
   |
77 |     pub(crate) args: IndexMap<Id, MatchedArg, S>,
   |                                             ^^^

error: aborting due to previous error

For more information about this error, try `rustc --explain E0107`.
error: could not compile `clap`

To learn more, run the command again with --verbose.
```

### Expected Behaviour

Compiles successfully when added as a dependency

### Additional Context

I recently completely reinstalled wsl with ubuntu 20.04 and I have nothing but the rust toolchain installed on top of the default installation and anything required to compile rust projects. I can compile other projects that do not use clap. Yes, I ran an apt update and apt upgrade. I tried building a project using clap and saw this output. I then reproduced the bug by adding the dependency to an empty project and I saw the same output of clap not being able to compile.

I know this works correctly in my windows install, but I also compiled that project yesterday in a ubuntu 18.04 not through wsl and it worked.

I'm not sure if the issue is with WSL or ubuntu 20.04

### Debug Output

_No response_

---

_Label `T: bug` added by @IceSentry on 2021-06-13 17:32_

---

_Comment by @pksunkara on 2021-06-13 18:45_

This looks like nothing wsl or Ubuntu related. Just pure rust issue. But I
don't understand why indexmap is failing to compile.

On Sun, Jun 13, 2021, 18:32 Charles Giguere ***@***.***>
wrote:

> Please complete the following tasks
>
>    - I have searched the discussions
>    <https://github.com/clap-rs/clap/discussions>
>    - I have searched the existing issues
>
> Rust Version
>
> rustc 1.52.1 (9bc8c42bb 2021-05-09)
> Clap Version
>
> 3.0.0-beta.2
> Minimal reproducible code
>
> fn main() {
> 	println!("Hello, world!");
> }
>
> in Cargo.toml
>
> [package]name = "clap-test"version = "0.1.0"authors = ["author"]edition = "2018"
> # See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
>
> [dependencies]clap = "3.0.0-beta.2"
>
> Steps to reproduce the bug with the above code
>
> run cargo build in wsl with ubuntu 20.04
> Actual Behaviour
>
>  > cargo build
>    Compiling clap v3.0.0-beta.2
> error[E0107]: this struct takes 3 type arguments but only 2 type arguments were supplied
>   --> /home/<username>/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-beta.2/src/parse/matches/arg_matches.rs:77:22
>    |
> 77 |     pub(crate) args: IndexMap<Id, MatchedArg>,
>    |                      ^^^^^^^^ --  ---------- supplied 2 type arguments
>    |                      |
>    |                      expected 3 type arguments
>    |
> note: struct defined here, with 3 type parameters: `K`, `V`, `S`
>   --> /home/<username>/.cargo/registry/src/github.com-1ecc6299db9ec823/indexmap-1.6.2/src/map.rs:76:12
>    |
> 76 | pub struct IndexMap<K, V, S> {
>    |            ^^^^^^^^ -  -  -
> help: add missing type argument
>    |
> 77 |     pub(crate) args: IndexMap<Id, MatchedArg, S>,
>    |                                             ^^^
>
> error: aborting due to previous error
>
> For more information about this error, try `rustc --explain E0107`.
> error: could not compile `clap`
>
> To learn more, run the command again with --verbose.
>
> Expected Behaviour
>
> Compiles successfully when added as a dependency
> Additional Context
>
> I recently completely reinstalled wsl with ubuntu 20.04 and I have nothing
> but the rust toolchain installed on top of the default installation and
> anything required to compile rust projects. I can compile other projects
> that do not use clap. Yes, I ran an apt update and apt upgrade. I tried
> building a project using clap and saw this output. I then reproduced the
> bug by adding the dependency to an empty project and I saw the same output
> of clap not being able to compile.
>
> I know this works correctly in my windows install, but I also compiled
> that project yesterday in a ubuntu 18.04 not through wsl and it worked.
>
> I'm not sure if the issue is with WSL or ubuntu 20.04
> Debug Output
>
> *No response*
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/clap-rs/clap/issues/2535>, or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABKU345KHMNXMNE6ZLAJWDTSTTSLANCNFSM46T522JQ>
> .
>


---

_Comment by @pksunkara on 2021-06-13 22:59_

Looks like it's an issue with rust and WSL and ubuntu. There's a workaround at https://github.com/bluss/indexmap/issues/144#issuecomment-666629293.

---

_Closed by @pksunkara on 2021-06-13 22:59_

---

_Locked by @clap-rs on 2021-06-13 22:59_

---

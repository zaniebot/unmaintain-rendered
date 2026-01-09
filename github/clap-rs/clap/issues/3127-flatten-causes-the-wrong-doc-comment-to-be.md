---
number: 3127
title: "`flatten` causes the wrong doc-comment to be respected"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2021-12-09T16:17:30Z
updated_at: 2021-12-10T20:07:29Z
url: https://github.com/clap-rs/clap/issues/3127
synced_at: 2026-01-07T13:12:19-06:00
---

# `flatten` causes the wrong doc-comment to be respected

---

_Issue opened by @epage on 2021-12-09 16:17_

<a href="https://github.com/epage"><img src="https://avatars.githubusercontent.com/u/60961?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [epage](https://github.com/epage)**
_Thursday Jan 16, 2020 at 13:46 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/333_

----

With `clap-verbosity-flag` "3.0.0"

```
use structopt::StructOpt;
use clap_verbosity_flag::Verbosity;

#[derive(Debug, StructOpt)]
/// Foo
struct Cli {
    #[structopt(flatten)]
    verbose: Verbosity,
}

fn main() {
    Cli::from_args();
}
```
will cause clap-verbosity-flag's documentation to be used for the help instead of the user-specified one
```
❯ ./target/debug/examples/simple --help
clap-verbosity-flag 0.3.0
Easily add a `--verbose` flag to CLIs using Structopt

# Examples

```rust use structopt::StructOpt; use clap_verbosity_flag::Verbosity;

/// Le CLI #[derive(Debug, StructOpt)] struct Cli { #[structopt(flatten)] verbose: Verbosity, } # # fn main() {} ```

USAGE:
    simple [FLAGS]

FLAGS:
    -h, --help
            Prints help information

    -q, --quiet
            Pass many times for less log output

    -V, --version
            Prints version information

    -v, --verbose
            Pass many times for more log output

            By default, it'll only report errors. Passing `-v` one time also prints warnings, `-vv` enables info
            logging, `-vvv` debug, and `-vvvv` trace.
```

I am working around it in clap-verbosity-flag by. not providing a doc comment (https://github.com/rust-cli/clap-verbosity-flag/pull/21).

A workaround is explained in https://github.com/TeXitoi/structopt/issues/333#issuecomment-712265332


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/epage"><img src="https://avatars.githubusercontent.com/u/60961?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [epage](https://github.com/epage)**
_Thursday Jan 16, 2020 at 13:46 GMT_

----

Based on reports (https://github.com/rust-cli/clap-verbosity-flag/issues/20) I'm guessing  https://github.com/TeXitoi/structopt/commit/e2270deae2c9b930541afecd4c28f41ddd1d669b broke this


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Thursday Jan 16, 2020 at 15:23 GMT_

----

> will cause clap-verbosity-flag's documentation to be used for the help instead of the user-specified one

But there's no user specified help here as far as I can see 😕 


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/epage"><img src="https://avatars.githubusercontent.com/u/60961?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [epage](https://github.com/epage)**
_Thursday Jan 16, 2020 at 15:24 GMT_

----

```rust
/// Foo
struct Cli {
```

The user's doc-comment is being overridden by the doc-comment from the chain.  Instead of "Foo", we're getting library documentation


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Jan 22, 2020 at 13:30 GMT_

----

This is caused by #290 since doc comments on top of struct/enum are top level attributes too :(


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/ssokolow"><img src="https://avatars.githubusercontent.com/u/46915?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [ssokolow](https://github.com/ssokolow)**
_Monday Jun 22, 2020 at 21:31 GMT_

----

I just noticed that this also causes the doc comment on my boilerplate opts (which get flattened into the main struct) to override the `about` attribute on the main struct.

I'm really starting to feel like I need to write a regression/integration suite on my `--help` output just to feel safe using `--help` generation.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/pizzamig"><img src="https://avatars.githubusercontent.com/u/57148?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [pizzamig](https://github.com/pizzamig)**
_Wednesday Jul 22, 2020 at 19:29 GMT_

----

> This is caused by #290 since doc comments on top of struct/enum are top level attributes too :(

Is there any update or way to fix this?
Currently I cannot document a struct and re-use it flattened and the workaround to move documentation to the lib doesn't work for me now, it would require quite some changes in my code.

The complexity of structopt is too high for me right now to try to find a fix.

thanks in advance


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Jul 22, 2020 at 20:49 GMT_

----

I tried to fix it. I couldn't. I'll take another round a bit later.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vorner](https://github.com/vorner)**
_Saturday Oct 17, 2020 at 09:22 GMT_

----

Hello

I guess the „proper“ way to fix it will be hard and will take time (I'm not sure if reordering the calls inside the `StructOptInternal::augment_clap` to call these after descending into flattened fields instead of at the beginning would work ‒ it seems `version` is called at the end, so possibly the `.about` and similar could too?).

But if it is hard, would it make sense to add some kind of `#[no_about]` parameter that would skip all the auto-generated application information, like `about` and `version`? I guess most people know that a struct will be used as flattened field and could annotate them.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Saturday Oct 17, 2020 at 10:30 GMT_

----

By default, there is no about, but doc comment are `about`, so the workaround would be no doc comment on these structs.


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vorner](https://github.com/vorner)**
_Saturday Oct 17, 2020 at 10:40 GMT_

----

I've figured that, but it is kind of bad for structs provided by libraries. I want to have doc comment to render on [docs.rs](https://docs.rs), I also tend to `#[warn(missing_docs)]`. But then `structdoc` tries to interpret it and hijacks it if someone flattens the structure into their top-level `Opts` structure, which is bad.

But as the author of the library I know my struct should not be used directly and that it should never provide the `about` and `--version`. I want to mark it as just a source of more fields, not to do anything with the `App` at all.

https://docs.rs/spirit-daemonize/0.3.2/spirit_daemonize/struct.Opts.html

Maybe the right name would be `#[ignore_doc_comment]` or `#[flattenable]` or like that? Or is this orthogonal to this bug?


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vorner](https://github.com/vorner)**
_Monday Oct 19, 2020 at 15:59 GMT_

----

Just for the record, I've figured a workaround that keeps them for the docs.rs, but doesn't hijacks them for the about:

```
#[cfg_attr(not(doc), allow(missing_docs))]
#[cfg_attr(doc, doc = r#"
The doc comment goes here

Following the doc comment
"#)]
```

It simply enables the doc comment only for the documentation generation (and seems to work with doc tests too)


---

_Comment by @epage on 2021-12-09 16:17_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Monday Oct 19, 2020 at 16:36 GMT_

----

I've put a link to your workaround in the description of the issue for ease of access.


---

_Label `A-derive` added by @epage on 2021-12-09 16:30_

---

_Label `C-bug` added by @epage on 2021-12-09 16:30_

---

_Referenced in [clap-rs/clap#3129](../../clap-rs/clap/issues/3129.md) on 2021-12-09 16:36_

---

_Comment by @epage on 2021-12-10 20:06_

Assuming this is a valid reproduction case, this is fixed
```rust
use clap::StructOpt;

#[derive(Debug, StructOpt)]
#[structopt(name = "structopt-bug", about = "Show a bug in Structopt")]
struct Opt {
    #[structopt(flatten)]
    v: Verbose,
}

/// Re-usable Verbose flag
#[derive(Debug, StructOpt)]
struct Verbose {
    /// Enable the verbosity messages
    #[structopt(short)]
    verbose: bool,
}
fn main() {
    let opt = Opt::from_args();
    println!("{:?}", opt);
}
```
```
structopt-bug 

Show a bug in Structopt

USAGE:
    test-clap [OPTIONS]

OPTIONS:
    -h, --help    Print help information
    -v            Enable the verbosity messages
```

---

_Closed by @epage on 2021-12-10 20:06_

---

_Comment by @epage on 2021-12-10 20:07_

To add, we still have https://github.com/clap-rs/clap/issues/2983 which is about about/long_about interactions being weird

---

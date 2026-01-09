---
number: 6098
title: "demo.md is doesn't match the output of `demo --help`"
type: issue
state: open
author: winksaville
labels:
  - C-bug
  - A-docs
assignees: []
created_at: 2025-08-09T22:22:18Z
updated_at: 2025-08-12T17:51:40Z
url: https://github.com/clap-rs/clap/issues/6098
synced_at: 2026-01-07T13:12:20-06:00
---

# demo.md is doesn't match the output of `demo --help`

---

_Issue opened by @winksaville on 2025-08-09 22:22_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.89.0 (29483883e 2025-08-04)

### Clap Version

4.5.43

### Minimal reproducible code

```
use clap::Parser;

/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(version, about, long_about = None)]
struct Args {
    /// Name of the person to greet
    #[arg(short, long)]
    name: String,

    /// Number of times to greet
    #[arg(short, long, default_value_t = 1)]
    count: u8,
}

fn main() {
    let args = Args::parse();

    for _ in 0..args.count {
        println!("Hello {}!", args.name);
    }
}
```

### Steps to reproduce the bug with the above code

I created a new repo with:
```
cargo new demo
cd demo
cargo add clap --features derive`
```
Changed main.rs to the above code and then installed it:
```
$ cargo install --path .
  Installing demo v0.1.0 (/home/wink/data/prgs/clap/demo)
    Updating crates.io index
     Locking 30 packages to latest Rust 1.89.0 compatible versions
   Compiling proc-macro2 v1.0.95
   Compiling unicode-ident v1.0.18
   Compiling utf8parse v0.2.2
   Compiling anstyle-query v1.1.4
   Compiling anstyle v1.0.11
   Compiling is_terminal_polyfill v1.70.1
   Compiling colorchoice v1.0.4
   Compiling strsim v0.11.1
   Compiling heck v0.5.0
   Compiling clap_lex v0.7.5
   Compiling anstyle-parse v0.2.7
   Compiling anstream v0.6.20
   Compiling clap_builder v4.5.43
   Compiling quote v1.0.40
   Compiling syn v2.0.104
   Compiling clap_derive v4.5.41
   Compiling clap v4.5.43
   Compiling demo v0.1.0 (/home/wink/data/prgs/clap/demo)
    Finished `release` profile [optimized] target(s) in 3.18s
   Replacing /home/wink/.cargo/bin/demo
    Replaced package `demo v0.1.0 (/home/wink/data/prgs/clap/demo)` with `demo v0.1.0 (/home/wink/data/prgs/clap/demo)` (executable `demo`)
```


### Actual Behaviour

```
$ demo --help
Simple program to greet a person

Usage: demo [OPTIONS] --name <NAME>

Options:
  -n, --name <NAME>    Name of the person to greet
  -c, --count <COUNT>  Number of times to greet [default: 1]
  -h, --help           Print help
  -V, --version        Print version
```

### Expected Behaviour

Based on the the documentation behavior should be:
```
$ demo --help
A simple to use, efficient, and full-featured Command Line Argument Parser

Usage: demo[EXE] [OPTIONS] --name <NAME>

Options:
  -n, --name <NAME>    Name of the person to greet
  -c, --count <COUNT>  Number of times to greet [default: 1]
  -h, --help           Print help
  -V, --version        Print version
```

### Additional Context

Actually, this can be fixed by changing demo.md or demo.rs, I am preparing a PR.

### Debug Output

N/A

---

_Label `C-bug` added by @winksaville on 2025-08-09 22:22_

---

_Referenced in [clap-rs/clap#6099](../../clap-rs/clap/pulls/6099.md) on 2025-08-09 22:25_

---

_Comment by @epage on 2025-08-11 14:28_

As seen by #6099, we have tests that verify the content of `demo.md`.

Also, based on #6099, I suspect the Actual and Expected might be backwards.

Anyways.

Whats happening is that the `#[command(about)]` attribute defaults the crate description, per [the reference](https://docs.rs/clap/latest/clap/_derive/index.html#command-attributes).  If that attribute was missing, then clap would use the doc comment.  And the crate description can be found at https://github.com/clap-rs/clap/blob/0096ace8b15b20a77e71806eab4c8df10cae278c/Cargo.toml#L113.

Doing the reverse and updating the doc comment to match the crate description I feel would confuse the matter further since it would look like it might be using the doc comment when reality it isn't.

---

_Label `A-docs` added by @epage on 2025-08-11 14:28_

---

_Comment by @winksaville on 2025-08-11 16:34_

I believe I've gone through the steps a new user would do while reading the  first page of the docs and the result does not match what is documented.

I've tried changing demo.md, but that breaks the build. Then I changed demo.rs doc comment to match demo.md, you seem to not like that.

As a user I think the most important consideration is that the documentation matches what a user would most likely do. Since the current example has a Doc comment and a default Cargo.toml doesn't have a description field the Doc comment will be used for about.

This can be fixed in multiple ways, what is your preference?

---

_Comment by @epage on 2025-08-11 16:45_

> the result does not match what is documented.

Could you point to the documentation in question?

> default Cargo.toml doesn't have a description field

Note that the examples do not say to do `cargo new && cargo add clap && echo ... src/main.rs`.  We shouldn't make assumptions of whether `package.description` is specified or not in the users manifest.

---

_Comment by @winksaville on 2025-08-11 17:58_

> Could you point to the documentation in question?

On the [first page](https://docs.rs/clap/latest/clap/index.html) there is code with a Doc comment. That doc comment will be used for the about line in the --help when compiled with a default Cargo.toml as there would be no description field. That output does not match the contents of demo.md shown on that first page.

> We shouldn't make assumptions of whether package.description is specified or not in the users manifest.

But demo.md is assuming the string "A simple to use, efficient, and full-featured Command Line Argument Parser" is set somewhere, how is a person reading the first page know where that line comes from?

---

_Comment by @epage on 2025-08-11 18:31_

> > > I believe I've gone through the steps a new user would do while reading the first page of the docs and the result does not match what is documented.

> > Could you point to the documentation in question?

> On the [first page](https://docs.rs/clap/latest/clap/index.html) there is code with a Doc comment. That doc comment will be used for the about line in the --help when compiled with a default Cargo.toml as there would be no description field. That output does not match the contents of demo.md shown on that first page.

Huh, I think there was confusion.  When you said the example + output didn't match what was documented, I took that to mean that where was some text documenting what the behavior should be that didn't match the results for that example.

> But demo.md is assuming the string "A simple to use, efficient, and full-featured Command Line Argument Parser" is set somewhere, how is a person reading the first page know where that line comes from?

That is a frontpage example on a library meant to quickly show what can be done.  It doesn't any of its behavior, instead needing to go to the reference or tutorial which is linked immediately after.

We could drop the `#[command(about)]` attribute so the doc comment is used but then that would be a little less idiomatic (depending on what the user is trying to do).

---

_Comment by @winksaville on 2025-08-11 19:11_

> We could drop the #[command(about)] attribute so the doc comment is used but then that would be a little less idiomatic (depending on what the user is trying to do).

Personally, I think the example on the first page should be idiomatic, let me know what you'd like me to do.

---

_Comment by @epage on 2025-08-11 19:54_

If we're in agreement that the demo should be idiomatic, then I'm not seeing what there is to change and would be inclined to close this issue.

---

_Comment by @winksaville on 2025-08-11 22:16_

Yes we're in agreement it should be idiomatic.

That first page says "And try it out" and they do, likely without a description, and they will not see "A simple to use, efficient, and full-featured Command Line Argument Parser" in the `--help` output, that feels wrong to me.



---

_Comment by @epage on 2025-08-12 13:21_

So I feel we've come to a common understanding, even if not agreement, on several points but I worry this is going to be spinning in circles, so let's summarize our options to see if there is reasonable way forward

Options
- Remove `#[command(about)]` making it non-idiomatic
- Make the doc-comment match `package.description`, adding a layer of confusion
- Stop validating the example (increasing risk that examples do not do what we expect)
- Remove the doc-comment (maybe move it to a mod comment), still having a discrepancy when there isn't a `package.description` but its more apparent
- Close this issue as not-planned

Any others that I'm missing?

---

_Comment by @winksaville on 2025-08-12 14:24_

I'd add one other:

 * Add a test case specifically for demo.rs/demo.md and change demo.md to have the correct about message.



---

_Comment by @epage on 2025-08-12 16:43_

`demo.md` **is** a test.  We use the `trycmd` crate to execute the `console` blocks in our `md` files.  That is why tests failed when #6099 tried to change `demo.md` only.  So `demo.md` does have the correct usage for `demo.rs` as an example within the `clap` package.

---

_Comment by @winksaville on 2025-08-12 17:42_

Actually l, I thought of another approach, add a sentence in the first page the possible sources for the version/about/... commands, maybe something like; "The contents for #command([version](<URL>), [about](<URL>)) come multiple sources, click the links for details." And those links would go directly to the appropriate docs for those commands. Just a thought.

---

_Comment by @epage on 2025-08-12 17:51_

I would lean against that as it opens the question for explaining other parts which is the point of the docs we then link to after the example

---

---
number: 3569
title: Add a proc_macro attribute for the main function similar to how paw does it
type: issue
state: closed
author: x0f5c3
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-author
assignees: []
created_at: 2022-03-23T15:14:14Z
updated_at: 2022-05-04T18:19:59Z
url: https://github.com/clap-rs/clap/issues/3569
synced_at: 2026-01-07T13:12:19-06:00
---

# Add a proc_macro attribute for the main function similar to how paw does it

---

_Issue opened by @x0f5c3 on 2022-03-23 15:14_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

I was moving my project from StructOpt to Clap and wanted something to replace the paw integration that structopt had, and I thought that moving the whole macro to the Clap source code to avoid pulling in paw would be better than implementing `ParseArgs` trait that paw uses for every type that implements Parser. 

### Describe the solution you'd like

Have clap expose a proc-macro that can be used on the main function to add an argument that implements Parser.

```rust
use clap::Parser;

#[derive(Parser)]
struct Command {
   pub arg: String
}

#[clap::main]
fn main(argv: Command) {
   println!("{}", argv.arg)
}
```

A prototype of this exists in #3568

### Alternatives, if applicable

_No response_

### Additional Context

The macro will insert every other attr it encounters above the main function and will check for asyncness so it can be put on top of other macros like `#[quit::main]` or `#[tokio::main]` and should work without an issue. Also it checks if the function returns a result or nothing and will either put a `?` at the end of the `try_parse()` method or `.unwrap()` if the function returns nothing.

---

_Label `C-enhancement` added by @x0f5c3 on 2022-03-23 15:14_

---

_Comment by @epage on 2022-03-23 15:29_

FYi I've updated the Issue text to match the template.  Please fill out the use case.  What is the problem you are trying to solve?  Then in the description, please clarify why this macro solves the problem.  Ideally, the Issue title would also focus on the problem.

In essence, I'm trying to understand why you feel its important for this to exist in clap.

As for the solution, some things we should consider
- How does this compose with other `main` macros like for async runtimes?
- How does this compose with `main` that returns errors?
- What does the path look like for the user updating a prototype to be more complete by having more detailed exit codes?
- How will this fit in if we ever decided to merge in something like [fncmd](https://github.com/yuhr/fncmd)?
- How does this compose with "before everything" helpers like human-panic?  Should those always go after parsing or do we need to allow them to happen before somehow?
  - I had also considered setting up colors (e.g. I don't use clap's `Auto`) but I'd rather focus on improving `Auto`
- What is the documentation impact, including tutorial, examples, reference?

---

_Label `A-derive` added by @epage on 2022-03-23 15:29_

---

_Label `S-waiting-on-author` added by @epage on 2022-03-23 15:30_

---

_Comment by @x0f5c3 on 2022-03-27 17:00_

Some things I could come up with at this point:

- I added examples on how it can be used along with macros like `#[tokio::main]`
- `fncmd` integration would cause the user to specify the arguments and flags as arguments for the main function, the macro I'm proposing would allow passing one argument to main that implements `Parser` so it would be a choice for the user, either they specify the arguments in `fn main` or they create a struct that implements `Parser` so this way it gives more options to the user without collision
- The documentation changes would be minor because the whole usage consists of creating a struct as you would always and annotating the main function with `#[clap::main]` which I think is pretty foolproof

I added answers to other questions to the issue.



---

_Comment by @epage on 2022-03-28 13:03_

> Describe your use case
>
> I was moving my project from StructOpt to Clap and wanted something to replace the paw integration that structopt had, 

While this does describe the problem you are trying to solve, what I'm looking for is what problem this is solving for users of clap generally.  What problem for a clap user is this solving?  Say for my various programs I maintain, what advantage would I get porting to this macro?  Or for someone writing their first CLI in Rust?

>  Also it checks if the function returns a result or nothing and will either put a ? at the end of the try_parse() method or .unwrap() if the function returns nothing.

We should probably not be using `try_parse` and `?` as the users error might not be compatible, this will lose clap's exit code, and an `unwrap` would produce the wrong type of output.

When asking about returning errors, it was more about making sure we don't interfere with the user doing it.  It appears your macro preserves the error type, so as long as we  test it, that should work.

> fncmd integration would cause the user to specify the arguments and flags as arguments for the main function, the macro I'm proposing would allow passing one argument to main that implements Parser so it would be a choice for the user, either they specify the arguments in fn main or they create a struct that implements Parser so this way it gives more options to the user without collision

So we would provide two separate categories of function attribute macros that don't interect?  What if a `main` macro would be useful for fncmd?

> The documentation changes would be minor because the whole usage consists of creating a struct as you would always and annotating the main function with #[clap::main] which I think is pretty foolproof

Should we make this attribute the default in tutorials and examples?  How much do we need to document how it composes with other macros or with error handling?  How much and where do we need to talk about life-before-`parse`, exit codes, customizing `parse`s behavior, etc?  Before, we exposed a function and people could easily see what that function did.  Now we would be providing an opaque macro that is scarrier to dig into and we need to consider how we would help users with that.

---

Questions I didn't see addressed:

> What does the path look like for the user updating a prototype to be more complete by having more detailed exit codes?
>
> How does this compose with "before everything" helpers like human-panic? Should those always go after parsing or do we need to allow them to happen before somehow? 

---

_Comment by @epage on 2022-05-04 18:19_

Without further details, it is unclear what problem is being solved by this request and how to safely interact with other features or potential features.  I'm going to go ahead and close this out.  We can re-evaluate if/when more details become available.

---

_Closed by @epage on 2022-05-04 18:19_

---

---
number: 3513
title: "derive feature: an attribute to add a prefix to all arg names in a struct, for use with flatten"
type: issue
state: open
author: wfraser
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2022-02-26T09:16:09Z
updated_at: 2025-11-26T23:55:50Z
url: https://github.com/clap-rs/clap/issues/3513
synced_at: 2026-01-10T01:27:42Z
---

# derive feature: an attribute to add a prefix to all arg names in a struct, for use with flatten

---

_Issue opened by @wfraser on 2022-02-26 09:16_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.2

### Describe your use case

I work on a large Rust project that has many different modules which have command-line options to add to the program. For readability and to reduce collisions, we prefix the name of the flags with the name of the module it belongs to. For example, we have a module `log`, with a struct with several fields, and each of them has a corresponding flag like `--log.whatever`. The main program then combines all these together into one big options struct.

We've been using docopt, which lets you omit the prefix from the struct field name at least, but you still have to type it manually in the help text for each flag, which doesn't scale well.

Clap lets me use `#[clap(flatten)]` to separate these options into their own struct like I've been doing with docopt, but I'd have to add the prefix to all the field names, which then leaks into all the code that accesses it (yuck). It also doesn't let me preserve the `--prefix.name` argument style we've already established.

What I'd like is some way to add a prefix to all the arguments used with a particular struct.

### Describe the solution you'd like

A similar feature was discussed in https://github.com/clap-rs/clap/issues/3117, which proposed a `prefix` attribute to be placed alongside `flatten`, at the point of the sub-struct's inclusion into the main one. This was ultimately dismissed as being ugly and/or difficult to implement given how the derive code works.

This proposal is to attach the attribute to the sub-struct itself, which is much more easily implemented, and, at least for my use case, works just as well.

I have an implementation ready (https://github.com/wfraser/clap/tree/flatten-prefix) which makes it another type of casing style, using the `rename_all` attribute. The precise syntax is `#[clap(rename_all = "prefix:foo")]`. It can be combined with another prefix style as well: `#[clap(rename_all = "prefix:foo,snake")]`.

Example:
```rust
#[derive(Parser, Debug, PartialEq)]
struct Main {
    #[clap(long)]
    some_string: String,

    #[clap(flatten)]
    foo_args: Foo,

    #[clap(flatten)]
    bar_args: Bar,
}

#[derive(Parser, Debug, PartialEq)]
#[clap(rename_all = "prefix:foo", next_help_heading = "Foo options")]
struct Foo {
    #[clap(long)]
    some_param: String,
}

#[derive(Parser, Debug, PartialEq)]
#[clap(rename_all = "prefix:bar,pascal", next_help_heading = "Bar options")]
struct Bar {
    #[clap(long)]
    another_param: String,
}
```

help text:
```
clap-nesting

USAGE:
    clap-nesting [OPTIONS]

OPTIONS:
    -h, --help                         Print help information
        --some-string <SOME_STRING>

Foo options:
        --foo.some-param <SOME_PARAM>

Bar options:
        --bar.AnotherParam <ANOTHER_PARAM>
```

If this approach looks acceptable, I can submit my branch as a PR.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @wfraser on 2022-02-26 09:16_

---

_Comment by @pksunkara on 2022-02-26 10:14_

I was pretty sure we had an issue about this somewhere but I couldn't find it now.

This relates to #3221 

---

_Renamed from "derive feature: an attribute to add a prefix to all arg names, for use with flatten" to "derive feature: an attribute to add a prefix to all arg names in a struct, for use with flatten" by @wfraser on 2022-02-26 11:15_

---

_Label `A-derive` added by @epage on 2022-02-26 13:43_

---

_Label `S-duplicate` added by @epage on 2022-02-26 13:43_

---

_Comment by @epage on 2022-02-26 13:44_

This sounds like https://github.com/clap-rs/clap/issues/3117 which we had previously rejected.  I'm going to go ahead and close this as well but if I'm mistaken, feel free to speak up!

---

_Closed by @epage on 2022-02-26 13:44_

---

_Comment by @wfraser on 2022-02-26 21:08_

@epage: this is intended to be different from #3117, which was closed because implementing it is problematic.

Instead of #3117 which proposes to let the *user* of a struct determine the prefix of all its args, I want to let *the struct itself* determine its own prefix (the attribute is on the struct, not where it is flattened into). This greatly simplifies the implementation, because now two different Args impls don't need to cooperate.

@pksunkara sort of similar, but this doesn't have to do with env vars, and I propose implementing it in a pretty different way. In particular, this is only a `clap_derive` feature, and doesn't change `Args` at all.

Basically, I think this is substantially simpler than both of those proposals. Maybe that limits its utility too much, I don't know, but it would solve a problem for me that I don't see being solved any other way.

---

_Reopened by @epage on 2022-02-27 02:06_

---

_Comment by @epage on 2022-02-27 02:12_

Since the use case is slightly different, I'm re-opening for now.

While this more limited-scope proposal doesn't run into most of the issues in #3117, it does run into the issue of not being able to work with `flatten`.  We could make it a compile error to set a prefix/namespace for IDs *and* flatten.  I am concerned about having random holes within the functionality where things don't work.  Documenting the derive is already tricky and documentation is only a half solution to any problem.  This isn't a "no" but I am interested in hearing more thoughts on this and and to give this time to simmer to see how things might look over time.

Some quick thoughts on details of the proposal
- Stringly-specified APIs are not ideal
- We can't reuse `rename_all` for this anyways because we will be shifting its focus to only renaming `value_name`, see #3453 and #2475

---

_Label `S-duplicate` removed by @epage on 2022-02-27 02:12_

---

_Label `S-waiting-on-decision` added by @epage on 2022-02-27 02:12_

---

_Comment by @wfraser on 2022-02-28 20:59_

Yeah those are good points.

I agree, a separate attribute would probably be better. I just piggy-backed on `rename_all` because it made implementation so easy; a separate attribute is probably not that much more difficult though. I'll try and write up a proof-of-concept that way.

Not working with `flatten` is a little unfortunate, but it seems like that's a pretty inherent difficulty due to how the code generator works. From reading previous discussions, and now the code, I don't really see how that could be made to work. That said, I think attaching the prefix to the struct itself solves most use cases. The only one I don't thing it works for is people who want to re-use an args struct multiple times, with a different prefix each time.

---

_Comment by @wfraser on 2022-02-28 21:23_

Here's a proof-of-concept of a new `prefix` attr that can be applied at the struct level: https://github.com/wfraser/clap/commit/prefix-attr

example:
```rust
#[derive(Parser, Debug)]
struct Main {
    #[clap(short)]
    s: Option<String>,

    #[clap(long, default_value = "spaghetti")]
    some_string: String,

    #[clap(long)]
    same_name: Option<String>,

    #[clap(flatten)]
    foo_args: Foo,

    #[clap(flatten)]
    bar_args: Bar,
}

#[derive(Parser, Debug, Default)]
#[clap(prefix = "foo", next_help_heading = "Foo options")]
struct Foo {
    #[clap(long)]
    some_param: Option<String>,

    #[clap(long)]
    same_name: Option<String>, // without prefix, would conflict with the one in Main
}

#[derive(Parser, Debug, Default)]
#[clap(prefix = "bar", next_help_heading = "Bar options")]
struct Bar {
    #[clap(long, env)]
    another_param: Option<String>,
}
```

```
clap-prefixes

USAGE:
    clap-prefixes [OPTIONS]

OPTIONS:
    -h, --help                         Print help information
    -s <S>
        --same-name <SAME_NAME>
        --some-string <SOME_STRING>    [default: spaghetti]

Foo options:
        --foo.same-name <FOO_SAME_NAME>
        --foo.some-param <FOO_SOME_PARAM>

Bar options:
        --bar.another-param <BAR_ANOTHER_PARAM>    [env: BAR_ANOTHER_PARAM=]
```

---

_Comment by @epage on 2022-02-28 21:55_

>  The only one I don't thing it works for is people who want to re-use an args struct multiple times, with a different prefix each time.

The problem is this seems to be the majority of the cases.  This is the first time someone has brought up this combination

I think this gives me a way to express a thought I was having that I couldn't put into words before.  Maintainership requires managing expectations.  I worry that implementing this, though it helps some people, will make it harder for us to manage expectations for reusable structs and adapting this combination of features to people's needs.

> Yeah those are good points.

Some other things I seemed to have missed in the original
- `.` separators are not idiomatic for option names and not something we'd want to be a default, instead `-` should be a default.  I'd be concerned about making this yet-another-configurable behavior.
- We are coupling namespacing in `ArgMatches` with namespacing of long option names (but not shorts), and namespacing of env variables.
  - I'd imagine users will want one of these without some of the others
  - We'd need to error on shorts being set

Overall, this is seeming too narrow of a use case to justify the extra logic and documentation when a user can implement all of this themselves.

Overall, I'm still leaning towards rejecting this.

---

_Referenced in [grapl-security/grapl#1855](../../grapl-security/grapl/pulls/1855.md) on 2022-08-01 01:47_

---

_Referenced in [clap-rs/clap#4250](../../clap-rs/clap/pulls/4250.md) on 2022-09-23 06:13_

---

_Comment by @Pure-Peace on 2022-12-09 14:25_

Looking forward to this feature. 

Currently I use a declarative macro to create prefixed configuration structs.

```rust
#[macro_export]
macro_rules! define_db_config {
    ($vis: vis $struct_name: ident, $prefix: ident) => {
        paste::paste! {
            /// Database configurations
            #[derive(Parser, Debug)]
            $vis struct $struct_name {
                /// Database connection URL.
                #[arg(long)]
                pub [<$prefix _db_url>]: String,

                /// Set the maximum number of connections of the pool.
                #[arg(long)]
                pub [<$prefix _db_max_connections>]: Option<u32>,

                /// Set the minimum number of connections of the pool.
                #[arg(long)]
                pub [<$prefix _db_min_connections>]: Option<u32>,

                /// Set the timeout duration when acquiring a connection.
                #[arg(long)]
                pub [<$prefix _db_connect_timeout>]: Option<u64>,

                /// Set the maximum amount of time to spend waiting for acquiring a connection.
                #[arg(long)]
                pub [<$prefix _db_acquire_timeout>]: Option<u64>,

                /// Set the idle duration before closing a connection.
                #[arg(long)]
                pub [<$prefix _db_idle_timeout>]: Option<u64>,

                /// Set the maximum lifetime of individual connections.
                #[arg(long)]
                pub [<$prefix _db_max_lifetime>]: Option<u64>
            }
        }
    };
}

define_db_config!(pub DatabaseA, a);
define_db_config!(pub DatabaseB, b);

#[derive(Parser, Debug)]
pub struct DbServiceConfig {
    #[command(flatten)]
    pub db_a: DatabaseA,

    #[command(flatten)]
    pub db_b: DatabaseB,
}
```

---

_Referenced in [clap-rs/clap#5050](../../clap-rs/clap/issues/5050.md) on 2023-08-01 01:06_

---

_Referenced in [clap-rs/clap#4556](../../clap-rs/clap/issues/4556.md) on 2023-11-09 16:11_

---

_Referenced in [clap-rs/clap#5374](../../clap-rs/clap/issues/5374.md) on 2024-02-24 21:39_

---

_Comment by @wfraser on 2024-05-10 22:53_

It seems unlikely that this feature will ever get accepted, and I got tired of maintaining a large patch to the clap codebase, so I implemented this functionality as a proc macro that can be used alongside clap instead: https://github.com/wfraser/clap_wrapper

---

_Referenced in [posit-dev/ark#708](../../posit-dev/ark/issues/708.md) on 2024-10-11 14:30_

---

_Comment by @killzoner on 2024-12-09 18:38_

After working for a while with `paste` after https://github.com/clap-rs/clap/issues/3513#issuecomment-1344372578 suggestion, `paste` recently moved to unmaintained, so I gave it a try to wrap the whole logic in a proc macro.

The project is published at https://docs.rs/clappen, feel free to give it a try. It's basically a macro generating declarative macro. 

This macro is then able to create a struct with prefix that is maintained within `flatten`, relying on default behaviour of `long` and `env` args. Prefix is on per struct basis and field basis (both can be combined)

Apart from the macro wrapping, it's all standard `clap` without any other layer to the lib

---

_Comment by @cbeck88 on 2025-11-26 23:55_

Two years later, I have an alternate version of `clap-derive` which was built with a different architecture, so that it can support all the things clap does, but also permits the kind of runtime information passing between parent and child that makes it easy to support flatten-with-prefix. It is not 1.0 at time of writing, but it is reaching maturity.

https://github.com/cbeck88/conf-rs

Hope this helps someone

---

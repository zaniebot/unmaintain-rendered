---
number: 1807
title: Better than display_order and DeriveDisplayOrder argument positioning
type: issue
state: open
author: I60R
labels:
  - C-enhancement
  - A-help
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2020-04-11T14:06:51Z
updated_at: 2022-09-16T20:29:58Z
url: https://github.com/clap-rs/clap/issues/1807
synced_at: 2026-01-10T01:27:07Z
---

# Better than display_order and DeriveDisplayOrder argument positioning

---

_Issue opened by @I60R on 2020-04-11 14:06_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Describe your use case

The same as for `Arg::display_order(usize)` and `DeriveDisplayOrder` but should be easier to apply

### Describe the solution you'd like

- Works well with `structopt`/`clap_derive` flattening
- Doesn't move both arguments to top or to the bottom like `.display_order` (because 999 is default for every argument).
- `DeriveDisplayOrder` shouldn't override it

### Alternatives, if applicable

Keep using `Arg::display_order(usize)` and `DeriveDisplayOrder` 

### Additional context

---

---

_Label `T: new feature` added by @I60R on 2020-04-11 14:06_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-11 14:19_

---

_Label `C: args` added by @pksunkara on 2020-04-11 14:20_

---

_Label `C: usage strings` added by @pksunkara on 2020-04-11 14:20_

---

_Label `P4: nice to have` added by @pksunkara on 2020-04-11 14:20_

---

_Comment by @kbknapp on 2020-04-29 03:32_

This might be pretty hairy to implement, especially if we end up trying to detect loops (`A` says display after `B`, and `B` says display after `A`).

I'm leaning towards closing this feature, as it'd significantly impact the code used to generate help messages for (my subjectively) little benefit. 

@I60R could you elaborate on why doesn't `DeriveDisplayOrder` work in your case? Or `Arg::help_heading` doesn't fit your use case?

---

_Comment by @I60R on 2020-05-06 06:42_

>This might be pretty hairy to implement, especially if we end up trying to detect loops (A says display after B, and B says display after A).

Okay, I understand. 

>@I60R could you elaborate on why doesn't DeriveDisplayOrder work in your case?

I actually use it. The problem was that it don't play well with `structopt` flattening, more specifically it doesn't allow to move argument into separate struct while keeping its order in --help message. This could be easily `structopt` problem, however it also seems that it occurs because current `display_order` functionality in `clap` is somehow limited.

`Arg::display_after` in `clap` was simply the most straightforward solution here that I was able to imagine

---

_Comment by @kbknapp on 2020-05-11 18:41_

> I actually use it. The problem was that it don't play well with structopt flattening, more specifically it doesn't allow to move argument into separate struct while keeping its order in --help message. This could be easily structopt problem, however it also seems that it occurs because current display_order functionality in clap is somehow limited.

Aaah ok, I understand. Yeah to me that sounds like either a feature/bug we could work on as part of `clap_derive` (what was `structopt`).

I'm open to others opinions as well, but even when flattening the order should be preserved. That sounds like a new issue to me, or perhaps we re-name this issue and update the OP with this new constraint/requirement.

---

_Comment by @pksunkara on 2020-05-11 18:43_

@I60R Could you provide us a sample code where it doesn't work? AFAIK, even flattening should make it work.

---

_Renamed from "Add display_after for easier arg positioning in --help message" to "Better than display_order and DeriveDisplayOrder argument positioning" by @I60R on 2020-05-18 20:03_

---

_Comment by @I60R on 2020-05-18 20:11_

>I'm open to others opinions as well, but even when flattening the order should be preserved. That sounds like a new issue to me, or perhaps we re-name this issue and update the OP with this new constraint/requirement.

Done. I also think that I've found what argument positioning I want:

1) With `DeriveDisplayOrder` we increment each arg `display_order` starting from 1000 but only for those that had it unspecified. Then, if multiple args sharing the same `display_order` they would be sorted alphabetically (default behavior).

2) Instead or alongside with we may provide `display_all` method for `App` and `ArgGroup` which would be used like this:

```rust
    display_all = &[
        ("FLAGS", &[
            "first",
            "second",
            "x",
            ...
        ]),
        ("OPTIONS", &[
            "nth",
            "nth+1",
            ...
        ]),
        ...,
        ("CUSTOM SECTION", &[
            ...,
            "last-1",
            "last"
        ])
    ],
```

it could substitute `display_order` and `display` functions, plus add more styling options like custom section headers, separators, etc.

@kbknapp is it possible to be implemented?

---

_Comment by @pksunkara on 2020-05-18 20:22_

> custom section headers

We have them. `help_heading`.

> is it possible to be implemented?

You are not exactly giving us a sample of the code where `DeriveDisplayOrder` is not working in clap_derive. As I said before, flattening the struct should work with the option well.

---

_Comment by @I60R on 2020-05-18 20:44_

>We have them. help_heading.

The whole point isn't in having them, but in having them in the same place and not using them explicitly.

>You are not exactly giving us a sample of the code where DeriveDisplayOrder is not working in clap_derive. As I said before, flattening the struct should work with the option well.

That's actually the problem e.g. if you have this unflattened struct:

```rust
#[derive(StructOpt)]
#[structopt(global_settings=&[DeriveDisplayOrder])]
struct Opt {
    a: bool,
    b: bool,
    c: bool,
    ...
}
```

and then refactors it to flattened

```rust
#[derive(StructOpt)]
#[structopt(global_settings=&[DeriveDisplayOrder])]
struct Opt {
    b: bool,
    #[structopt(flatten)]
    flattened: Flattened,
    ...
}

#[derive(StructOpt)]
struct Flattened {
    a: bool,
    c: bool,
}
```

you may find that the original order is messed up (b-a-c) and there's no straightforward way to fix it

---

_Comment by @pksunkara on 2020-05-19 08:39_

> With DeriveDisplayOrder we increment each arg display_order starting from 1000 but only for those that had it unspecified. Then, if multiple args sharing the same display_order they would be sorted alphabetically (default behavior).

Yes, that can be implemented. 

---

_Referenced in [epage/clapng#153](../../epage/clapng/issues/153.md) on 2021-12-06 20:12_

---

_Label `C: args` removed by @epage on 2021-12-08 20:04_

---

_Label `C: usage strings` removed by @epage on 2021-12-08 20:04_

---

_Label `A-help` added by @epage on 2021-12-08 20:04_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:13_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:13_

---

_Comment by @epage on 2021-12-09 22:24_

> only for those that had it unspecified

This is already done.  We detect defaults and don't override them.  The only thing remaining is what the starting display order should be when deriving.

---

_Referenced in [clap-rs/clap#3138](../../clap-rs/clap/pulls/3138.md) on 2021-12-09 22:25_

---

_Comment by @epage on 2021-12-09 22:33_

So the crux of the issue is that if the struct hierarchy doesn't match your visual hierarchy, you have to manually specify all of the display orders.

I was wondering if we could do something like `App::help_heading` (which has naming issues similar to #1553) but the problem is a derive user doesn't have the opportunity to set an app function outside of when they do the flatten or define the struct.  We can't just do a backup-restore approach because they would need to set where to pick up the counting.

---

_Removed from milestone `3.1` by @epage on 2021-12-09 22:33_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 22:33_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 22:33_

---

_Comment by @pksunkara on 2021-12-10 14:06_

> you have to manually specify all of the display orders.

I think the case for that would be pretty niche. I think people would just find it easy to reorder the struct.

---

_Comment by @epage on 2021-12-10 17:23_

If we feel its niche enough, is there anything left to do for this issue?

---

_Comment by @pksunkara on 2021-12-10 17:28_

> The only thing remaining is what the starting display order should be when deriving.

I think that. Unless another issue covers that? I will leave it up to you.

---

_Comment by @epage on 2021-12-10 17:33_

For the starting display order value for deriving, I think we'd need more information on why one value should be picked over another.

---

_Comment by @epage on 2021-12-13 19:29_

Had some ideas:

In #1553, we are proposing renaming `help_heading` to `next_help_heading`.

What if we add a `next_display_order` that acts similar to `next_help_heading` in the builder API?  When passing in `None`, we default all of the display orders, making it like the user doesn't set `DeriveDisplayOrder` (this is similar to what is pproposed in #2808).

In the derive API, the one change is we won't save/restore the display order because that will be disruptive to the "do nothing" user.

For derive users, at minimum this gives them the ability to set the next display order.  The problem is they can't then change the display order mid-way to correct for wanting to jump around.  To solve that, we turn the `clap` attribute into a shorthand for `clap::app`, `clap::arg` , etc (depending on the context), see #1553.  We then add support for the user to specify `clap::app` on `Args` fields.  These would be app calls made before adding the `Arg` to the `App`.  This would allow both `next_help_heading` and `next_display_order` to be called by the user whenever they want, like they can with the builder API.

---

_Referenced in [clap-rs/clap#2983](../../clap-rs/clap/issues/2983.md) on 2021-12-14 17:20_

---

_Referenced in [clap-rs/clap#3269](../../clap-rs/clap/issues/3269.md) on 2022-01-09 01:59_

---

_Referenced in [clap-rs/clap#3221](../../clap-rs/clap/issues/3221.md) on 2022-01-25 15:51_

---

_Comment by @epage on 2022-02-08 01:21_

#3414 did
- `help_heading` -> `next_help_heading`
- Added `next_display_order`

The only remaining work is to allow setting app attributes on arguments in the derive.

---

_Referenced in [clap-rs/clap#3444](../../clap-rs/clap/pulls/3444.md) on 2022-02-11 01:33_

---

_Comment by @epage on 2022-05-09 20:17_

> The only remaining work is to allow setting app attributes on arguments in the derive.

Huh, so derive helper attributes can only be identifiers.  Item paths seem to be used for attribute macros.

---

_Comment by @epage on 2022-05-09 20:34_

**Context**: Clap has context-specific builder functions.  `next_help_heading` is one example where it will set a field on all future `Arg`s added to the `Command`.
```rust
    let cmd = Command::new("blorp")
        .author("Will M.")
        .about("does stuff")
        .version("1.4")
        .arg(
            arg!(-f --fake "some help")
                .required(true)
                .value_names(&["some", "val"])
                .takes_value(true)
                .use_value_delimiter(true)
                .require_value_delimiter(true)
                .value_delimiter(':'),
        )
        .next_help_heading(Some("NETWORKING"))
        .arg(
            Arg::new("no-proxy")
                .short('n')
                .long("no-proxy")
                .help("Do not use system proxy settings"),
        )
        .args(&[Arg::new("port").long("port")]);
```

**Problem**: this is not currently exposed in the derive API.  [`Command` and `Arg` attributes can only show up in specific places](https://github.com/clap-rs/clap/tree/master/examples/derive_ref#overview), so you can't create an attribute that affects all future fields / `Arg`s by calling a `Command` function via an attribute.  The one exception is when `#[clap(flatten)]`ing
```rust
    #[derive(Debug, Clone, Parser)]
    struct CliOptions {
        #[clap(flatten)]
        options_a: OptionsA,
    }

    #[derive(Debug, Clone, Args)]
    #[clap(next_help_heading = "HEADING A")]
    struct OptionsA {
        #[clap(long)]
        should_be_in_section_a: u32,
    }
```

How do we support context-specific builder methods like this without forcing a `#[clap(flatten)]`?

**Possible solutions**:

**Key off of the inner-attribute name** and call switch which we can from that (if attr inner name is `next_help_heading`, then know to call that on the `Command` rather than the `Arg`).  This seems brittle, only working for whats been hard coded (we might forget things, we might make assumptions for what people want to do), and can run into conflicts in function names between `Arg`s and `Command`s.

**Have the user explicitly state what the attribute is for**.  I feel like this would also make what is happening clearer.

How does the user specify which kind of attribute it is?  Some ideas include:
- ~~`#[clap::command(next_help_heading = "NETWORKING")]`: outer-attribute namespace~~ Derive helper attributes can only be identifiers, not item paths
- `#[clap(command::next_help_heading = "NETWORKING")]`: inner-attribute namespaces
- `#[clap_command(next_help_heading = "NETWORKING")]`: psuedo-outer-attribute namespacing
- `#[clap(command(next_help_heading = "NETWORKING"))]`: fully own the nested parens

**Per-use outer-attributes**

- `#[command(next_help_heading = "NETWORKING")]`: we'd have `#[command(...)]`, `#[arg(...)]`, etc.

I'm leaning towards inner-attribute namespaces

---

_Comment by @Muscraft on 2022-05-11 19:02_

`psuedo-outer-attribute namespacing` should not be considered unless it comes from a different crate. It could cause problems when someone is new to the codebase and wants to look up how `clap_command` works and there is no crate for it.  `#[clap(command....)]` is more descriptive of where it comes from and what is going on. 

As for `inner-attribute namespaces` vs `fully own the nested parens` I prefer the latter. This is mostly because I have seen other crates use it (e.g. [`#[serde(rename_all = "...")]`](https://serde.rs/container-attrs.html#rename_all)). While there is no "official" style, keeping consistent with other crates is a benefit

---

_Comment by @epage on 2022-05-11 19:13_

> As for inner-attribute namespaces vs fully own the nested parens I prefer the latter. This is mostly because I have seen other crates use it (e.g. `#[serde(rename_all = "...")]`(https://serde.rs/container-attrs.html#rename_all)). While there is no "official" style, keeping consistent with other crates is a benefit

To clarify, we already do what serde does.  Note the example is `#[clap(next_help_heading = "SECTION")]`.  That is the outer/inner attribute style of serde.  I'm not aware of other crates using namespacing for attributes,. of any kind.  If anything, they use additional attributes but I can't remember what examples there are.  Forgot to include that in the options

---

_Referenced in [clap-rs/clap#3799](../../clap-rs/clap/pulls/3799.md) on 2022-06-08 16:01_

---

_Added to milestone `4.0` by @epage on 2022-06-08 16:01_

---

_Referenced in [clap-rs/clap#4129](../../clap-rs/clap/pulls/4129.md) on 2022-08-26 20:55_

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-08-27 02:18_

---

_Referenced in [clap-rs/clap#4179](../../clap-rs/clap/pulls/4179.md) on 2022-09-02 19:07_

---

_Referenced in [clap-rs/clap#4180](../../clap-rs/clap/pulls/4180.md) on 2022-09-02 20:48_

---

_Comment by @epage on 2022-09-02 20:56_

https://github.com/clap-rs/clap/pull/4180 renames the attributes:
```rust
/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    /// Name of the person to greet
    #[arg(short, long)]
    name: String,

    /// Number of times to greet
    #[arg(short, long, default_value_t = 1)]
    count: u8,
}
```

My original plan was to support:
```rust
/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    /// Name of the person to greet
    #[command(next_help_heading = "Foo")]
    #[arg(short, long)]
    name: String,

    /// Number of times to greet
    #[command(next_help_heading = "Bar")]
    #[arg(short, long, default_value_t = 1)]
    count: u8,
}
```
but alternatively we could do something like
```rust
/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    #[command(next_help_heading = "Foo")]
    _foo: (),

    /// Name of the person to greet
    #[arg(short, long)]
    name: String,

    /// Number of times to greet
    #[command(next_help_heading = "Bar")]
    _bar: (),

    #[arg(short, long, default_value_t = 1)]
    count: u8,
}
```
- Pro: More visually distinct
- Pro: Less confusion over mixing of attributes on a field
- Pro: Less hacky implemenation
- Con: Requires naming the no-op field (should be neutral at compile time)

Thoughts?

---

_Referenced in [clap-rs/clap#4181](../../clap-rs/clap/pulls/4181.md) on 2022-09-03 00:59_

---

_Comment by @epage on 2022-09-05 17:52_

In planning this, I forgot about https://github.com/clap-rs/clap/issues/1553 which has `command` attributes both for variants and the container.  We can''t use the attribute alone to distinguish.   Maybe good enough because doing things with subcommands is so rare?

---

_Referenced in [clap-rs/clap#4184](../../clap-rs/clap/pulls/4184.md) on 2022-09-06 17:55_

---

_Comment by @epage on 2022-09-06 17:57_

Another problem I ran into with this is that `#[command(subcommand)]` would be great to allow both container attributes and (for enums) child variant attributes.

---

_Comment by @epage on 2022-09-06 18:39_

To recap

Ideally, we would like to support use of at least `next_display_order` and `next_help_heading` in the middle of structs and enums (see also #1553) as there isn't a way for derive users today to apply a help heading to a group of args (or subcommands after #1553) without either doing it one by one or by flattening which can be disruptive to the code.

I had made the `#[clap(...)]` attributes more specific (`arg`, `command`, etc) and was looking to
- allow `#[command(...)]` attributes on arg fields so they apply to the container
- open up so `subcommand`, `flatten`, and `external_subcommand` could accept container attributes.

Problems
- `subcommand` on a variant already accepts attributes for the variant itself because it will be a subcommand
- There no way to force `#[command(...]` attribute on a variant to apply to the container

One alternative I had considered is just special casing `next_help_heading` and `next_display_order` but
- That muddles the waters for explaining to users the different attributes
- It still runs unto problems with `#[command(subcommand)]` enum variants

Out-there ideas
- Punt and allow this to work for deriving `Args` but not `Subcommand since most of this customization will be on `Subcommand`
  - Should `#[command(subcommand)]` still not allow other attributes within an `Args` so that the attributes have a single meaning?  If so, the workaround for anything applied there is to put it on the enum.
  - Lack of consistency can make learning more difficult
- Add a new `#[parent(...)]` or `#[super(...)]` attribute
  - Not as clear that it is tied to clap, could run into conflicts
- Add `#[command(super::next_help_heading = ...)]` support
- Add a `#[command(super, next_help_heading = ...)]` kind
  - This would require adding no-op fields and variants.  The latter would be unergonomic for a user to deal with due to exhaustiveness of match

@pksunkara and anyone else, I'd appreciate input on this.

---

_Comment by @epage on 2022-09-07 15:30_

I don't think any of the proposed options require breaking changes beyond what is already merged, so going to push this out of 4.0.0.

---

_Removed from milestone `4.0` by @epage on 2022-09-07 15:30_

---

_Label `A-derive` added by @epage on 2022-09-16 20:29_

---

_Referenced in [quickwit-oss/quickwit#2236](../../quickwit-oss/quickwit/issues/2236.md) on 2022-11-03 15:02_

---

_Referenced in [clap-rs/clap#975](../../clap-rs/clap/issues/975.md) on 2023-02-20 13:49_

---

_Referenced in [fluvio-community/fluvio#3234](../../fluvio-community/fluvio/pulls/3234.md) on 2023-05-08 16:43_

---

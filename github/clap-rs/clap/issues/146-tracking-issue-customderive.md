---
number: 146
title: Tracking issue CustomDerive
type: issue
state: closed
author: kbknapp
labels:
  - E-hard
assignees: []
created_at: 2015-07-06T01:22:46Z
updated_at: 2018-08-02T03:29:40Z
url: https://github.com/clap-rs/clap/issues/146
synced_at: 2026-01-07T13:12:19-06:00
---

# Tracking issue CustomDerive

---

_Issue opened by @kbknapp on 2015-07-06 01:22_

## To Do

### Derive FromArgMatches

```rust
#[derive(FromArgMatches)]
struct MyApp {
    verb: bool
}

fn main() {
    let m: MyApp = App::new("test")
        .arg_from_usage("-v, --verbose 'turns on verbose printing'")
        .get_matches()
        .into();

    println!("Verbose on: {:?}", m.verb);
}
```

- [ ] `#[derive(FromArgMatches)]`
  - [ ] Docs
  - [ ] Tests
  - [ ] Ensure requirements can't be violated (i.e. use `Default` where possible and document such)

### Derive App

```rust
/// Does stuff
#[derive(App)]
struct MyApp {
    /// Turns on verbose printing
    #[clap(short = 'v', long = "verbose")]
    verb: bool
}
```

- [ ] `#[derive(App)]`
  - [ ] Docs
  - [ ] Tests


### Derive Subcommands

```rust
#[derive(SubCommands)]
pub enum Commands {
    Test(Test),
}
```

- [ ] `#[derive(SubCommands)]`
  - [ ] Docs
  - [ ] Tests

### Derive SubcommandFromArgMatches

```rust
#[derive(SubCommandFromArgMatches)]
pub enum Commands {
    Test(Test),
}
```

- [ ] `#[derive(DefineSubCommands)]`
  - [ ] Docs
  - [ ] Tests

### Derive TryFromArgMatches

```rust
#[derive(TryFromArgMatches)]
struct MyApp {
    verb: bool
}

fn main() {
    let m: Result<MyApp, clap::Error> = App::new("test")
        .arg_from_usage("-v, --verbose 'turns on verbose printing'")
        .get_matches()
        .try_into();
}
```
- [ ] `#[derive(FromArgMatches)]`
  - [ ] Docs
  - [ ] Tests



---

_Added to milestone `v2.0` by @kbknapp on 2015-07-06 01:23_

---

_Label `feature request` added by @kbknapp on 2015-07-06 01:23_

---

_Label `D: hard` added by @kbknapp on 2015-07-15 04:18_

---

_Label `P3: want to have` added by @kbknapp on 2015-07-15 04:18_

---

_Label `C: matches` added by @kbknapp on 2015-07-15 04:18_

---

_Referenced in [clap-rs/clap#251](../../clap-rs/clap/issues/251.md) on 2015-09-12 13:56_

---

_Referenced in [clap-rs/clap#249](../../clap-rs/clap/pulls/249.md) on 2015-09-18 10:53_

---

_Comment by @WildCryptoFox on 2015-09-18 10:53_

RustcSerialize/Deserialize? Serde2?


---

_Comment by @kbknapp on 2015-09-18 20:10_

I'm thinking more about creating a `docopt` like struct instead of using string based (read "error prone") `ArgMatches` approach.

i.e. imagine all valid options are set the same as they are now, but instead of using `matches.value_of("some_arg")` you'd access the value via a `matches.some_arg`

As a _secondary_ feature, it would be nice to actually serialize basic types using any of the above listed methods.


---

_Comment by @sru on 2015-09-19 02:12_

I think @james-darkfox is asking what kind of library we would use if we are going to do this.

I don't think it is possible without library and without syntax extension because of the custom fields.


---

_Comment by @WildCryptoFox on 2015-09-19 03:56_

No syntax extension required. We have macros for this already. I could modify my builder macro to build a `struct`, and `impl` with `fn new` doing the current building that we have already. The `struct` would either need one of these libraries or, alternatively the macro could also build a function to `Matches { foo: matches.value_of("foo").unwrap_or("default") }` which does not need any external library either.

**Note**: This also makes the default value accessible to the builder macro. :wink: 

---

For decoding into types. This depends on what types we want to support? I don't think we'd need RustcSerialize/deser, or serde2 as they are for more generic purposes. Currently we only have strings; I'd expect integers (u8 i8 u16 i16 u32 i32 u64 i64 f32 f64...), booleans (1/0, true/false, yes/no), paths, ???, **Note**: Inherit validation ensures that "foo" is not a number and as such not a valid value for an argument accepting a number.


---

_Comment by @sru on 2015-09-19 05:32_

@james-darkfox Actually, that works quite nice, besides forcing user to use macro if they want the custom struct (which I think is somewhat trivial because the builder macro is simple enough).


---

_Comment by @WildCryptoFox on 2015-09-19 05:38_

Doesn't force the user to use it. As they can always use the current behavior or manually construct their own struct and `From<App>` or from matches. Or whatever.


---

_Comment by @WildCryptoFox on 2015-09-19 05:40_

I'm actually thinking about how clap could be refactored down... It is _REALLY_ quite a large implementation for what everyone would like to call a simple task. Docopt was initially created to be simple, quick, and easy. I have a number of ideas that would refactor clap down **heavily** but this would be a _VERY_ breaking change.


---

_Comment by @sru on 2015-09-19 05:49_

@james-darkfox I see what you are getting at now.

On the other hand, you can always open up issues with RFC tag.


---

_Comment by @WildCryptoFox on 2015-09-19 05:57_

@sru Maybe when I have the basic concepts working. I'm currently throwing together some ideas for a minimal clap implementation. Tackling the problem much the same way as I have for my own builder & macro which constructs a dummy registry of crates.

Notably: `From<FooBuilder> for Foo` and simplifying the global flags into a pseudo subcommand 'clap_global'.

Once I have this quick hack ready. We will see which of the following is better: porting the ideas over to the current clap (preferable); or extending the minimal implementation (much more work).


---

_Referenced in [clap-rs/clap#259](../../clap-rs/clap/issues/259.md) on 2015-09-19 10:37_

---

_Label `W: 2.x` added by @kbknapp on 2015-09-22 13:48_

---

_Removed from milestone `v2.0` by @kbknapp on 2016-01-23 15:53_

---

_Comment by @kbknapp on 2016-10-28 02:29_

Thanks to @kamalmarhubi at RustBelt, but thanks to macros 1.1 this may be a possibility. I need to look into this further!


---

_Renamed from "Serialize matches into struct" to "Serialize matches into struct (macros 1.1?)" by @kbknapp on 2016-10-28 02:29_

---

_Comment by @WildCryptoFox on 2016-10-28 03:07_

@kbknapp Macros 1.1 are no better for this job than normal macros. Feel free to jump on IRC `irc.mozilla.org SSL 6697 #clap-rs`.


---

_Comment by @Nemo157 on 2016-11-12 20:12_

So I was playing round with something like this over the last week, basically a macros 1.1 builder that generates a `clap::App` from a set of struct and enum definitions along with a parser back from the `clap::ArgMatches` to the expected struct. A super simple example is:

``` rust
#[derive(StompCommand)]
pub struct MyApp {
    /// Sets a custom config file
    #[stomp(short = 'c', value_name = "FILE")]
    config: Option<String>,
}

fn main() {
    let app = MyApp::parse();
    if let Some(c) = app.config {
        println!("Value for config: {}", c);
    }
}
```

There's a full rewrite of the main clap example at https://github.com/Nemo157/stomp-rs/blob/master/examples/01b_quick_example.rs along with a rewrite of the app I'm currently using clap in at https://github.com/Nemo157/ipfsdc-rs/commit/41e5d5f3b9562f0f4c6eab90c0c9a49e282ec6c1

One of the nice things about this is how much can be inferred from just the types given; if something is a `bool` then it's a simple flag option; if it's an `Option<T>` then it's optional, otherwise it's required; if it's a `Vec<T>` then it takes multiple values; if it implements `FromStr` then it can have an auto-generated validator that will report why parsing the value failed:

```
cargo run --example 12_typed_values -- 9b
     Running `target/debug/examples/12_typed_values 9b`
error: failed to parse value "9b" for argument 'seq': invalid digit found in string
```


---

_Comment by @kamalmarhubi on 2016-11-12 21:11_

@Nemo157 yes! That's what I had in mind. Awesome that you put together a proof of concept.


---

_Comment by @kamalmarhubi on 2016-11-12 21:14_

> One of the nice things about this is how much can be inferred from just the types given; if something is a `bool` then it's a simple flag option; if it's an `Option<T>` then it's optional, otherwise it's required; if it's a `Vec<T>` then it takes multiple values; if it implements `FromStr` then it can have an auto-generated

These are all really cool ideas. :-)


---

_Referenced in [clap-rs/clap#817](../../clap-rs/clap/issues/817.md) on 2017-01-18 01:08_

---

_Referenced in [clap-rs/clap#818](../../clap-rs/clap/issues/818.md) on 2017-01-18 14:13_

---

_Comment by @kamalmarhubi on 2017-01-29 23:24_

@Nemo157 @kbknapp is the best place to make progress here or in stomp-rs?

---

_Comment by @kamalmarhubi on 2017-01-29 23:56_

(Since 1.15 and macros 1.1 is landing soon, I'd love if we can make the CLI story in Rust beautiful!)

---

_Comment by @kbknapp on 2017-01-30 00:04_

@Nemo157 I can't believe I missed these comments somehow! Apologies to both you and @kamalmarhubi!

This is a very cool proof of concept that's super exciting! I think this could either be continued in `stomp-rs` or if @Nemo157 would be willing, it could potentially be merged into `clap` proper. Merging seems like the best user experience, but I'd have to look through the entirity of the code to ensure it matches all edge cases and such. If merging is the route taken, I'd also like to go with standard naming conventions such as `StompCommand`->`ClapCommand`, etc. At that point I'd also like to get @Nemo157's assistance as a contributor to help maintain that portion since it's a decently large addition that I'm sure quite a few people are going to want/use.

There are, however, two parts that could have potential issues

 * Storing Options in a `Vec<T>` doesn't ensure the order of values, which some CLIs rely on. Also it'd potentially nice to store single options in a `T` just like positional args, and infer when multiples are used that it should be a `Vec<T>` (or which ever data structure ends up getting used).
 * Using `T` instead of `Option<T>` for requirements could be dangerous when things like requirements get overriden, conditional requirements, etc. We could bound that the `T` implement `Default`, which may work well to overcome this issue.
 * The subcommands portion would be even nicer if the enum could be specified inline

This is amazing work, and I'm really excited by it!

---

_Comment by @kamalmarhubi on 2017-01-30 01:01_

@kbknapp 
> I can't believe I missed these comments somehow!

That explains a lot! I'd be willing to help with merging this into clap and looking over the API for other changes like you suggested.

---

_Comment by @kamalmarhubi on 2017-01-30 01:01_

I'd love for this to land within the 1.15 cycle!

---

_Comment by @kbknapp on 2017-01-30 02:45_

@kamalmarhubi 

> I'd love for this to land within the 1.15 cycle!

Absolutely, me too! If we can resolve those outstanding questions above (values in order, ensuring `T` is bound with `Default` if not using `Option<T>`) and ensure the naming conventions of the APIs are inline with each other I don't see why this couldn't happen!

The subcommand enum not being inlined I'm fine with.

@Nemo157 and @kamalmarhubi 

I forgot to mention in the last post the example above merges two distinct ideas into one, but I'd also want the ability to do one *or* the other and not be forced to use both. I haven't dug into the source yet, so if it's already possible ignore this comment 😜 

What I mean is, I view both of these as distinct ideas (also, I'm using `clap` names in this example, but if it remains in the stomp crate, it'd be `stomp`):

 * Create an `App` struct from `MyApp` using the `#[clap(short='c', long="config")]`
 * Create `MyApp` struct from `ArgMatches` using the `#[derive(ClapCommand)]`

Being able to do these two things separatly would greatly increase adoptability. i.e. "I've already got an `App`, so now I just write out my `MyApp` struct and place a `#[derive]` attribute on there and I'm off to the races.

Likewise, if for some unknown reason they don't want to use the `ArgMatches`->`MyApp` conversion, but still want to take advantage of the `MyApp`->`App` they could. Although I see this as less beneficial merely a biproduct.

---

_Added to milestone `3.0` by @kbknapp on 2017-01-30 04:35_

---

_Comment by @Nemo157 on 2017-01-30 07:24_

😃 I would definitely be happy to make this a part of `clap` proper.

> look through the entirity of the code to ensure it matches all edge cases and such

At the moment, definitely not. I was basically using my application and the primary `clap` example as a feature list for what to implement, so there are many things missing.

> Using T instead of Option<T> for requirements could be dangerous when things like requirements get overriden, conditional requirements, etc.

Yeah, again because of the test cases I was using I did not consider that at all. Hopefully, at least for the case where the user both generates the `App` and the conversion from `ArgMatches`, it should be possible to detect issues at compile time.

> The subcommands portion would be even nicer if the enum could be specified inline

Depends which way you mean, an anonymous enum in the parent command would be cool, but probably not really doable without proper anonymous enum support in Rust. An enum with struct variants definitely should be possible, it would result in some massive generated code blocks, but that shouldn't be too bad, and could be fixed in the future with [types for enum variants](https://github.com/rust-lang/rfcs/pull/1450) allowing delegating variant specific parsing.

> I forgot to mention in the last post the example above merges two distinct ideas into one, but I'd also want the ability to do one or the other and not be forced to use both.

The trait doesn't allow it, but the macro is basically following two distinct paths for each so would be simple to split the trait for it.

Also, one thought I had soon after I implemented this was that the macro is doing too much. It should be possible to split a lot of what the macro is doing based on types (which is actually based on type names, so could very easily be broken with type aliases etc.) out to trait implementations. For example something like `trait FillArg { fn fill_arg(arg: Arg) -> Arg }` (superbad name 😞) for filling out the details derived from the type and something else used during parsing.

I think I should have some time this week I could spend on this, I can definitely try and do a quick port into the `clap` codebase and do a bit of documentation on how it currently works and what's missing.

---

_Comment by @Nemo157 on 2017-01-30 17:44_

Just pushed [a branch with a quick port of the changes into the `clap` source tree](https://github.com/kbknapp/clap-rs/compare/master...Nemo157:stomp). Renamed most stuff, left the changes to `clap` itself in a `stomp` module pending bikeshedding the trait names and where in the hierarchy they might fit. While moving this I also split the traits into two, one for creating the `App` and one for parsing the resulting `ArgMatches` back, so users could manually implement one if they wanted (although, they would need to know exactly what the derive would expect to avoid runtime failures).

Currently it fails to build the example, and I'm not sure exactly why. There was some changes to `proc_macro_derive` since my initial implementation so the macro can't actually modify the item it's deriving for. I used to strip out all the `#[stomp(...)]` attributes to get around unknown attribute warnings, but now the compiler is meant to do that for you for a list of known attributes you give it. I've done that but I'm still getting an error from the compiler for having the attribute there:

```
→ cargo +nightly run --example 01b_quick_example_stomp
   Compiling clap v2.20.1 (file:///Users/Nemo157/sources/clap-rs)
error: macro undefined: 'clap!'
  --> examples/01b_quick_example_stomp.rs:36:1
   |
36 | #[clap(name = "MyApp", version = "1.0")]
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

---

_Comment by @Nemo157 on 2017-01-30 17:48_

> an anonymous enum in the parent command would be cool, but probably not really doable without proper anonymous enum support in Rust

That's definitely off the table now that procedural macros can't modify the item at all.

---

_Comment by @kbknapp on 2017-01-30 17:48_

I think the `#[x(y="z")]` was removed from macros 1.1

---

_Comment by @Nemo157 on 2017-01-30 17:50_

It's currently allowed in master (and beta): https://github.com/rust-lang/rust/blob/master/src/test/compile-fail-fulldeps/proc-macro/proc-macro-attributes.rs

---

_Comment by @Nemo157 on 2017-01-30 18:05_

Looks like it might be related to some of the features I was still using, removed them all and `cargo +beta run --example 01b_quick_example_stomp -- --help` works now 😄 

---

_Comment by @kbknapp on 2017-01-30 18:26_

The part that people have been really asking for is the `ArgMatches`->`MyApp` piece. So I would even ben fine if we had to pull the `MyApp`->`App` part out, or simply do a true procedural macro/compiler plugin and mark it with `unstable` (clap features).

Either way this will eventually merge as `clap` 3.x and the custom derive part will remain using the clap `unstable` feature until Rust 1.17 is released for compatibility reason (so Current - 2 releases can be supported on stable Rust).

---

_Comment by @kbknapp on 2017-01-30 18:53_

> Just pushed a branch with a quick port of the changes into the clap source tree. 

Excellent!!! I'm going to be looking through this over the next few days, feel free to continue working on it, and if I get some time I'll probably push some commits as well. Full disclosure, I'll be traveling pretty heavily for the next two weeks so my replies may not be instant :wink: 

My initial bikeshed thoughts are to move the traits into a `code_gen` module. I think we could also use easier names, such as `DefineApp`->`App`, `DefineSubCommands`->`SubCommands`. `FromArgMatches` is great because it follows the stdlib example.

For UX, I think `#[derive(SubCommands)]` can imply `#[derive(SubCommands, SubCommandsFromArgMatches)]`

So the example would go to:

```rust
/// Does awesome things.
#[derive(App, FromArgMatches)]
#[clap(name = "MyApp", version = "1.0")]
#[clap(author = "Nemo157 <clap@nemo157.com>")]
pub struct MyApp {
    /// Sets a custom config file
    #[clap(short = "c", value_name = "FILE")]
    config: Option<String>,

    /// Sets an optional output file
    #[clap(index = "1")]
    output: Option<String>,

    /// Turn debugging information on
    #[clap(counted, short = "d", long = "debug")]
    debug_level: u64,

    #[clap(subcommand)]
    subcommand: Option<Commands>,
}

#[derive(SubCommands)]
pub enum Commands {
    Test(Test),
}

/// does testing things
#[derive(App, FromArgMatches)]
pub struct Test {
    /// lists test values
    #[clap(short = "l")]
    list: bool,
}
```

I'm going to go ahead and submit a PR so we can use it for tracking purposes and as a standard place for easily seeing the diffs.

---

_Referenced in [clap-rs/clap#835](../../clap-rs/clap/pulls/835.md) on 2017-01-30 19:05_

---

_Comment by @kbknapp on 2017-01-30 19:23_

#835 is open. Feel free to discuss exact implementation details in that PR, but also know that I'll be keeping that PR quite clean and deleting old comments and such as well as updating the OP to keep track of current discussion.

If talk around abstract implementation details need to continue to happen, I'd prefer to use this issue for historical data reasons.

---

_Renamed from "Serialize matches into struct (macros 1.1?)" to "Tracking issue for Macros 1.1 custom derive" by @kbknapp on 2017-02-16 02:01_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:55_

---

_Label `W: 2.x` removed by @kbknapp on 2017-05-09 18:55_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-08-22 04:18_

---

_Renamed from "Tracking issue for Macros 1.1 custom derive" to "Tracking issue CustomDerive" by @kbknapp on 2017-11-12 19:16_

---

_Comment by @kbknapp on 2017-11-12 19:53_

This issue was moved to kbknapp/clap-derives#4

---

_Closed by @kbknapp on 2017-11-12 19:53_

---

_Referenced in [clap-rs/clap#1661](../../clap-rs/clap/issues/1661.md) on 2020-02-03 09:24_

---

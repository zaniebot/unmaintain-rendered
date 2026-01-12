```yaml
number: 3120
title: "Structopt derive doesn't like type-parametric structs"
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:14:41Z
updated_at: 2021-12-09T16:41:00Z
url: https://github.com/clap-rs/clap/issues/3120
synced_at: 2026-01-12T16:14:14Z
```

# Structopt derive doesn't like type-parametric structs

---

_@epage_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [vorner](https://github.com/vorner)**
_Thursday Jul 26, 2018 at 20:30 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/128_

----

Probably better showed with some code:

```rust
struct Wrapper<O> {
    #[structopt(flatten)]
    sub: O,
}
```

I get this error message:

```
error[E0412]: cannot find type `O` in this scope
  --> src/lib.rs:31:10
   |
31 | #[derive(StructOpt)]
   |          ^^^^^^^^^ did you mean `OW`?

error[E0243]: wrong number of type arguments: expected 1, found 0
  --> src/lib.rs:31:10
   |
31 | #[derive(StructOpt)]
   |          ^^^^^^^^^ expected 1 type argument
```

Similar thing works with serde, where it automatically places correct type bounds on the generated impl block.


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday Jul 27, 2018 at 10:10 GMT_

----

StructOpt was not design with generics in mind. That's a complicated thing.

What is your use case?


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vorner](https://github.com/vorner)**
_Friday Jul 27, 2018 at 10:55 GMT_

----

Yes, I already noticed it's a bit harder than just making sure the impl gets the type bounds, because some things are not part of the trait. But I've found a workaround for my own use case.

I'm building a helper start-me-a-daemon library. Some of the options will be specific for each daemon, while some others will be common for all of them (like logging and option not to go to background). So I want to compose the common ones (defined in the library) with the daemon-specific ones provided by the user of the library as a generic.

For now, I have this (I haven't tried it yet, but I guess it should work).


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday Jul 27, 2018 at 12:29 GMT_

----

Others are doing it in the other way, see https://crates.io/crates/clap-verbosity-flag and https://crates.io/crates/clap-port-flag


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vorner](https://github.com/vorner)**
_Friday Jul 27, 2018 at 12:48 GMT_

----

Yes, but that's a different use case ‒ they want to expose the verbosity flag to the end application. I don't want to give the application the logging configuration etc ‒ I just want to logging to be configured automagically. That means my library needs access to the common configuration (if I went the verbosity way, I'd have to let the user tell me where the logging configuration is) and it would pollute the user's configuration structure.


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vorner](https://github.com/vorner)**
_Friday Jul 27, 2018 at 12:49 GMT_

----

(Oh, I haven't included the link in the post up there ↑ ‒ this is what I do now: https://github.com/vorner/spirit/blob/master/src/lib.rs#L59)


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday Jul 27, 2018 at 12:59 GMT_

----

Great workaround!


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Tuesday Dec 03, 2019 at 01:21 GMT_

----

Let's get back to the original issue. Do we want to support generic structs and what the pitfalls are?
Or will we just close it as "wontfix"?


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Tuesday Dec 03, 2019 at 08:46 GMT_

----

wontfix is fine for me.


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Dec 25, 2019 at 20:00 GMT_

----

@TeXitoi actually, there is [a use case](https://www.reddit.com/r/rust/comments/efi4kb/who_else_depends_on_structopts_internal_api/fc0w1cu?utm_source=share&utm_medium=web2x) where this would be pretty useful.

What if we just copy-paste the bounds?
```rust

#[derive(StructOpt)] 
struct Opt<T, U: Read> {}

// expands to 
impl<T, U: Read> StructOpt for Opt<T, U> {
    // ...
}
```

I understand that it's not enough in some corner cases (like type items, implicit `Sized` stuff and all) and yet we will have 99% covered, including the case above.

As an alternative, we could mimic [serde](https://serde.rs/attr-bound.html):
```rust
// `T: StructOpt` will be added automatically 
// since there's no `#[structopt(bound(...))]` annotation  
// But no bounds for `U` will be generated since it's `skip`
#[derive(StructOpt)]
struct StructOpt<T, U> {
    arg: T,

    #[structopt(skip)]
    skipped: U
}

// `T: StructOpt` will NOT be added  
// since there's an explicit `#[structopt(bound(...))]` annotation  
#[derive(StructOpt)]
#[structopt(bound("S: FromStr + StructOpt"))]
struct StructOpt<S, T> {
    #[structopt(parse(from_str))]
    arg: S,

    #[structopt(skip)]
    skipped: T,
}
```
Both alternatives are pretty easy to implement


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Monday Dec 30, 2019 at 19:14 GMT_

----

I'm reopening this since it looks like there are people interested in this feature out there (#317).

A PR implementing this serde-like design will be accepted:
```rust
// Every generic that occurs in a field annotated with 
// either `#[structopt(flatten)]` or `#[structopt(subcommand)]` 
// will be given `G: StructOpt` bound by default 
// (i.e when no `#[structopt(bound(...))]` is present).
//
// `T: StructOpt` will be added automatically
// since there's no `#[structopt(bound(...))]` annotation and 
// it occurs in `#[structopt(flatten)]`. Same story for `U` (because of `subcommand`).
//
// No `StructOpt` bounds for B should be generated 
// since none of the fields it occurs in
// are marked with `subcommand` nor `flatten`.
//
// Additionally, each field marked with `#[structopt(parse(...))]` 
// (with no explicit function) has to be bounded with one of
//
// * G: std::convert::From<&'_ str> (for `from_str`)
// * G: std::str::FromStr<&'_ str> (for `try_from_str`, including implicit)
// * G: std::convert::From<&'_ OsStr> (for `from_os_str`)
// * G: std::convert::From::from<bool> (for `from_flag`)
//
// No "parse" bounds will be generated for `from_occurences` (subject for future design)
// and `try_from_os_str`, as well as no "parse" bounds will be generated for explicit 
// `#[structopt(parse(type = explicit::function))]`
// 
#[derive(StructOpt)]
struct ImplicitBounds<T, U, B> {
    #[structopt(flatten)]
    arg: T,

    #[structopt(subcommand)]
    subcmd: U,

    #[structopt(skip)]
    skipped: B,

    other: B,

    // implicit `parse(try_from_str)`
    //
    // T: std::str::FromStr<&'_ str>
    arg2: T, 

    // U: std::convert::From<&'_ str>
    #[structopt(parse(from_str))]
    arg3: U,

    // B: std::convert::From<&'_ OsStr>
    #[structopt(parse(from_os_str))]
    arg4: B,

    // A: std::convert::From::from<bool>
    #[structopt(parse(from_flag))]
    arg5: A,
}

// Generated:
impl<T, U, B, A> StructOpt for ImplicitBounds<T, U, B, A> 
where
    T: StructOpt + FromStr<&'_ str>,
    U: StructOpt + From<&'_ str>,
    B: From<&'_ OsStr>,
    A: From::from<bool>
{
    // ...
} 


// User is able to override the automatic bounds via 
// `#[structopt(bound(...))]` attribute
//
#[derive(StructOpt)]
struct ExplicitBounds<S, T, U> {
    #[structopt(bound(S: FromStr + AnotherTrait)), parse(from_str))]
    arg: S,

    #[structopt(flatten)]
    #[structopt(bound(T: StructOpt + AnotherTrait))]
    flatten: T,

    // The bounds will be generated automatically 
    // since there's no `bound` attribute present 
    error: U 
}

// Generated:
impl<S, T, U> StructOpt for ExplicitBounds<S, T, U> 
where
    T: StructOpt + AnotherTrait,
    S: FromStr + AnotherTrait,
    U: FormStr
{
    // ...
}
```

cc @TeXitoi do you agree with this design?


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Tuesday Jan 07, 2020 at 20:38 GMT_

----

I don't think bound should be on fields, more on the struct.

Automatic bound seems complicated to anticipate for the user.

It might complexify the code a lot.

I'm afraid it will be a lot of work for little usage. 

I'm not totally against this feature, but it must be simple enough to understand, implement, document and maintain.


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Jan 08, 2020 at 05:38 GMT_

----

I must confess, I just stole the design from [`serde`](https://serde.rs/attr-bound.html).

> I don't think bound should be on fields, more on the struct.

I think that "fields vs struct bound attr" is quite irrelevant here, either will do. 

> Automatic bound seems complicated to anticipate for the user.

Not really. I tried to design the automatic bounding so it will work in 99% cases, as any good heuristic would. Thus, in 99% cases users shouldn't even be aware of it since everything "just works". 

Explicit `bound` attributes are for cases like `Foo<T>`, where it's unclear what to do: `Foo<T>: Trait` vs `T: Trait` (i.e if the generic is just a part of the type, not the type itself). In such cases, no bounds will be generated anyway ¯\\_(ツ)_/¯

I honestly can't see a case where this auto bounding may misfire, can you?

> It might complexify the code a lot. I'm afraid it will be a lot of work for little usage.

Not really. In essence, all we need is:
* Traverse the graph (already implemented)
* Collect the info (partially implemented)
* Pass it to the top level (this is where dragons be, but should be fairly easy after the refactoring).
* Generate and emit the `where` clause (easy). 




---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/nathan-at-least"><img src="https://avatars.githubusercontent.com/u/4369700?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [nathan-at-least](https://github.com/nathan-at-least)**
_Wednesday Sep 01, 2021 at 19:53 GMT_

----

I just ran into this with a similar use case to @vorner: I want an "app framework" library that provides consistent logging options across all apps, and then also initializes logging for apps, so I naively just reached for this:

```
trait Command: StructOpt {
    fn execute(&self) -> std::io::Result<()>;
}

#[derive(StructOpt)]
pub struct Options<T: Command> {
    #[structopt(flatten)]
    log_opts: loglib::LogOptions,

    #[structopt(subcommand)]
    cmd: T,
}

fn do_main<T: Command>() -> std::io::Result<()> {
    let opts = Options::<T>::from_flags();
    opts.log_opts.init_logging()?;
    opts.cmd.execute()
}
```

When I follow the link in https://github.com/TeXitoi/structopt/issues/128#issuecomment-408409015 I don't see any code relevant to `structopt`, so I don't know what the workaround was.


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Wednesday Sep 01, 2021 at 20:44 GMT_

----

I'll try to look at it tomorrow, but, as a first read, don't you want to flatten your T instead is subcommand?


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/vorner"><img src="https://avatars.githubusercontent.com/u/11783500?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vorner](https://github.com/vorner)**
_Thursday Sep 02, 2021 at 06:29 GMT_

----

Ah, sorry, the code moved to another place since then :-(.

It is now here and the workaround is updated since then a bit. This link probably should keep its content:
https://github.com/vorner/spirit/blob/ce2ec171320b33ff34e645250c8951a5cce3a926/src/cfg_loader.rs#L76


---

_Comment by @epage on 2021-12-09 16:22_

I believe this is now resolved, check out the tests at https://github.com/clap-rs/clap/blob/master/tests/derive/generic.rs.

if I missed anything in this thread, feel free to let me know

---

_Closed by @epage on 2021-12-09 16:22_

---

_Label `A-derive` added by @epage on 2021-12-09 16:41_

---

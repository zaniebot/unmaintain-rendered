---
number: 818
title: Allow specifying types for arguments
type: issue
state: closed
author: quodlibetor
labels: []
assignees: []
created_at: 2017-01-18T14:13:50Z
updated_at: 2018-08-02T03:29:59Z
url: https://github.com/clap-rs/clap/issues/818
synced_at: 2026-01-10T01:26:36Z
---

# Allow specifying types for arguments

---

_Issue opened by @quodlibetor on 2017-01-18 14:13_

Feature discussion/request: add something like Python's [click library's "types"](http://click.pocoo.org/5/parameters/#parameter-types).

There are two big reasons to do this:

* It could be used to unify/specify the conversion and validation logic that would be necessary for auto-derived apps or apps with differently-typed matches (mentioned in #817 and #146, #459, maybe some others)
* It would allow types to define how they generate auto-completion behavior for the various shells that clap supports[1].

It seems like a:

```rust
trait ParamType {
    type Into;

    fn convert(&self) -> Result<Self::Into, [some::click::error]>;

    fn shell_complete(option_name: Optiona<&str>,
                      parameter_name: Option<&str>) -> Option<String> {
        parameter_name
    }

    fn zsh_complete() -> String {
        Self::shell_complete()
    }

    // etc
}
```

Might be a good-enough start.


1: Maybe this should be a separate bug, but: it's *mildly* annoying that ripgrep's auto-completion behavior (in zsh) is as follows:

```console
$ rg <TAB>
$ rg PAT
$ rg PAT<TAB>
PATH     -- A file or directory to search.
PATTERN  -- A regular expression used for searching.
```

And in particular that once I've got part of the path that I want to search it refuses to complete it, so I have this behavior:

```console
$ ls
$ tmp
$ rg t<TAB>     # error
```


---

_Comment by @kbknapp on 2017-01-19 01:29_

I agree that custom shell completion is needed, which is the discussion for #568 but I think these three issues (type conversion for args, stringly typed arg keys, and custom shell completions) are separate issues that I wouldn't want to mix. 

I'll expand a little bit.

---

### Type Conversions

This can already be done via macros ([`value_t!`](https://docs.rs/clap/2.20.0/clap/macro.value_t.html) and [`value_t_or_exit!`](https://docs.rs/clap/2.20.0/clap/macro.value_t_or_exit.html), plus plurals) or manually (via the [`std::str::FromStr`](https://doc.rust-lang.org/std/str/trait.FromStr.html) trait). The problem is manually defining all these conversions is painful and time consuming. As you correctly pointed out, #146 aims to do this with a single `#[derive()]` directive to remove those pain points).

IMO, but I'm up for discussion, `clap` doesn't need to provide any traits for this because it can already be done with `FromStr`|`From`|`Into`. `clap` would need to *return* the type `T`, but that isn't possible today:

```rust
fn value_of(&self, arg: &str) -> T {
   // ...
}
```

Unless I'm misunderstanding what you're asking for. What I'd like to do is provide a way someone to either specify upfront what the type is (thing something like a functional `Arg::type::<T>()` ), or simply infer the type from the name or something `int_foo` and use one of these two to do the converting for you upon converting `ArgMatches` into something else, such as `Args` as discussed in #817 using `From<ArgMatches>`.

The point being all the work is done for you, minus maybe specifying the type, which isn't any worse than what you're already doing by specifying the parameters for the arg.

This should look something like

```rust
#[derive(FromMatches)]
struct Args {
    num: u64
}

// ---

let args: Args = matches.into();
if args.num > 28 { /* do stuff */ }

// Instead of
if matches.value_of("num").unwrap() > 27 { /* do stuff */ }
```

### Stringly Typed Arg Keys

This can be done today by using enums and impl'ing `AsRef<str>` for said enum. The problem again is it's painful and tedious to do by hand. I had a branch which did this via a macro, but with Macros 1.1 quickly arriving I'm waiting in order to use another `#[derive()]` directive and make this even easier.

It will look like:

```rust
#[derive(ArgKey)]
enum Foo {
    Bar,
    Baz,
    Qux
}

// ---

let value = matches.value_of(Foo::Baz);
```

Which has a huge benefit of compile time checking of keys and one of biggest pain points of `clap`. This is all discussed in #459 like you said.


### Custom Shell Completion

As discussed in #568 this is something I really want. But unlike the two above, there really isn't a way to do this today. When I originally wrote the shell completions portions of `clap` I was expecting they'd be used as a starting point for people to further customize, but it turns out shell completions are hard and `clap` gets close enough that it's not really worth the extra effort since you'd have to stay in sync with changes to your CLI and the nice thing about `clap` generating it for you, is you're never out of sync. A catch 22.

The problem with implementing this is I just haven't had a good time to sit down and think about *how* (because of work, holidays, family, etc.). I want a way to specify this that abstracts well enough to work for all shells. The easiest way is to say, "Put your arbitrary completion shell script here inside this `Arg::complete_with(&str)`" but that feels super hacky to me, and potentially unsafe. What I'd like to do is provide a `Arg::complete_with(Fn(&str, &str)->String)` (and a `Arg::complete_with_os(Fn(&str,&OsStr)->OsString)` where an arbitrary Rust function is called...but herein lies the problem; shell completions are run *before* the program executes. This has led to some people using hidden args or something like, `$ prog --complete me<tab>` calling a shell completer that actually runs `$ prog --_complete_arg "complete" --_complete_prefix "me"` which generates the possible completions and returns them to the shell. I'm not against doing that, but again feels strange because you're injecting hidden args into a CLI. Although typing this out right now does make me lean towards this solution.

---

Sorry for the long discussion, but please let me know if I'm off on any bases. 

---

_Label `T: RFC / question` added by @kbknapp on 2017-01-19 01:29_

---

_Referenced in [clap-rs/clap#568](../../clap-rs/clap/issues/568.md) on 2017-01-19 01:30_

---

_Added to milestone `3.0` by @kbknapp on 2017-01-30 04:36_

---

_Comment by @kbknapp on 2017-01-30 05:06_

I'm going to close this issue since it's covered by individual other issues. Please let me know if it needs to be re-opened for something outside the issues listed above.

---

_Closed by @kbknapp on 2017-01-30 05:06_

---

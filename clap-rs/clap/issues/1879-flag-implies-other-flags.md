---
number: 1879
title: Flag implies other flags
type: issue
state: closed
author: camsteffen
labels: []
assignees: []
created_at: 2020-04-29T15:20:52Z
updated_at: 2020-04-30T16:40:03Z
url: https://github.com/clap-rs/clap/issues/1879
synced_at: 2026-01-10T01:27:08Z
---

# Flag implies other flags

---

_Issue opened by @camsteffen on 2020-04-29 15:20_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I have an application that generates a bunch of files depending on several flags. I want to have a "save-all" flag that **implies** all of the more granular "save-x" flags. I'm surprised that I can't find any similar request through searching.

Here is an example: (simplified for this post)

```rust
App::new("MyApp")
    .arg(Arg::with_name("save_all")
        .long("save-all")
// *** this is what I want to do *** //
        .implies_all(&["save_image", "save_stats", "save_text"]))
    .arg(Arg::with_name("save_image")
        .long("save-image"))
    .arg(Arg::with_name("save_stats")
        .long("save-stats"))
    .arg(Arg::with_name("save_text")
        .long("save-text"))
```

### Describe the solution you'd like

An `implies_all` option for `Arg` is what I'm interested in. But I would also add `implies` (singular) and `implied_by`. I believe this is similar to `default_value_if` except that it is for flags.

### Alternatives, if applicable

I don't know of another convenient way to do this. Am I missing something?

Perhaps it should be named similarly to `default_value_*`? The `default_value_*` functions imply `takes_value`. Changing this is probably not an option. So it seems to me that the `default_value_*` space is too different. The word "implies" is intuitive to me.

---

_Label `T: new feature` added by @camsteffen on 2020-04-29 15:20_

---

_Comment by @pksunkara on 2020-04-29 15:33_

On master branch, you should be able to do this with the following:

```rust
App::new("MyApp")
    .replace("--save-all", &["--save-image", "--save-stats", "--save-text"])
    .arg(Arg::with_name("save_all")
        .long("save-all"))
    .arg(Arg::with_name("save_image")
        .long("save-image"))
    .arg(Arg::with_name("save_stats")
        .long("save-stats"))
    .arg(Arg::with_name("save_text")
        .long("save-text"))
```

---

_Comment by @kbknapp on 2020-04-29 15:42_

@camsteffen once you can confirm the proposed solution works for you and meats your intent we can close the issue :smile: 

---

_Comment by @camsteffen on 2020-04-29 19:12_

Interesting, that might work. Just some minor concerns.

The example given in the [docs](https://github.com/clap-rs/clap/blame/5b9dbee5db540f44c336c2db54ece444624c30ef/src/build/app/mod.rs#L861-L877) for `replace` is to create what I would call a "subcommand alias" where "install" is an alias for "module install". For a "subcommand alias" I would expect it to only work when the alias is the _first_ or _only_ argument. But for my use case, "save-all" can appear anywhere in the arguments. This should be considered and documented better.

I would expect my use case to be possible at the `Arg` level instead of `App`, but oh well.

Ideally, I would like the `implies` functionality to show in the help text for that Arg. But again, this is less important.

---

_Comment by @kbknapp on 2020-04-29 19:40_

@camsteffen we're in the process of going through all docs, so I'll make sure that one gets some additional explanation to make it clear that it's not just for subcommands. You can think of it literally replacing/inserting the left hand side item (`--save-all`) with all of the right hand side items (`--save-image --save-stats --save-text`)prior to parsing.

So the CLI invocation goes from:

```
$ app --save-all

  # TO

$ app ---save-image --save-stats --save-text
```

You're correct it doesn't automatically add verbiage to the help message of `--save-all` that it sets the others, for now that is left up to the consumer to add.

---

_Comment by @camsteffen on 2020-04-29 21:34_

@kbknapp Sounds good.

I'm still wondering about the example for `replace` though. Should `app module install` become `app module module install`?

---

_Comment by @kbknapp on 2020-04-30 01:17_

`replace` is not an alias function. We will find a better way to word the example, but the example isn't creating an alias, it's creating a "shortcut" so to speak.

Imagine `app` has one subcommand `module`, and `app module` has a subcommand `install`. What the example is doing is creating a fake second subcommand of `app` called `install` which actually invokes the `app module install` subcommand. `rustup` does something similar, just without the `replace` functionality. While it's similar to an alias, aliases don't let you inject and thus invoke further into the CLI. Aliases only let you refer to a subcommand *on the same level* by a different name. For example, in our `app` example, if we gave the `module` subcommand and alias of `mod`, it would  mean both of these are functionally equivalent: 

```
$ app module install
$ app mod install
```

Likewise, if *instead of* `replace` we'd used an alias, and gave `module` an alias of `install`, the following would be functionally equivalent (although nonsensical):

```
$ app module install
$ app install install
```

Hopefully this clears it up a little more.

I'm going to close this issue, but feel free to continue asking questions if you need more explanation :wink: 

---

_Closed by @kbknapp on 2020-04-30 01:17_

---

_Comment by @pksunkara on 2020-04-30 02:51_

> Ideally, I would like the implies functionality to show in the help text for that Arg. 

> You're correct it doesn't automatically add verbiage to the help message of --save-all that it sets the others, for now that is left up to the consumer to add.

That's true for for all the other conditions too. `requires*`, `conflicts*` similarly doesn't set any help text.

> replacing/inserting the left hand side item with all of the right hand side items prior to parsing.

It does the replacing during parsing which allows to let it work only on certain apps (or subcommands)

`app module install` would not change because replacer is checking for `install` on the `app` `App`. Here `install` appears at the `module` subcommand `App`.

I am actually using it like the below in one of the CLI tools I am working on.

```rust
App::new("app")
    .replace("ll", &["ls", "-l"])
    .replace("la", &["ls", "-a"])
    .subcommand(App::new("ls")
        .arg(Arg::new("all").short('a'))
        .arg(Arg::new("long").short('l'))
```

---

_Comment by @camsteffen on 2020-04-30 13:51_

> That's true for for all the other conditions too. `requires*`, `conflicts*` similarly doesn't set any help text.

Ok, makes sense.

> `app module install` would not change because replacer is checking for `install` on the `app` `App`. Here `install` appears at the `module` subcommand `App`.

Ah, that answers my question!

Ok, I'm happy with `replace` 😃. Will it be in a release soon?

---

_Comment by @kbknapp on 2020-04-30 15:41_

>  Will it be in a release soon?

You can already use it via [git dependency](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#specifying-dependencies-from-git-repositories) in Cargo.toml

```
[dependencies]
clap = { git = "https://github.com/clap-rs/clap" }
```

You can specify by branch or version (`replace` is in version `3.0.0-beta.1`). Once we finish up the 3.0 release, it will be uploaded to crates.io as well.

---

_Comment by @camsteffen on 2020-04-30 16:10_

Alas, I finally tried it and it did not work as expected.

```rust
let matches = clap::App::new("MyApp")
    .replace("--save-all", &["--save-a", "--save-b"])
    .arg(Arg::with_name("flag").short('f'))
    .arg(Arg::with_name("save_all").long("save-all"))
    .arg(Arg::with_name("save_a").long("save-a"))
    .arg(Arg::with_name("save_b").long("save-b"))
    .get_matches_from(&["-f", "--save-all"]);
assert!(matches.is_present("flag")); // fails here
assert!(matches.is_present("save_a"));
assert!(matches.is_present("save_b"));
```

---

_Comment by @kbknapp on 2020-04-30 16:13_

You're missing the binary name. It should be:

```rust
    .get_matches_from(&["myapp", "-f", "--save-all"]);
```

If you don't want to include a binary name you must use [`AppSettings::NoBinaryName`](https://docs.rs/clap/2.33.0/clap/enum.AppSettings.html#variant.NoBinaryName)

---

_Comment by @camsteffen on 2020-04-30 16:40_

🤦 

---

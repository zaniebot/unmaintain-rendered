---
number: 2035
title: "Create own error message in clap's style"
type: issue
state: closed
author: LoganDark
labels: []
assignees: []
created_at: 2020-07-25T11:40:54Z
updated_at: 2021-12-09T00:58:08Z
url: https://github.com/clap-rs/clap/issues/2035
synced_at: 2026-01-10T01:27:12Z
---

# Create own error message in clap's style

---

_Issue opened by @LoganDark on 2020-07-25 11:40_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

`clap` emits its own error messages. They look quite nice, they have color, all that good stuff. Maybe I want to take advantage of this feature - perhaps instead of using `panic!`, or manually emitting a message to stderr and then exiting, I want to emit an error message that matches the ones `clap` gives.

The main reason for this is **consistency**. When your error message looks different from `clap`'s, it kind of feels weird to the end user. I've never encountered a utility that has different error messages when parsing arguments than when running the program itself, and it would be weird to make the developer try to _mimic `clap`_ instead of offering them an API for it.

### Describe the solution you'd like

My use case specifically would benefit from a `panic!`-like solution that emits an error message of my own choosing and then exits. However, that's a very minimal solution and there are better ones, see below.

### Alternatives, if applicable

(package names might not be 100% accurate as I'm using file paths)

Really any way to format error messages in the same style as `clap` would be nice. For example, you could just make `clap::output::fmt::Colorizer` public. Although, if you do that, you should make `clap::parse::errors::start_error` public too.

Making these things public would benefit consumers of your API as they'd be able to format their error messages and color their messages the same way as `clap` does, allowing the application to look more consistent. They could even use the `Colorizer` for general output.

However, since these things are implementation details at the moment it's possible that yet another solution could be implemented instead, more well-suited to consumers of `clap`'s API. I could come up with many such solutions, but I think it should be up to the maintainers of `clap`.

---

_Label `T: new feature` added by @LoganDark on 2020-07-25 11:40_

---

_Comment by @CreepySkeleton on 2020-07-25 20:43_

You can create a custom error with `Error::with_description` (I think better rename it to `custom`). It doesn't cover coloring though.

I like the proposal, but I think I would wait until after we're done with the refactorings and stuff.

---

_Label `C: colorizing` added by @CreepySkeleton on 2020-07-25 20:44_

---

_Label `C: errors` added by @CreepySkeleton on 2020-07-25 20:44_

---

_Label `C: other` added by @CreepySkeleton on 2020-07-25 20:44_

---

_Label `W: postponed` added by @CreepySkeleton on 2020-07-25 20:44_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-07-25 20:44_

---

_Comment by @LoganDark on 2020-07-25 20:45_

> I think I would wait until after we're done with the refactorings and stuff.

That is a reasonable goal, but you are in the process of refactoring, why not make this change while you can? Major version 3 still isn't fully released, so you have more freedom. Once the version 3 API is stabilized, you can't make any more breaking changes (like renaming that method for example).

---

_Comment by @CreepySkeleton on 2020-07-25 20:55_

Because it's not yet decided what the colorizer will look like. Once we're settled on that, we can make it public and document.

If you have any alternative solutions, I'm all ears.

---

_Referenced in [clap-rs/clap#2196](../../clap-rs/clap/issues/2196.md) on 2020-10-31 21:07_

---

_Label `W: postponed` removed by @pksunkara on 2021-05-26 11:06_

---

_Comment by @epage on 2021-11-29 14:37_

In clap3, we now have a [`App::error()`](https://docs.rs/clap/3.0.0-beta.5/clap/struct.App.html#method.error) (added in #2890) which provides the full clap-style error message, including the usage, recommendation for help, and `WaitOnError` support (#2967), unlike `with_description`.  It does not provide a smart usage (adapting to your specific command or to your place in a subcommand hierarchy) and it doesn't expose the coloring API to end users (the clap boiler plate does get colored though).

---

_Comment by @medwards on 2021-12-03 15:22_

Thanks @epage that works quite nicely for my case at least (not OP).

Small paper-cut for future users: youhave to pre-construct the error or clone their app because once `get_matches` is called it consumes the entire `App`. 
```rust
    let mut app = clap::App::new("app")
        .subcommand(
            clap::App::new("run")
                .about("a silly subcommand but makes my app look nice")
                .arg(clap::Arg::new("COMMAND").required(true)),
        );  
    let subcommand_error = app.error(
        clap::ErrorKind::MissingSubcommand,
        "Missing subcommand which wasn't expected. Did you mean 'run'?",
    );  
    let matches = app.get_matches();
    if let Some(subcommand) = matches.subcommand_matches("run") {
        subcommand.value_of("COMMAND").expect("command was not provided");
    } else {
        subcommand_error.exit();
    } 
```

---

_Referenced in [epage/clapng#160](../../epage/clapng/issues/160.md) on 2021-12-06 20:17_

---

_Referenced in [epage/clapng#168](../../epage/clapng/issues/168.md) on 2021-12-06 20:19_

---

_Comment by @epage on 2021-12-08 20:19_

I'm going to go ahead and close this out, assuming `App::error` is sufficient.

---

_Closed by @epage on 2021-12-08 20:19_

---

_Comment by @pksunkara on 2021-12-09 00:53_

> It does not provide a smart usage (adapting to your specific command or to your place in a subcommand hierarchy) and it doesn't expose the coloring API to end users (the clap boiler plate does get colored though).

Do we want to keep this open to address those 2 issues? Or you want to create new issues to track them?

---

_Comment by @epage on 2021-12-09 00:58_

I'd recommend users opening the issues in response to their usage of `App::error` so we are collecting more real world use cases and not just us extrapolating what all is needed and making our backlog too big to handle.

---

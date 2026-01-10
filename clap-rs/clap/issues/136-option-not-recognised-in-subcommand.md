---
number: 136
title: Option not recognised in subcommand
type: issue
state: closed
author: bmcorser
labels: []
assignees: []
created_at: 2015-06-01T08:02:09Z
updated_at: 2018-08-02T03:29:40Z
url: https://github.com/clap-rs/clap/issues/136
synced_at: 2026-01-10T01:26:24Z
---

# Option not recognised in subcommand

---

_Issue opened by @bmcorser on 2015-06-01 08:02_

Not sure if this is an error on my part, but in [this branch](https://github.com/bmcorser/git-tags-rs/tree/clap-opt-issue) the `-m, --message` option isn't recognised when I pass it.

To reproduce (with Rust nightly):

```
git clone git@github.com:bmcorser/git-tags-rs.git
cd git-tags-rs
git checkout clap-opt-issue
cargo build
target/debug/tag release --message="hello"
target/debug/tag release -m "hello"
```

It will print `"nicht"` both times.


---

_Comment by @kbknapp on 2015-06-01 11:57_

Thanks for taking the time to file the issue! I'll look into it today and report back here.


---

_Comment by @kbknapp on 2015-06-01 12:27_

I just found the reason. It's because a subcommand's matches are separate from the parent command's matches. It forms a tree hierarchy, and allows you re-use names and differintiate between different "commands" matches without them all getting mixed together, its also a performance boost.

The fix for you is to change [your src/bin/main.rs](https://github.com/bmcorser/git-tags-rs/blob/2c9d06c9ee2fe8c9dbfe63e1e877e2c47d11125b/src/bin/main.rs#L16-20) to this:

``` rust
match matches.subcommand() {
        ("release", Some(m)) => release::run(m),
        ("lookup", Some(m))  => lookup::run(m),
        _ => (),
    }
```

the `.subcommand()` returns a tuple of a slice of the name, along with an `Option<ArgMatches>` of it's matched arguments. Whereas with the `.subcommand_name()` you were passing the _parent_ (i.e. `tag`'s) matches to `release::run()`

After that, just change your `::run()` function to accept a borrow  (`&clap::ArgMatches`)

**Note:** I changed the child `matches` to `m` just to visually differentiate. It works just fine if you use `Some(matches)) => release::run(matches),` because the parent `matches` gets shadowed ;)


---

_Closed by @kbknapp on 2015-06-01 12:29_

---

_Comment by @bmcorser on 2015-06-01 12:40_

:+1: thanks @kbknapp


---

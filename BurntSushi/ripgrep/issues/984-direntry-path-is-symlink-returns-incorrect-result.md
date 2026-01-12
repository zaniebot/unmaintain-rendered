```yaml
number: 984
title: "DirEntry::path_is_symlink returns incorrect result for first result from ignore iterators"
type: issue
state: closed
author: ssokolow
labels:
  - bug
assignees: []
created_at: 2018-07-17T13:02:03Z
updated_at: 2018-08-22T01:42:47Z
url: https://github.com/BurntSushi/ripgrep/issues/984
synced_at: 2026-01-12T16:13:22Z
```

# DirEntry::path_is_symlink returns incorrect result for first result from ignore iterators

---

_@ssokolow_

#### What version of ripgrep are you using?
I'm using the `ignore` crate, version 0.4.2, but you don't seem to have a separate repository for it.

#### How did you install ripgrep?
I added `ignore = "0.4"` to my `Cargo.toml`

#### What operating system are you using ripgrep on?

    ssokolow@monolith ignore_test [master] % lsb_release -a
    No LSB modules are available.
    Distributor ID: Ubuntu
    Description:    Ubuntu 14.04.5 LTS
    Release:        14.04
    Codename:       trusty

#### Describe your question, feature request, or bug.

Iterators from the `ignore` crate incorrectly identify the argument passed to them as a symlink when they return it during the first iteration.

This is actually quite the annoyance for a project I've just started, because it means I'm going to need to design some kind of minimally ugly hack to do a second syscall to get the *actual* value but only on the first iteration to avoid needlessly bogging down the following iterations.

#### If this is a bug, what are the steps to reproduce the behavior?

    use std::fs::metadata;

    extern crate ignore;
    use ignore::Walk;

    fn main() {
        for result in Walk::new("./") {
            if let Ok(entry) = result {
                let verification = metadata(entry.path()).unwrap().file_type().is_symlink();
                println!("{}: {}", entry.path_is_symlink() == verification, entry.path().display());
            }
        }
    }

#### If this is a bug, what is the actual behavior?

    ssokolow@monolith ignore_test [master] % cargo run
       Compiling ignore_test v0.1.0 (file:///home/ssokolow//src/ignore_test)
        Finished dev [unoptimized + debuginfo] target(s) in 4.72 secs
         Running `target/debug/ignore_test`
    false: ./
    true: ./Cargo.lock
    true: ./src
    true: ./src/main.rs
    true: ./target
    true: ./target/debug
    [output truncated for brevity as the rest is irrelevant]

(Note that the first entry is `false`, indicating that the result returned by `ignore`'s `path_is_symlink` doesn't match the result returned by explicitly querying the filesystem.)

#### If this is a bug, what is the expected behavior?

    ssokolow@monolith ignore_test [master] % cargo run
       Compiling ignore_test v0.1.0 (file:///home/ssokolow//src/ignore_test)
        Finished dev [unoptimized + debuginfo] target(s) in 4.72 secs
         Running `target/debug/ignore_test`
    true: ./
    true: ./Cargo.lock
    true: ./src
    true: ./src/main.rs
    true: ./target
    true: ./target/debug
    [output truncated for brevity as the rest is irrelevant]

(Note that the first entry is `true` as well)

---

_Comment by @BurntSushi on 2018-07-17 13:12_

Nice bug. This bug is actually present in `walkdir` in addition to the parallel walker in `ignore` as well. (I believe that's a total of two distinct bugs, since the single threaded `ignore` walker mostly just defers to `walkdir` internally.)

Example, riffing off of yours:

```rust
extern crate ignore;
extern crate walkdir;

use std::fs::metadata;

use ignore::{Walk, WalkBuilder, WalkState};
use walkdir::WalkDir;

fn main() {
    for result in WalkDir::new("./") {
        if let Ok(entry) = result {
            let verification = metadata(entry.path())
                .unwrap()
                .file_type()
                .is_symlink();
            println!(
                "{}: {}",
                entry.path_is_symlink() == verification,
                entry.path().display(),
            );
        }
    }

    println!("\n----------------------------------------------\n");

    for result in Walk::new("./") {
        if let Ok(entry) = result {
            let verification = metadata(entry.path())
                .unwrap()
                .file_type()
                .is_symlink();
            println!(
                "{}: {}",
                entry.path_is_symlink() == verification,
                entry.path().display(),
            );
        }
    }

    println!("\n----------------------------------------------\n");

    let par = WalkBuilder::new("./").build_parallel();
    par.run(|| {
        Box::new(|result| {
            if let Ok(entry) = result {
                let verification = metadata(entry.path())
                    .unwrap()
                    .file_type()
                    .is_symlink();
                println!(
                    "{}: {}",
                    entry.path_is_symlink() == verification,
                    entry.path().display(),
                );
            }
            WalkState::Continue
        })
    });
}
```

The above shows the same bug in all three instances.

---

_Comment by @ssokolow on 2018-07-17 13:24_

\*chuckle* I seem to have been finding quite a few good bugs in the project I just started.

For example, only an hour or two after I made a note to come back to that, I got `serde_json` to [panic](https://github.com/serde-rs/json/issues/464) because it turns out I have a file on one of my `ext3` partitions with a POSIX timestamp of `-1`.

---

_Comment by @BurntSushi on 2018-07-17 13:28_

This shouldn't be too hard to fix. It looks like the bug is that [the initial path is turned into an entry using `from_link`](https://github.com/BurntSushi/walkdir/blob/4b21b55f2180bdd27d99a8999ae390a0568658da/src/lib.rs#L654), which always sets `follow_link` to `true`, which in turn implies that [`path_is_symlink` always returns `true`](https://github.com/BurntSushi/walkdir/blob/4b21b55f2180bdd27d99a8999ae390a0568658da/src/lib.rs#L982).

My guess is that I erroneously used `from_link` for the initial path because the initial path doesn't have a `DirEntry`. But this of course shouldn't always cause it to be interpreted as a symlink.

Investigating a bit more, it looks like I introduced this bug in commit https://github.com/BurntSushi/walkdir/commit/373278952ff96fd01287c6ac89f1bf4b74c6490d, which appears motivated by the notion that the path given should always be followed, even if it's a symlink, which seems fine to me.

I think the question I have is why `follow_link` is used at all in the implementation of `path_is_symlink`. I'll have to noodle on that one.

And yeah, nice serde bug. :-)

---

_Label `bug` added by @BurntSushi on 2018-07-17 13:29_

---

_Closed by @BurntSushi on 2018-08-22 01:42_

---

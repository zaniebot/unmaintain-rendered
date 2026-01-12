```yaml
number: 439
title: rg --help outputs search results
type: issue
state: closed
author: bluss
labels:
  - bug
assignees: []
created_at: 2017-04-05T14:14:13Z
updated_at: 2017-04-09T14:37:00Z
url: https://github.com/BurntSushi/ripgrep/issues/439
synced_at: 2026-01-12T16:13:22Z
```

# rg --help outputs search results

---

_@bluss_

I don't understand this.

rg 0.5.0 installed using cargo install -f ripgrep compiled using `rustc 1.18.0-nightly (5309a3e31 2017-04-03)` platform: Linux x86-64.

`~/.cargo/bin/rg -h` outputs the help as expected, `~/.cargo/bin/rg --help` outputs..something that looks like every line of all the files?

---

_Comment by @kbknapp on 2017-04-05 14:55_

`cargo install` doesn't use/respect the `Cargo.lock` (TMK). See #438 for details on this issue.

---

_Comment by @BurntSushi on 2017-04-05 14:56_

Strange. I can't seem to reproduce. There's been a little bit of churn lately on handling `-h/--help`, so I wonder if compiling from `master` has the same bug. Would you be willing to try that?

---

_Comment by @BurntSushi on 2017-04-05 14:56_

@kbknapp Oh. Derp. Thanks for that reminder. :-)

---

_Comment by @BurntSushi on 2017-04-05 14:57_

> I can't seem to reproduce.

Of course, I didn't use the same steps as @bluss. Instead, I did this, which I thought was equivalent before @kbknapp's reminder:

```
$ git clone git://github.com/BurntSushi/ripgrep
$ cd ripgrep
$ git checkout 0.5.0
$ cargo build --release
$ ./target/release/rg --help
```

---

_Comment by @kbknapp on 2017-04-05 15:06_

I just pushed the new version to crates.io so once the ripgrep tests pass it should be safe to merge ;-)

---

_Comment by @BurntSushi on 2017-04-05 15:39_

OK, the PR is merged. I will close this one out once I put out a new release of ripgrep and test that `cargo install` works. Thanks again @kbknapp!

---

_Comment by @bluss on 2017-04-05 17:42_

Cool. What does cargo install --locked do? It's not entirely clear.

---

_Comment by @bluss on 2017-04-05 18:20_

I'm confused too, was just looking at cargo install and wondering if it were possible to ask it to respect the lock file. Then I learned it's not packaged.

---

_Comment by @kbknapp on 2017-04-05 18:49_

>What does cargo install --locked do? It's not entirely clear.
> [..] was just looking at cargo install and wondering if it were possible to ask it to respect the lock file. Then I learned it's not packaged.

So, the `Cargo.lock` is never packaged? I'm wondering what `--locked` and `--frozen` do too, then. Seems strange to me, as it's pretty much the whole purpose for *binaries* so that things like this exact issue don't happen. I'd have thought (before just recently) that it'd be the *default* for `cargo install`

---

_Comment by @yarko on 2017-04-09 02:26_

glad that simply following @BurntSushi 's clone/build idiom works for just copying the built command into place.


---

_Label `bug` added by @BurntSushi on 2017-04-09 12:51_

---

_Comment by @BurntSushi on 2017-04-09 14:32_

I can confirm that `cargo install ripgrep` now works in `0.5.1`. Thanks for the report @bluss!

---

_Closed by @BurntSushi on 2017-04-09 14:32_

---

_Comment by @bluss on 2017-04-09 14:33_

Thanks for fixing. I'm glad you continue to support cargo install, it's very convenient.. for rusties.

---

_Comment by @bluss on 2017-04-09 14:37_

Fix confirmed on this end, by the way.

---

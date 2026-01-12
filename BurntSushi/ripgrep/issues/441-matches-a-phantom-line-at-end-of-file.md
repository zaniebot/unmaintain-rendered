```yaml
number: 441
title: ^$ matches a phantom line at end of file
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - help wanted
assignees: []
created_at: 2017-04-08T00:30:11Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/441
synced_at: 2026-01-12T16:13:22Z
```

# ^$ matches a phantom line at end of file

---

_@BurntSushi_

This is simple to demonstrate. Note the behavior difference between GNU grep and ripgrep:

```
$ cat scratch 
a
b

c


d
$ rg '^$' scratch
3:
5:
6:
8:
$ egrep -n '^$' scratch
3:
5:
6:
```

The `^$` pattern is a useful idiom for detecting empty lines in a file. ripgrep's behavior in particular outputs a line numbered 8, even though there are only 7 lines in the file:

```
$ wc -l scratch
7 scratch
```

More generally, ripgrep should never emit a line with a number greater than the number of lines in the file, even if the file ends with a trailing `\n`.

See also https://github.com/BurntSushi/ripgrep/issues/416 and https://github.com/rust-lang/regex/issues/355#issuecomment-292679709 for the root cause

---

_Comment by @BurntSushi on 2017-04-08 00:30_

cc @BatmanAoD

---

_Label `bug` added by @BurntSushi on 2017-04-08 00:41_

---

_Comment by @BatmanAoD on 2017-04-10 18:18_

Thanks for filing this. I agree that it's reasonable to keep the `regex` crate behavior.

(Also, I know you asked for example use-cases for this, and unfortunately, although I know I've used something like `ack '^$'` in the past, I can't actually remember the specifics. I did find some StackOverflow questions with numerous upvotes about how to search for empty lines, but I didn't see anyone discussing specifically why they wanted to do that. This may not be a common enough use-case to be a high-priority bug.)

---

_Comment by @BurntSushi on 2017-04-10 18:22_

@BatmanAoD Right. When I asked that, I hadn't thought of "match empty lines." Once I spent a couple of minutes thinking, I remembered it. That's a strong enough use case as far as I'm concerned. :-)

---

_Comment by @BurntSushi on 2017-04-10 18:22_

(I wouldn't expect someone to care about matching empty lines necessarily, but I could see someone being interested in counting them.)

---

_Comment by @blankname on 2017-04-10 18:34_

I use matching empty lines in conjunction with the --invert-match option to filter out empty lines when piping commands.

---

_Comment by @BatmanAoD on 2017-04-10 19:35_

@blankname That actually shouldn't be a problem! You'll "erroneously" filter out a phantom line--which should have no effect.

@BurntSushi Counting seems like an interesting application indeed, but as soon as I thought about that I thought "isn't that just a sort of crumby, broken `loc`?"

---

_Comment by @BurntSushi on 2017-04-10 20:26_

I've never heard of, or used, `loc`. :-)

---

_Comment by @BatmanAoD on 2017-04-10 21:07_

`loc` and `tokei` are Rust clones of `cloc`, which counts the lines of code in a project and produces a nice little table where the rows are different languages (autodetected) and the columns are things like blank lines, comments, and actual code. I've used `cloc` a lot more than `loc` (it's a lot older), but I figured `loc` would be the right one to mention here!

---

_Comment by @BurntSushi on 2017-05-08 22:14_

Here are a pair of regression tests:

```rust
// See: https://github.com/BurntSushi/ripgrep/issues/441
clean!(regression_441_mmap, "^$", "test", |wd: WorkDir, mut cmd: Command| {
    wd.create("test", "a\nb\n\nc\n\n\nd\n");
    cmd.arg("--mmap");

    let lines: String = wd.stdout(&mut cmd);
    assert_eq!(lines, "\n\n\n");
});

// See: https://github.com/BurntSushi/ripgrep/issues/441
clean!(regression_441_no_mmap, "^$", "test", |wd: WorkDir, mut cmd: Command| {
    wd.create("test", "a\nb\n\nc\n\n\nd\n");
    cmd.arg("--no-mmap");

    let lines: String = wd.stdout(&mut cmd);
    assert_eq!(lines, "\n\n\n");
});
```

Interestingly, the output of the `mmap` test is `\n\n\n\n` and the output of the `no_mmap` test is `\n\n\n\n\n`.

---

_Label `help wanted` added by @BurntSushi on 2017-05-08 22:14_

---

_Comment by @BurntSushi on 2017-05-08 22:21_

And also a more precise comparison with grep:

```
$ cat scratch
a
b

c


d
$ xxd scratch
00000000: 610a 620a 0a63 0a0a 0a64 0a              a.b..c...d.
$ grep '^$' scratch | xxd
00000000: 0a0a 0a                                  ...
$ rg --mmap '^$' scratch | xxd
00000000: 0a0a 0a0a                                ....
$ rg --no-mmap '^$' scratch | xxd
00000000: 0a0a 0a0a 0a                             .....
```

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-06-03 21:02_

---

_Comment by @BurntSushi on 2018-06-03 21:08_

I believe this bug should be fixed in libripgrep.

---

_Comment by @BatmanAoD on 2018-06-05 17:00_

"Should" meaning "ought to be"? Since the libripgrep issue itself states that libripgrep won't be a proper "thing unto itself", do you have an idea of which specific subcomponent should handle this?

---

_Comment by @BurntSushi on 2018-06-05 17:17_

libripgrep is a complete rewrite of the search components of ripgrep. As I progress through that rewrite, I'm looking at bugs in this space that I can fix.

The libripgrep milestone is tracked here: https://github.com/BurntSushi/ripgrep/issues?q=is%3Aopen+is%3Aissue+milestone%3Alibripgrep

> Since the libripgrep issue itself states that libripgrep won't be a proper "thing unto itself"

This just means that there isn't going to be a singular ripgrep library. There will be many. The `grep` crate will be the focal point.

---

_Comment by @BatmanAoD on 2018-06-05 17:29_

Ah, okay, I thought it was more of a refactoring/splitting than a complete rewrite.

Might #416 (or even a partial solution like #929) be another candidate for fixing in the rewrite?

---

_Comment by @BurntSushi on 2018-06-05 17:55_

Unlikely. The fix for #416 is to fix the regex engine.

> Ah, okay, I thought it was more of a refactoring/splitting than a complete rewrite.

They are both true. I don't think the distinction matters much.

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

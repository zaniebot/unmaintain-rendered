```yaml
number: 26
title: add -l / --files-with-matches option?
type: issue
state: closed
author: Roguelazer
labels:
  - enhancement
assignees: []
created_at: 2016-09-23T18:32:38Z
updated_at: 2016-09-25T18:53:31Z
url: https://github.com/BurntSushi/ripgrep/issues/26
synced_at: 2026-01-12T18:23:11Z
```

# add -l / --files-with-matches option?

---

_@Roguelazer_

I use the `-l` option to grep / ack / ag / etc quite a lot to list files containing matches for further processing in a shell pipeline. Does it seem reasonable as a thing to add to ripgrep?


---

_Comment by @BurntSushi on 2016-09-24 01:14_

Does `-c`/`--count` satisfy your use case?


---

_Comment by @andyleejordan on 2016-09-24 01:52_

@BurntSushi at least my use case is generally `ag -l something | xargs ...`, so `-c` doesn't work without an intermediate `awk` or something to strip the count.


---

_Comment by @BurntSushi on 2016-09-24 01:55_

That's reasonable. `grep` also has these options, so it seems fine for `rg` to grow them as well.


---

_Label `enhancement` added by @BurntSushi on 2016-09-24 01:55_

---

_Comment by @andyleejordan on 2016-09-24 01:56_

I was looking at adding it to `ucg` yesterday... now I'm looking at doing it here (I do like Rust more!) (but no promises; recovering from a cold and might give up to play some video games).


---

_Comment by @BurntSushi on 2016-09-24 02:02_

@andschwa No worries! I might actually bang this out tonight.


---

_Comment by @andyleejordan on 2016-09-24 02:43_

@BurntSushi Hey I just got it working! Let me get some tests and docs written and cleaned up, and I'll open a PR.


---

_Comment by @BurntSushi on 2016-09-24 02:46_

@andschwa Oh you're quick, awesome, thank you!


---

_Comment by @andyleejordan on 2016-09-24 02:52_

@BurntSushi Working, but not finished. Right now it operates almost exactly the same as `count` but calls `printer.path` instead of `printer.path_count`, but doing this _right_ I think includes terminating the search after the first match.

It also should be just as easy to add `-L/--files-without-match` that just sets `files_with_matches` and `invert_match`.

I also think I could share a bit more logic with `count`... I might open a review to ask for some refactor help.


---

_Comment by @andyleejordan on 2016-09-24 02:53_

Tada:

``` sh
$ ./target/debug/rg -l files_with_matches
src/search_buffer.rs
src/search_stream.rs
src/args.rs
```


---

_Comment by @BurntSushi on 2016-09-24 03:01_

Nice!

>  but doing this right I think includes terminating the search after the first match.

Absolutely, yes. This will probably need to go in both of the searchers, which means making them aware of this parameter as well.


---

_Comment by @BurntSushi on 2016-09-24 03:02_

I'd be happy to do a review. The printer is due for a bit of refactoring, so don't feel too bad about it.


---

_Comment by @andyleejordan on 2016-09-24 03:08_

Aye yai yai... figured out with `-L` wasn't working (but my quick addition of `--files-without-match` was); it's already used for `--follow`, but grep uses `-L` for this. What do you do in this case?


---

_Comment by @BurntSushi on 2016-09-24 03:11_

Bah. How about we prefer compatibility with `grep` (although I got `-L` from `find`)... Hmm...

Maybe we can just drop the short hand for `--files-without-match` altogether?


---

_Closed by @BurntSushi on 2016-09-25 18:53_

---

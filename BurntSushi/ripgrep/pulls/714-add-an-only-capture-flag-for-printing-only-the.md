```yaml
number: 714
title: "Add an `only-capture` flag for printing only the text in the specified capture group"
type: pull_request
state: closed
author: markprzepiora
labels: []
assignees: []
base: master
head: only-capture
created_at: 2017-12-15T03:06:49Z
updated_at: 2018-01-29T21:17:22Z
url: https://github.com/BurntSushi/ripgrep/pull/714
synced_at: 2026-01-12T18:23:13Z
```

# Add an `only-capture` flag for printing only the text in the specified capture group

---

_@markprzepiora_

This is the first Rust code I've ever written so I apologize in advance if it's horrible.

This option implies `--only-matching`, and will result in only the given capture group (specified by its number) being printed.

I often find myself wanting this functionality in `rg`/`ag`/`ack`... I can imitate *some* use-cases with look-behinds/look-aheads, but not all. (Look-aheads and look-behinds need to be fixed-length, in particular.)

---

_Comment by @okdana on 2017-12-15 03:12_

Are you familiar with the `--replace` option? It can do this (and more). I'm on mobile but it's something like:

```
rg --replace '$1' '.*foo(bar)baz.*'
```

---

_Comment by @markprzepiora on 2017-12-15 03:18_

Oof, how embarrassing! How did I never know about this?

Sorry to waste your time with this PR. :) At least I learned a little bit of Rust in the process.

---

_Comment by @markprzepiora on 2017-12-15 03:20_

(In my defence, it looks like silver searcher doesn't have this option, which is what I was using until I found ripgrep recently, so that would explain why I didn't know about this.)

---

_Review comment by @BurntSushi on `src/printer.rs`:93 on 2017-12-18 13:11_

For reference, this should probably be an `Option<usize>`, where `Some(0)` is equivalent to `--only-matching`.

---

_Review comment by @BurntSushi on `src/printer.rs`:291 on 2017-12-18 13:14_

This is interesting behavior. If the capture index doesn't exist in the match, then the result is to use the entire match? That doesn't feel quite right to me. It kind of feels like it should use an empty string. And in that case, the line emitted would be empty I guess, which feels a little weird but is consistent with `--only-matching` and `--replace` I believe.

---

_Review comment by @BurntSushi on `tests/tests.rs`:1833 on 2017-12-18 13:15_

I suspect we need more tests than this. In particular, the case of testing the use of a capture index that sometimes doesn't exist, depending on the match.

---

_@BurntSushi requested changes on 2017-12-18 13:17_

Thanks for this PR! It is an interesting feature. @okdana is of course correct in that the `--replace` flag already provides an avenue for achieving this. However, I do somewhat appreciate the UX of this particular flag, and I like its seeming parallel with `--only-matching`. @okdana What do you think about adding this? Is it worth it?

Also, it looks like CI is failing because the docs need to be updated. (I think it would be nice to include something like the phrase "`--only-capture 0` is equivalent to `--only-matching`.")

---

_Comment by @okdana on 2017-12-18 19:55_

>@okdana What do you think about adding this? Is it worth it?

There are two nice things about it compared to `--replace`, imo: It eliminates the need to explicitly surround the pattern by `.*` (which some users have seemed confused by), and, if granted the suggested short option, it's way easier to type:

```
rg --replace '$1' '.*(foo)bar.*'
rg -O1 '(foo)bar'
```

~~(Personally i think the `--replace` option is so useful that it deserves its own short option, but that's another thing i guess)~~

Edit: lol, it turns out `--replace` already has a short option. Whoops. Guess i should read the manual sometimes too. Still shorter though

It doesn't seem like a huge maintenance burden either. I would probably use it once in a while, tbh

The documentation and completion function need updated to reflect both the short and long options, if added, though (that seems to be why the CI failed)

---

_Comment by @BurntSushi on 2017-12-18 21:30_

@okdana Awesome, I think I agree. I also like the the idea of having a short flag for this too. `-O` doesn't appear to be used in grep, so I think it'd be OK to use that.

@markprzepiora If you would like to respond to the feedback here and get this PR updated, then I think this can be merged!

---

_Comment by @markprzepiora on 2017-12-19 18:34_

Thanks for the feedback, everyone! :)

I have a busy day at work for the next two days but I will grok the changes you requested and try to implement them Thursday evening.

---

_Comment by @beyondcompute on 2018-01-22 15:32_

Isn’t the same thing addressed in #593? ☺️ 

---

_Comment by @BurntSushi on 2018-01-29 21:17_

@beyondcompute Indeed! `rg 'fn (\w+)' -or '$1'` does the expected thing. In light of that, I think I'm going to close this out. Documentation improvements to increase awareness of combining `--only-matching` and `--replace` would be great though. Thanks @markprzepiora for the PR!

---

_Closed by @BurntSushi on 2018-01-29 21:17_

---

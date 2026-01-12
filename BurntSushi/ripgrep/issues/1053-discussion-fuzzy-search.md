```yaml
number: 1053
title: "[Discussion] Fuzzy Search"
type: issue
state: closed
author: Miserlou
labels:
  - wontfix
assignees: []
created_at: 2018-09-13T18:11:58Z
updated_at: 2022-08-09T16:37:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1053
synced_at: 2026-01-12T16:13:22Z
```

# [Discussion] Fuzzy Search

---

_@Miserlou_

(This isn't a bug report, just an idea for discussion/feature request!)

I've been using `ack`, `ag`, and now `rg` for years now. Thanks ever so much for this tool - I use it literally hundreds of times a day!

Something that I've always wanted in this family is the ability to perform a fuzzy search. For instance, an `rg search --fuzzy` would include all of the places that I've created the typo `saerch` in my code. (The behavior is not too dissimilar to `--smart-case`, but for character changes rather than capitalization changes.)

Perhaps, if the implementation allows, this could be extended to allow an `--edit-distance`, such that `rg search --fuzzy --edit-distance 2` would return `saecrh`, but `rg search --fuzzy --edit-distance 1` wouldn't.

I realize that this is probably quite difficult given how much everything relies on regular expressions, and this might require a change to Rust's regex under the hood, which is a huge ask. But, I just wanted to throw it out there to see if this is at all possible. It'd make my life a lot easier and would add powerful new capabilities to the tool.

Thanks for your consideration and time!

---

_Comment by @BurntSushi on 2018-09-13 18:21_

Thanks for the idea! While I agree this would be quite cool, you are indeed correct that this is a rather large feature request and unlikely to happen at all. At least, inside of ripgrep. For that reason, I'm going to close this.

With that said, ripgrep recently gained the ability to use any kind of regex engine. With that functionality, it would in theory be possible to build a literal-only matching engine that used a levenshtein automaton to implement edit distance like search. It's a substantial thing to do, but it's tractable and the ripgrep architecture is amenable to it.

More generally, to handle your use case in a more robust way, I think I'd like to see a high quality code oriented fulltext search tool. It's something I've always wanted to build, and even thought about including it in ripgrep at one point: https://github.com/BurntSushi/ripgrep/issues/95 --- Alas, there is only so much time in the day.

---

_Closed by @BurntSushi on 2018-09-13 18:21_

---

_Label `wontfix` added by @BurntSushi on 2018-09-13 18:21_

---

_Comment by @forrestthewoods on 2018-09-14 09:43_

Funny story, I came here to ask this exact question. Lo and behold someone beat me to the punch by 15 hours! Funny how that works.

Well, almost exact. @Miserlou appears to be specifically asking a levenshtein distance search. I'm more interested in fuzzy matching the way it's done for filenames in Sublime Text or VS Code.

A couple of years back I wrote a blog post about my attempts to reverse engineer sublime text's fuzzy match. It works quite well imho. https://blog.forrestthewoods.com/reverse-engineering-sublime-text-s-fuzzy-match-4cffeed33fdb

Different ranking formulas would be more appropriate for different inputs. I only attempted to craft one for things such as filenames and function names. I do not currently handle character swaps.

So, I also think fuzzy matching would be useful in ripgrep. :) I wonder if there is a path that could provide high value at lower cost? What if you didn't care about character swaps? Maybe that simplification doesn't help enough for it to be worthwhile. I don't know.

---

_Comment by @BurntSushi on 2018-09-14 13:23_

@forrestthewoods I don't have the bandwidth to do the leg work or the mentoring required to bring a major feature like this into ripgrep. Instead, I recommend that someone come up with a vision for how it works and go build it. ripgrep is itself split into libraries, so plenty of code reuse is possible.

---

_Comment by @ijt on 2019-10-05 16:09_

@Miserlou, I published a crate that could be a building block for something closer to what you had in mind: [github.com/ijt/trigram](https://github.com/ijt/trigram).

---

_Comment by @ijt on 2019-10-07 17:18_

I wrote a tool to do this based on trigrams: https://github.com/ijt/fum. You can try it with `cargo install fum` and then `fum pattern`.

---

_Comment by @Miserlou on 2019-10-07 17:20_

Issac! This is awesome!

---

_Comment by @ijt on 2019-10-07 17:22_

Thanks @Miserlou, it was fun to write. Can you tell me a bit more about the use cases that motivated you to start this issue? I think `fum` may have to be changed radically to make it performant, and it would help to understand what are the most important ways it will be used.

---

_Comment by @Miserlou on 2019-10-07 17:25_

Basically my use case is that I have a typo or I can't remember what I called a variable in my codebase, so if I `fum search` it will find where I wrote `saerch`, or `Search`. 

---

_Comment by @ijt on 2019-10-07 19:32_

Got it. As it stands, `fum` will not find `saerch` when queried with `search` since those have a similarity of 0.27 but the default threshold is 0.4. A threshold flag would help, but you'd have to know to change it. I suspect a more effective approach to the problem of typos in the code would be some sort of spell checker / fixer. For Rust code it would mostly be needed for comments, since the compiler would probably catch most typos in the code anyway, assuming the things being called or constructed don't already have typos in their names.

---

_Comment by @jcguu95 on 2020-03-17 12:28_

@ijt your `trigram` is cool! I'm also thinking about a searching tool that doesn't just search lines, as most of my plain-text notes are organized to have less than 72 characters per line. Your tool might be useful when I build it. Thank you!

---

_Comment by @taleinat on 2020-05-10 14:06_

Anyone thinking of working on this is welcome to take a look at the code in my https://github.com/taleinat/fuzzysearch repo, which does Levenshtein-distance based ad-hoc fuzzy searches. It's not a command line tool, but the algorithms could be easily re-used, though they are written in Python (with some optimizations in Cython and C).

If anyone wants to take a shot at this, I'd be happy to provide guidance and support!

---

_Comment by @moeabdol on 2022-05-04 03:01_

It is possible to interactivly switch between ripgrep and fzf during search. Two well respected tools that perfectly fit this scenario.
[ripgrep with fzf](https://github.com/junegunn/fzf/blob/master/ADVANCED.md#switching-between-ripgrep-mode-and-fzf-mode)

---

_Comment by @temberature on 2022-08-09 16:37_

> It is possible to interactivly switch between ripgrep and _fzf_ during search. Two well respected tools that perfectly fit this scenario. [ripgrep with _fzf_](https://github.com/junegunn/fzf/blob/master/ADVANCED.md#switching-between-ripgrep-mode-and-fzf-mode)

ripgrep and fzf are piped together, not as a unify function.

---

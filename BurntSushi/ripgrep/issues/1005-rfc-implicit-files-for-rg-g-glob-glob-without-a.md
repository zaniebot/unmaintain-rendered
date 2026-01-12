```yaml
number: 1005
title: "RFC: Implicit --files for rg -g, --glob GLOB without a pattern"
type: issue
state: closed
author: kiryph
labels:
  - question
assignees: []
created_at: 2018-08-07T06:57:23Z
updated_at: 2018-08-07T19:38:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1005
synced_at: 2026-01-12T16:13:22Z
```

# RFC: Implicit --files for rg -g, --glob GLOB without a pattern

---

_@kiryph_

Tools such as ack and ag allow you to search conveniently only for matching filenames with

```
$ ag -g PATTERN
$ ack -g PATTERN
```

Ripgrep provides the flag `-g` for globs. However, you have to add `--files` if you do not want to search inside the files for a pattern

```
$ rg --files -g GLOB
```

Ripgrep complains anyway about a missing pattern and could therefore assume if a user does not specify a pattern, he wants to use `--files`.

What do you think about this change? 


---

_Comment by @okdana on 2018-08-07 07:24_

That seems like it would introduce some UX weirdness, mainly because (since `rg` can't reliably tell the difference between `rg -g <glob> <path>` and `rg -g <glob> <pattern>`) it would only work when implicitly searching the current directory. If i were a random end-user i feel like i'd be really confused by that.

I do sometimes wish `rg` had a dedicated easy-mode option for this, like `ag`'s `-G`, but i guess a simple enough work-around is just to do like `alias rgf='rg --files'`.

---

_Comment by @kiryph on 2018-08-07 07:32_

I have overlooked that ripgrep allows for several globs. Adding `--` to terminate the list of globs could help. However, this means you are not far away of typing `--files`.

---

_Comment by @kiryph on 2018-08-07 07:43_

The implicit `--files` could be applied if there is a single glob and no path. I agree that these kind of special cases make the UX more complicated. However, cli tools are used by people who are willing to read a manpage and are accustomed to UX weirdness. And in this case I do not feel that this is completely unpredictable. Of course this should be documented at several places in the manpage:

1. Synopsis
  `rg [OPTIONS] -g GLOB`
2. option -g --glob:
 `If only a single GLOB and no PATH is given, the option --files is automatically set.`
3. option --files
  `This option is automatically set, if no PATH and the option -g with a single GLOB is given.`

---

_Comment by @BurntSushi on 2018-08-07 11:52_

It's a cute idea, but I'd rather not complicate the UX here even more than it already is. If this sort of thing is really that important to you, then I think my advice is the same as @okdana's: define an alias.

Interestingly, I personally rarely use `rg --files -g GLOB` but rather `rg --files | rg PATTERN`.

---

_Closed by @BurntSushi on 2018-08-07 11:52_

---

_Label `question` added by @BurntSushi on 2018-08-07 11:52_

---

_Comment by @kiryph on 2018-08-07 18:11_

I like the short input of ack and ag for searching for filenames and that they ignore by default `.git` directory.
No need to craft a `find . -name 'pattern'` with ignore options.

I'd also hoped that most feature differences between ack, ag, and rg, e.g. listed on https://beyondgrep.com/feature-comparison/, would vanish with time.

Anyhow, thanks for your consideration and thoughts. Keep up the good work!

---

_Comment by @BurntSushi on 2018-08-07 18:15_

That chart is pretty dated at this point, and is missing several entries for features that ripgrep does indeed have.

I didn't set out to build a tool that had everything under the sun, but as far as I'm concerned, ag/ack/ripgrep all provide the feature you're requesting here. The only difference is in how it's invoked. If you really do not like the additional letters, then I see no good reason why an alias isn't sufficient for you.

If you want something that's more focused on finding files rather than searching them, you might consider using [`fd`](https://github.com/sharkdp/fd), which uses the same ignore logic that ripgrep does by default.

---

_Comment by @kiryph on 2018-08-07 19:38_

I didn't know fd. Thanks for pointing it out. I will give it a try.

---

```yaml
number: 3102
title: "complete/zsh: improve --hyperlink-format completion"
type: pull_request
state: closed
author: okdana
labels:
  - rollup
assignees: []
base: master
head: dana/complete-hyperlink-format
created_at: 2025-07-13T23:47:38Z
updated_at: 2025-09-20T01:08:47Z
url: https://github.com/BurntSushi/ripgrep/pull/3102
synced_at: 2026-01-12T18:23:15Z
```

# complete/zsh: improve --hyperlink-format completion

---

_@okdana_

here's better completion for `--hyperlink-format`

three known annoyances:

* zsh doesn't perform brace expansion on single words, so users might be inclined to leave the variable braces unquoted, as in `--hyperlink-format=foo://{path}`. but i guess it generally assumes that it might and handles it at an earlier stage, which means our function doesn't even know the `{` is there. this causes unexpected behaviour. there is also a bug (i assume zsh's rather than mine but idk) where it re-inserts the `{` in a weird place, e.g. `\{ho{st\}`

* zsh converts the the argument in `--opt=arg` style options to the `\`-escaped (rather than quoted) form when completing it, which is not pleasant with the syntax used here and exacerbates the above issue

* if you have like `--hyperlink-format '{}`, press tab between the `{}`, and then press `}` after adding one of the matches, it won't swallow the extra `}`

off the top of my head idk if there's anything to be done about the first two. the last one can probably be fixed with `compadd -R` or sth. but tbh i got bored of the problem

if these limitations bother you i can simplify it so it only tries to complete the aliases i guess

i also made it so helper functions aren't re-defined if they exist. this is a convention of many completion functions that allows end-users to more easily define their own helpers if they want them to behave differently. i just forgot to do it before

---

_Comment by @ltrzesniewski on 2025-07-15 21:03_

We were discussing with @ilyagr about the removal of duplicate hyperlink alias names in #3096  ([here's my draft](https://github.com/BurntSushi/ripgrep/compare/3b7fd442a6f3aa73f650e763d7cbb902c03d700e...ltrzesniewski:ripgrep:41a7d17349b87555f3bc2adb6fe7dd5804ba7a80) of this idea if you're interested).

If this would end up being accepted, what would you think about adding a `!HYPERLINK_ALIAS_NAMES!` variable (or similar) to be replaced, just like `!ENCODINGS!` is today?


---

_Comment by @okdana on 2025-07-15 21:34_

oh. i was semi-conscious of the fact that i was getting e-mails about that, which is why i thought to do this. but i must've forgot about it already by the time i went to actually work on it, i never even looked at it

yes, that would be convenient

though i think, in contrast to the encodings, it makes sense for these to have descriptions for what they actually do. since they're auto-generated it could be as simple as the strings they expand to. so something like `file:file://{host}{path}` (or whatever delimiter)? if that's too cute just the names will work ofc, it's just not quite as helpful

then again the man page isn't explicit about them either so w/e

---

_Comment by @ltrzesniewski on 2025-07-15 22:07_

Either having the strings they expand to or adding a custom description to `HYPERLINK_PATTERN_ALIASES` would be possible. I think I'd also prefer having a description, especially since I like the ones you provided.

Lots of variants are possible here, and also for the main page.

---

_Comment by @ltrzesniewski on 2025-07-16 20:41_

I made #3103 to be used as a baseline, but didn't add descriptions yet since they wouldn't be used. Adding them won't be a problem if needed.


---

_Comment by @BurntSushi on 2025-09-06 18:41_

Thank you! I took this as-is, except I added descriptions to the new `HyperlinkAlias` and auto-generated that part of the completion script.

---

_Label `rollup` added by @BurntSushi on 2025-09-06 18:42_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

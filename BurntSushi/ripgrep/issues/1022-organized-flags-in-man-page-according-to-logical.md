```yaml
number: 1022
title: organized flags in man page according to logical section?
type: issue
state: closed
author: BurntSushi
labels:
  - question
assignees: []
created_at: 2018-08-20T19:49:22Z
updated_at: 2018-08-22T03:05:54Z
url: https://github.com/BurntSushi/ripgrep/issues/1022
synced_at: 2026-01-12T16:13:22Z
```

# organized flags in man page according to logical section?

---

_@BurntSushi_

The number of flags ripgrep has is truly staggering. A quick count appears to put it at `80`. Right now, these flags are listed alphabetically in the man page. I'm wondering whether it would be better to group them into logical sections.

For example, when PCRE2 support is enabled with the `--pcre2`, its Unicode support is enabled by default. ripgrep provides a flag, `--no-pcre2-unicode`, to disable PCRE2's Unicode support. However, the man page lists this as `--pcre2-unicode`, and states that it is enabled by default and can be disabled by using `--no-pcre2-unicode`. I did it this way so that the flag would be "discoverable" next to the `--pcre2` flag. But perhaps the better thing to do here is to just group flags.

I am somewhat inspired to do this based on ripgrep's zsh [completion script](https://github.com/BurntSushi/ripgrep/blob/master/complete/_rg), which defines its own groups. I'm not sure we want to make the groups in the man page as granular as that, but with the number of flags ripgrep has, it seems like a decent idea.

@okdana I'm especially interested to hear your opinion on this. :-)

---

_Label `question` added by @BurntSushi on 2018-08-20 19:49_

---

_Comment by @okdana on 2018-08-20 20:32_

Hmm.

I personally find it useful to be able to scan through man pages alphabetically when i don't know the name of an option or i just want to learn about the tool's feature set. The worst man page i can think of is the one for `rsync`, which like `rg` has a million options, but unlike `rg` there is no immediately discernible order to them in the manual. (Close inspection reveals that they're grouped roughly by functionality — e.g., all of the options related to symlinks and hard links are together — but there are way too many groups and they're not labelled.) Trying to browse through it is a nightmare, it's unreal.

Honestly, i feel a bit hypocritical even about the grouping in the zsh function, because one of my first complaints about the old one was that the options were arranged in an order that wasn't obvious to me. I fixed that in my first rewrite, but now it's ended up kind of back where it was. (e.g., there's no reason that anyone looking at that function should expect to find `--vimgrep` listed under `p`.) But it's functional, so we'll live with it i guess.

Anyway... what i'm getting at is that i think grouping them into sections would be an improvement **_if_** their number is kept fairly low. Like if we stuck to maybe five broad categories — 'output options', 'file options', 'pattern options'.... Something like that. I feel like that would accomplish the goal of grouping related functionality together without compromising browsability. Once you start to get loads of categories you have to think about how to order the categories themselves, precisely what names to give them, whether there's overlap amongst them, &c. It's not bad if it's just a handful.

---

_Comment by @BurntSushi on 2018-08-20 20:43_

Yeah, I hear ya. Those are good points. I think that's kind of why I went to the alphabetical strategy in the first place. I think the options used to be in groups ("common" and "less common" ring a bell), and I could never predict where a flag should be. I can see that happening with any number of groups unfortunately.

Maybe the right thing to do here is to leave the man page as is and (automatically generate) another doc listing the flags according to some grouping mechanism. Still not a huge fan of that either to be honest. Hmm.

---

_Comment by @okdana on 2018-08-20 21:29_

Some very complex tools like Perl and zsh break their documentation up into totally separate man pages, which i guess is kind of a similar idea, but i don't think `rg` is anywhere near complicated enough for that.

Another thing that some tools do is take an optional argument to `--help`, so if you did have a way to produce output both with and without categories, you could provide something like `--help=with-categories` or whatever. But honestly i suspect most people don't feel strongly about it either way. I doubt you'd get many complaints if you just changed it (within reason obv).

---

_Comment by @BurntSushi on 2018-08-21 15:22_

Thinking about this more, yeah, I think I want to keep the flag listing alphabetical. Another approach to solving my original problem is to simply list out "related" flags in certain places. e.g., The docs for `--pcre2` could mention `--no-pcre2-unicode` (presuming we switch to that as the canonical flag for consistency).

---

_Closed by @BurntSushi on 2018-08-22 03:05_

---

```yaml
number: 243
title: Should --column imply --line-number?
type: issue
state: closed
author: birkenfeld
labels:
  - bug
  - enhancement
  - question
assignees: []
created_at: 2016-11-21T19:02:27Z
updated_at: 2017-01-18T06:23:56Z
url: https://github.com/BurntSushi/ripgrep/issues/243
synced_at: 2026-01-12T16:13:21Z
```

# Should --column imply --line-number?

---

_@birkenfeld_

Otherwise, the output is quite hard to grasp if line numbers are not on by default (redirect to a file, for example).

---

_Comment by @BurntSushi on 2016-11-21 19:15_

Hmm... I get that the output is hard to grasp, *but that's what the user requested*.

grep doesn't have an analogous `column` flag, but it does have a `-b/--byte-offset` flag. Compare:

```
$ grep unencumbered UNLICENSE 
This is free and unencumbered software released into the public domain.
$ grep -b unencumbered UNLICENSE 
0:This is free and unencumbered software released into the public domain.
$ grep -n unencumbered UNLICENSE 
1:This is free and unencumbered software released into the public domain.
$ grep -nb unencumbered UNLICENSE 
1:0:This is free and unencumbered software released into the public domain.
```

I'm inclined to side with grep on this one.

---

_Comment by @birkenfeld on 2016-11-21 19:22_

Interesting, but looks like the two aren't really comparable since the byte offset is usable on its own. The column is quite literally unusable without knowing the line number.

Hate to bring up ag all the time, but it circumvents this by having the line number always by default, and making it opt-out by an option. What is the reasoning for making line numbering by default depend on isatty?

---

_Comment by @BurntSushi on 2016-11-21 19:30_

`rg` isn't just meant to supplant the silver searcher, it's also meant to supplant grep itself. Line numbers are typically what you want when browsing search results, but if you want to filter a file by content, then you probably don't want line numbers. The "atty" detection is meant to display human readable output. But as soon as you pipe it, ripgrep can't make any reasonable assumptions about what you want to do with the output, so it therefore does the safest most conservative thing possible: *do what grep does*.

I personally think making line numbers the default is off the table. I also think it's completely orthogonal to your specific issue here.

Having `--column` imply `--line-number` seems more likely. Your point is one I really hadn't considered. It does seem a bit tenuous to claim that `--column` is useful without `--line-number`. Certainly, I agree that it's useless if you're trying to use it in an automated process that's reading ripgrep's output. Maybe an end user wants the column number for some reason?

I'll noodle on it and see if I can come up with a better objection, but as of now, I think I'm on board. Note though that this is a breaking change, so it wouldn't get into ripgrep until the next minor version release.

---

_Label `bug` added by @BurntSushi on 2016-11-21 19:31_

---

_Label `enhancement` added by @BurntSushi on 2016-11-21 19:31_

---

_Label `question` added by @BurntSushi on 2016-11-21 19:31_

---

_Comment by @birkenfeld on 2016-11-21 19:33_

Thanks for considering it! Considering that a level of compatibility with `grep` is desired, I fully understand that

> I personally think making line numbers the default is off the table.

---

_Comment by @leeoniya on 2016-11-21 19:39_

@BurntSushi 

> Note though that this is a breaking change, so it wouldn't get into ripgrep until the next minor version release.

Off topic, and perhaps the semver movement has lulled me into a false sense of security, but why not simply adopt semver instead of relying on grepping or [meta] ripgrepping the changelog of each release for "breaking"? Is there some 1.0 milestone that just hasn't been reached yet or just a historical phobia of linux libs fearing numbers > 0?

---

_Comment by @BurntSushi on 2016-11-21 21:08_

> Considering that a level of compatibility with grep is desired

Just to nitpick on this (sorry for being a pedant!), I don't think it's necessarily the case the *compatibility* with grep itself is the goal, but that I do think it's the right default if one wants to use ripgrep in similar use cases as grep. For example, an operation like "I want file X, except only give me lines matching foo," is really common in pipelines. If ripgrep defaulted to showing line numbers, that wouldn't really work right, and would lead to surprising behavior for folks accustomed to grep.

There are also a lot of flags in ripgrep named specifically to match up with grep. This isn't so much because strict compatibility is desired, but that it's better to not introduce our own names for the same/similar operation.

(I'm only pointing this out because it's useful to have shared values on these things for future decisions.)

---

_Comment by @BurntSushi on 2016-11-21 21:15_

> Off topic, and perhaps the semver movement has lulled me into a false sense of security, but why not simply adopt semver instead of relying on grepping or [meta] ripgrepping the changelog of each release for "breaking"?

What makes you think ripgrep *doesn't* use semver? :P In fact, it does! Note that strictly speaking, in semver, pre-1.0 is "anything goes." I would be within semver, for example, to do a breaking change in a `0.3.1` release.

However, in the Rust ecosystem, we have adopted a *convention* (that Cargo knows about) that when a crate is pre-1.0, it should only introduce breaking changes in a *minor* release, so that patch releases shouldn't have breaking changes.

With all that said, just because we follow semver doesn't mean I want to rapidly produce breaking changes. I would rather be methodical about it and still try to bias against it where it makes sense. For example, once we hit 1.0, I'd like there to be a very strong bias against breaking changes. It would be so strong that I'd probably throw a suggestion like this out the window on that principle alone if we were post-1.0.

Semver says it's OK to make the breaking change so long as I release ripgrep 2. But if we do too much of that, people will get annoyed.

> Is there some 1.0 milestone that just hasn't been reached yet or just a historical phobia of linux libs fearing numbers > 0?

I'm not sure if you meant or not, but "phobia" implies it's an *irrational fear*, and I don't think there's anything irrational about not wanting to break stuff.

With respect to a 1.0 milestone... No, no such milestone exists. There are still some big things I'd want to do before even considering it:

1. Finish libripgrep.
2. Experiment and probably add support for additional text encodings.
3. Experiment and possibly add fulltext search (see #95).

1+2 might get rolled into one thing and 3 is a pretty giant can-of-worms, but its risk/reward ratio is quite tempting.

---

_Comment by @leeoniya on 2016-11-21 21:59_

> I'm not sure if you meant or not, but "phobia" implies it's an irrational fear, and I don't think there's anything irrational about not wanting to break stuff.

Phobia of version numbers > 0. Not a phobia of BC. I said this because every `apt-get update` pulls hundreds of libs that are widely deployed, many years old and with versions still < 1, like LXDE [1].

Yes, technically semver allows BC for any 0.x release. Which makes every release susceptible to BC. If there is a Rust/Cargo convention that's shifts the api stability guarantees over to the right, then okay...but I find this rather strange and un-intuitive.

Anyhow, this is not the place for that bikeshed, but I understand.

[1] https://en.wikipedia.org/wiki/LXDE

---

_Comment by @BurntSushi on 2016-11-21 22:22_

> Yes, technically semver allows BC for any 0.x release. Which makes every release susceptible to BC. If there is a Rust/Cargo convention that's shifts the api stability guarantees over to the right, then okay...but I find this rather strange and un-intuitive.

I think this is backwards? semver allows BC for any `0.x.y` release. The convention in Rust land is to only allow BC in any `0.x` release.

> Phobia of version numbers > 0. Not a phobia of BC. I said this because every apt-get update pulls hundreds of libs that are widely deployed, many years old and with versions still < 1, like LXDE [1].

I see. If ripgrep goes down the fulltext search route, then I'd expect that to significantly delay 1.0. If we don't go down that path, then a 1.0 could be maybe a year away or so.

---

_Label `breaking-change` added by @BurntSushi on 2016-11-23 12:45_

---

_Closed by @BurntSushi on 2017-01-11 23:53_

---

_Comment by @birkenfeld on 2017-01-18 06:23_

Thanks!

---

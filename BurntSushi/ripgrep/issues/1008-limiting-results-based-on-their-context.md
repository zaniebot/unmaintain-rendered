```yaml
number: 1008
title: Limiting results based on their context
type: issue
state: closed
author: thijsvandien
labels:
  - question
assignees: []
created_at: 2018-08-07T19:05:34Z
updated_at: 2018-09-21T19:25:31Z
url: https://github.com/BurntSushi/ripgrep/issues/1008
synced_at: 2026-01-12T16:13:22Z
```

# Limiting results based on their context

---

_@thijsvandien_

Frequently when grepping through code, I am searching for rather generic names. Whether a result is relevant depends on the context. I haven't found a good way to limit the results to those with some specific mentions in their context. Therefore I'm wondering if this kind of functionality would be a welcome addition to `rg`. Here's a simple example:

```
$ cat sherlock
For the Doctor Watsons of this world, as opposed to the Sherlock
Holmeses, success in the province of detective work must always
be, to a very large extent, the result of luck. Sherlock Holmes
can extract a clew from a wisp of straw or a flake of cigar ash;
but Doctor Watson has to have it taken out for him and dusted,
and exhibited clearly, with a label attached.

$ rg Doctor sherlock -C 1
1:For the Doctor Watsons of this world, as opposed to the Sherlock
2-Holmeses, success in the province of detective work must always
--
4-can extract a clew from a wisp of straw or a flake of cigar ash;
5:but Doctor Watson has to have it taken out for him and dusted,
6-and exhibited clearly, with a label attached.

$ rg Doctor sherlock -C 1 --in-context cigar
4-can extract a clew from a wisp of straw or a flake of cigar ash;
5:but Doctor Watson has to have it taken out for him and dusted,
6-and exhibited clearly, with a label attached.
```

Before getting into all the specifics of how it should behave under different circumstances, I'll wait for some feedback if something like this looks interesting at all.

---

_Comment by @BurntSushi on 2018-08-07 19:36_

I like the idea (I believe it has been suggested before in one manner or another), and I can see how some might find it useful, but I don't think it's a good fit for ripgrep. [sift](https://github.com/svent/sift) is an example of a tool that provides some support for this style of searching via its ["conditions" flags (scroll down a bit)](https://sift-tool.org/docs). You can see how the types of queries you might want to perform can get quite a bit more complicated than just "find a match in the context."

This also introduces a host of new possibilities for output options and introduces a second way to feed patterns into ripgrep, which increases the surface area of a feature like this even more.

I think there is probably a lot of experimentation that needs to be done to figure out how a feature like this should work. It is not something that I believe anyone can just sit down and design, and I don't really have the bandwidth or motivation to drive a feature like this forward.

Instead, I would rather that someone build a new tool that provides these types of contextual search features. This sounds like it might be hard to do, but libripgrep is getting very close to landing on ripgrep master. Once that's done, the biggest pieces of ripgrep core will be its argv handling. I won't say that building a ripgrep clone would be "simple," but should be easier by a couple orders of magnitude compared to rolling everything on your own. I believe libripgrep is itself flexible enough to accommodate searching like this, and it can even be done without needing to roll your own support for searching and finding the contextual lines in the first place. But, that's just instinctual. I don't know for sure.

Finally, you can often achieve something similar through some clever use of shell pipelines. For example, instead of `rg Doctor sherlock -C 1 --in-context cigar`, I might do:

```
$ rg Doctor sherlock -C1 | rg cigar -A2
can extract a clew from a wisp of straw or a flake of cigar ash;
but Doctor Watson has to have it taken out for him and dusted,
and exhibited clearly, with a label attached.
```

Which, yeah, isn't great. And it's pretty easy to hit the limits of this type of work around pretty easily compared to a dedicated tool for searching in contexts.

---

_Label `question` added by @BurntSushi on 2018-08-07 19:36_

---

_Comment by @leeoniya on 2018-08-07 23:54_

this reminds me of Altavista's `NEAR` syntax [1]. RIP :(

[1] http://jkorpela.fi/altavista/

---

_Comment by @BurntSushi on 2018-08-19 14:00_

I'm going to close this out since I'd rather see other folks experiment with features like this in other programs.

---

_Closed by @BurntSushi on 2018-08-19 14:00_

---

_Comment by @BatmanAoD on 2018-09-21 19:25_

Sift looks very cool; thanks for linking to it!

---

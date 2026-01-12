```yaml
number: 571
title: Regex feature expansion?
type: issue
state: closed
author: voider1
labels: []
assignees: []
created_at: 2017-08-07T02:36:54Z
updated_at: 2017-08-09T11:26:26Z
url: https://github.com/BurntSushi/ripgrep/issues/571
synced_at: 2026-01-12T16:13:22Z
```

# Regex feature expansion?

---

_@voider1_

I've read the part about regex in the README, is there no way you would consider adding more regex syntax to ripgrep? I really like it, and I'd love to use it, but I use a lot of regex. Things like:

```regex
rg -tpy 'def (?=[A-Z])[a-zA-Z\-_]+\(.+\):'`
```

don't even work and forces me to write things like:

```regex
rg -tpy '[a-zA-Z\-_]+\(.+\):' | ggrep -P 'def (?=[A-Z])'
```

I might as well just use (GNU) grep, which is quite a shame. I really enjoy ripgrep's sane defaults and usability.

---

_Comment by @okdana on 2017-08-07 02:53_

The `regex` crate is based on / inspired by [RE2](https://en.wikipedia.org/wiki/RE2_(software)), which is deliberately designed to avoid back-tracking for safety and edge-case-performance reasons; as a consequence it doesn't support look-around features like this.

I can't figure out why you need look-ahead for the case you've provided, but anyway — [`ag`](https://github.com/ggreer/the_silver_searcher) uses PCRE, which does support these features, if that's something that's critically important to you.

---

_Comment by @BurntSushi on 2017-08-07 03:04_

Thanks @okdana, that covered it. :)

Another tool that supports fancier features is Universal Code Grep, which uses PCRE2: https://github.com/gvansickle/ucg

---

_Closed by @BurntSushi on 2017-08-07 03:04_

---

_Comment by @BurntSushi on 2017-08-07 03:07_

And yes, I also can't figure out why you need look around for the given example. Your broader point is reasonably accurate though. Sometimes the lack of lookaround forces you to use a pipeline, but that should eliminate the lookaround, unlike in your example.

---

_Comment by @okdana on 2017-08-08 23:41_

Unless i'm misunderstanding, it seems like you should just be able to do `def [A-Z][A-Za-z_-]+` or w/e

---

_Comment by @BurntSushi on 2017-08-08 23:41_

(Looks like @voider1 deleted their comment, but yeah, @okdana that's what I was going to say too! You're quick. :P)

---

_Comment by @voider1 on 2017-08-09 11:26_

@BurntSushi I posted the comment and then realized I could do that too.

---

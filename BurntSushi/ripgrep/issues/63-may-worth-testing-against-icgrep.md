```yaml
number: 63
title: May worth testing against icGrep?
type: issue
state: closed
author: upsuper
labels: []
assignees: []
created_at: 2016-09-24T13:31:54Z
updated_at: 2016-09-27T17:03:23Z
url: https://github.com/BurntSushi/ripgrep/issues/63
synced_at: 2026-01-12T18:23:11Z
```

# May worth testing against icGrep?

---

_@upsuper_

[icGrep](http://parabix.costar.sfu.ca/wiki/ICgrep) is announced to be a "full-featured grep program with [world-beating performance](http://parabix.costar.sfu.ca/wiki/GigabytePerSecondGrep)" from academia. It uses a different idea (parabix) for parallelizing.

If parabix is proven to be a good idea, probably Rust regex can consider adopting that algorithm?


---

_Comment by @BurntSushi on 2016-09-24 15:29_

It's probably worth it. Certainly, from the description, it looks promising. (I have seen it before.)

The build appears nightmarish (it requires compiling `llvm`), but I kicked one off.

FWIW, if their claims of 1 GB/s are true, then that's roughly twice the speed of Rust's _regex_ engine. (Of course, a huge part of a regex engine is its literal optimizations, and that often propels it to several GB/s, so if `icgrep` isn't doing that---and from their description it seems like they aren't---then they're missing a really common usage scenario.)

Anyway, I'll check it out.

> If parabix is proven to be a good idea, probably Rust regex can consider adopting that algorithm?

We are always considering new algorithms and there is always room for improvement. The list is probably a mile long.

The other tool in this space is [Hyperscan](https://github.com/01org/hyperscan) which is also _super_ fast, but it is geared more towards deep packet inspection and matching massive lists of regexes. (By design, it has slightly different match semantics then you might expect from a grep tool.)


---

_Comment by @BurntSushi on 2016-09-24 22:50_

OK, I got `icgrep` running. Here's a good example of what I meant by missing the literal optimizations:

```
$ time rg '\w+\s+Holmes\s+\w+' OpenSubtitles2016.raw.sample.en | wc -l
317

real    0m0.255s
user    0m0.190s
sys     0m0.063s
$ time icgrep '\w+\s+Holmes\s+\w+' OpenSubtitles2016.raw.sample.en | wc -l
317

real    0m4.406s
user    0m4.350s
sys     0m0.053s
```

If we try the `\p{Greek}` pattern, we can see where `icgrep` beats `ripgrep` (which is impressive!):

```
$ time rg '\p{Greek}' OpenSubtitles2016.raw.sample.en | wc -l
13426

real    0m1.757s
user    0m1.713s
sys     0m0.043s
$ time icgrep '\p{Greek}' OpenSubtitles2016.raw.sample.en | wc -l
13426

real    0m1.414s
user    0m1.320s
sys     0m0.093s
```

But it doesn't get case insensitivity right:

```
$ time rg -i '\p{Greek}' OpenSubtitles2016.raw.sample.en | wc -l
13671

real    0m1.838s
user    0m1.790s
sys     0m0.047s
$ time icgrep -i '\p{Greek}' OpenSubtitles2016.raw.sample.en | wc -l
13426

real    0m1.422s
user    0m1.380s
sys     0m0.040s
```

Also, `icgrep`'s regex engine doesn't seem to be always faster than Rust's:

```
$ time rg '\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}' OpenSubtitles2016.raw.sample.en | wc -l
13

real    0m2.048s
user    0m1.997s
sys     0m0.050s
$ time icgrep '\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}' OpenSubtitles2016.raw.sample.en | wc -l
13

real    0m4.982s
user    0m4.930s
sys     0m0.050s
```

And `icgrep` appears to pay a steep penalty when searching on Cyrllic. `ripgrep` slows down a bit too, but is still much faster: (It is nice to see that `icgrep` gets Unicode right!)

```
$ time rg '\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}' OpenSubtitles2016.raw.ru | wc -l
41

real    0m3.419s
user    0m3.333s
sys     0m0.083s
$ time icgrep '\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}\s+\w{5}' OpenSubtitles2016.raw.ru | wc -l
41

real    0m9.598s
user    0m9.517s
sys     0m0.077s
```

I think I should do more investigation into `icgrep`, since it sounds like they're doing some really cool stuff. It does look like they need to handle some of the (comparatively) simpler optimizations before they can compete in more common usage scenarios.


---

_Closed by @BurntSushi on 2016-09-24 22:50_

---

_Comment by @upsuper on 2016-09-24 23:05_

It is said that the compilation of regex is expensive for icgrep, so it would only shine when the data being greped is large enough. (I heard the users of their algorithm currently are mainly firewall and big data.)


---

_Comment by @BurntSushi on 2016-09-24 23:29_

@upsuper Did you read my blog post? I benchmarked it on the same data used in the blog. `OpenSubtitles2016.raw.sample.en` is 918MB and `OpenSubtitles2016.raw.ru` is 1.6GB.

But yes, Hyperscan is also primarily used for DPI.


---

_Comment by @BurntSushi on 2016-09-27 17:03_

See also this exchange between myself and one of the authors of icgrep: https://news.ycombinator.com/item?id=12586928


---

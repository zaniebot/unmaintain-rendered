```yaml
number: 11
title: Incorporated faster line count
type: pull_request
state: closed
author: llogiq
labels: []
assignees: []
base: master
head: fast_linecount
created_at: 2016-09-23T03:00:07Z
updated_at: 2016-11-06T05:34:18Z
url: https://github.com/BurntSushi/ripgrep/pull/11
synced_at: 2026-01-12T18:23:12Z
```

# Incorporated faster line count

---

_@llogiq_

Thanks for the hint; here's the faster linecount for your ripgrep utility.


---

_Comment by @llogiq on 2016-09-23 16:53_

See [newlinebench](/llogiq/newlinebench) for more information.


---

_Comment by @Byron on 2016-09-25 10:21_

I am so looking forward to see this PR land, just because line counts are enabled by default and are something people do expect to see anyway. Looking at the benchmarks on @BurntSushi 's blog, being faster at this will be absolutely noticeable !


---

_Comment by @BurntSushi on 2016-09-25 12:53_

It will likely only be noticeable when searching very large files.

But yes, I believe @llogiq is working on putting this in a crate since there have been other developments. I would also like to understand the algorithm and audit it for safety before merging, so it might be a bit. Hang tight.


---

_Comment by @Byron on 2016-09-25 13:08_

Sounds very reasonable ! I will happily use the fastest and best 'out-of-the' best experience then, to soon be able to tout it just became a little bit faster :).


---

_Comment by @llogiq on 2016-09-25 13:14_

Yeah, I want to strip down the use of `unsafe`, put that in its own crate, document the bit twiddling and the rationale better and then I'll update.

Of course you can pull this now (as it'll already be faster), and I'll push a new PR when it's ready.


---

_@BurntSushi reviewed on 2016-09-27 11:03_

---

_Review comment by @BurntSushi on `Cargo.toml`:1 on 2016-09-27 11:03_

I don't understand why you rewrote my Cargo.toml? Could you please not do that? (There are several stylistic choices that I don't like.)


---

_Comment by @BurntSushi on 2016-09-27 11:06_

@llogiq Thanks! I have a nit about changes to `Cargo.toml`.

Other than that, I need to audit `bytecount` before I can merge this. I'm not sure when I'll get to that. Maybe this weekend? It would help if there was an explanation of how it works. cc @Veedrac


---

_Comment by @Veedrac on 2016-09-27 18:14_

@BurntSushi I've got [a new version](https://github.com/llogiq/bytecount/pull/2) that should be easier to audit.

The basic idea is that the function `vector_not` turns any null bytes in a `usize` into `1`s and any others into `0`s. I go along four `usize`s at a time, adding these into a `counts` array. Every 255 quadruplets, I flush the bytewise counts into the total count to prevent any byte from overflowing.


---

_Comment by @llogiq on 2016-09-27 18:34_

I have a somewhat thorough explanation on [my blog](https://llogiq.github.com/2016/09/27/count.html).


---

_Comment by @llogiq on 2016-10-09 21:25_

Any updates on this?


---

_Comment by @BurntSushi on 2016-10-09 22:10_

No.

On Oct 9, 2016 5:25 PM, "llogiq" notifications@github.com wrote:

> Any updates on this?
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/pull/11#issuecomment-252513709,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34gzTxlpTX36PrUZNFnmCevI73D8aks5qyVvkgaJpZM4KEkOo
> .


---

_Comment by @Veedrac on 2016-10-10 13:32_

FWIW, a newer version has SIMD support, which is a lot faster.


---

_Comment by @BurntSushi on 2016-11-06 02:30_

I merged this in 4368913d8fc174cf21fcb9d04b24136ceb87a393 with a squash and a regression test for #128. Sorry it took so long @llogiq but thank you!


---

_Closed by @BurntSushi on 2016-11-06 02:39_

---

_Comment by @BurntSushi on 2016-11-06 02:40_

This did wind up with quite a nice speed up! Comparing old master (`rg-master`) with `rg` using `avx-accel`.

First, a base line with no line counting:

```
[andrew@Cheetah subtitles] time rg-master 'Sherlock Holmes' OpenSubtitles2016.raw.en | wc -l
5107

real    0m1.578s
user    0m1.213s
sys     0m0.363s
[andrew@Cheetah subtitles] time rg 'Sherlock Holmes' OpenSubtitles2016.raw.en | wc -l
5107

real    0m1.533s
user    0m1.140s
sys     0m0.390s
```

And now with line counting:

```
[andrew@Cheetah subtitles] time rg-master -n 'Sherlock Holmes' OpenSubtitles2016.raw.en | wc -l
5107

real    0m4.260s
user    0m3.867s
sys     0m0.397s
[andrew@Cheetah subtitles] time rg -n 'Sherlock Holmes' OpenSubtitles2016.raw.en | wc -l
5107

real    0m2.131s
user    0m1.810s
sys     0m0.323s
```

Twice as fast!


---

_Comment by @llogiq on 2016-11-06 05:34_

Cool!


---

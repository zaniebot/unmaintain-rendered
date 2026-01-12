```yaml
number: 680
title: Unexpected behavior with --dfa-size-limit 0
type: issue
state: closed
author: pevalme
labels: []
assignees: []
created_at: 2017-11-15T11:13:43Z
updated_at: 2017-11-15T14:42:08Z
url: https://github.com/BurntSushi/ripgrep/issues/680
synced_at: 2026-01-12T16:13:22Z
```

# Unexpected behavior with --dfa-size-limit 0

---

_@pevalme_

I want to count the matches of the regular expression `[a-z]{4}` on [this file](https://drive.google.com/file/d/0B3oOQ14-tellNzhlU0tKT2xFSW8/view), which contains some English text, using ripgrep with `--dfa-size-limit 0`.

With this setup, ripgrep is much faster when receiving the input file through a pipe than when reading it by itself.
```
time rg -c --dfa-size-limit 0 "[a-z]{4}" tmp.txt → 4,76s user 0,00s system 99% cpu 4,770 total
time (cat tmp.txt | rg -c --dfa-size-limit 0 "[a-z]{4}") → 2,99s user 0,11s system 102% cpu 3,011 total
```
Given that `cat | rg` executes both commands in parallel I would expect it to require, at least, the time required by the slowest of both commands.

This *unexpected* behavior occurs only for really small values of `--dfa-size-limit`, up to `1K`.
```
time rg -c --dfa-size-limit 1K "[a-z]{4}" tmp.txt → 4,73s  user 0,02s system 99% cpu 4,751 total
time (cat tmp.txt | rg -c --dfa-size-limit 1K "[a-z]{4}") → 2,98s  user 0,12s system 103% cpu 3,010 total
```
```
time rg --dfa-size-limit 2K -c "[a-z]{4}" tmp.txt →0,39s user 0,01s system 99% cpu 0,401 total
time (cat tmp.txt | rg --dfa-size-limit 2K -c "[a-z]{4}") →0,42s user 0,08s system 111% cpu 0,455 total
```
```
time rg -c "[a-z]{4}" tmp.txt →0,40s user 0,00s system 99% cpu 0,407 total
time (cat tmp.txt | rg -c "[a-z]{4}") →0,43s user 0,08s system 111% cpu 0,460 total
```
**Why does ripgrep exhibit such behavior?**

---

_Comment by @BurntSushi on 2017-11-15 12:36_

First of all, why are you lowering `--dfa-size-limit` all the way down to `0`? Is there a specific problem you're trying to solve, or are you just curious?

Secondly, the key differentiator here isn't whether it's a pipe or not, but whether ripgrep is using memory maps or not. Your first command uses a memory map but your second does not. If you force your first command to not use memory maps, then you get a similar speed up. You can do that by passing the `--no-mmap` flag.

Thirdly, when you reduce `--dfa-size-limit` all the way down to zero, then you wind up forcing ripgrep to use its (much) slower regex engine. Namely, if the DFA has no memory available to it, then it can't be used effectively. The point of this flag is to make it possible to *increase* the limit, not *decrease* it. :-)

With that said, why is there a performance difference here? Good question. My guess is that whether you use memory maps or not results in yet another split choice between regex engines. Namely, if both your regex and your input are sufficiently small enough, then a bounded backtracking algorithm is used. When all else fails, a full simulation of the NFA is used (the so-called "Pike VM"). The bounded backtracker is faster, but because it's bounded, it needs to store memory proportional to the size of the input times the size of the regex, which means we only run it when that product is small.

Why does mmap/no-mmap cause this split? Because when we don't use memory maps, we use the incremental searcher, which reads your input in [chunks of 8KB](https://github.com/BurntSushi/ripgrep/blob/d2a3b61220dc2d17c92dbb6b073776fd1223af66/src/search_stream.rs#L20-L21). Combine that with the fact that your regex is exceptionally small (6 VM instructions), then you have an overall space requirement of about 6KB for the bounded backtracker (`6 * 8000 / 8` since we only need one bit for each possible state). 6KB is less than the [256KB limit for the bounded backtracker](https://github.com/rust-lang/regex/blob/37bfbd9d96676297372713dfe4a60afc5f6966a8/src/backtrack.rs#L37), so the backtracker is used.

We can test this by increasing the size of the regex. If the regex is big enough, then it should always force the NFA simulation, and therefore, result in consistent speeds across both forms of search (with memory map and without memory map). Here's one such example:

```
$ time rg --regex-size-limit 1G --dfa-size-limit 0 --no-mmap -c '\pL{1000}' /tmp/Opensubtitles100MB.txt 

real    0m10.862s
user    0m10.794s
sys     0m0.065s
$ time rg --regex-size-limit 1G --dfa-size-limit 0 --mmap -c '\pL{1000}' /tmp/Opensubtitles100MB.txt 

real    0m10.405s
user    0m10.348s
sys     0m0.055s
```

There's still a small difference where the memory map version is faster, but this seems somewhat expected since memory maps likely have less overhead for a single large file than the incremental reader.

Another interesting note is that in my previous test case, `--dfa-size-limit` is actually optional. The regex is so big that it blows the default DFA cache size anyway, thus resulting in a similar effect as setting it to `0`. If you bump the size limit way up, then ripgrep becomes fast again.

TL;DR - regex engines are complicated. I wrote the damn thing and it took me a bit of time to figure this out. :-)

---

_Comment by @pevalme on 2017-11-15 13:40_

That's a really good explanation!

With respect to the first part of your comment, I reduced `--dfa-size-limit` to `0` for research purposes. I just wanted to observe the behavior of ripgrep when working by simulating the NFA without any sort of determinization. Within this scenario, I have one more question.

As I understand, the regex engine knows that the bounded backtracking algorithm requires too much memory and, therefore, it can only be used for *small pairs* `(input_regex, input_file)`. Then, **why does the algorithm aims to *handle* the whole file at once rather than splitting it?**
**Is there a particular scenario where Pike VM works better than incremental bounded backtracking?**

I guess that when using back-references the bounded backtracking algorithm will cause some trouble but let me know if I'm wrong or there is something else I'm missing.

---

_Comment by @BurntSushi on 2017-11-15 13:48_

> Then, why does the algorithm aims to handle the whole file at once rather than splitting it?

I don't think I understand your question. Could you please rephrase it without using the words "it" and "the algorithm"? Please consider the decision on which regex engine to use and which file searching strategy to use are largely decoupled.

> Is there a particular scenario where Pike VM works better than incremental bounded backtracking?

I don't know. My guess is no (other than large regex/input pairs, or pairs that result in a full exploration of the state space, e.g., lots of backtracking).

> I guess that when using back-references the bounded backtracking algorithm will cause some trouble but let me know if I'm wrong or there is something else I'm missing.

ripgrep's regex engine doesn't support back-references. Back-references and bounded backtracking are incompatible. (This is a deep statement, in the sense that if you implemented back-references using bounded backtracking, then I think you could construct a proof that P=NP.)

---

_Comment by @pevalme on 2017-11-15 14:16_

> I don't think I understand your question.

When the regex engine decides that it cannot use bounded backtracking because the 256KB bound is reached, why does it fall down to Pike VM? 

If bounded backtracking cannot be used but the regex size is lower than 32, wouldn't it be more efficient to perform incremental searching, since `32*8KB/1000 = 256KB`, and bounded backtracking (even if the whole file is already loaded due to mmap)? At least, this would improve the performance of my first experiments:
```
rg -c --dfa-size-limit 0 "[a-z]{4}" tmp.txt
```

---

_Comment by @BurntSushi on 2017-11-15 14:31_

I see. Could you explain why my note

> Please consider the decision on which regex engine to use and which file searching strategy to use are largely decoupled.

doesn't clarify this for you? Or are you asking: "why not couple them?"

To a first approximation, optimizing this case isn't worth anyone's time. I'm fine with exploring this as an academic exercise, but crossing over into proposing actual changes requires more than curiosity. We need use cases that justify the complications to the code. This in particular would either require a heuristic where ripgrep "guesses" at what the regex engine will do, or would require deeper coupling that simply isn't (and likely won't) be exposed by the regex engine. Remember, ripgrep's regex engine is [just Rust's regex crate](https://crates.io/crates/regex).

---

_Comment by @pevalme on 2017-11-15 14:41_

My bad, I misunderstood your note. 
I just wanted to satisfy my curiosity, not suggest any change.

Thank you for the explanations! ;)

---

_Closed by @pevalme on 2017-11-15 14:41_

---

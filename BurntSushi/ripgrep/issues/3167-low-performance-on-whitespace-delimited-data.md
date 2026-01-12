```yaml
number: 3167
title: Low performance on whitespace-delimited data
type: issue
state: closed
author: LarsHadidi
labels:
  - wontfix
assignees: []
created_at: 2025-10-04T15:53:18Z
updated_at: 2025-10-12T15:07:42Z
url: https://github.com/BurntSushi/ripgrep/issues/3167
synced_at: 2026-01-12T16:13:25Z
```

# Low performance on whitespace-delimited data

---

_@LarsHadidi_

**Ripgrep** demonstrates performance comparable to that of **ugrep**, but when applied to whitespace-delimited data such as MPS files, its performance appears to be several times slower.

### Steps to reproduce:

1. Build ugrep from source:
`./build.sh`
2. Build ripgrep from source: 
`RUSTFLAGS="-Ctarget-cpu=native" cargo build --release --features 'pcre2'`
3. Download and extract textfile:
https://miplib.zib.de/WebData/instances/hawaiiv10-130.mps.gz
4. Benchmark ugrep:
`hyperfine 'ug --mmap -n -P \'^\w+$\' hawaiiv10-130.mps > out'`
5. Benchmark ripgrep:
`hyperfine 'rg --mmap -n \'^\w+$\' hawaiiv10-130.mps > out'`

### My results:

<img width="863" height="238" alt="Image" src="https://github.com/user-attachments/assets/2907ba27-4e45-486d-8b96-13f2923a4288" />

---

_Comment by @BurntSushi on 2025-10-07 00:42_

Nice find! Note though that you're using PCRE2 with ugrep and not with ripgrep. Not really sure why to be honest, given that the regex you're using is pretty basic and not specific to any one engine. PCRE2 in particular seems to optimize this particular regex quite well. However, ripgrep and ugrep seem to use PCRE2 differently. You can make ripgrep have ugrep's speed here by using PCRE2 and enabling multi-line support:

```
$ hyperfine --output pipe \
    "rg -c '^\w+$' hawaiiv10-130-small.mps" \
    "rg -c --no-unicode '^\w+$' hawaiiv10-130-small.mps" \
    "rg -c -P '^\w+$' hawaiiv10-130-small.mps" \
    "rg -c -P -U '^\w+$' hawaiiv10-130-small.mps" \
    "ugrep-7.5.0 -c '^\w+$' hawaiiv10-130-small.mps" \
    "ugrep-7.5.0 -c -P '^\w+$' hawaiiv10-130-small.mps"
Benchmark 1: rg -c '^\w+$' hawaiiv10-130-small.mps
  Time (mean ± σ):      1.022 s ±  0.006 s    [User: 1.004 s, System: 0.017 s]
  Range (min … max):    1.014 s …  1.031 s    10 runs

Benchmark 2: rg -c --no-unicode '^\w+$' hawaiiv10-130-small.mps
  Time (mean ± σ):     288.9 ms ±   2.7 ms    [User: 270.9 ms, System: 17.4 ms]
  Range (min … max):   285.2 ms … 292.1 ms    10 runs

Benchmark 3: rg -c -P '^\w+$' hawaiiv10-130-small.mps
  Time (mean ± σ):     415.3 ms ±   7.8 ms    [User: 397.1 ms, System: 17.4 ms]
  Range (min … max):   397.4 ms … 426.8 ms    10 runs

Benchmark 4: rg -c -P -U '^\w+$' hawaiiv10-130-small.mps
  Time (mean ± σ):     127.1 ms ±  17.2 ms    [User: 108.3 ms, System: 18.5 ms]
  Range (min … max):   104.6 ms … 150.4 ms    22 runs

Benchmark 5: ugrep-7.5.0 -c '^\w+$' hawaiiv10-130-small.mps
  Time (mean ± σ):     415.8 ms ±   6.1 ms    [User: 361.0 ms, System: 54.1 ms]
  Range (min … max):   410.7 ms … 429.9 ms    10 runs

Benchmark 6: ugrep-7.5.0 -c -P '^\w+$' hawaiiv10-130-small.mps
  Time (mean ± σ):     171.1 ms ±   5.0 ms    [User: 118.8 ms, System: 51.9 ms]
  Range (min … max):   155.6 ms … 179.5 ms    17 runs

Summary
  rg -c -P -U '^\w+$' hawaiiv10-130-small.mps ran
    1.35 ± 0.19 times faster than ugrep-7.5.0 -c -P '^\w+$' hawaiiv10-130-small.mps
    2.27 ± 0.31 times faster than rg -c --no-unicode '^\w+$' hawaiiv10-130-small.mps
    3.27 ± 0.45 times faster than rg -c -P '^\w+$' hawaiiv10-130-small.mps
    3.27 ± 0.45 times faster than ugrep-7.5.0 -c '^\w+$' hawaiiv10-130-small.mps
    8.05 ± 1.09 times faster than rg -c '^\w+$' hawaiiv10-130-small.mps
```

I also added a run above with ripgrep's default regex engine that disables Unicode. In particular, when Unicode is disabled, the `regex` crate is able to optimize the pattern a little differently than it can with Unicode mode enabled.

Aside from making the `regex` crate do whatever PCRE2 is doing for this regex, the main issue here I think is why you need to enable multi-line mode to get the most out of PCRE2 here in ripgrep but not for ugrep. IIRC, the reason for this is that with multi-line mode disabled, ripgrep falls back to its slow line-at-a-time search since it cannot be sure that PCRE2 won't match across a line. In particular, the fast line search works by feeding the regex engine a bunch of lines all at once without parsing out each individual line. This works _especially_ well when there are few matches relative to the haystack size, as is the case here.

The `regex` crate exposes some lower level APIs that permit analysis of the regex to make inferences about what it's allowed to match. Or even _change_ the pattern to make it impossible for it to match through a line terminator by construction. PCRE2 doesn't allow this, so I'm not sure what ugrep is doing here exactly. In particular, ugrep does seem to avoid having PCRE2 match through a line terminator in at least this basic case:

```
$ cat simple-test
abc
xyz
$ ugrep-7.5.0 -P '\w+\s+\w+' simple-test
$
```

Buuuuuuuuuuuut... if I change the pattern up a little bit......

```
$ ugrep-7.5.0 -P '\w+[\x09-\x20]+\w+' simple-test
abc
xyz
```

I don't know if this is intended behavior on ugrep's part. In contrast, ripgrep preserves line oriented matching semantics in all cases unless you specifically opt out of it:

```
$ rg -P '\w+[\x09-\x20]+\w+' simple-test
$ rg -P -U '\w+[\x09-\x20]+\w+' simple-test
1:abc
2:xyz
```

Since ripgrep is more conservative about applying this sort of optimization, it's slower in your specific case but correct in other cases.

With that said, ugrep's default regex engine seems to have similar behavior here:

```
$ ugrep-7.5.0 '\w+[\x09-\x20]+\w+' simple-test
abc
xyz
$ ugrep-7.5.0 '\w+\s+\w+' simple-test
```

OK, looking at ugrep's README, I see this:

> full Unicode extended regex pattern syntax with multi-line pattern matching without any special command-line options

And [this](https://github.com/Genivia/ugrep?tab=readme-ov-file#matching-multiple-lines-of-text).

This seems weird to me especially because of `\s`, where since ugrep has no explicit opt-in, I guess it always has to remove `\n` from the `\s` character class? Let's try some other examples:

```
$ ugrep-7.5.0 '\w+[[:space:]]+\w+' simple-test
$
```

So that means `\n` is removed from `[[:space:]]` as well. And this seems to happen even if we attempt to match a `\n` elsewhere in the pattern:

```
$ ugrep-7.5.0 '\w+\n?[[:space:]]+\w+' simple-test
$
```

Is `\n` removed from `\W` or other character class negations?

```
$ ugrep-7.5.0 '\w+[^a-z]+\w+' simple-test
$ ugrep-7.5.0 '\w+\W+\w+' simple-test
abc
xyz
$ rg '\w+\W+\w+' simple-test
$ rg '\w+[^a-z]+\w+' simple-test
$
```

... so, sometimes yes, sometimes no. Odd. I tried looking in ugrep's source to see how this is implemented, but couldn't find it after 5 minutes of searching (my C++ is only barely passable).

So from what I can tell, ugrep is generally behaving as it is described on the tin. By making multi-line searching effectively always enabled by virtue of what the pattern matches (with some changes to the definitions of classes like `\s` to presumably behave as most might expect in _common_ cases), ugrep can run PCRE2 without fear on big batches of lines. ripgrep can do the same, but it requires you to opt into multi-line mode.

Since I have no plans of adopting ugrep's mutli-line behavior here (I _like_ that there is a special opt-in flag for it), I think this performance difference between `rg -P` and `ugrep -P` is probably working as intended. As for the perf difference between the `regex` crate and PCRE2, I created https://github.com/rust-lang/regex/issues/1301 to track that.

---

_Closed by @BurntSushi on 2025-10-07 00:42_

---

_Label `wontfix` added by @BurntSushi on 2025-10-07 00:42_

---

_Comment by @LarsHadidi on 2025-10-12 15:07_

Thanks a lot for the thorough and insightful explanation. It was engaging and quite educational to follow your deep dive into how ripgrep and ugrep handle PCRE2 differently and why multiline mode affects performance. 

Knowing the right combination of flags makes ripgrep the fastest one.
I really appreciate the time and effort you put into this detailed investigation.

---

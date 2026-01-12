```yaml
number: 2819
title: "ripgrep is slower than LTREP matching `[A-Z][A-Z]+`"
type: issue
state: closed
author: Bricktech2000
labels:
  - question
assignees: []
created_at: 2024-05-26T07:57:10Z
updated_at: 2024-05-26T21:36:12Z
url: https://github.com/BurntSushi/ripgrep/issues/2819
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep is slower than LTREP matching `[A-Z][A-Z]+`

---

_@Bricktech2000_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.0 (rev 35160a1cdb)

features:-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.
```

### How did you install ripgrep?

From source.

### What operating system are you using ripgrep on?

NixOS 23.05 x86_64

### Describe your bug.

ripgrep is slower than LTREP (a wrapper around my toy regex engine [LTRE](https://github.com/Bricktech2000/LTRE)) when matching for `[A-Z][A-Z]+`. LTRE has no literal optimizations and so always takes 1.3s to match against enwik9 on my machine. ripgrep is faster than LTRE until `[A-J][A-J]+` (takes 0.6s against enwik9) but becomes slower from `[A-K][A-K]+` onwards (takes 1.5s against enwik9).

### What are the steps to reproduce the behavior?

Download then unzip enwik9 from <https://mattmahoney.net/dc/textdata.html>.

```bash
git clone https://github.com/Bricktech2000/LTRE
cd LTRE/
git checkout 1297041
make ltrep # -O2 on GCC 12.2.0
time bin/ltrep -c '[A-Z][A-Z]+' enwik9
# 1212981
# real	0m1.309s
# user	0m1.275s
# sys	0m0.034s
```

```bash
git clone https://github.com/BurntSushi/ripgrep
cd ripgrep
git checkout 35160a1
cargo build --release
time target/release/rg --debug -c '[A-Z][A-Z]+' enwik9
# rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
# rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 1
# rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? true
# rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: nixos
# rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
# rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
# rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
# 1212981
# real	0m1.529s
# user	0m1.495s
# sys	0m0.034s
```

### What is the actual behavior?

ripgrep is slower than LTREP.

### What is the expected behavior?

ripgrep to be faster than LTREP.

---

_Comment by @BurntSushi on 2024-05-26 11:46_

Nice find. At first I thought this might be a case of the latency of the regex engine playing a role here (because it generally has more overhead than [what you have here](https://github.com/Bricktech2000/LTRE/blob/12970412082b14cd4b44e9c857b004d5e67ddc51/ltre.c#L631-L636)). But some other examples reveal this probably isn't the case?

```
$ time rg -c '[A-Z]+' enwik9
7421426

real    0.604
user    0.547
sys     0.057
maxmem  959 MB
faults  0

$ time ltrep-1297041 -c '[A-Z]+' enwik9
7421426

real    0.904
user    0.843
sys     0.060
maxmem  954 MB
faults  0

$ time rg -c '[A-Z][A-Z]+' enwik9
1212981

real    1.141
user    1.077
sys     0.063
maxmem  959 MB
faults  0

$ time ltrep-1297041 -c '[A-Z][A-Z]+' enwik9
1212981

real    0.925
user    0.884
sys     0.040
maxmem  954 MB
faults  0
```

So I think this probably warrants some investigation.

Also, just because something is a toy doesn't mean it is expected to never be faster than something that isn't a non-toy. For example, this is the "toy" version of `memchr`:

```rust
fn memchr(needle: u8, haystack: &[u8]) -> Option<usize> {
    haystack.iter().position(|&b| b == needle)
}
```

But sometimes this will be faster than a SIMD optimized version. Because the SIMD optimized version needs a preamble, a fallback for the case when the haystack is smaller than the vector size, creating the vector and so on. And the SIMD version might not get inlined if it's using AVX2 and the surrounding code wasn't compiled with AVX2 enabled. (Which is the common case, although the rise of things like `x86-64-v3` is changing that.)

And like, despite your regex engine being a toy, your search code is still very tight and about as good as you can do with a DFA: you transition from state to state and report whether a match state was seen. ripgrep's regex engine basically does the same, [but with more frills](https://github.com/rust-lang/regex/blob/ddeb85eaa3bdf79d6306cc92a9d8bd89d839b5cd/regex-automata/src/dfa/search.rs#L83-L125).

---

_Comment by @BurntSushi on 2024-05-26 14:27_

My hypothesis here is that your toy regex engine is benefiting precisely from the coupling that comes from the nature of it being a toy. That is, in your grep, the code for searching the input and the code for walking the DFA are effectively intertwined. That generally isn't true for a general purpose regex engine: there's usually an abstraction boundary that separates them. To test this, I generated a file with the same number of lines as `enwik9` but where every line was just `AZ`:

```rust
use std::io::Write;

fn main() -> anyhow::Result<()> {
    let lines = 13_147_025;
    let mut out = std::io::stdout().lock();
    for _ in 0..lines {
        writeln!(out, "AZ")?;
    }
    Ok(())
}
```

Then I benchmarked ripgrep, ltrep and GNU grep with `hyperfine`:

```
$ hyperfine --output pipe "LC_ALL=C grep -E -c '[A-Z][A-Z]' enwik9-two-capitals" "rg --no-config -c '[A-Z][A-Z]' enwik9-two-capitals" "ltrep-1297041 -c '[A-Z][A-Z]' enwik9-two-capitals"
Benchmark 1: LC_ALL=C grep -E -c '[A-Z][A-Z]' enwik9-two-capitals
  Time (mean ± σ):     112.7 ms ±   2.5 ms    [User: 110.7 ms, System: 2.2 ms]
  Range (min … max):   110.5 ms … 120.4 ms    25 runs

Benchmark 2: rg --no-config -c '[A-Z][A-Z]' enwik9-two-capitals
  Time (mean ± σ):     327.9 ms ±   1.5 ms    [User: 326.3 ms, System: 1.7 ms]
  Range (min … max):   325.4 ms … 331.0 ms    10 runs

Benchmark 3: ltrep-1297041 -c '[A-Z][A-Z]' enwik9-two-capitals
  Time (mean ± σ):      22.5 ms ±   3.4 ms    [User: 20.6 ms, System: 2.0 ms]
  Range (min … max):    16.0 ms …  31.3 ms    91 runs

Summary
  ltrep-1297041 -c '[A-Z][A-Z]' enwik9-two-capitals ran
    5.00 ± 0.76 times faster than LC_ALL=C grep -E -c '[A-Z][A-Z]' enwik9-two-capitals
   14.55 ± 2.20 times faster than rg --no-config -c '[A-Z][A-Z]' enwik9-two-capitals
```

ltrep doesn't just beat ripgrep, it also beats GNU grep. GNU grep doesn't have as much abstraction as ripgrep, but it has more than ltrep. Also, at a certain point, the feature support has a role to play here. As the features and optimizations and use cases grow, so to does the abstractions.

But ltrep's advantage here is data dependent. Your particular implementation examines every byte of input, and in exchange, your code is simpler but potentially significantly slower. For example, I wrote this program to generate a similar file as above, but with an extra 100 bytes after the initial `AZ`:

```rust
use std::io::Write;

fn main() -> anyhow::Result<()> {
    let lines = 13_147_025;
    let mut out = std::io::stdout().lock();
    let filler = "az".repeat(100);
    for _ in 0..lines {
        write!(out, "AZ")?;
        write!(out, "{filler}")?;
        writeln!(out, "")?;
    }
    Ok(())
}
```

Then I re-ran the benchmarks:

```
$ hyperfine --output pipe "LC_ALL=C grep -E -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler" "rg --no-config -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler" "ltrep-1297041 -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler"
Benchmark 1: LC_ALL=C grep -E -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler
  Time (mean ± σ):     330.5 ms ±   7.5 ms    [User: 161.5 ms, System: 168.8 ms]
  Range (min … max):   321.7 ms … 341.6 ms    10 runs

Benchmark 2: rg --no-config -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler
  Time (mean ± σ):     531.5 ms ±  11.2 ms    [User: 460.0 ms, System: 71.1 ms]
  Range (min … max):   502.5 ms … 544.9 ms    10 runs

Benchmark 3: ltrep-1297041 -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler
  Time (mean ± σ):      2.294 s ±  0.019 s    [User: 2.238 s, System: 0.055 s]
  Range (min … max):    2.261 s …  2.312 s    10 runs

Summary
  LC_ALL=C grep -E -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler ran
    1.61 ± 0.05 times faster than rg --no-config -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler
    6.94 ± 0.17 times faster than ltrep-1297041 -c '[A-Z][A-Z]' enwik9-two-capitals-with-filler
```

Both GNU grep and ripgrep know to stop searching on each line after seeing the `AZ`. But ltrep continues. Your code is less branchy because of it, but now it's doing a bunch of wasted work.

I think there's overall room for ripgrep to improve here, but I'd consider this difference to be "overall small." And once the abstraction genie is out of the box, it's hard to roll it back.

---

_Closed by @BurntSushi on 2024-05-26 14:27_

---

_Label `question` added by @BurntSushi on 2024-05-26 14:28_

---

_Comment by @Bricktech2000 on 2024-05-26 21:30_

Short-circuiting out when on an accepting state in the middle of a partial match is such a low-hanging fruit I can't believe I didn't think of it. In the general case I believe this boils down to preprocessing the DFA by flagging states which either always or never reach an accepting state. And I wouldn't be surprised if the additional compare and branch needed in the hot loop nullified LTRE's "head start".

Thanks for taking the time to provide such a great response.

---

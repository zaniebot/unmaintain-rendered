```yaml
number: 12943
title: "Use `FoldHash` instead of `FxHash`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
base: main
head: charlie/foldhash
created_at: 2024-08-16T22:06:21Z
updated_at: 2024-11-12T21:40:21Z
url: https://github.com/astral-sh/ruff/pull/12943
synced_at: 2026-01-12T15:55:42Z
```

# Use `FoldHash` instead of `FxHash`

---

_@charliermarsh_

## Summary

Entirely out of curiosity, it was pretty easy.


---

_Review requested from @carljm by @charliermarsh on 2024-08-16 22:06_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-08-16 22:06_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-08-16 22:06_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-08-16 22:06_

---

_@charliermarsh reviewed on 2024-08-16 22:06_

---

_Review comment by @charliermarsh on `Cargo.toml`:108 on 2024-08-16 22:06_

I left it in red-knot, it was giving me some trouble around lack of a `Default` impl.

---

_Comment by @codspeed-hq[bot] on 2024-08-16 22:30_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/foldhash)

### Merging #12943 will **degrade performances by 6.45%**

<sub>Comparing <code>charlie/foldhash</code> (400732a) with <code>main</code> (6359e55)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/foldhash)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/foldhash` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -6.45% |


---

_Comment by @charliermarsh on 2024-08-16 22:31_

Locally I see such a large regression that I have to assume I did something wrong here:

```
❯ hyperfine --warmup 10 --runs 100 "./target/release/foldhash check ../cpython/ --no-cache --silent -e" "./target/release/fxhash check ../cpython --no-cache --silent -e"
Benchmark 1: ./target/release/foldhash check ../cpython/ --no-cache --silent -e
  Time (mean ± σ):      79.2 ms ±   1.4 ms    [User: 842.1 ms, System: 104.1 ms]
  Range (min … max):    76.8 ms …  86.1 ms    100 runs

Benchmark 2: ./target/release/fxhash check ../cpython --no-cache --silent -e
  Time (mean ± σ):      65.1 ms ±   1.3 ms    [User: 617.5 ms, System: 105.7 ms]
  Range (min … max):    62.3 ms …  69.6 ms    100 runs

Summary
  ./target/release/fxhash check ../cpython --no-cache --silent -e ran
    1.22 ± 0.03 times faster than ./target/release/foldhash check ../cpython/ --no-cache --silent -e
```

---

_Closed by @charliermarsh on 2024-08-16 22:43_

---

_Comment by @orlp on 2024-08-16 22:54_

@charliermarsh  That is quite odd, out of curiosity, what happens if you use `lto = "fat"`? What are the typical things it hashes in the ruff codebase?

---

_Comment by @charliermarsh on 2024-08-16 23:01_

Let me try `lto = "fat"`! It's also possible I messed something up here. In Ruff it's generally short strings (Python identifiers) or `u32` IDs.


---

_Comment by @orlp on 2024-08-16 23:18_

On my machine (Apple M2 laptop):

```rust
Benchmark 1: ./target/release/ruff check ~/tmp/cpython/ --no-cache --silent -e
  Time (mean ± σ):     104.1 ms ±   2.6 ms    [User: 752.9 ms, System: 87.4 ms]
  Range (min … max):   100.2 ms … 116.4 ms    100 runs

Benchmark 2: ../ruff-foldhash/target/release/ruff check ~/tmp/cpython --no-cache --silent -e
  Time (mean ± σ):     117.8 ms ±   1.7 ms    [User: 884.8 ms, System: 87.4 ms]
  Range (min … max):   114.1 ms … 122.6 ms    100 runs

Summary
  ./target/release/ruff check ~/tmp/cpython/ --no-cache --silent -e ran
    1.13 ± 0.03 times faster than ../ruff-foldhash/target/release/ruff check ~/tmp/cpython --no-cache --silent -e
```

So I can reproduce, somewhat. With `lto = "fat"` the results are mostly the same as the above.


FWIW, [my benchmarks](https://gist.github.com/orlp/1271ad5b8b775c651cc55773888858eb) already showed that I'm slower than fxhash for `u32`s, so if that's a significant portion of your runtime, it's no surprise foldhash is slower. Foldhash often trades places with fxhash for a variety of inputs like strings, depending on the distribution.

Foldhash also provides much better guarantees to be solid regardless of input distribution, fxhash is very fast but has serious quality issues for certain inputs (which may or may not be a problem for ruff).

---

_Comment by @orlp on 2024-08-17 00:06_

@charliermarsh I think there might be something else at play... I can *also* reproduce the slowdown... when I do `foldhash = { path = ".." }`, and in my local copy of foldhash I replace the internals of foldhash with that of rustc-hash (I was trying to see which portions are slower than rustc-hash).

As a sanity check, I also made `foldhash` always return 0 and that did blow up the runtime by 4x, so it is actually being ran/benchmarked.

---

_Comment by @orlp on 2024-08-17 00:44_

I have to sleep now, will investigate further tomorrow. Copy/pasting the `rustc_hash` source in its entirety and using that from the `foldhash` crate gives good speed, so it must be something with my hasher/`RandomState` setup, since it's slow even with `rustc_hash` internals. Perhaps something is not being inlined, although the assembly looked good in the one spot I looked.

---

_Comment by @orlp on 2024-08-17 00:48_

I tried one more thing quickly before bed, and I found the source of the slowdown. It's the `RandomState` initialization routine, which should be incredibly cheap, but does run every time you make a hash map (even if it remains empty).

@charliermarsh Does ruff happen to instantiate... thousands and thousands of (empty) hash maps?

The results on my machine if I monkey-patch `foldhash`'s `RandomState` to always use a fixed constant:

```rust
Benchmark 1: ../ruff/target/release/ruff check ~/tmp/cpython/ --no-cache --silent -e
  Time (mean ± σ):     103.4 ms ±   2.0 ms    [User: 755.9 ms, System: 88.8 ms]
  Range (min … max):   100.2 ms … 112.2 ms    100 runs

Benchmark 2: ../ruff-foldhash/target/release/ruff check ~/tmp/cpython --no-cache --silent -e
  Time (mean ± σ):     104.7 ms ±   2.1 ms    [User: 760.3 ms, System: 88.7 ms]
  Range (min … max):   100.5 ms … 116.1 ms    100 runs

Summary
  ../ruff/target/release/ruff check ~/tmp/cpython/ --no-cache --silent -e ran
    1.01 ± 0.03 times faster than ../ruff-foldhash/target/release/ruff check ~/tmp/cpython --no-cache --silent -e
```

The offending code (`GlobalSeed::new` is not problematic):

```rust
impl Default for RandomState {
    fn default() -> Self {
        // We use our current stack address in combination with
        // PER_HASHER_NONDETERMINISM to create a new value that is very likely
        // to have never been used as a random state before.
        //
        // PER_HASHER_NONDETERMINISM ensures that even if we create two
        // RandomStates with the same stack location it is still highly unlikely
        // you'll end up with the same random seed.
        //
        // PER_HASHER_NONDETERMINISM is loaded and updated in a racy manner, but
        // this doesn't matter in practice - it is impossible that two different
        // threads have the same stack location, so they'll almost surely
        // generate different seeds, and provide a different possible update for
        // PER_HASHER_NONDETERMINISM. If we would use a proper atomic update
        // then hash table creation would have a global contention point, which
        // users could not avoid.
        //
        // Finally, not all platforms have a 64-bit atomic, so we use usize.
        static PER_HASHER_NONDETERMINISM: AtomicUsize = AtomicUsize::new(0);
        let nondeterminism = PER_HASHER_NONDETERMINISM.load(Ordering::Relaxed) as u64;
        let stack_ptr = &nondeterminism as *const _ as u64;
        let per_hasher_seed = folded_multiply(nondeterminism ^ stack_ptr, ARBITRARY2);
        PER_HASHER_NONDETERMINISM.store(per_hasher_seed as usize, Ordering::Relaxed);

        Self {
            per_hasher_seed,
            global_seed: GlobalSeed::new(),
        }
    }
}
```

---

_Comment by @orlp on 2024-08-17 01:04_

The problem is contention on `PER_HASHER_NONDETERMINISM`, if I only use the stack pointer everything is fast again.

---

_Comment by @ibraheemdev on 2024-08-17 02:08_

I think the solution for us specifically is to use [`FixedState`](https://docs.rs/foldhash/latest/foldhash/fast/struct.FixedState.html) to avoid the contention since we don't really care about the per-hashmap randomization (rustc-hash does this by default), at least for a fair comparison.

---

_Reopened by @charliermarsh on 2024-08-17 02:39_

---

_Comment by @charliermarsh on 2024-08-17 02:42_

Wow, thanks for taking a look @orlp! Please don't feel obligated of course -- only if it's interesting for you.

> Does ruff happen to instantiate... thousands and thousands of (empty) hash maps?

I would say... yes. At least, over the course of that command. CPython contains ~2,000 Python files, and we probably instantiate at least 10 hash maps per file -- so I'd guess something like ~20,000 hash maps?


---

_Comment by @charliermarsh on 2024-08-17 02:48_

> rustc-hash does this by default

Oh TIL. I could modify to use `FixedState` everywhere and re-run.

---

_Comment by @orlp on 2024-08-17 09:35_

@charliermarsh I will try to release 0.1.1 today that gets rid of the contention. I would rather not see people use `FixedState` everywhere, as it provides zero DoS resistance and can potentially cause [quadratic behavior](https://accidentallyquadratic.tumblr.com/post/153545455987/rust-hash-iteration-reinsertion) (all deterministic hashers have this problem).

---

_Comment by @orlp on 2024-08-17 12:29_

@charliermarsh

> At least, over the course of that command. CPython contains ~2,000 Python files, and we probably instantiate at least 10 hash maps per file -- so I'd guess something like ~20,000 hash maps?

I'm afraid your guess is off by an order of magnitude and a half. Out of curiosity I inserted a count tracker in `RandomState::new`, and it is called 870,000 times by ruff on the CPython codebase.

---

_Comment by @MichaReiser on 2024-08-17 12:34_

Doesn't sound unreasonable. We have some rules that create hash maps internally to detect duplicates. They are called for every node of a specific type. We could probably optimize some of those hash maps by re-using hash maps over calls instead of creating a new hash map whenever the function is called. It's one of the main downside of our rules system today. They're all stateless functions and there's no option to run any setup or teardown code.

---

_Comment by @orlp on 2024-08-17 13:47_

@charliermarsh I just pushed `foldhash 0.1.1` which has fixed the contention issue by using a thread-local counter.

---

_Label `performance` added by @MichaReiser on 2024-08-17 13:57_

---

_Comment by @orlp on 2024-08-17 14:06_

With that fix in place I ran ~~four~~ six benchmarks with `hyperfine --warmup 10 --runs 100 "ruff check ~/tmp/cpython/ --no-cache --silent -e"`, giving the following times on my Apple M2 machine:

```rust
foldhash:        Time (mean ± σ):     103.4 ms ±   2.0 ms
foldhash(norng): Time (mean ± σ):     104.4 ms ±   1.8 ms
rustc-hash:      Time (mean ± σ):     105.0 ms ±   3.5 ms
ahash(norng):    Time (mean ± σ):     105.9 ms ±   4.7 ms
siphash:         Time (mean ± σ):     114.3 ms ±   4.7 ms
ahash:           Time (mean ± σ):     120.5 ms ±   8.4 ms
```

(Note that in earlier benchmarks I got 103.4 and 104.1 as times for `rustc-hash`, so the difference between `foldhash` and `rustc-hash` for ruff on my machine is likely within noise.)

The `ahash` benchmark was generated by essentially taking this PR and doing a search/replace for `foldhash` with `ahash`. The `siphash` benchmark was done based on this PR by editing the `foldhash` crate locally, changing `src/convenience.rs` as follows:

```rust
/// Type alias for [`std::collections::HashMap<K, V, foldhash::fast::RandomState>`].
// pub type HashMap<K, V> = std::collections::HashMap<K, V, RandomState>;
pub type HashMap<K, V> = std::collections::HashMap<K, V>;

/// Type alias for [`std::collections::HashSet<T, foldhash::fast::RandomState>`].
// pub type HashSet<T> = std::collections::HashSet<T, RandomState>;
pub type HashSet<T> = std::collections::HashSet<T>;
```

The poor performance for `ahash` is likely also due to [having contention in the per-hasher seed generation](https://docs.rs/ahash/latest/src/ahash/random_state.rs.html#155).

EDIT: I also added a run `ahash(norng)` which uses `BuildHasherDefault<AHasher>` as the hasher in `convenience.rs`, and `foldhash(norng)` which uses `FixedState` to the above benchmark, confirming that `ahash`s slowdown is primarily in the random seed generation.

---

_Comment by @charliermarsh on 2024-08-17 20:10_

Thanks @orlp, that's awesome. Appreciate you doing all this analysis here. Any idea why `foldhash(norng)` is faster than `foldhash`? Is that just noise?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-08-17 20:11_

---

_Comment by @orlp on 2024-08-17 23:19_

@charliermarsh I think you meant the other way around? Anyway, I would assume that is noise, unless you happen to fill a hashmap/set from the iterator of another hashmap/set in some part of the code (because that is quadratic if they share the same hasher).

---

_@MichaReiser reviewed on 2024-08-18 07:47_

I don't have a strong opinion on this change but I'm leaning towards not merging it, considering that there's no significant performance improvement. 

* It introduces a new dependency and it's unclear if we can convince upstream dependencies to make the same switch, considering that there's no clear performance win.
* The PR might cause a few merge conflicts. They're easy to solve but it requires extra work. 
* We all have trained our memory mussels to use `FxHashSet`. Using `foldhash` will take some time to get used to. 


Are there any advantages other than performance for using foldhash over rustc that offset my concern above? I read that it provides some basic DoS mitigation, although I'm not sure if that's worth it, considering that it only raises the bar but doesn't prevent them in anyway.

---

_Comment by @orlp on 2024-08-18 08:34_

@MichaReiser 

> It introduces a new dependency and it's unclear if we can convince upstream dependencies to make the same switch, considering that there's no clear performance win.

Only two upstream dependencies of ruff use `rustc-hash`, `salsa` which I believe you control, and `countme` which was already a duplicate dependency because it uses `rustc-hash` 1.1.0.

> We all have trained our memory mussels to use FxHashSet. Using foldhash will take some time to get used to.

What we do in polars is that we have a `PlHashMap` and `PlHashSet` we use throughout the codebase so it's easy to switch implementations if necessary. Not sure if that'll help here with the mussels in the short term, but should help long-term if you have a common dependency you can put the definition :)

>  I read that it provides some basic DoS mitigation, although I'm not sure if that's worth it, considering that it only raises the bar but doesn't prevent them in anyway.

Perhaps I was a bit too careful in my [wording](https://github.com/orlp/foldhash). `foldhash` does not claim to provide HashDoS resistance against a *clever interactive* attacker, that is one which can make the program hash something, study its output, and produce new strings to hash iteratively.

I don't believe that is a realistic attacker model for ruff. Against *non-interactive* attackers I believe it is essentially impossible to construct inputs which will always collide regardless of the seed.

So assuming you don't have someone heavily studying a long-running server executing some incredibly-long-running `ruff` command, it should be impossible to cause HashDoS when using foldhash. With rustc-hash it's like a 60 minute exercise to create an arbitrary size dictionary of words that all collide to the exact 64-bit hash. In fact, when I have some time later today or tomorrow I will do that to demonstrate my point. And once you do that you can use this dictionary of words forever, against everyone that uses rustc-hash.

> Are there any advantages other than performance for using foldhash over rustc that offset my concern above? 

Ultimately they all come down to performance. Hashmaps/sets are *probabilistic* data structures, that use the random nature of hashes for their time expectancy arguments. If you take away the randomization you lose the expected O(1) time per operation guarantee.

This can either bite you with malicious inputs that intentionally cause collisions, or it can happen by accident. For example if you iterate over one hashset to fill another hashset, and they both use linear probing with the same random seed, the expected time is O(n^2). Using foldhash prevents all of these issues categorically (if not facing an interactive attacker). If foldhash isn't any slower for typical input, why not also ensure you're fast for edge cases / malicious input?

---

_Comment by @MichaReiser on 2024-08-18 11:02_

> salsa which I believe you control

Yes, I'm a contributor but such a change would still require prior discussion.

> What we do in polars is that we have a PlHashMap and PlHashSet we use throughout the codebase so it's easy to switch implementations if necessary.

We do this in red-knot too. It does help to minimize the change but we called `FxHashMap`, so it still needs time to get used to.

>  If foldhash isn't any slower for typical input, why not also ensure you're fast for edge cases / malicious input?

Thanks, I wasn't aware of that

I don't feel strongly about the change but my concerns remain. It's not a "free-win" and requires additional work that I don't consider a priority right now. 

---

_Review requested from @MichaReiser by @MichaReiser on 2024-08-18 11:02_

---

_Review request for @MichaReiser removed by @MichaReiser on 2024-08-18 11:24_

---

_@MichaReiser reviewed on 2024-08-18 11:24_

---

_Review comment by @MichaReiser on `Cargo.toml`:108 on 2024-08-18 11:24_

We should resolve this before merging

---

_Comment by @orlp on 2024-08-18 14:38_

> With rustc-hash it's like a 60 minute exercise to create an arbitrary size dictionary of words that all collide to the exact 64-bit hash. In fact, when I have some time later today or tomorrow I will do that to demonstrate my point.

Alright, that wasn't too hard. No need even for a dictionary, every string of the form 
```
fajMdq1tAvfJmnS1________GqhUEoJ0
```
will collide with the exact same hash using rustc-hash 2.0.0, regardless of which 8-byte string you use to replace `_`:

```rust
use std::hash::BuildHasher;
let hasher = rustc_hash::FxBuildHasher::default();
dbg!(hasher.hash_one("AcRwqMBJW5iFhxpF________csQwAdk2"));
dbg!(hasher.hash_one("AcRwqMBJW5iFhxpF_foo____csQwAdk2"));
dbg!(hasher.hash_one("AcRwqMBJW5iFhxpF_foobar_csQwAdk2"));
dbg!(hasher.hash_one("AcRwqMBJW5iFhxpF__orlp__csQwAdk2"));
```

```
[src\main.rs:75:1] hasher.hash_one("AcRwqMBJW5iFhxpF________csQwAdk2") = 5670621083967722445
[src\main.rs:76:1] hasher.hash_one("AcRwqMBJW5iFhxpF_foo____csQwAdk2") = 5670621083967722445
[src\main.rs:77:1] hasher.hash_one("AcRwqMBJW5iFhxpF_foobar_csQwAdk2") = 5670621083967722445
[src\main.rs:78:1] hasher.hash_one("AcRwqMBJW5iFhxpF__orlp__csQwAdk2") = 5670621083967722445
```

So just using alphanumeric strings that's already an instant `62**8 = 218340105584896` element collision set.

---

_Comment by @orlp on 2024-08-18 14:58_

@MichaReiser To make this extra concrete, the following script outputs a 3.5MB large Python script:

```python
import random, string

TEMPLATE = "AcRwqMBJW5iFhxpF________csQwAdk2 = 0"
ALPHANUM = string.ascii_uppercase + string.ascii_lowercase + string.digits

for _ in range(10**5):
    r = ''.join(random.choices(ALPHANUM, k=8))
    print(TEMPLATE.replace("________", r))
```

On my machine with this PR the output file takes ~64ms to process using `ruff check malicious.py --no-cache --silent -e`. With the `main` branch it takes 28 seconds. And this is all quadratic behavior, increase the input size by 10x, and now that 28 seconds is probably 2800 seconds - almost an hour.

---

_Comment by @charliermarsh on 2024-11-12 21:40_

Closing for now though still interested in this. Maybe over in uv at some point.

---

_Closed by @charliermarsh on 2024-11-12 21:40_

---

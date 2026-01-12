```yaml
number: 1086
title: "globset: case-insensitive matching of literal globs is unnecessarily slow"
type: issue
state: open
author: Deewiant
labels:
  - enhancement
  - help wanted
  - icebox
assignees: []
created_at: 2018-10-17T12:31:46Z
updated_at: 2019-01-27T18:10:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1086
synced_at: 2026-01-12T16:13:22Z
```

# globset: case-insensitive matching of literal globs is unnecessarily slow

---

_@Deewiant_

Case-insensitive matching forces regex matching, and with a large amount of patterns this can be noticeably slow. (Benchmark below.) It's also easy to hit the [regex size limit](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#how-do-i-get-around-the-regex-size-limit) merely by enabling case insensitivity.

I ran into this since I had a small amount of `*.foo`-style patterns, for which I wanted case insensitivity, alongside a fairly large amount of literal patterns for which I didn't really care, so for simplicity I enabled case insensitivity for everything. Eventually I ran into the regex size limit and added a simple hack: I only enabled case insensitivity for strings containing the `*` character. This solved my problem and naturally also considerably sped up matching.

It should be possible to optimize case-insensitive literal patterns similarly to case-sensitive ones, by keeping a separate set dedicated to them. Here's a Criterion benchmark that uses `ignore::overrides` (I used the API I was most familiar with for convenience but I'm sure the results can be reproduced with just `globset`) for both case-sensitive and case-insensitive matching, and demonstrates the performance of a `BTreeSet<UniCase>` (from [`unicase`](https://crates.io/crates/unicase)) as a fast case-insensitive literal matcher:

```rust
#[macro_use]
extern crate criterion;
extern crate ignore;
extern crate unicase;

use std::collections::BTreeSet;
use std::rc::Rc;

use criterion::Criterion;
use ignore::overrides;
use unicase::UniCase;

fn create_overrides(case_insensitive: bool) -> overrides::Override {
    let mut builder = overrides::OverrideBuilder::new("main-dir");
    builder.case_insensitive(case_insensitive).unwrap();
    for i in 0..0x6000 {
        builder.add(&format!("/{:x}", i)).unwrap();
    }
    builder.build().unwrap()
}

fn bench(c: &mut Criterion, case_insensitive: bool) {
    let overrides = Rc::new(create_overrides(case_insensitive));
    {
        let overrides = overrides.clone();
        c.bench_function(
            &format!("no-match case_insensitive={}", case_insensitive),
            move |b| b.iter(|| assert!(overrides.matched("something-else", false).is_ignore())),
        );
    }
    {
        let overrides = overrides.clone();
        c.bench_function(
            &format!("near-match case_insensitive={}", case_insensitive),
            move |b| b.iter(|| assert!(overrides.matched("4abcnope", false).is_ignore())),
        );
    }
    {
        let overrides = overrides.clone();
        c.bench_function(
            &format!("match case_insensitive={}", case_insensitive),
            move |b| b.iter(|| assert!(overrides.matched("5ffa", false).is_whitelist())),
        );
    }
}

fn bench_sensitive(c: &mut Criterion) {
    bench(c, false);
}

fn bench_insensitive(c: &mut Criterion) {
    bench(c, true);
}

fn bench_insensitive_simulated(c: &mut Criterion) {
    let mut set = BTreeSet::new();
    for i in 0..0x6000 {
        set.insert(UniCase::new(format!("{:x}", i)));
    }
    let set = Rc::new(set);
    {
        let set = set.clone();
        c.bench_function(&format!("no-match simulated case-insensitive"), move |b| {
            b.iter(|| assert!(!set.contains(&UniCase::new("something-else".to_owned()))))
        });
    }
    {
        let set = set.clone();
        c.bench_function(
            &format!("near-match simulated case-insensitive"),
            move |b| b.iter(|| assert!(!set.contains(&UniCase::new("4abcnope".to_owned())))),
        );
    }
    {
        let set = set.clone();
        c.bench_function(&format!("match simulated case-insensitive"), move |b| {
            b.iter(|| assert!(set.contains(&UniCase::new("5ffa".to_owned()))))
        });
    }
}

criterion_group!(
    benches,
    bench_sensitive,
    bench_insensitive,
    bench_insensitive_simulated
);
criterion_main!(benches);
```

One set of results I got from this:

```
no-match case_insensitive=false
                        time:   [288.10 ns 288.36 ns 288.66 ns]
Found 23 outliers among 100 measurements (23.00%)
  1 (1.00%) low severe
  2 (2.00%) high mild
  20 (20.00%) high severe

near-match case_insensitive=false
                        time:   [251.46 ns 251.73 ns 252.10 ns]
Found 12 outliers among 100 measurements (12.00%)
  12 (12.00%) high severe

match case_insensitive=false
                        time:   [327.48 ns 328.12 ns 328.91 ns]
Found 16 outliers among 100 measurements (16.00%)
  1 (1.00%) low mild
  3 (3.00%) high mild
  12 (12.00%) high severe

no-match case_insensitive=true
                        time:   [34.779 us 34.786 us 34.795 us]
Found 15 outliers among 100 measurements (15.00%)
  1 (1.00%) low severe
  1 (1.00%) low mild
  1 (1.00%) high mild
  12 (12.00%) high severe

near-match case_insensitive=true
                        time:   [2.1756 ms 2.1861 ms 2.1981 ms]
Found 12 outliers among 100 measurements (12.00%)
  7 (7.00%) high mild
  5 (5.00%) high severe

match case_insensitive=true
                        time:   [2.1379 ms 2.1719 ms 2.2157 ms]
Found 12 outliers among 100 measurements (12.00%)
  4 (4.00%) high mild
  8 (8.00%) high severe

no-match simulated case-insensitive
                        time:   [179.83 ns 180.32 ns 180.81 ns]
Found 10 outliers among 100 measurements (10.00%)
  2 (2.00%) low mild
  5 (5.00%) high mild
  3 (3.00%) high severe

near-match simulated case-insensitive
                        time:   [157.66 ns 158.36 ns 159.19 ns]
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) high mild
  2 (2.00%) high severe

match simulated case-insensitive
                        time:   [228.11 ns 228.82 ns 229.55 ns]
Found 3 outliers among 100 measurements (3.00%)
  1 (1.00%) high mild
  2 (2.00%) high severe
```

So, for this particular contrived case, we see that performance relative to the case-sensitive looks as follows:

||case-sensitive|case-insensitive|`BTreeSet<UniCase>`
-|-|-|-
|no match|1.00|120|0.62
|near match|1.00|8680|0.63
|match|1.00|6620|0.70

Of course the fact that `UniCase` is faster is due to the fact that we're doing less work here by only incorporating one low-level aspect instead of going through the full matching process. This is also all in ASCII and doesn't actually test cross-case matching, alongside a bunch of other caveats. Still, I'd expect this to be representative in the sense that the "actual" performance would at least be in the same order of magnitude instead of 2–4 orders away, so there's definitely room to improve.

---

_Comment by @BurntSushi on 2018-10-17 12:59_

Thanks for the great analysis! I'm not sure when I'll personally dive into this, but PRs would be welcome.

---

_Label `enhancement` added by @BurntSushi on 2018-10-17 12:59_

---

_Label `help wanted` added by @BurntSushi on 2018-10-17 12:59_

---

_Label `icebox` added by @BurntSushi on 2019-01-27 18:10_

---

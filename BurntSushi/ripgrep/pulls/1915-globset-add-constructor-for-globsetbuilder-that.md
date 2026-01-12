```yaml
number: 1915
title: "globset: add constructor for GlobSetBuilder that preallocates space"
type: pull_request
state: closed
author: AlexTMjugador
labels: []
assignees: []
base: master
head: master
created_at: 2021-06-28T11:30:24Z
updated_at: 2021-07-14T12:09:34Z
url: https://github.com/BurntSushi/ripgrep/pull/1915
synced_at: 2026-01-12T18:23:14Z
```

# globset: add constructor for GlobSetBuilder that preallocates space

---

_@AlexTMjugador_

This constructor helps avoiding reallocations when the number of globs that will be added is known or can be reasonably well estimated in advance. This speeds up the builder a bit, especially for larger glob sets.

---

_Comment by @BurntSushi on 2021-06-29 11:29_

@AlexTMjugador Thanks for the PR!

I'm having a hard time imagining a case where this is useful/meaningful. Do you have a real world benchmark that you can show me?

---

_Comment by @AlexTMjugador on 2021-06-30 10:52_

I don't know of any real world benchmark that highlights any noticeable performance difference with this change, but I did a quick `cargo bench` with rustc version `1.55.0-nightly (7c3872e6b 2021-06-24)`.

<details>
<summary>Benchmark source code</summary>

```rs
fn lots_of_globs_no_capacity() {
    let mut builder = GlobSetBuilder::new();

    for i in 0..10000 {
        builder.add(GlobBuilder::new(&format!("hello*{}", i)).build().unwrap());
    }

    builder.build().unwrap();
}

fn lots_of_globs_with_capacity() {
    let mut builder = GlobSetBuilder::with_capacity(10000);

    for i in 0..10000 {
        builder.add(GlobBuilder::new(&format!("hello*{}", i)).build().unwrap());
    }

    builder.build().unwrap();
}

#[cfg(test)]
mod tests {
    use super::*;
    use test::Bencher;

    #[bench]
    fn no_capacity(b: &mut Bencher) {
        b.iter(|| lots_of_globs_no_capacity());
    }

    #[bench]
    fn with_capacity(b: &mut Bencher) {
        b.iter(|| lots_of_globs_with_capacity());
    }
}
```
</details>

The results on my Linux 5.10.0 machine with 16 GiB 1333 MHz dual-channel DDR3 RAM and a Intel Core i3-2100 CPU where as follows:

```
    Finished bench [optimized] target(s) in 2.10s
     Running unittests (target/release/deps/benchmark-b49d7e8bd2ddd614)

running 2 tests
test tests::no_capacity   ... bench:  97,001,928 ns/iter (+/- 547,969)
test tests::with_capacity ... bench:  96,271,824 ns/iter (+/- 302,626)

test result: ok. 0 passed; 0 failed; 0 ignored; 2 measured; 0 filtered out; finished in 58.22s
```

So passing a capacity to `GlobSetBuilder` definitely offers an improvement for this test case, consistently reducing in almost 1 millisecond the time needed to build a set of 10000 globs.

I also ran the benchmarks included in this repository, adding a new `new_reglob_many_with_capacity` benchmark, modeled after `new_reglob_many`. In this case the performance is almost identical (note the reduced variance):

```
    Finished bench [optimized] target(s) in 0.05s
     Running unittests (target/release/deps/bench-6b2d350b3a9a4166)

running 9 tests
...
test many_short_regex_set               ... bench:         306 ns/iter (+/- 22)
test many_short_regex_set_with_capacity ... bench:         307 ns/iter (+/- 3)
...
```

Overall, I'd argue this is not a game-changing improvement, but I can imagine that, if for some reason a networking application needs to match quite a bit of globs against incoming data, any improvement in its response times would be welcome. On the other hand, these changes have a negligible impact in code size and layout and doing less reallocations is definitely better, so I'd say they can't "backfire" as something that slows down already existing use cases, as shown by the maybe more typical benchmarks already included in the repository.

---

_Comment by @BurntSushi on 2021-07-14 10:55_

When updating the PR, could you please rebase? ripgrep doesn't use merge commits anywhere. Thanks.

Glancing at your benchmark... I'm not sure I agree with your conclusion:

> So passing a capacity to GlobSetBuilder definitely offers an improvement for this test case, consistently reducing in almost 1 millisecond the time needed to build a set of 10000 globs.

The measured difference is less than 1%. And moreover, the difference is well within the noise.

More generally, it just doesn't make sense to me that pre-allocating space like this would have any meaningful impact. Namely, allocating memory is a pittance compared to what a globset builder is actually doing.

---

_Comment by @AlexTMjugador on 2021-07-14 11:22_

> When updating the PR, could you please rebase? ripgrep doesn't use merge commits anywhere. Thanks.

Sorry, I have just updated the branch to the latest master without thinking too much about whether I should rebase or merge. Thanks for the reminder!

> The measured difference is less than 1%. And moreover, the difference is well within the noise.
>
> More generally, it just doesn't make sense to me that pre-allocating space like this would have any meaningful impact. Namely, allocating memory is a pittance compared to what a globset builder is actually doing.

I also think that it probably doesn't matter much. I did some tests later with bigger sets and the difference was a bit more noticeable, exceeding the 1% difference threshold, but still pretty minor, and the set size was getting ridiculous. I agree with your opinion that this is a pittance compared to other things more relevant to performance. In retrospect, this looks like a premature optimization for every conceivable use case I can come up with. Should I close this PR, then?

---

_Comment by @BurntSushi on 2021-07-14 12:09_

Yeah let's go ahead and close this, thanks!

---

_Closed by @BurntSushi on 2021-07-14 12:09_

---

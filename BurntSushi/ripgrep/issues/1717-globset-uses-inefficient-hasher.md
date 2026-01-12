```yaml
number: 1717
title: Globset uses inefficient hasher
type: issue
state: closed
author: tkaitchuck
labels:
  - question
assignees: []
created_at: 2020-10-28T04:12:16Z
updated_at: 2023-01-05T14:43:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1717
synced_at: 2026-01-12T16:13:24Z
```

# Globset uses inefficient hasher

---

_@tkaitchuck_

Globset currently is using `fnv` hasher for a HashMap that is keyed with a `Vec<u8>`.
This is very inefficient because the `fnv` algorithm requires processing one byte at a time, and cannot be vectorized. 
On [my benchmarks](https://github.com/tkaitchuck/aHash#speed) anything over the 25-32 byte range it actually slower than the default sip-hash implementation. 

---

_Comment by @BurntSushi on 2020-10-28 06:31_

Your benchmarks are on hash functions, and not on globset. There is maybe a correlation between the performance of the hash function and the performance of globset matching, but it is far from clear. For example, I would bet that keys less than 25 bytes are actually quite common.

I think the next step here would be a globset benchmark. I'm pretty sure I used fnv originally because I noticed an improvement over the default, but that was a long time ago and wouldn't mind seeing it relitigated. But as it stands, this issue lacks actionable data.

---

_Label `question` added by @BurntSushi on 2020-10-28 06:31_

---

_Comment by @tkaitchuck on 2020-10-31 07:19_

Looking at the benchmark code https://github.com/BurntSushi/ripgrep/blob/master/crates/globset/benches/bench.rs#L78
I notice two things:
1. None of the inputs insert strings over 6 characters into the hashmap
2. The map is created and immediately thrown away, (IE there aren't many puts/gets per map creation).

Are both of these realistic? If so the construction cost of the map will completely dominate and the actual hashing algorithm will matter little. 
I just ran the benchmark `many_short_regex_set` (Are there any other relevant tests?) with all of the stdlib default sip-13, aHash, fnv, and fxHash, and the difference between them was within the (fairly large) noise from run to run. If you noticed a difference years ago it could have been before https://github.com/rust-lang/rust/pull/31356 which made a big difference in map construction time. 

If the map really is only likely to be used for a single lookup it may be worth considering another data structure all together.



---

_Comment by @BurntSushi on 2023-01-05 14:41_

I don't think there is anything presently actionable here at the moment. There are likely many different avenues for improving performance in `globset`, and it should start with a holistic review.

> The map is created and immediately thrown away, (IE there aren't many puts/gets per map creation).

As far as I can tell, this is not an accurate description of the benchmark. Here is the code as I see it:

```rust
#[bench]
fn many_short_regex_set(b: &mut test::Bencher) {
    let set = new_reglob_many(MANY_SHORT_GLOBS);
    b.iter(|| assert_eq!(2, set.matches(MANY_SHORT_SEARCH).iter().count()));
}
```

That creates a single `GlobSet` and then repeatedly benchmarks its search speed. That is, internally, a map is created and then queried many times.

---

_Closed by @BurntSushi on 2023-01-05 14:41_

---

```yaml
number: 3713
title: Override range to avoid patching pubgrub
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konsti/pubgrub-range
created_at: 2024-05-21T18:09:06Z
updated_at: 2024-10-14T21:48:51Z
url: https://github.com/astral-sh/uv/pull/3713
synced_at: 2026-01-10T12:53:31Z
```

# Override range to avoid patching pubgrub

---

_Pull request opened by @konstin on 2024-05-21 18:09_

Create a newtype `struct PubGrubRange(pubgrub::range::Range<Version>)` to avoid overriding the `Display` implementation on `Range` in pubgrub itself, reducing our diff with upstream. Used methods taking an `&Self` or returning `Self` were overridden, the rest of the methods succeeds through deref.

An additional advantage is that we can start using the range type in more places without depending on pubgrub directly (https://github.com/astral-sh/uv/pull/4041).

The alternative would be moving range formatting into the error formatter, but this would make pubgrub's api strange, deviating from rust's `Display` standard.


---

_Review requested from @zanieb by @konstin on 2024-05-21 18:09_

---

_Marked ready for review by @konstin on 2024-06-03 15:50_

---

_Comment by @zanieb on 2024-06-06 00:30_

We discussed this in Discord and we both think this is a lot of boilerplate and a bit of an ugly solution but I don't think there's a better way if you want the ergonomics of `Display`. The alternative is probably relying on a PubGrub formatter method that we call into explicitly? I am not sure resolving this divergence from upstream is necessarily worth the added code here. It seems like we'd be taking on _more_ maintenance burden to keep our wrapper type up to date. However, @konstin mentioned wanting to use PubGrub ranges outside the resolver which may justify the effort?



---

_Comment by @zanieb on 2024-10-08 19:02_

@konstin can we close this?

cc @BurntSushi I feel like we were recently talking about using `Range` outside the resolver

---

_Comment by @BurntSushi on 2024-10-09 13:35_

I hadn't seen this PR before.

Personally, I think the added code here is fine and I think it's worthwhile to reduce our pubgrub diff as much as possible with upstream.

With that said, my understanding is that we want to use _something like_ a pubgrub `Range` outside of `uv-resolver` in `uv-pep508` for the `MarkerTree` implementation. I believe there was talk of making a new crate that both upstream pubgrub depends on and we depend on that exposes a `Range` type.

---

_Closed by @zanieb on 2024-10-14 21:48_

---

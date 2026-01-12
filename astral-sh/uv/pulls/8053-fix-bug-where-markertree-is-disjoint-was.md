```yaml
number: 8053
title: "fix bug where MarkerTree::is_disjoint was sometimes not commutative"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-disjointness-commutative
created_at: 2024-10-09T19:11:12Z
updated_at: 2024-10-12T18:14:20Z
url: https://github.com/astral-sh/uv/pull/8053
synced_at: 2026-01-12T16:08:08Z
```

# fix bug where MarkerTree::is_disjoint was sometimes not commutative

---

_@BurntSushi_

This PR fixes a bug where disjointness checking didn't always
satisfy commutativity. And it *should*. The `is_disjoint_commutative`
test added here demonstrates a regression test. Before this commit,
its second assertion failed.

That is, given `m1 = extra == "A" and extra != "B"` and
`m2 = extra == "A"`, we were saying that m1 was disjoint with
m2 (wrong) but that m2 was not disjoint with m1 (right).

It turned out that this was a "simple" matter of not using the
correct parent node when calling negation. Likely just a
transcription snafu.

This bug does not seem restricted in scope to extras, which is
how I found it, so it's not clear why we haven't noticed it until
now. I noticed it because I was formulating markers in a similar
format for resolver forking based on conflicting extras, and this
resulted in incorrectly filtering out dependencies due to `is_disjoint`
returning a false positive.


---

_Review requested from @ibraheemdev by @BurntSushi on 2024-10-09 19:12_

---

_Review requested from @konstin by @BurntSushi on 2024-10-09 19:12_

---

_@zanieb approved on 2024-10-09 19:14_

Nice find

---

_Label `bug` added by @zanieb on 2024-10-09 19:14_

---

_Comment by @BurntSushi on 2024-10-09 19:17_

It is absolutely wild to me that this bug fix has _zero_ impact on any of the resolutions in our tests. I feel like I should be slightly horrified by that.

---

_@ibraheemdev approved on 2024-10-09 19:44_

Incredible catch. Also shocked that this managed to escape our testing, but because the `Edges::Boolean` node representation only applies to `extra` and `in` expressions.. that seems plausible.

---

_Comment by @ibraheemdev on 2024-10-09 19:44_

I think the same issue is also present in `apply`. https://github.com/astral-sh/uv/blob/d3df97cc620110e9685e980ad9763390ebab1fc5/crates/uv-pep508/src/marker/algebra.rs#L863

---

_Comment by @BurntSushi on 2024-10-10 12:03_

> but because the Edges::Boolean node representation only applies to extra and in expressions.. that seems plausible

Oooooooooooo. That makes more sense and makes me feel not-horrified about our tests hah. I naively assumed the boolean node was used for more stuff.

---

_Comment by @BurntSushi on 2024-10-10 12:19_

> I think the same issue is also present in `apply`.
> 
> https://github.com/astral-sh/uv/blob/d3df97cc620110e9685e980ad9763390ebab1fc5/crates/uv-pep508/src/marker/algebra.rs#L863

Yeah nice catch. I fixed this too.

---

_Merged by @BurntSushi on 2024-10-10 13:46_

---

_Closed by @BurntSushi on 2024-10-10 13:46_

---

_Branch deleted on 2024-10-10 13:46_

---

_@konstin reviewed on 2024-10-12 18:14_

Good catch!

---

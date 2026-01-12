```yaml
number: 2388
title: "fix(globset/serde1): allow deserializing owned strings into `Glob`"
type: pull_request
state: closed
author: jeertmans
labels:
  - rollup
assignees: []
base: master
head: globset/fix-serde-owned
created_at: 2023-01-01T13:35:46Z
updated_at: 2023-09-10T20:37:30Z
url: https://github.com/BurntSushi/ripgrep/pull/2388
synced_at: 2026-01-12T18:23:14Z
```

# fix(globset/serde1): allow deserializing owned strings into `Glob`

---

_@jeertmans_

This PR modifies how `Glob` is deserialized such that it now allows for owned strings (previously, only borrowed) to be deserialized into `Glob`. This takes inspiration from what is done in the [`regex_serde` crate](https://github.com/tailhook/serde-regex/blob/336bb456ecd146ba9e3fcc2fef71870f603d72c5/src/lib.rs#L179-L191).

For more details, see the discussion in #2386.

The first commit includes tests, which fail without the solution.
The second commit include the solution.

This fixes #2386.

I'd be happy to hear from your thoughts and I'm open to any review :-)

---

_Comment by @jeertmans on 2023-01-08 10:36_

Like so @LPGhatguy? I have updated the code according to your comment.

I have merged `StrVisitor` and `StringVisitor` implementation into `CowStrVisitor` (see [here](https://rust-random.github.io/rand/src/serde/de/impls.rs.html#599)).

After checking, the first test uses a `Cow::Owned`, while the two others use a `Cow::Borrowed`.

---

_Comment by @LPGhatguy on 2023-01-09 08:26_

`Cow` doesn't need to be involved here. You should make a `GlobVisitor` that constructs the glob directly from the value instead of pulling it into an intermediate `Cow`.

I don't think that `visit_borrowed_bytes` or `visit_byte_buf` will be reached. Because we're calling `deserialize_str`, we're telling Sede that we're going to deserialize a string, not an arbitrary sequence of bytes.

---

_Comment by @jeertmans on 2023-01-09 10:38_

Thanks, @LPGhatguy! Indeed, after my commit, I realized that using `Cow` was no longer needed. I implemented a new version where I directly parse a string into a `Glob`.

---

_Review comment by @LPGhatguy on `crates/globset/src/serde_impl.rs`:22 on 2023-01-14 03:51_

This should probably be "a glob pattern" or something more descriptive here.

---

_@LPGhatguy reviewed on 2023-01-14 03:52_

Last comment, then LGTM.

---

_@jeertmans reviewed on 2023-01-14 08:15_

---

_Review comment by @jeertmans on `crates/globset/src/serde_impl.rs`:22 on 2023-01-14 08:15_

Thanks for noticing! There you go :)

---

_@jonasbb approved on 2023-01-18 20:52_

---

_@LPGhatguy approved on 2023-01-27 07:42_

lgtm

---

_Comment by @jeertmans on 2023-02-11 10:53_

Hello @BurntSushi, any plan on merging this? Or is it waiting for something else?

---

_Review comment by @BurntSushi on `crates/globset/src/serde_impl.rs`:3 on 2023-02-11 12:43_

Please group imports. std imports should come first, then an empty line, then third party imports (like `serde`) and then finally crate local imports.

---

_Review comment by @BurntSushi on `crates/globset/src/serde_impl.rs`:3 on 2023-02-11 12:43_

Or just write `std::fmt::Formatter` and `std::fmt::Result` below.

---

_Review comment by @BurntSushi on `crates/globset/src/serde_impl.rs`:44 on 2023-02-11 12:44_

Same nit here about import grouping.

---

_@BurntSushi requested changes on 2023-02-11 12:45_

I think this LGTM and looks much better than what it was before. There are some style nits though.

---

_Comment by @BurntSushi on 2023-02-11 12:45_

> Or is it waiting for something else?

It's waiting on me to review it. In general, my review latency is pretty high.

---

_Review comment by @jeertmans on `crates/globset/src/serde_impl.rs`:3 on 2023-02-11 12:55_

Like so (see recent commit)?

---

_@jeertmans reviewed on 2023-02-11 12:55_

---

_Comment by @jeertmans on 2023-02-11 12:56_

> It's waiting on me to review it. In general, my review latency is pretty high.

No problem, take your time :-)

I think I have updated the code accordingly.

---

_Review requested from @BurntSushi by @jeertmans on 2023-02-11 12:56_

---

_@BurntSushi approved on 2023-07-07 18:40_

LGTM

---

_Comment by @BurntSushi on 2023-07-07 18:43_

In the future, for changes this small, they should be one commit. If you need to make changes after opening the PR, then modify the commit via `git commit --amend` and force push.

Basically, treat commits like code. Optimize for the reader.

---

_Label `rollup` added by @BurntSushi on 2023-07-07 18:45_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

_Branch deleted on 2023-09-10 20:37_

---

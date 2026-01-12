```yaml
number: 933
title: "clippy: Fix some clippy lints"
type: pull_request
state: closed
author: rumpelsepp
labels: []
assignees: []
base: master
head: master
created_at: 2018-05-30T06:15:05Z
updated_at: 2018-07-21T19:02:18Z
url: https://github.com/BurntSushi/ripgrep/pull/933
synced_at: 2026-01-12T18:23:13Z
```

# clippy: Fix some clippy lints

---

_@rumpelsepp_

I ran clippy and fixed some lints. :)

---

_Review comment by @BurntSushi on `globset/src/lib.rs`:452 on 2018-05-30 13:00_

This is a change to the API. It seems fine in principle. But could you please remove the `derive(Default)` and write out an explicit `Default` impl that calls `GlobSetBuilder::new()`? This way, we make sure the implementations are exactly the same.

---

_Review comment by @BurntSushi on `globset/src/lib.rs`:497 on 2018-05-30 13:01_

I don't agree with this change. The status quo is simpler and shorter.

---

_Review comment by @BurntSushi on `globset/src/lib.rs`:570 on 2018-05-30 13:02_

I'm not sure I agree with this change either, nor the other uses of `or_insert_with`.

---

_Review comment by @BurntSushi on `globset/src/pathutil.rs`:12 on 2018-05-30 13:02_

Please add the explicit lifetime parameter back. :-)

---

_Review comment by @BurntSushi on `globset/src/pathutil.rs`:37 on 2018-05-30 13:02_

Please add the explicit lifetime parameter back. :-)

---

_Review comment by @BurntSushi on `grep/src/literals.rs`:236 on 2018-05-30 13:03_

Typo?

---

_Review comment by @BurntSushi on `grep/src/literals.rs`:238 on 2018-05-30 13:03_

I don't understand this change. Can you justify it?

---

_@BurntSushi requested changes on 2018-05-30 13:04_

Thanks for submitting this!

The problem with Clippy (and why I don't use it) is that it produces a number of warnings that I disagree with. I tried to note them in this PR.

---

_Review comment by @academician on `globset/src/lib.rs`:497 on 2018-06-20 19:01_

Can I ask why you disagree with it? It's a micro-optimization, but lazily creating the OsStr seems better to me. Eg: https://godbolt.org/g/YyXgWQ

---

_@academician reviewed on 2018-06-20 19:01_

---

_Review comment by @academician on `grep/src/literals.rs`:238 on 2018-06-20 19:14_

IIUC, `as` can perform lossy conversions, whereas `From` is only implemented for lossless ones, so if the type of `r.end()` and `r.start()` changed to something like `u64` it would truncate. In this case that would almost certainly never happen, so it's unlikely to be a real problem here, but I think the idea is that it's usually the safer default so clippy lints against it.

---

_@academician reviewed on 2018-06-20 19:14_

---

_@BurntSushi reviewed on 2018-06-20 19:17_

---

_Review comment by @BurntSushi on `globset/src/lib.rs`:497 on 2018-06-20 19:17_

I don't disagree with it per se. I just think the change should be justified. If you can produce a useful benchmark in the context of `globset` that shows a meaningful difference, then I'd be happy to reconsider it.

---

_@BurntSushi reviewed on 2018-06-20 19:20_

---

_Review comment by @BurntSushi on `grep/src/literals.rs`:238 on 2018-06-20 19:20_

`ClassByteRange` is always going to be a byte range. But I don't feel that strongly. Happy to keep this.

---

_@BurntSushi reviewed on 2018-06-20 19:23_

---

_Review comment by @BurntSushi on `globset/src/lib.rs`:497 on 2018-06-20 19:23_

RE your godbolt link: that sort of evidence is _basically_ meaningless to me because it's measuring the wrong thing. The codegen for globset's code in particular would be better, and a real benchmark is best.

---

_Comment by @BurntSushi on 2018-07-21 17:31_

@rumpelsepp Thanks for submitting this PR, but given the inactivity and my general disagreement with a lot of these lints, I think I'm just going to close this out.

With Clippy headed towards stable Rust, it's possible that I will capitulate and start running Clippy in CI along with a specific whitelist/blacklist of lints that I do and don't like. But until then, I think ad hoc fixes from Clippy probably just aren't worth it.

---

_Closed by @BurntSushi on 2018-07-21 17:31_

---

_Comment by @rumpelsepp on 2018-07-21 19:02_

No problem, sorry for being inactive on this. Just being quite busy the last weeks and I forgot about this... :) As you mentioned, maybe it makes sense to rely on clippy once it is available on stable. There is already some activity: https://internals.rust-lang.org/t/clippy-is-available-as-a-rustup-component/7967

---

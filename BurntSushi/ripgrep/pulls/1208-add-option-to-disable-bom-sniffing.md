```yaml
number: 1208
title: add option to disable bom sniffing
type: pull_request
state: closed
author: LesnyRumcajs
labels: []
assignees: []
base: master
head: master
created_at: 2019-03-04T16:20:50Z
updated_at: 2019-04-06T14:35:09Z
url: https://github.com/BurntSushi/ripgrep/pull/1208
synced_at: 2026-01-12T18:23:13Z
```

# add option to disable bom sniffing

---

_@LesnyRumcajs_

https://github.com/BurntSushi/ripgrep/issues/1207

---

_Comment by @BurntSushi on 2019-04-06 13:34_

@LesnyRumcajs Thanks so much for this! I ended up re-working this PR a bit and re-submitted it: #1237. I think I might have no explained the right path to take very well. In particular, a problem here is that `EncodingMode` was added to the public API of the `grep-searcher` crate _and_ a breaking change was introduced by changing the method signature of the `SearcherBuilder::encoding` builder. What I had intended was for `EncodingMode` to be an internal detail used in ripgrep's core `src/args.rs`. That's what I meant by "forwarding" the `bom_sniffing` option that you added to `encoding_rs_io` up through `grep-searcher::SearcherBuilder`.

More subtle than that was that when BOM sniffing was disabled, the BOM was still being stripped, and I don't think that's correct since the point of this is to get at the raw bytes directly, even if there's a BOM and including the BOM. This led to uncovering another subtle but in `encoding_rs_io`, which I fixed in [this commit](https://github.com/BurntSushi/encoding_rs_io/commit/4146e3d96a131944fe0e8576e71ad8c951472b11).

Thanks again for your work on this. Mostly everything you wrote I kept; it just needed to be shuffled around a bit. :-)

---

_Closed by @BurntSushi on 2019-04-06 14:35_

---

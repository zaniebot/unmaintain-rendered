```yaml
number: 1697
title: UTF-8 BOM Sniffing
type: pull_request
state: closed
author: alessandroasm
labels:
  - rollup
assignees: []
base: master
head: utf8-bom-sniffing
created_at: 2020-10-02T20:24:01Z
updated_at: 2021-06-01T01:51:24Z
url: https://github.com/BurntSushi/ripgrep/pull/1697
synced_at: 2026-01-12T18:23:14Z
```

# UTF-8 BOM Sniffing

---

_@alessandroasm_

UTF-8 encoded files with BOM didn't sniff the BOM from results, regardless of
config.bom_sniffing; ripgrep already implemented this option for UTF-16 files
correctly.

Fixes #1638

---

_Review comment by @BurntSushi on `crates/searcher/src/searcher/mod.rs`:798 on 2020-10-03 13:14_

I think it would be better to implement this the same way that UTF-16 is. Namely, tweak this to:

```
(self.config.bom_sniffing && slice_has_bom(slice))
```

and then create `slice_has_bom` by merging `slice_has_utf16_bom` and `slice_has_utf8_bom`. And then remove the `slice_has_utf8_bom_to_sniff` helper that you created and the corresponding check in `search_slice`. The `search_reader` implementation should then handle it.

The main problem here is that UTF-8 BOM stripping only _sometimes_ doesn't work. Namely, it doesn't work in precisely the case where ripgrep searches via memory maps. That's why, if a UTF-16 BOM is detected, then it falls back to `search_reader` because `search_reader` will do all the necessary transcoding and BOM stripping.

e.g., looking at #1638, while `rg foo utf8bom --json` does indeed show the bug, `rg foo utf8bom --json --no-mmap` does not, because `search_reader` gets this right. So I think we should just divert to that like we do for UTF-16.

---

_@BurntSushi requested changes on 2020-10-03 13:15_

Thank you! While I agree this fixes the issue, I think it would be simpler to fix it in the same way that the UTF-16 BOM is handled. See my comment below.

Also, can you add a an integration test in `tests/regression.rs` as well? Thanks!

---

_@alessandroasm reviewed on 2020-10-03 14:48_

---

_Review comment by @alessandroasm on `crates/searcher/src/searcher/mod.rs`:798 on 2020-10-03 14:48_

Thanks for reviewing this! I'll push those changes you requested shortly.

https://github.com/BurntSushi/ripgrep/blob/def993bad1a9275cdc249f04048e5b2065b79f05/crates/searcher/src/searcher/mod.rs#L748-L752
When implementing the fix, this comment on line 748 implies that `search_reader` doesn't have the same perfomance that the other methods have. That's why I implemented UTF-8 BOM handling after `search_reader` option is discarded. But indeed the code will look much cleaner with all BOM sniffing done by `search_reader`.


---

_@alessandroasm reviewed on 2020-10-03 14:53_

---

_Review comment by @alessandroasm on `crates/searcher/src/searcher/mod.rs`:798 on 2020-10-03 14:53_

https://github.com/BurntSushi/ripgrep/blob/def993bad1a9275cdc249f04048e5b2065b79f05/crates/searcher/src/searcher/mod.rs#L976-L980
The same performance warning is mentioned on `slice_has_utf16_bom`

---

_Review requested from @BurntSushi by @alessandroasm on 2020-10-05 11:21_

---

_@BurntSushi approved on 2021-05-30 17:02_

Thank you!

---

_Label `rollup` added by @BurntSushi on 2021-05-30 17:02_

---

_@BurntSushi reviewed on 2021-05-30 17:33_

---

_Review comment by @BurntSushi on `crates/searcher/src/searcher/mod.rs`:798 on 2021-05-30 17:33_

Yes. Technically, mmaps can be a bit faster in some cases. But UTF-8 BOM's are quite rare, and it's not worth the code complexity IMO.

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

```yaml
number: 356
title: Add option --max-columns to truncate long lines
type: pull_request
state: closed
author: RalfJung
labels: []
assignees: []
base: master
head: longlines
created_at: 2017-02-11T18:30:05Z
updated_at: 2017-03-13T08:54:36Z
url: https://github.com/BurntSushi/ripgrep/pull/356
synced_at: 2026-01-12T18:23:12Z
```

# Add option --max-columns to truncate long lines

---

_@RalfJung_

This implements what we discussed in <https://github.com/BurntSushi/ripgrep/issues/129#issuecomment-276946474>.

One open question from my side: how bad is it that by always using `CountingReplacer`, we lose the optimization offered by returning something from `Replacer::no_expansion`? In principle, we could use `CountingReplacer` only if `max_columns` is `Some`, therefore only incurring a performance cost if `--max-columns` is set. Is that worth the additional effort?

TODO: Add some tests.

Fixes #129.

---

_Review comment by @BurntSushi on `src/app.rs`:462 on 2017-02-18 16:47_

We should try to keep a consistent style. One space after periods please. :-)

---

_Review comment by @BurntSushi on `src/printer.rs`:291 on 2017-02-18 16:50_

I think we should write code that is consistent with the style of the surrounding code. Could you please adhere to a 79 column limit (inclusive)? Thanks.

---

_Review comment by @BurntSushi on `src/printer.rs`:295 on 2017-02-18 16:53_

`if self.max_columns.map_or(false, |m| line.len() > m)`

---

_Review comment by @BurntSushi on `src/printer.rs`:297 on 2017-02-18 16:55_

I don't think there should be a leading space here.

---

_Review comment by @BurntSushi on `src/printer.rs`:296 on 2017-02-18 16:55_

I think I would rather see this use the match color instead of the line number color.

---

_Review comment by @BurntSushi on `src/printer.rs`:315 on 2017-02-18 16:56_

`if self.max_columns.map_or(false, |m| buf.len() > m)`

---

_Review comment by @BurntSushi on `src/printer.rs`:318 on 2017-02-18 16:57_

I don't think we need the leading space here.

---

_Review comment by @BurntSushi on `src/printer.rs`:317 on 2017-02-18 16:57_

"match" color instead of "line" color.

---

_Review comment by @BurntSushi on `src/printer.rs`:365 on 2017-02-18 16:58_

`if self.max_columns.map_or(false, |m| end - start > m)`

---

_Review comment by @BurntSushi on `src/printer.rs`:367 on 2017-02-18 16:58_

Drop leading space.

---

_@BurntSushi requested changes on 2017-02-18 17:00_

The overall approach looks OK, but I have several nits, and I do think tests are necessary before this can be merged.

I think something we need to think about are the likely features that will be requested as a result of this addition. I can imagine, for example, that some folks may not want to see the various "ignored long line" messages, might want to drop the long lines completely. We don't need to add that functionality in this PR, but if we did, how would it impact this feature?

---

_Comment by @BurntSushi on 2017-02-18 17:01_

@RalfJung Thank you for working on this. :-)

---

_Comment by @BurntSushi on 2017-03-13 01:11_

I'd like to push a new release out soon and you got this PR so close to completion that I picked it up in #406. Thanks so much!

---

_Closed by @BurntSushi on 2017-03-13 01:11_

---

_Comment by @RalfJung on 2017-03-13 08:54_

Ah, so sorry for not coming back to this earlier. Busy times. I was hoping to do something this week. Thanks for completing this!

---

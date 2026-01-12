```yaml
number: 1390
title: Add --no-context-separator option
type: pull_request
state: closed
author: MoSal
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2019-09-26T11:57:50Z
updated_at: 2020-02-17T22:16:31Z
url: https://github.com/BurntSushi/ripgrep/pull/1390
synced_at: 2026-01-12T18:23:13Z
```

# Add --no-context-separator option

---

_@MoSal_

 `--context-separator=""` still adds a new line separator, and changing
 that would be a breaking change.

 This new option has clear intent, and preserves backward compatibility.

---

_Comment by @BurntSushi on 2019-09-26 12:04_

What is your use case for this?

If we can make `--context-separator ''` work, then I'd prefer that. Even if it's technically a breaking change, that's the kind of change I'm generally okay with. We can just introduce it in the ripgrep 12 release.

---

_Comment by @MoSal on 2019-09-26 13:36_

> What is your use case for this?

Let's take filtering M3U playlist files as an example. One can easily get the URL/file corresponding to the filtered playlist item with `-A1`. So basically you can `echo #EXTM3U` which is the header, and do the filtering, and you get a valid sub-playlist as output.

> If we can make --context-separator '' work, then I'd prefer that.

The issue with that solution is providing a way for users to get the old behavior back. Using `\n` as a separator would give two empty lines not one. If we special-case `\n`, then `\n` would print one new line and `\n\n` would print three!

If you still want to go that route, the patch is ready.

---

_Review comment by @BurntSushi on `src/app.rs`:1377 on 2019-09-26 14:42_

We should remove the `-X` short flag here. I see this as an extremely niche use case, and not deserving of one of the few remaining short options.

---

_@BurntSushi requested changes on 2019-09-26 14:42_

I see. OK, I think I buy this, but we should not add a short flag for this.

---

_Renamed from "Add -X/--no-context-separator option" to "Add --no-context-separator option" by @MoSal on 2019-09-26 15:20_

---

_Closed by @BurntSushi on 2020-02-15 13:24_

---

_Reopened by @BurntSushi on 2020-02-15 13:24_

---

_Label `rollup` added by @BurntSushi on 2020-02-15 15:21_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

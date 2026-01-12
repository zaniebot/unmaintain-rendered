```yaml
number: 694
title: Omit context separators when using a contextless option like -c or -l
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: bug/omit-context-separator
created_at: 2017-11-27T18:56:18Z
updated_at: 2017-11-29T17:55:43Z
url: https://github.com/BurntSushi/ripgrep/pull/694
synced_at: 2026-01-12T18:23:13Z
```

# Omit context separators when using a contextless option like -c or -l

---

_@okdana_

Fixes #693.

When `rg` has been placed in a 'contextless mode' by the use of `--count`, `--files-with-matches`, or `--files-without-match`, it should omit the context separator even if given a context option like `-A` or `-C`. This matches how (e.g.) `grep` behaves.

Before:
```
% \rg -cC1 context.separator
complete/_rg:1
--
doc/rg.1.md:1
--
src/app.rs:3
--
src/printer.rs:9
--
src/args.rs:7
```

After:
```
% target/release/rg -cC1 context.separator
complete/_rg:1
doc/rg.1.md:1
src/app.rs:3
src/args.rs:7
src/printer.rs:9
```

---

_@okdana reviewed on 2017-11-27 18:58_

---

_Review comment by @okdana on `src/args.rs`:164 on 2017-11-27 18:58_

I changed the `>0` test slightly just so the condition would fit on one line 🤷‍♀️ 

---

_@BurntSushi reviewed on 2017-11-29 14:02_

---

_Review comment by @BurntSushi on `src/args.rs`:164 on 2017-11-29 14:02_

Haha nice, we've all been there. But... I actually think this changes behavior. I think if someone did `rg -A 9223372036854775808 -B 9223372036854775808 foo`, then the sum would wrap to exactly `0` and therefore not show any context at all. (Do we really care? Probably not...) Probably best to just use the same condition and wrap the conditional (or lift it out of the `if` into its own intermediate name). Or perhaps even better, put the logic into a method or something. Up to you!

---

_@okdana reviewed on 2017-11-29 16:29_

---

_Review comment by @okdana on `src/args.rs`:164 on 2017-11-29 16:29_

Oh, i didn't think about that. I've switched it back. Let me know if the style is wrong

---

_@BurntSushi approved on 2017-11-29 16:40_

Nice thanks :-)

---

_Comment by @BurntSushi on 2017-11-29 17:55_

Bah, looks like darwin is backlogged: https://www.traviscistatus.com/ --- Looks good enough for me.

---

_Merged by @BurntSushi on 2017-11-29 17:55_

---

_Closed by @BurntSushi on 2017-11-29 17:55_

---

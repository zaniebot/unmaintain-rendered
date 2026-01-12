```yaml
number: 2683
title: "printer: implement --max-columns-preview-before"
type: pull_request
state: closed
author: kkocdko
labels: []
assignees: []
draft: true
base: master
head: master
created_at: 2023-12-11T16:35:03Z
updated_at: 2025-07-04T14:18:39Z
url: https://github.com/BurntSushi/ripgrep/pull/2683
synced_at: 2026-01-12T18:23:14Z
```

# printer: implement --max-columns-preview-before

---

_@kkocdko_

See #1352 .

---

_Comment by @kkocdko on 2023-12-11 17:27_

Currently:

```sh
rg --max-columns 30 --max-columns-preview --max-columns-preview-before 10 --per-match '=require\('
```

![screenshot_0](https://github.com/BurntSushi/ripgrep/assets/31189892/ccd67b78-c598-4b84-a807-32e824790a21)

Should we reduce the tips text length? And replace the "0 more matches" with "[... omitted end of long line]" when per-match enabled?

---

_Review comment by @kkocdko on `crates/core/flags/defs.rs`:3787 on 2023-12-11 22:24_

Maybe a better name?

---

_Review comment by @kkocdko on `crates/core/flags/hiargs.rs`:68 on 2023-12-11 22:30_

We can combine this to `max_columns_preview: Option<u64>`, but will cause breaking changes. Is this acceptable?

---

_Review comment by @kkocdko on `crates/core/flags/defs.rs`:5401 on 2023-12-11 22:30_

Expose this to cli.

---

_Review comment by @kkocdko on `crates/printer/src/standard.rs`:1384 on 2023-12-11 22:33_

Compare to the full `--column-context-before` + `--column-context-after` support that needs to modify everywhere, the implementation code here is quiet simple.

---

_@kkocdko reviewed on 2023-12-11 22:34_

---

_Comment by @kkocdko on 2024-01-14 14:31_

Hi, @BurntSushi , what is your opinion about this design? Many people work on JavaScript bundler and web reverse engineering rely on this feature because we needs to deal with very long line and many huge files.

It's fine if you don't like this. I will still maintenance this patch for my friends.

---

_Comment by @BurntSushi on 2024-01-14 14:54_

Dunno. I haven't reviewed this yet. I find the flag name long and overall unwieldy to use IMO. And I'm immediately skeptical of exposing `per-match` due to the problems with `--vimgrep`. (The size of the output is worst case quadratic in the size of the input.)

I don't have any timeline on when I'll review this, sorry.

To be clear, I'm open to adding some kind of feature for this functionality.

---

_Comment by @BurntSushi on 2025-07-04 14:18_

I think I'm going to pass on this patch. I don't think this is the right way to approach this problem. I think that if we're going to have this functionality, we should do it better than forcing each match to print on its own line. It's a clever hack to keep the implementation simple, I'll give you that, but this isn't something I want to commit to long term.

---

_Closed by @BurntSushi on 2025-07-04 14:18_

---

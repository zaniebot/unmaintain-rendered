```yaml
number: 6009
title: "feat(clap-complete): Introduce fallible generator"
type: pull_request
state: merged
author: gtema
labels: []
assignees: []
merged: true
base: master
head: complete_try_generate
created_at: 2025-05-22T15:29:03Z
updated_at: 2025-05-27T17:24:06Z
url: https://github.com/clap-rs/clap/pull/6009
synced_at: 2026-01-12T16:14:17Z
```

# feat(clap-complete): Introduce fallible generator

---

_@gtema_

It may happen that writing completion fails due to whichever reason. Currently clap-complete panics without letting calling code to react on that error (i.e. ignore). Introduce fallible version of the `generate` function by mostly only getting rid of except and instead exposing Result.  For backwards compatibility make default trait implementation of `try_generate` function to call `generate` not to break users defining their own `generate`.

Issue: #5993

<!--
Thanks for helping out!

Please link the appropriate issue from your PR.

If you don't have an issue, we'd recommend starting with one first so the PR can focus on the
implementation (unless its an obvious bug or documentation fix that will have
little conversation).
-->


---

_@epage reviewed on 2025-05-23 14:38_

---

_Review comment by @epage on `clap_complete/src/aot/generator/mod.rs`:73 on 2025-05-23 14:38_

>  For backwards compatibility make default trait implementation of generate function to call try_generate.

This needs to go the other way: `try_generate` needs to have an inherent function that calls `generate`

---

_@gtema reviewed on 2025-05-26 07:51_

---

_Review comment by @gtema on `clap_complete/src/aot/generator/mod.rs`:73 on 2025-05-26 07:51_

right, I needed to force myself to do this against the normal sense but I totally understand why is it required. Added note in the code to explain and leave a reminder to turn it around once a major release is being prepared.

---

_@epage reviewed on 2025-05-26 17:30_

---

_Review comment by @epage on `clap_complete/src/aot/generator/mod.rs`:102 on 2025-05-26 17:30_

I am not a fan of tracking backlog items in code; that should be handled through issues.  As-is, this will not be acted on for the next release.

I also do not like user names being littered through code.  It is irrelevant noise that can be determined through `git blame`.

---

---
number: 6181
title: "docs(complete): Fix instructions for sourcing PowerShell completions"
type: pull_request
state: merged
author: jgreitemann
labels: []
assignees: []
merged: true
base: master
head: jg/kqvvzuzuzywv
created_at: 2025-11-09T15:55:07Z
updated_at: 2025-11-10T20:03:02Z
url: https://github.com/clap-rs/clap/pull/6181
synced_at: 2026-01-07T13:12:20-06:00
---

# docs(complete): Fix instructions for sourcing PowerShell completions

---

_Pull request opened by @jgreitemann on 2025-11-09 15:55_

The previous instructions were wrong in that they set the `COMPLETE` env var only while the `$PROFILE` script was written. This was akin to

    COMPLETE=bash echo "source <(your_program)" >> ~/.bashrc

whereas the corrected version sets `COMPLETE` as part of the `$PROFILE` script, mirroring the instructions for sourcing completion in the other shells.

Extending `$PROFILE` in this way also requires that scripts are allowed to be executed. This requires changing the execution policy from its default value `Restricted` on Windows. There's no such issue when using PowerShell on macOS or Linux.

<!--
Thanks for helping out!

Please link the appropriate issue from your PR.

If you don't have an issue, we'd recommend starting with one first so the PR can focus on the
implementation (unless its an obvious bug or documentation fix that will have
little conversation).
-->


---

_Review comment by @epage on `clap_complete/src/env/mod.rs`:40 on 2025-11-10 15:46_

Please revert the unrelated changes

---

_@epage reviewed on 2025-11-10 15:46_

---

_Review comment by @jgreitemann on `clap_complete/src/env/mod.rs`:40 on 2025-11-10 16:21_

Ok, sorry about conflating things. My thinking was that a little structure would help to discern the Powershell-related note from the shell "headings". In particular, I'm still not 100% sure whether the line "To disable completions, you can set `COMPLETE=` or `COMPLETE=0`" is supposed to apply to all shells or just zsh.

Do you want me to revert the list entirely, or do you just want me to split the commits into one reformatting the module doc comment and one fixing the PowerShell bug? If you don't like the list, I could instead reformat this page further to introduce hierarchy though different levels of Markdown headings.

---

_@jgreitemann reviewed on 2025-11-10 16:21_

---

_Review comment by @epage on `clap_complete/src/env/mod.rs`:40 on 2025-11-10 16:31_

If we move forward with it, it should be two commits.  By isolating the diffs, it makes it easier to inspect the content under review.

I am not thrilled with using a bullet list for the shells.  I'm fine with them being bolded.

No strong thoughts on how to handle the note for Powershell or our generic note at the end.

---

_@epage reviewed on 2025-11-10 16:31_

---

_@jgreitemann reviewed on 2025-11-10 17:12_

---

_Review comment by @jgreitemann on `clap_complete/src/env/mod.rs`:40 on 2025-11-10 17:12_

Split off the bold headings, removed the bullet list. Got no solution for the generic note, so it remains ambiguous for now.

---

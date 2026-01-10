---
number: 5985
title: "bash: improve current-word detection"
type: pull_request
state: merged
author: mernen
labels: []
assignees: []
merged: true
base: master
head: bash-cur
created_at: 2025-05-05T16:30:54Z
updated_at: 2025-05-11T22:11:41Z
url: https://github.com/clap-rs/clap/pull/5985
synced_at: 2026-01-10T01:28:26Z
---

# bash: improve current-word detection

---

_Pull request opened by @mernen on 2025-05-05 16:30_

Fixes #5979, along with a few other issues:

- Dynamic completion didn't work properly when the argument is quoted (e.g. `exhaustive 'hi`) — static completion works because `compgen` apparently ignores leading quotes in the input word
- Both static and dynamic completion gave wrong results when the cursor was not at the end of the word (e.g. `exhaustive --h⇥XYZ` should result in `cargo --helpXYZ`, where `⇥` represents the position the cursor should be when pressing Tab)

The fix for static completion is fairly straightforward: rather than `cur="${COMP_WORDS[COMP_CWORD]}"`, we use `cur="$2"`. `$2` omits leading quotes (which are inserted back by Bash automatically) and only returns the word up until the cursor point. The latter point is what also addresses #5979.

I couldn't find any downsides to this in Bash 4+ — back in 3.x days `$COMP_WORDS` would ignore `$COMP_WORDBREAKS`, so this was a way to e.g. capture `--foo=`, but sadly Bash's behavior keeps changing in subtle ways. I included a conditional to keep old behavior for <4, as it seems the least bad option.

The change for dynamic completion, though, is quite the hack — to keep the same contract and minimize impact, I still pass `$COMP_WORDS` as arguments to the binary, but replace `$COMP_WORDS[$COMP_CWORD]` with `$2`. For the common case (cursor at the last position) everything I said for static completion still applies, but otherwise it does destroy some information — for example, if the user types `exhaustive hint ⇥ --dir /foo`, the binary will receive the arguments `["exhaustive", "hint", "", "/foo"]`, completely missing `--dir`. No issue for completers that don't inspect the argument list, but if `--dir /foo` was for example relevant for context, it wouldn't be parsed. This can be worked around by injecting `${COMP_LINE:$COMP_POINT}` until the rest of the word as a new item in the array, but I found it caused a few other corner cases, so I'm not sure it's worth the trouble.

Completely by accident, this also partially, arguably improves `--foo=` Bash ≥4 — the binary now receives `["--foo", ""]` and has one shot at providing a completion. If any character is entered, however, `--foo=b` turns into `["--foo", "=", "b"]`, and stops working again. I'm afraid the way to *properly* solve this along with the previously mentioned loss of data is to parse `$COMP_LINE`, as was previously discussed on #5512.


---

_@epage reviewed on 2025-05-05 19:11_

---

_Review comment by @epage on `clap_complete/src/aot/shells/bash.rs`:28 on 2025-05-05 19:11_

I'm much more of a fan of explicit conditionals (e.g. our other version check in here)

---

_@epage reviewed on 2025-05-05 19:12_

---

_Review comment by @epage on `clap_complete/src/env/shells.rs`:41 on 2025-05-05 19:12_

ditto

---

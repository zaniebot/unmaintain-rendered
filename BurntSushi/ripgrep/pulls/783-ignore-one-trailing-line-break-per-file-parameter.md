```yaml
number: 783
title: Ignore (one) trailing line break per --file parameter
type: pull_request
state: closed
author: mqudsi
labels: []
assignees: []
base: master
head: ignore_trailing_line
created_at: 2018-02-09T18:43:42Z
updated_at: 2018-02-10T01:34:05Z
url: https://github.com/BurntSushi/ripgrep/pull/783
synced_at: 2026-01-12T18:23:13Z
```

# Ignore (one) trailing line break per --file parameter

---

_@mqudsi_

Currently ripgrep interprets a trailing line break at the end of a
pattern file specified via `--file` or similar as a global "match-all"
pattern. Given that many editors are configured to automatically add a
trailing new line at the end of a file on save, this is not necessarily
a good thing.

This patch removes a single new line from the end of each pattern file
contents by simply dropping an empty rule from the pattern vector if it
is the last rule in the vector after loading rules from a file. Multiple
line breaks are not ignored, and no overhead is added by omitting this
check from within the for loop (not that it would really matter since
this is just init time). Additionally, line breaks not at the end of the
file are similarly kept.

---

_@mqudsi reviewed on 2018-02-09 18:48_

---

_Review comment by @mqudsi on `src/args.rs`:514 on 2018-02-09 18:48_

This can be replaced with `while Some(....)` to make it drop all trailing line breaks.

---

_Comment by @BurntSushi on 2018-02-09 19:01_

@mqudsi Thanks! Could you add a test for this in `tests/tests.rs`?

---

_Comment by @mqudsi on 2018-02-09 19:36_

@BurntSushi I did, but to be honest, I wasn't sure what it should look like. I tried to just copy the syntax from a nearby test.

---

_Comment by @BurntSushi on 2018-02-09 21:21_

Hmm... OK, so it occurred to me that these are exactly the kinds of cases where it is good to be consistent with other tools. grep supports this flag, and it actually looks like ripgrep's current behavior is consistent with grep's behavior. To me, this is a subtle enough of a change that it makes more sense to be consistent with existing tools instead of trying to be smarter.

Apologies for asking you to write a test and then rejecting the PR, but the test does look good! Thank you!

---

_Closed by @BurntSushi on 2018-02-09 21:21_

---

_Comment by @mqudsi on 2018-02-10 01:22_

No problem. Perhaps I'm missing something however - is there ever a time where you'd want a match-anything rule (loaded from a file, at that)?

---

_Comment by @BurntSushi on 2018-02-10 01:24_

@mqudsi Possibly. At least for GNU grep, it might be useful as a trick to show every line and color only those things which match. ripgrep does have a `--passthru` flag (recently added) that lets you explicitly do this. It's also consistent with the fact that the empty file will match every pattern.

With that said, even if there weren't a compelling use case (and honestly, the above isn't particularly compelling to me), then matching existing behavior with well established tools in subtle cases like this is important for its own sake.

---

_Comment by @mqudsi on 2018-02-10 01:25_

Cheers!

---

```yaml
number: 1931
title: Support line filter / diff input
type: issue
state: closed
author: xmo-odoo
labels:
  - question
  - core
assignees: []
created_at: 2023-01-17T10:29:01Z
updated_at: 2023-01-17T15:40:04Z
url: https://github.com/astral-sh/ruff/issues/1931
synced_at: 2026-01-12T15:54:41Z
```

# Support line filter / diff input

---

_@xmo-odoo_

This is probably a use case which is more relevant for large (and old) codebases, when trying to "ratchet" rules progressively: in CI, it's useful to only check / fail on failures of the code which has been modified, as it allows progressively improving code-state without needing massive one-shot rewrites which can make history traversal and blames a lot more complicated.

This can be done by post-filtering finds emitted by ruff, but it would be useful as a pre / builtin feature, as it would avoid having to provide both input (`--include`/`--exclude`) and output (lines) filters.

---

_Comment by @not-my-profile on 2023-01-17 10:41_

I think it would make more sense to have a dedicated tool for that, which supports one of the output formats of ruff so then you could just pipe the output of ruff to that filter tool e.g.

```
ruff . --format=junit | junit_filter_git
```

I see no reason why this should be implemented in `ruff` directly.

---

_Comment by @xmo-odoo on 2023-01-17 10:57_

> I see no reason why this should be implemented in `ruff` directly.

This objection is literally covered by the second paragraph.

---

_Comment by @not-my-profile on 2023-01-17 11:24_

Ruff is fast enough that you could just run it on the whole codebase and have the filter program filter out violations for all other files. If you care about wasted CPU cycles you could just invoke ruff like:

```
ruff  --format=junit -- ${{needs.changedfiles.outputs.all}} | junit_filter_git
```

where `${{needs.changedfiles.outputs.all}}` would be some variable listing all the changed files, see for example [this explanation for GitHub](https://dev.to/scienta/get-changed-files-in-github-actions-1p36).

Something like this could very well be provided by a GitHub action so you wouldn't even have to configure that yourself. So yes I still see no reason why this should be implemented in `ruff` directly.

---

_Comment by @charliermarsh on 2023-01-17 12:29_

Thanks for filing :) I see the value in what you're describing, and -- since it's come up here -- in general, I'm open to implementing things directly in Ruff that could be accomplished by chaining together other tools. Not always, since everything we implement comes with some upfront and ongoing cost, but on a case-by-case basis. (One way I think about this: Black has Jupyter support built-in, even though the same behavior can be accomplished with `nbQA`, and that's useful.)

I think in this case, I'm unlikely to prioritize it if there's a reasonable workaround so I'd love to better understand the use-case. Are you imaging that this would only run over changed files? Or over changed lines? How do you think Ruff should determine the changed files?


---

_Label `question` added by @charliermarsh on 2023-01-17 12:30_

---

_Label `core` added by @charliermarsh on 2023-01-17 12:30_

---

_Comment by @xmo-odoo on 2023-01-17 12:44_

> Not always, since everything we implement comes with some upfront and ongoing cost, but on a case-by-case basis. [...] I think in this case, I'm unlikely to prioritize it if there's a reasonable workaround

Certainly fair enough.

> so I'd love to better understand the use-case. Are you imaging that this would only run over changed files? Or over changed lines?

I'd assume ruff would need to run over the entire file in order to correctly analyze the contents. Especially as the modified lines might not even provide a valid AST subtree. This'd be more of a "one-stop" shop to filter (`include`/`exclude`) and output (lines filter).

>  How do you think Ruff should determine the changed files?

I know that some tools can take a unified diff as input and extract the filtering from that, but I don't know if there's a ready-made crate for this, so as described above an alternative could be to extend the include/exclude arguments to allow file subranges.

---

_Comment by @charliermarsh on 2023-01-17 12:45_

> I'd assume ruff would need to run over the entire file in order to correctly analyze the contents. Especially as the modified lines might not even provide a valid AST subtree. This'd be more of a "one-stop" shop to filter (include/exclude) and output (lines filter).

Ah sorry, what I meant to ask was: would you expect to see errors reported only from changed _files_ (more errors), or from changed _files_ (stricter)?

---

_Comment by @xmo-odoo on 2023-01-17 13:40_

> Ah sorry, what I meant to ask was: would you expect to see errors reported only from changed _files_ (more errors), or from changed _files_ (stricter)?

Assuming the second occurrence of _files_ should be _line_, then that.

And yes I do realise that a change in one line can cause an error in an other (which was not touched), but I think getting this perfect (or as close to as possible) would be complicated as it would be necessary to start from a snapshot of the full output, take a new full output (or at least file-wise in both cases since I assume ruff currently mostly operate on a per-file basis much as flake does), then use the diff's information to adjust all the messages in order to match pre- and post- content to determine whether an error is truly novel or is just a pre-existing error moved a few lines because code was inserted somewhere above it.

---

_Comment by @charliermarsh on 2023-01-17 14:22_

Uhh, yes, second occurrence should be _line_! Sorry about that.

Ok noted, let's keep this open! (There's also #1149 which accomplishes the same thing IIUC.)

---

_Comment by @xmo-odoo on 2023-01-17 14:30_

> Uhh, yes, second occurrence should be _line_! Sorry about that.

No worries.

> Ok noted, let's keep this open! (There's also #1149 which accomplishes the same thing IIUC.)

Oh I missed it sorry, didn't know about the term "baseline" (and almost certainly made my search using keywords like "diff", "ci", "git", ...).

If that's an option / consideration then it would be *a lot* better than this here proposal (which is a lot more simplistic).

---

_Comment by @charliermarsh on 2023-01-17 15:40_

> Oh I missed it sorry, didn't know about the term "baseline" (and almost certainly made my search using keywords like "diff", "ci", "git", ...).

Me neither, before that Issue :)

> If that's an option / consideration then it would be a lot better than this here proposal (which is a lot more simplistic).

It is! I'll close this for now in favor of that issue then.

---

_Closed by @charliermarsh on 2023-01-17 15:40_

---

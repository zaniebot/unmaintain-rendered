---
number: 4307
title: "Generate `Fix` for suppressing violations"
type: issue
state: open
author: MichaReiser
labels:
  - core
assignees: []
created_at: 2023-05-09T12:58:32Z
updated_at: 2024-04-02T14:33:32Z
url: https://github.com/astral-sh/ruff/issues/4307
synced_at: 2026-01-10T01:22:43Z
---

# Generate `Fix` for suppressing violations

---

_Issue opened by @MichaReiser on 2023-05-09 12:58_

This is not fully-fledged, it's more a description of an idea that I would like to explore. 

Currently, Ruff serializes the `noqa_row` as part of the JSON messages and the VS code plugin uses that row number to insert the `noqa` comment at the right place. The issue that I have with the `noqa_row` is that it leaks internal details and complicates the LSP because it has to know how to correctly add a `noqa` comment. 

What if ruff would instead generate a second `Fix` to add the `noqa` suppression comment? 

---

_Comment by @bluetech on 2023-05-09 14:38_

I had the same thought: https://github.com/charliermarsh/ruff-lsp/pull/76#issuecomment-1454257946

It does feel much cleaner, however these are not regular fixes (+ it makes every lint "fixable"...) so should be distinguished in some way - how to do it?

---

_Label `core` added by @charliermarsh on 2023-05-09 15:33_

---

_Comment by @MichaReiser on 2023-05-09 16:22_

> I had the same thought: https://github.com/charliermarsh/ruff-lsp/pull/76#issuecomment-1454257946

Oh nice, I didn't know about this. Thanks for cross-referencing! 

> It does feel much cleaner, however these are not regular fixes (+ it makes every lint "fixable"...) so should be distinguished in some way - how to do it?

Hmm, that's a good point. I guess we could either:

* Use the newly introduced `Fix::applicability` and only show violations as *fixable* if they have at least one non-manual fix. 
* This would be harder to do in Ruff today because it doesn't support this capability, but we could provide this as a code action if the cursor is positioned in a range with a violation. 

---

_Comment by @bluetech on 2023-05-10 09:44_

I was thinking, since this is not useful for regular cli usage, but only for LSP-like scenarios, that the LSP should just tell Ruff "please generate these", in one way or another.

---

_Comment by @T-256 on 2024-01-04 04:36_

Since it'd be used mostly by LSP, I'd recommend hidden CLI option `--suggest-noqa`.


---

_Comment by @dhruvmanila on 2024-02-12 05:41_

\cc @snowsignal You might be interested in this as it would easier to implement in the native LSP implementation.

---

_Comment by @MichaReiser on 2024-03-30 09:10_

@snowsignal do you think this issue is still relevant after your new LSP implementation?

---

_Comment by @snowsignal on 2024-04-02 14:33_

@MichaReiser I think this is still relevant because we still need to support noqa suppression comments in the new extension, which calls into `ruff_linter` directly. We'll still need to figure out how to generate fixes for noqa comments, though it would probably be a separate method called after the linter is run.

Re: https://github.com/astral-sh/ruff/issues/4307#issuecomment-1540490537
> This would be harder to do in Ruff today because it doesn't support this capability, but we could provide this as a code action if the cursor is positioned in a range with a violation.

With the new Ruff LSP, we should be able to support this, though it will require the implementation of diagnostic caching first.

---

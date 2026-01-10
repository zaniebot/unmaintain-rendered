```yaml
number: 7352
title: "CLI: Show fixes by default in `ruff check`"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2023-09-13T16:21:19Z
updated_at: 2025-08-29T13:53:06Z
url: https://github.com/astral-sh/ruff/issues/7352
synced_at: 2026-01-10T11:09:49Z
```

# CLI: Show fixes by default in `ruff check`

---

_Issue opened by @zanieb on 2023-09-13 16:21_

Requires #7350 for opt-out of display.

`ruff check` should display fixes that Ruff can apply. Currently this is only possible for the whole file with `--diff`.

Some fixes may modify content across a file, e.g. when renaming a binding. We should limit the display of fixes to roughly match the range of source context displayed (#7349). If we do not display the entire fix, we should include a notice of the number of additional lines that will be modified if the fix is applied. Naively, we may be able to implement this by only selecting a portion of the fix. In a more comprehensive implementation, we may need to add a new diagnostic range attribute for this display and address it in each problem case.

> Clippy has a nice example user experience of showing fixes:
> 
> 
> ```
> error[E0308]: mismatched types
> --> crates/ruff_python_semantic/src/model.rs:224:21
>     |
> 224 |             source: &self.stmt_id,
>     |                     ^^^^^^^^^^^^^ expected `Option<NodeId>`, found `&Option<NodeId>`
>     |
>     = note:   expected enum `std::option::Option<node::NodeId>`
>             found reference `&std::option::Option<node::NodeId>`
> help: consider removing the borrow
>     |
> 224 -             source: &self.stmt_id,
> 224 +             source: self.stmt_id,
>     |
> 
> For more information about this error, try `rustc --explain E0308`.
> ```

---

_Label `cli` added by @zanieb on 2023-09-13 16:26_

---

_Comment by @pjonsson on 2024-03-28 12:08_

I came here from #8677 because I wanted a non-zero exit code when there are things to be fixed in my source code (even if Ruff can't fix them).

Based on the description of this issue, I don't understand the details but I get the impression it will somehow eliminate the need for `--diff`, will `ruff check <without --diff?> --output-format junit -o ruff-check-report.xml` exit with a non-zero exit code and produce junit reports after this is fixed?

---

_Comment by @zanieb on 2024-03-29 20:39_

I'm honestly not sure... this is different than `--diff` in that we just show fixes for the relevant source context. I don't think this would have any affect on the exit code.

---

_Comment by @zanieb on 2024-03-29 20:39_

A note regarding the implementation.. we probably need to gate this with preview to match #7349 

---

_Comment by @pjonsson on 2024-03-30 10:18_

If this has no effect on exit code, I feel that the comments in the tickets concerning exit codes that are linking here (https://github.com/astral-sh/ruff/issues/4093#issuecomment-1920622666, https://github.com/astral-sh/ruff/issues/8677#issuecomment-1810700437, https://github.com/astral-sh/ruff/issues/8584#issuecomment-1803968274) are confusing. I'm not a native speaker, but to me, all those tickets were filed because the user was expecting a non-zero exit code when Ruff is unhappy. I think part of the reason people are surprised is because it's so easy to get the non-zero exit code with `ruff format --diff`.

Since this ticket does not help with exit codes, should the non-zero exit code part of this issue be dealt with by #8677?

---

_Comment by @zanieb on 2024-03-30 14:23_

Ah I see what you're saying:

1. You want to run Ruff and see all the fixes it has to offer
2. The only way to do this now is with `--diff`
3. But when you use `--diff` the exit code is zero

Yes this should address your use-case, in that:

1. We will show fixes by default so `--diff` is not necessary
2. The exit code semantics should be the same as Ruff without `--diff`
3. If any violations are remaining, Ruff will exit with a non-zero code

---

_Closed by @ntBre on 2025-08-29 13:53_

---

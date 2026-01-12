```yaml
number: 11803
title: "[POC] `ruff server` Expand format action capability"
type: pull_request
state: closed
author: T-256
labels:
  - server
assignees: []
base: main
head: format-actions
created_at: 2024-06-08T16:16:01Z
updated_at: 2024-06-12T10:12:59Z
url: https://github.com/astral-sh/ruff/pull/11803
synced_at: 2026-01-12T15:55:39Z
```

# [POC] `ruff server` Expand format action capability

---

_@T-256_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
implement https://github.com/astral-sh/ruff/issues/11756#issuecomment-2155720286 which is currently not ready for notebook documents:
```json
{
    "ruff.format.action": {
        "fix": true,
        "isort": true,
        "style": true,
    }
}
```

This PR also moves `ruff.fixAll` and `ruff.organizeImports` under `ruff.sourceAction` superset.


## Test Plan

Not tested yet

(self-note: also mention revealment issues on ruff-vscode/ruff-lsp)

---

_@T-256 reviewed on 2024-06-08 16:17_

---

_Review comment by @T-256 on `crates/ruff_server/src/fix.rs`:169 on 2024-06-08 16:17_

feel free to rewrite this function. I think it could be better implementation at here.

---

_Comment by @github-actions[bot] on 2024-06-08 17:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `server` added by @snowsignal on 2024-06-10 21:15_

---

_Comment by @snowsignal on 2024-06-11 04:14_

Thank you for opening this! I'll give this a through review tomorrow.

---

_Review requested from @snowsignal by @MichaReiser on 2024-06-11 06:20_

---

_Comment by @charliermarsh on 2024-06-11 12:15_

Meta note: we should decide whether we want to support this functionality before reviewing the implementation itself.

---

_Comment by @T-256 on 2024-06-11 12:43_

> Meta note: we should decide whether we want to support this functionality before reviewing the implementation itself.

For better decision, here is list of issues that this change may have affect on them:
- https://github.com/astral-sh/ruff/issues/11756
- https://github.com/astral-sh/ruff/issues/8232
- https://github.com/astral-sh/ruff-lsp/issues/409
- https://github.com/astral-sh/ruff-lsp/issues/335

Also, in my preference, I don't have any code action/format on save enablement, instead After bunch of time and complete write on that file I manually run `fixAll`, `orginizeImport` and `Format Document` commands to finalizing codes. I'll be happy to have these three commands in one place and by only executing `Format Document` all others executed.

---

_Renamed from "`ruff server` Expand format action capability" to "[POC] `ruff server` Expand format action capability" by @T-256 on 2024-06-11 12:49_

---

_Comment by @gegoune on 2024-06-11 14:08_

As a user I would like to have all of those three actions executed on save. At the moment I am relying on external tool to provide some of that automation (for Neovim) and would really like to have it streamlined.

I can imagine race conditions and ordering issues resulting from using two tools interacting.

Basically, to phrase it differently, since I am already running `ruff server` I would like it to handle `--fix` as well (instead of having to also run it separately). Not sure if that's against LSP protocol though.

---

_Comment by @snowsignal on 2024-06-12 00:00_

@T-256 We're still deliberating on this, but we should have an update soon.

---

_Comment by @MichaReiser on 2024-06-12 06:11_

I'm commenting here because it seems that most of the discussion happened on the PR to this point and not in the issue. 

I agree that this solves a real user problem, but I'm not sure if this, or even a custom command that groups these different actions is the right solution. 

I've two main concerns:

First, the new settings don't work with range formatting. Ruff doesn't support applying *organize imports* or fixing stylistic lints to a sub range. These operations always apply to the entire document. That means that we either can't support range formatting anymore if the options are enabled or that ruff would always "format" the entire document, ignoring the users *format changed lines only* setting. I think either isn't a good outcome. 

The alternative of having a custom command that runs formatting, isort, and stylistic fixes at once suffers from the same problem where.

Second, there are now multiple ways to configure the same behavior, except that the behavior is slightly different. My worry is that users will copy paste together their configuration without fully understanding what they're configuring. I do that too. But I worry that this will lead to "almost" functional configurations that are then hard to debug. What if they have organize imports on save enabled in their VS code settings AND included import sorting as part of formatting. Maybe everything goes well and Ruff just spends a few cycles doing unnecessary import resorting or it leads to very subtle bugs that are hard to reproduce. 

That's why I prefer that we have one way to configure a specific behavior over multiple ones. 

Now, this doesn't solve the problem for editors that don't support running multiple commands on save. Are there editor specific solutions that we could implement (outside of `ruff_server`), e.g. a custom plugin? Are there existing issues for these editors for adding support for running multiple commands on save? What's their progress? 

---

_Comment by @T-256 on 2024-06-12 10:12_

> That means that we either can't support range formatting anymore if the options are enabled or that ruff would always "format" the entire document

As I said in https://github.com/astral-sh/ruff/issues/11756#issuecomment-2155720286, Only _Format Document_ will respect these settings, _Format Selection_ or _Format changed lines on save_ ignores these settings.

Your second concern is True, and I agree to it.

This was just a POC of idea I had in my mind. I close this PR because of it comes with few UX complexity.

---

_Closed by @T-256 on 2024-06-12 10:12_

---

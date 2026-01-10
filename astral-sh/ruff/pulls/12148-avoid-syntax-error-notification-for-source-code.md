```yaml
number: 12148
title: Avoid syntax error notification for source code actions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/syntax-error-code-action
created_at: 2024-07-02T11:13:09Z
updated_at: 2024-07-04T04:34:58Z
url: https://github.com/astral-sh/ruff/pull/12148
synced_at: 2026-01-10T21:56:00Z
```

# Avoid syntax error notification for source code actions

---

_Pull request opened by @dhruvmanila on 2024-07-02 11:13_

## Summary

This PR avoids the error notification if a user selects the source code actions and there's a syntax error in the source.

Before https://github.com/astral-sh/ruff/pull/12134, the change would've been different. But that PR disables generating fixes if there's a syntax error. This means that we can return an empty map instead as there won't be any fixes in the diagnostics returned by the `lint_fix` function.

For reference, following are the screenshot as on `main` with the error:

**VS Code:**

<img width="1715" alt="Screenshot 2024-07-02 at 16 39 59" src="https://github.com/astral-sh/ruff/assets/67177269/62f3e99b-0b0c-4608-84a2-26aeabcc6933">

**Neovim:**

<img width="1717" alt="Screenshot 2024-07-02 at 16 38 50" src="https://github.com/astral-sh/ruff/assets/67177269/5d637c36-d7f8-4a3b-8011-9a89708919a8">

fixes: #11931

## Test Plan

Considering the following code snippet where there are two diagnostics (syntax error and useless semicolon `E703`):
```py
x;

y =
```

### VS Code

https://github.com/astral-sh/ruff/assets/67177269/943537fc-ed8d-448d-8a36-1e34536c4f3e

### Neovim

https://github.com/astral-sh/ruff/assets/67177269/4e6f3372-6e5b-4380-8919-6221066efd5b



---

_Label `bug` added by @dhruvmanila on 2024-07-02 11:13_

---

_Label `server` added by @dhruvmanila on 2024-07-02 11:13_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-02 11:13_

---

_Comment by @github-actions[bot] on 2024-07-02 11:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>




---

_@MichaReiser approved on 2024-07-03 06:41_

The code changes look good, but I'm not sure if we want to keep some indicator to why the action failed? 

Let's say you have a very long document and the syntax error is outside the visible area. It might now be unclear to users why the manually triggered action does *nothing*. 

RustRover does show an error when running `rustfmt` failed because of a syntax error and I find this useful information.

---

_Comment by @dhruvmanila on 2024-07-03 08:41_

> Let's say you have a very long document and the syntax error is outside the visible area. It might now be unclear to users why the manually triggered action does _nothing_.
> 
> RustRover does show an error when running `rustfmt` failed because of a syntax error and I find this useful information.

That is interesting. Does it show on every save? I don't mind providing a notification.

---

_Comment by @MichaReiser on 2024-07-03 09:51_

> That is interesting. Does it show on every save? I don't mind providing a notification.

Only when I issue the *Reformat code* manually. It doesn't show a notification when the formatting on save fails.

---

_Comment by @dhruvmanila on 2024-07-04 04:06_

> Only when I issue the _Reformat code_ manually. It doesn't show a notification when the formatting on save fails.

I think it's difficult for a language server to know whether something was invoked manually or automatically by the client except for code actions for which there's `CodeActionTriggerKind` but that's also optional. I'll make this change in a follow-up PR.

I guess RustRover is able to do this _reliably_ because the entire stack is in their control.

---

_Merged by @dhruvmanila on 2024-07-04 04:07_

---

_Closed by @dhruvmanila on 2024-07-04 04:07_

---

_Branch deleted on 2024-07-04 04:07_

---

_Comment by @dhruvmanila on 2024-07-04 04:34_

I've opened https://github.com/astral-sh/ruff/issues/12176 for now and I can tackle it later.

---

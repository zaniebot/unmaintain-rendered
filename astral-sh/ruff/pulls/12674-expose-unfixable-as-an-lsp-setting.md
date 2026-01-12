```yaml
number: 12674
title: "Expose `unfixable` as an LSP setting"
type: pull_request
state: closed
author: charliermarsh
labels:
  - server
assignees: []
base: main
head: charlie/unfix
created_at: 2024-08-05T02:51:14Z
updated_at: 2024-08-06T06:08:10Z
url: https://github.com/astral-sh/ruff/pull/12674
synced_at: 2026-01-12T15:55:41Z
```

# Expose `unfixable` as an LSP setting

---

_@charliermarsh_

## Summary

It's common for users to want to disable `F401` fixes in the editor. Otherwise, imports you just added get removed on-save. I think we should add this as a first-class editor setting.

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-08-05 02:51_

---

_Label `server` added by @charliermarsh on 2024-08-05 02:51_

---

_Comment by @charliermarsh on 2024-08-05 02:51_

@dhruvmanila -- I feel like, ideally, the Quick Fix actions would still be available here. What do you think?

---

_Comment by @github-actions[bot] on 2024-08-05 03:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-08-05 03:07_

> I feel like, ideally, the Quick Fix actions would still be available here. What do you think?

Yeah, I think that's correct but we need to make sure that it's only available as a _Quick Fix_ actions and not _Source_ code actions. So, the `source.fixAll.ruff` action shouldn't fix this. I can take that up as a follow-up if you want.

---

_Review comment by @dhruvmanila on `docs/editors/settings.md`:500 on 2024-08-05 03:09_

I think you meant to add a new `unfixable` section, right? It seems like this is replacing the `extendIgnore` section.

---

_@dhruvmanila reviewed on 2024-08-05 03:09_

---

_Review comment by @charliermarsh on `docs/editors/settings.md`:500 on 2024-08-05 03:16_

It was intentional to remove though unrelated to the PR. Is extendIgnore supported? I couldn’t find any reference to it in the server crate.

---

_@charliermarsh reviewed on 2024-08-05 03:16_

---

_@dhruvmanila reviewed on 2024-08-05 03:21_

---

_Review comment by @dhruvmanila on `docs/editors/settings.md`:500 on 2024-08-05 03:21_

Oh, that's correct. Not sure why I thought there's a `extendIgnore` when I wrote the docs. Thanks

---

_@dhruvmanila approved on 2024-08-05 03:22_

---

_Comment by @charliermarsh on 2024-08-05 13:01_

@dhruvmanila - Do you think that should apply _always_ for `--unfixable`, or only when it's set through the editor like this?

---

_Comment by @dhruvmanila on 2024-08-05 13:42_

> @dhruvmanila - Do you think that should apply _always_ for `--unfixable`, or only when it's set through the editor like this?

By `--unfixable`, do you mean whether, in the CLI, we should display the fixes but not apply them?

This actually reminds me of `eslint.codeActionsOnSave.rules` which is provided by the Eslint VS Code extension and is a list of rules that should be fixed on save which is similar to `fixable` option.

It seems that the ES Lint extension doesn't apply the code actions if it's negated in the above list but it still shows the diagnostics and provides code actions to fix it manually. This manual application also includes the "Fix all" code action. So, it's only the code actions on save where it's excluded.

I think this is an ideal behavior from an editor perspective and the "manual" / "automatic" invocation of code actions can be known by the server as it's encoded in the request payload.

---

_Comment by @charliermarsh on 2024-08-05 13:53_

Yeah, I think we want something like `codeActionsOnSave` but for rules. So maybe what I have in this PR isn't correct.

---

_Comment by @dhruvmanila on 2024-08-05 14:37_

I guess that's right, we cannot use the existing `fixable` / `unfixable` option without affecting the CLI. We'd want an server (and editor) specific setting and perform the filter at the server level instead.

---

_Comment by @charliermarsh on 2024-08-06 02:28_

Yeah, sounds right to me. So we'd need both a setting in `ruff-server` and then in the VS Code extension to connect it together. I can just close this then.

---

_Closed by @charliermarsh on 2024-08-06 02:28_

---

_Comment by @dhruvmanila on 2024-08-06 06:08_

Opened https://github.com/astral-sh/ruff/issues/12709 to keep track of this.

---

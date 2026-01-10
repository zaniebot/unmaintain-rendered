```yaml
number: 12343
title: Enabling specific preview rules
type: issue
state: open
author: adrinjalali
labels:
  - configuration
assignees: []
created_at: 2024-07-16T09:13:37Z
updated_at: 2025-06-18T09:10:36Z
url: https://github.com/astral-sh/ruff/issues/12343
synced_at: 2026-01-10T11:09:54Z
```

# Enabling specific preview rules

---

_Issue opened by @adrinjalali on 2024-07-16 09:13_

Our usecase https://github.com/scikit-learn/scikit-learn/pull/29477 is that we'd like to enable `CPY001`. Since it's a `preview` rule, one needs to enable preview mode.

However, preview mode enables many other rules which don't work as intended on our repo, give false positives, ...

That's why I looked into enabling preview rules only explicitly. However, when one does something like `select = ["E", "F", "W", "I", "CPY001"]` in the configuration, everything under those categories are enabled, regardless of them being in preview mode or not.

I'd argue that the above `select` doesn't really explicitly select those preview rules under `E`, `F`, ..., however, they're selected.

This is related to https://github.com/astral-sh/ruff/issues/7434, and I think `explicit-preview-rules` introduced in https://github.com/astral-sh/ruff/pull/7390 somewhat intends to fix the issue, but in this case it doesn't.

What should one do in this case?

In effect, I think I'd expect this config:

```
[tool.ruff.lint]
# This enables us to use CPY001: copyright header check
preview = true
# This enables us to use the explicit preview rules that we want only
explicit-preview-rules = true
# all rules can be found here: https://beta.ruff.rs/docs/rules/
select = ["E", "F", "W", "I", "CPY001"]
```
to only include stable rules under those categories, and enable `CPY001` on top of them, and no other preview rule should be enabled.

---

_Comment by @MichaReiser on 2024-07-16 09:56_

Thanks for the detailed write up. 

> However, preview mode enables many other rules which don't work as intended on our repo, give false positives, ...

This is somewhat intentional because we want to encourage users to give feedback on preview rules. Whether we promote a rule to stable depends on the feedback we receive from users. Without feedback, we'll end up promoting a rule that then creates a lot of churn downstream. 

> This is related to https://github.com/astral-sh/ruff/issues/7434, and I think explicit-preview-rules introduced in https://github.com/astral-sh/ruff/pull/7390 somewhat intends to fix the issue, but in this case it doesn't.

Could you explain which part isn't working for you?

I tried to reproduce your setup by enabling the same rules as in your example and using `explcit-preview-rules` ([playground](https://play.ruff.rs/7bc40fd0-2806-44fc-806a-0b89cf2cf819)). To me it is working as expected because Ruff doesn't report an error for the preview rule [`E201`](https://docs.astral.sh/ruff/rules/whitespace-after-open-bracket/) that flags the whitespace after `[ `.


I can set `explicit-preview-rules` to `false` and the `E201` violation then gets reported ([playground](https://play.ruff.rs/dc479d6f-0f0c-41f2-86a8-9e32b7ce6271))

---

_Comment by @adrinjalali on 2024-07-16 10:46_

So on scikit-learn, adding

```
preview = true
# This enables us to use the explicit preview rules that we want only
explicit-preview-rules = true
```
results in:

```
Found 364 errors.
No fixes available (308 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

while on `main`, we get:

```
Found 14 errors.
```

```
$ ruff --version
ruff 0.5.1
```

Here are the outputs of `ruff check .` on the repo with and without the preview config added:

[stable.txt](https://github.com/user-attachments/files/16247434/stable.txt)

[preview.txt](https://github.com/user-attachments/files/16247433/preview.txt)




---

_Comment by @MichaReiser on 2024-07-16 11:12_

Thanks, I now see what's happening. What you're experience isn't that it enables additional preview-only rules, but that you see new violations because of some preview-only changes to *stable* rules. 

We don't support enabling preview functionality for specific rules only. Enabling preview mode enables the preview functionality for all rules (if they have any).  `explicit-preview-rules` only controls if preview-only rules should be enabled when using a selector like `E`. 

---

_Comment by @adrinjalali on 2024-07-17 14:34_

Oh now I understand. However, that puts us in a weird position, because:

- we need preview to enable `CPY001`
- that enables preview sub-rules of already active rules
- those give a lot of false positives / things we don't want to change
- we end up outright disabling those rules
- those rules end up not detecting legit things now since they're completely disabled

I feel like there should be a middle ground there? I hope this makes sense.

---

_Comment by @zanieb on 2024-07-17 14:52_

If those rule changes are giving you false positives in preview mode, I'd highly recommend opening issues because the behavior is _likely_ to become stable soon.

Unfortunately we don't have enough bandwidth to manage different preview modes, that's why we have a single global flag. `explicit-preview-rules` is already the compromise.

An option here is to allow explicit selection of preview rules without enabling preview mode. We'd probably need to display a warning that the rule is in preview, though.

---

_Comment by @jaraco on 2024-07-19 16:26_

Not sure if this is the same issue when dealing with formatting.

Related to this issue, the "single flag to rule them all" has caused [confusion and consternation](https://github.com/jaraco/skeleton/pull/133#issuecomment-2239441820). My projects adopted the setting in order to make "quote-style=preserve" work for compatibility when migrating from black, but that caused "hugged parenthesis" to be adopted implicitly. Later, when quote-style=preserve graduated to stable, we tried to remove the setting but that would roll back other behaviors.

In my projects, I'm opting to have "preview=true" by default (and the uncertainty that will bring), but at least that will allow the projects to continue to shift left (work at head).

I realize it's difficult to manage feature-specific flags, but just be aware that the user experience is muddled by the shifting meaning of this flag over time and releases.

---

_Comment by @ChrisLoveringSendient on 2025-01-15 15:18_

I came across this as we're having very similar issues. I want to enable `RUF029` which is currently in preview, but don't want to enable preview rules for the prefixes I have in my select.

I came across [explicit-preview-rules](https://docs.astral.sh/ruff/settings/#lint_explicit-preview-rules) which says 
> When enabled, preview rules will not be selected by prefixes — the full code of each preview rule will be required to enable the rule.

As stated above this doesn't seem to be working as documented, since I have the `F` prefix in my `select`, but after enabling preview & explicit preview rules, `F841` starts reporting violations, even though the full code isn't in my `select`

---

_Comment by @MichaReiser on 2025-01-15 15:36_

> As stated above this doesn't seem to be working as documented, since I have the F prefix in my select, but after enabling preview & explicit preview rules, F841 starts reporting violations, even though the full code isn't in my select

The option only changes how rule selectors work. `F841` isn't a preview rule; that's why it gets selected when using `F.` 

What you are looking for is a way to opt-in to preview behavior on a per-rule basis, which isn't supported.

---

_Comment by @ChrisLoveringSendient on 2025-01-15 15:55_

Oh, so it is. I might have stumbled across another oddity then.

I have this config which doesn't raise any issues when running `ruff check .` Other than the warning `RUF029` has no effect because preview is not enabled.
 ```
extend-select = ["I", "UP", "ASYNC105", "ASYNC220", "ASYNC222", "ASYNC230", "ASYNC251", "RUF029"]
``` 

However, adding this then shows all 8 `F841` failures in my repo, and then also shows `RUF029` failures, but no other preview rules.
```
preview = true
explicit-preview-rules = true
```
Doesn't this suggest that [explicit-preview-rules](https://docs.astral.sh/ruff/settings/#lint_explicit-preview-rules) is now working as documented in that link (and as OP requested) where only `RUF029` is being enabled as preview?

The unknown here is why `F841` wasn't running before, but is after enabling preview?

---

_Comment by @MichaReiser on 2025-01-15 16:48_

> Doesn't this suggest that [explicit-preview-rules](https://docs.astral.sh/ruff/settings/#lint_explicit-preview-rules) is now working as documented in that link (and as OP requested) where only RUF029 is being enabled as preview?

The author faces the same problem as you described. They see new errors for stable rules because they opted into the preview behavior.

---

_Comment by @iamtodor on 2025-06-18 08:58_

Hey folks,
I have the same struggle. Is there maybe some updates in this direction?

---

_Comment by @iamtodor on 2025-06-18 08:59_

Maybe add a couple of labels to be able to track and categorize this issue?

---

_Label `configuration` added by @MichaReiser on 2025-06-18 09:10_

---

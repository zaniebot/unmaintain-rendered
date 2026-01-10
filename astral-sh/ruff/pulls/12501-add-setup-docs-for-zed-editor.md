```yaml
number: 12501
title: Add setup docs for Zed editor
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/zed-setup
created_at: 2024-07-25T05:12:31Z
updated_at: 2024-08-26T06:55:03Z
url: https://github.com/astral-sh/ruff/pull/12501
synced_at: 2026-01-10T21:38:31Z
```

# Add setup docs for Zed editor

---

_Pull request opened by @dhruvmanila on 2024-07-25 05:12_

## Summary

This PR adds the setup documentation for using Ruff with the Zed editor.

Closes: #12388 

---

_Label `documentation` added by @dhruvmanila on 2024-07-25 05:12_

---

_@MichaReiser reviewed on 2024-07-25 06:10_

---

_Review comment by @MichaReiser on `docs/editors/setup.md`:473 on 2024-07-25 06:10_

This note is unclear to me. The formatter isn't in code actions. How can the other code actions be before the formatter?

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:473 on 2024-07-25 06:17_

What I meant was that in the `formatter` list, the code actions should be applied first, and then the language server should be used to run the formatter. So, the order in this list is important:
```
      "formatter": [
        {
          "code_actions": {
            "source.organizeImports.ruff": true,
            "source.fixAll.ruff": true
          }
        },
        {
          "language_server": {
            "name": "ruff"
          }
        }
      ]
```
Otherwise, if a list of unused imports are removed, the blank lines between the imports section and the following section remains.


https://github.com/user-attachments/assets/f3f00896-d249-4808-a79b-a776c9435621



---

_@dhruvmanila reviewed on 2024-07-25 06:17_

---

_@MichaReiser reviewed on 2024-07-25 06:21_

---

_Review comment by @MichaReiser on `docs/editors/setup.md`:473 on 2024-07-25 06:21_

ah, the language server sets the formatter. It's a bit confusing because the entire section is called *formatter*. Maybe rephrase to. ...the correct order of the code actions and the *formatter language server* settings.

---

_Review comment by @osiewicz on `docs/editors/setup.md`:422 on 2024-07-25 09:37_

Maybe we should be specific about the version (0.146.0) multi-formatters were made available in instead? As is, it sounds like I have to use Preview to use multiple formatters, when in reality this change is gonna be available on Stable channel in a week.

---

_@osiewicz reviewed on 2024-07-25 09:37_

---

_@dhruvmanila reviewed on 2024-07-25 11:05_

---

_Review comment by @dhruvmanila on `docs/editors/setup.md`:422 on 2024-07-25 11:05_

Yeah, that makes sense, thanks!

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-25 11:12_

---

_@MichaReiser approved on 2024-07-25 12:28_

---

_Merged by @dhruvmanila on 2024-07-25 13:09_

---

_Closed by @dhruvmanila on 2024-07-25 13:09_

---

_Branch deleted on 2024-07-25 13:09_

---

_Comment by @samueller on 2024-08-19 15:49_

I've read the setup docs, but still can't figure out how to add linting options. For example, where in Zed's `settings.json` do I add the option for [quote-style](https://docs.astral.sh/ruff/settings/#format_quote-style)? The docs give examples for `pyproject.toml` and `ruff.toml` and I've tried many possible places in `settings.json`, but can't figure it out.

---

_Comment by @MichaReiser on 2024-08-19 15:59_

@samueller sorry that you're struggling with this. 

The editor doesn't support all ruff settings. It only supports a [subset](https://docs.astral.sh/ruff/editors/settings/) and `quote-style` isn't part of that subset. To configure the `quote-style`, you have to create a `pyproject.toml` or `ruff.toml` and set the quote style in there.

---

_Comment by @samueller on 2024-08-19 16:02_

> It only supports a [subset](https://docs.astral.sh/ruff/editors/settings/) and `quote-style` isn't part of that subset.

I understand, thank you for letting me know

---

_Comment by @dhruvmanila on 2024-08-20 03:55_

@samueller Sorry that you struggled with this. Do you have any recommendation on how can we improve this in the documentation to make it clearer?

---

_Comment by @samueller on 2024-08-20 13:57_

> @samueller Do you have any recommendation on how can we improve this in the documentation to make it clearer?

Would have been great to have read which subset of features work in Zed's `settings.json` and which don't in the Zed configuration section.

---

_Comment by @dhruvmanila on 2024-08-23 05:40_

> Would have been great to have read which subset of features work in Zed's `settings.json` and which don't in the Zed configuration section.

I'm curious to know whether the following section in https://docs.astral.sh/ruff/editors/setup/#zed conveys this:

> To configure the language server, you can provide the [server settings](https://docs.astral.sh/ruff/editors/settings/) under the [lsp.ruff.initialization_options.settings](https://zed.dev/docs/configuring-zed#lsp) key:
> ```jsonc
> {
>   "lsp": {
>     "ruff": {
>       "initialization_options": {
>         "settings": {
>           // Ruff server settings goes here
>           "lineLength": 80,
>           "lint": {
>             "extendSelect": ["I"],
>           }
>         }
>       }
>     }
>   }
> }
> ```

---

_Comment by @samueller on 2024-08-23 18:10_

> I'm curious to know whether the following section in https://docs.astral.sh/ruff/editors/setup/#zed conveys this:

I had seen that, but it didn't tell me the settings that won't work, such as `quote-reply`. I had assumed all settings would work, I just needed to figure out how to specify them in `settings.json`.

---

_Comment by @dhruvmanila on 2024-08-26 06:55_

I see, thanks for the insight. I think it makes sense to explicitly clarify this point in the docs.

---

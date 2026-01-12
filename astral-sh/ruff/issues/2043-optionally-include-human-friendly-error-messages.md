```yaml
number: 2043
title: "Optionally include human-friendly error messages in inline comments added by `--add-noqa`"
type: issue
state: closed
author: cosmojg
labels: []
assignees: []
created_at: 2023-01-20T22:47:21Z
updated_at: 2023-01-23T19:08:09Z
url: https://github.com/astral-sh/ruff/issues/2043
synced_at: 2026-01-12T15:54:42Z
```

# Optionally include human-friendly error messages in inline comments added by `--add-noqa`

---

_@cosmojg_

I would like to be able to add human-readable error messages as well as error codes when running `ruff` with `--add-noqa`.

For example, instead of appending `# noqa: E722` to the end of offending lines, append `` # noqa: E722 Do not use bare `except` `` (or `` # noqa - E722 Do not use bare `except` `` to ensure compatibility with [flake8-noqa](https://github.com/plinss/flake8-noqa) and #850).

Perhaps this could be a separate flag? (e.g., `--add-noqa-verbose`) This would have the added benefit of making it easy to remove error messages by simply running `--add-noqa` again.

---

_Comment by @charliermarsh on 2023-01-20 22:51_

We're going to support this eventually! We've been laying the groundwork for it (every rule will have a kebab-cased name that can be used in lieu of the codes).

I'm gonna close this in favor of #1773 since that's kind of the consolidated issue.

---

_Closed by @charliermarsh on 2023-01-20 22:51_

---

_Comment by @cosmojg on 2023-01-20 23:09_

> I'm gonna close this in favor of #1773 since that's kind of the consolidated issue.

Thanks for responding so quickly! I saw that issue, and I guess I should've addressed it explicitly. I'm not sure the kebab-cased naming scheme is sufficient for what I had in mind. I'd really appreciate something more in line with the descriptive error messages that are normally printed to the terminal, particularly so that other contributors unfamiliar with even the kebab-cased names immediately know what they need to do to fix the issue without running to Google.

I have some free time tonight so I'd be happy to take a swing at implementing this feature myself if you agree that it's worth including.

---

_Comment by @charliermarsh on 2023-01-20 23:23_

Ah sorry, I didn't read the issue nearly closely enough, that's my fault.

I see what you're saying. How would it work in cases like `# noqa: E501, F841, ...`?

---

_Reopened by @charliermarsh on 2023-01-20 23:23_

---

_Comment by @cosmojg on 2023-01-20 23:58_

> How would it work in cases like `# noqa: E501, F841, ...`?

Good question! Off the top of my head, I suppose such cases could be handled by providing explanations on separate lines:

```
unused_string = "This line is waaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaay too long!" # noqa: E501, F841, ...
# E501 - Line too long (81 > 79 characters)
# F841 - Local variable `unused_string` is assigned to but never used
# ... - ...
```

In fact, as far as default behavior goes, this might even make more sense given that it ensures compatibility with [flake8-noqa](https://github.com/plinss/flake8-noqa) and https://github.com/charliermarsh/ruff/issues/850. 

---

_Comment by @not-my-profile on 2023-01-22 07:36_

> I'd really appreciate something more in line with the descriptive error messages that are normally printed to the terminal

I honestly think that this is not a good idea:

* improvements to the messages of ruff will not make it into your comments
* the comments will get out of date when the code changes e.g. `` Local variable `unused_string` `` is no longer accurate when the name of the variable changes

> particularly so that other contributors unfamiliar with even the kebab-cased names immediately know what they need to do to fix the issue without running to Google

I think you should rather instruct your contributors to use [ruff-lsp](https://github.com/charliermarsh/ruff-lsp), which since https://github.com/charliermarsh/ruff-lsp/pull/7 shows the error message when hovering over a rule code in a noqa comment.

---

_Comment by @charliermarsh on 2023-01-22 16:50_

I do appreciate the intent here, and I understand why this would be useful. But I think this is probably difficult to implement and maintain. Not impossible, but my gut is that the complexity wouldn't outweigh the benefit.

For example: we'd need to have the ability to recognize those explanations, parse them, and replace them when running with `--add-noqa`, otherwise they'd risk getting out of sync (and/or being added multiple times -- the command has to be idempotent). We may also need a way to flag that they're out-of-sync outside of `--add-noqa`.


---

_Closed by @charliermarsh on 2023-01-23 19:08_

---

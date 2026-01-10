---
number: 11905
title: Non-obvious behavior around notebook formatting
type: issue
state: open
author: mikolysz
labels:
  - documentation
assignees: []
created_at: 2024-06-17T13:05:46Z
updated_at: 2024-06-18T16:43:24Z
url: https://github.com/astral-sh/ruff/issues/11905
synced_at: 2026-01-10T01:22:51Z
---

# Non-obvious behavior around notebook formatting

---

_Issue opened by @mikolysz on 2024-06-17 13:05_

Jupyter's behavior around notebook formatting (excluding notebooks by default unless an explicit option is present) is extremely non-obvious. It's only documented in the faq (not in `ruff check --help` or the integrations section, which seem like logical places to look for this information for a novice user.

Even search doesn't always help here, for me, the FAQ is sometimes the second or third result depending on keywords, with the issue for implementing notebook formatting often ahead.

I suggest the following changes to rectify this (from least to most invasive):

- [ ] Add a short note to the "Integrations" section of the documentation about Ruff's integration with Jupyter notebooks, redirecting to the FAQ for more info.
- [ ] Possibly also add notes to other sections, this feature seems really well hidden and not mentioned anywhere else. Perhaps its existence should also be mentioned in the tutorial?
- [ ] - [ ] Add a short paragraph to the help text for `ruff check` and other commands about which files are included and excluded by default.
- [ ] If notebooks are present and haven't been explicitly included or excluded by a flag, add a warning that they haven't been formatted, along with a suggestion to include or exclude them in the project settings / on the command line.
- [ ] Add explicit `--include_notebooks` and `--exclude_notebooks` flags to make it even easier to configure this.

I can help out with some or all of those items if approved by the project maintainers.

---

_Label `documentation` added by @MichaReiser on 2024-06-17 15:47_

---

_Comment by @MichaReiser on 2024-06-17 15:48_

Thanks. Yeah that makes sense to me. It's a fairly important and substantial feature and it does deserve some more space in our documentation. Thanks for bringing this up.

---

_Comment by @dhruvmanila on 2024-06-18 05:45_

Thank you for bringing this up. From what I can infer with the task list, it seems that it was difficult to figure out how to include Jupyter Notebooks for linting and formatting via the CLI and editor. Is this correct?

There's also the [Jupyter Notebook discovery](https://docs.astral.sh/ruff/configuration/#jupyter-notebook-discovery) section in the docs. Does this help? I agree that we might want to do something about reducing the friction in how to configure Ruff for Jupyter Notebooks in general.

---

_Comment by @mikolysz on 2024-06-18 16:43_



>  From what I can infer with the task list, it seems that it was difficult to figure out how to include Jupyter Notebooks for linting and formatting via the CLI and editor. Is this correct?

Yes.

> There's also the Jupyter Notebook discovery <https://docs.astral.sh/ruff/configuration/#jupyter-notebook-discovery> section in the docs. Does this help? I agree that we might want to do something about reducing the friction in how to configure Ruff for Jupyter Notebooks in general.
> 

I didn’t notice this one, thanks for pointing it out.

---

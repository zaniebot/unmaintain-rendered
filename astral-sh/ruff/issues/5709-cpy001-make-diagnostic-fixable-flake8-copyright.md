```yaml
number: 5709
title: "CPY001: Make diagnostic fixable (flake8_copyright)"
type: issue
state: open
author: jvcop
labels:
  - fixes
  - needs-decision
assignees: []
created_at: 2023-07-12T13:11:21Z
updated_at: 2025-12-18T09:31:50Z
url: https://github.com/astral-sh/ruff/issues/5709
synced_at: 2026-01-10T11:09:48Z
```

# CPY001: Make diagnostic fixable (flake8_copyright)

---

_Issue opened by @jvcop on 2023-07-12 13:11_

I would like to propose making `CPY001` (`flake8_copyright`) fixable. This would be a new feature, as the functionality is not available in the original. The main motivating factors are that as far as I can see, fixing the diagnostic is relatively straightforward, and it would increase the utility of the rule. For example, in the company I am working for, we have our own pre-commit hook that automatically adds the copyright notice if necessary.

In my proposal, a new setting would be added alongside `author`, `notice_rgx` and `min_file_size`: `notice_tpl` (notice template). It would default to `Copyright (C) {year}`, where `{year}` is a special placeholder that will be replaced by the current year.

There are two relevant cases for fixing the diagnostic:

1. The notice is missing altogether. In this case, it would simply be generated and inserted based on `notice_tpl`. If the generated notice does not match `notice_rgx`, an error would be logged via `error!` and the fix would be skipped.
2. There is a notice, but `author` was specified and is missing at the end of the notice. In this case, the author would be inserted in the appropriate position.

Is there interest in fixing `CPY001`? If so, does this proposal make sense?

I have a working proof of concept. If there is interest, I could work on a pull request.

---

_Comment by @sbrugman on 2023-07-12 13:56_

Related issue https://github.com/astral-sh/ruff/issues/5306

---

_Label `autofix` added by @charliermarsh on 2023-07-12 18:23_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-12 18:23_

---

_Comment by @jvcop on 2023-09-26 15:29_

Is there anything I can do to help with the decision?

---

_Comment by @aaronsteers on 2024-05-07 22:30_

I'd love to see an auto-fix in place for this, as it would save us tons of time in making sure we are compliant with CPY1 - especially for codebases we are migrating, and for accepting contributions from community members who aren't going to know our spec for license requirements and what text to input.

X-posted from #5306:

> +1 to this request. And to raise an alternative more generic implementation - which would be "file_starts_with" customization.
> 
> This variant would allow auto-fixing of CPY1 indirectly, but with an approach more generic. The generic implementation would allow Ruff to check for (and auto-fix) a mandatory pre-amble for each python file.

---

_Comment by @spaceone on 2025-10-23 10:50_

I would also love to see this.
It should be able to insert the following license string anywhere in the top comment of the file (after the hashbang line):

```
# SPDX-FileCopyrightText: 2025 XXX GmbH
# SPDX-License-Identifier: AGPL-3.0-only
```
maybe with a placeholder `{year}` which inserts the current year.

---

_Comment by @keriksson-rosenqvist-oqc on 2025-12-18 09:31_

I'd really like to see this feature added. It would also be useful if the `notice_tpl` and `notice_rgx` could be connected, e.g. "using" the template as the regex.

---

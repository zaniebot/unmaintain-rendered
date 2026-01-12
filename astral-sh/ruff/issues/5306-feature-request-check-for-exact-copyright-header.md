```yaml
number: 5306
title: "Feature Request: Check for exact copyright header"
type: issue
state: open
author: rbebb
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-22T16:39:00Z
updated_at: 2024-06-18T15:23:40Z
url: https://github.com/astral-sh/ruff/issues/5306
synced_at: 2026-01-12T15:54:45Z
```

# Feature Request: Check for exact copyright header

---

_@rbebb_

Building off of the request for [`flake8-copyright`](https://github.com/astral-sh/ruff/issues/3579), it would be great to have the ability to check for an exact copyright header in any file!

---

_Comment by @edgarrmondragon on 2023-06-22 16:44_

There's regex support via [`flake8-copyright.notice-rgx`](https://beta.ruff.rs/docs/settings/#flake8-copyright-notice-rgx) so maybe you could use that to specify the exact header you want.

(Though an exact header setting would make it trivial to implement autofix :D)

---

_Comment by @charliermarsh on 2023-06-22 20:35_

Do you mind providing an example of the header you might want to enforce? Is it, like, the copyright notice content that you want to validate?

---

_Label `question` added by @charliermarsh on 2023-06-22 20:35_

---

_Label `rule` added by @charliermarsh on 2023-06-22 20:35_

---

_Comment by @rbebb on 2023-06-23 16:06_

Hey @charliermarsh, it is the copyright notice content that I'd want to validate. Here's an example!

https://github.com/cs3org/reva/blob/master/cmd/reva/app-tokens-remove.go#L1-L17

---

_Comment by @sbrugman on 2023-06-26 18:30_

This feature would also be useful for [popmon](https://github.com/ing-bank/popmon). We currently use Google's [addlicense](https://github.com/google/addlicense) (it supports popular licenses out-of-the-box, e.g. apache, bsd, mit, mpl).

Since that project already uses `ruff`, I'd love to switch. 

Example file: https://github.com/ing-bank/popmon/blob/master/popmon/analysis/functions.py#L1C1-L1C1

---

_Comment by @airwoodix on 2023-06-27 08:02_

`flake8-copyright` is a good starting point for this, although converting the header to validate into a regular expression is a bit tedious (lots of escapes).

However, the hardcoded [1024 bytes limit](https://github.com/astral-sh/ruff/blob/50a7769d698c93cb23c7b2854bb47d716d87aa0d/crates/ruff/src/rules/flake8_copyright/rules/missing_copyright_notice.rs#L35-L40) gets quickly in the way. Would it be an option to make it configurable?

---

_Comment by @zanieb on 2023-06-27 15:01_

Would it be useful to use the contents of the `LICENSE` file as the expected header?

---

_Comment by @airwoodix on 2023-06-27 16:24_

@zanieb looking at the [GPL guidelines](https://www.gnu.org/licenses/gpl-howto.html), I understand that the `LICENSE` file would contain a complete copy of the license, while each file only starts with the much shorter license notice. The [Apache-2.0 license](https://spdx.org/licenses/Apache-2.0.html) seems to have a similar distinction.

---

_Comment by @sbrugman on 2023-06-28 15:19_

Suggestion: provide a different message than "Missing copyright notice at top of file" when the contents does not match the regex/exact match. As a user, currently it's hard to debug the regex (especially multiline). 

Would it be an option to split missing/present and match/no match into two codes?

Minor detail: The [rules](https://beta.ruff.rs/docs/rules/#copyright-related-rules-cpy) are not listing the CPY001 atm.

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:09_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:09_

---

_Comment by @jeaboswell on 2023-09-22 07:02_

> `flake8-copyright` is a good starting point for this, although converting the header to validate into a regular expression is a bit tedious (lots of escapes).
> 
> However, the hardcoded [1024 bytes limit](https://github.com/astral-sh/ruff/blob/50a7769d698c93cb23c7b2854bb47d716d87aa0d/crates/ruff/src/rules/flake8_copyright/rules/missing_copyright_notice.rs#L35-L40) gets quickly in the way. Would it be an option to make it configurable?

I agree completely that the hardcoded limit of 1024 bytes is an issue.  If you are adhering to the [D100](https://docs.astral.sh/ruff/rules/undocumented-public-module/) rule, your copyright attribute gets pushed out of that limit quickly.

In my project, the only modules that don't need an exception to the rule are those that contain two to three functions.

I think ideally, if we just check if it's in the file at all, that would be a good start.  If we want to get really crazy, we could try to ensure it comes after the imports, and before any code.

---

_Comment by @ThiefMaster on 2023-11-17 16:54_

Shameless self-promotion but I wrote a script once that another colleague then converted into a standalone project: [unbeheader](https://github.com/unconventionaldotdev/unbeheader) - it basically manages your copyright (or whatever else) header comments in your codebase, has CI checks, and can automatically add/update them (locally, not in CI).

I'd love to see support for its functionality in ruff, but unless a codebase contains *only* Python maybe using ruff to enforce exact copyright headers is not ideal to begin with, since it would not handle any non-Python file types...

---

_Comment by @oliverfunk on 2023-11-29 13:28_

There should be support for SPDX file descriptors, given that they are being recommended more (Hatch for example uses them by default when creating a new project)

They generally follow the format of:
```python
# SPDX-FileCopyrightText: <year> <name> <contact>
# ^ can be multiple of these lines
#
# SPDX-License-Identifier: <licence>
```

The year and contact are optional, but the full spec is available.

References:
https://reuse.software/spec/
https://github.com/fsfe/reuse-tool
https://hatch.pypa.io/latest/config/project-templates/#licenses

I'm using the very ruff (pun not intended) regex for my config:
```toml
[tool.ruff.lint.flake8-copyright]
notice-rgx = "(?i)# SPDX-FileCopyrightText:\\s\\d{4}(-(\\d{4}|present))*"
```

---

_Comment by @aaronsteers on 2024-05-07 22:28_

+1 to this request. And to raise an alternative more generic implementation - which would be "file_starts_with" customization.

This variant would allow auto-fixing of CPY1 _indirectly_, but with an approach more generic. The generic implementation would allow Ruff to check for (and auto-fix) a mandatory pre-amble for each python file.

---

_Comment by @adrinjalali on 2024-06-18 15:23_

I just ended up here cause we wanted to enable this rule in https://github.com/scikit-learn/scikit-learn/, and encountered issues which took us quite a while to figure out.

For now I've put #11927 here which should fix at least a lot more people's issues.

---

```yaml
number: 1235
title: "Implement `pandas-vet`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: pandas-vet
created_at: 2022-12-14T06:06:19Z
updated_at: 2022-12-18T16:55:33Z
url: https://github.com/astral-sh/ruff/pull/1235
synced_at: 2026-01-12T15:55:06Z
```

# Implement `pandas-vet`

---

_@edgarrmondragon_

- [ ] PD001: pandas should always be imported as 'import pandas as pd'
- [x] PD002: 'inplace = True' should be avoided; it has inconsistent behavior
- [x] PD003: '.isna' is preferred to '.isnull'; functionality is equivalent
- [x] PD004: '.notna' is preferred to '.notnull'; functionality is equivalent
- [ ] PD005: Use arithmetic operator instead of method
- [ ] PD006: Use comparison operator instead of method
- [x] PD007: '.ix' is deprecated; use more explicit '.loc' or '.iloc'
- [x] PD008: Use '.loc' instead of '.at'. If speed is important, use numpy.
- [x] PD009: Use '.iloc' instead of '.iat'. If speed is important, use numpy.
- [x] PD010 '.pivot_table' is preferred to '.pivot' or '.unstack'; provides same functionality
- [x] PD011 Use '.array' or '.to_array()' instead of '.values'; 'values' is ambiguous
- [x] PDO12 '.read_csv' is preferred to '.read_table'; provides same functionality
- [x] PD013 '.melt' is preferred to '.stack'; provides same functionality
- [x] PD015 Use '.merge' method instead of 'pd.merge' function. They have equivalent functionality.
- [x] PD901 'df' is a bad variable name. Be kinder to your future self.

Closes https://github.com/charliermarsh/ruff/issues/1141

---

_Comment by @edgarrmondragon on 2022-12-14 06:12_

I'll try to implement `PDV005` and `PDV006` in the coming days.

---

_Comment by @delulu52 on 2022-12-14 08:49_

@edgarrmondragon Just fyi I had opened following ticket with pandas vet bit ago, that don't know if worth considering (again would double check if true, but wanted to bring up just in case) https://github.com/deppen8/pandas-vet/issues/113

---

_Comment by @delulu52 on 2022-12-14 08:57_

@edgarrmondragon Additionally do remember then having some what unresolved issue with https://github.com/deppen8/pandas-vet/issues/74 where some checks could give false negatives, don't know if concern or if still run into here but wanted to bring up just in case

---

_Comment by @edgarrmondragon on 2022-12-15 00:27_

> @edgarrmondragon Additionally do remember then having some what unresolved issue with [deppen8/pandas-vet#74](https://github.com/deppen8/pandas-vet/issues/74) where some checks could give false negatives, don't know if concern or if still run into here but wanted to bring up just in case

@ksdaftari I have not implemented `PD005` but we can leave it out for now. TLDR it's hard to solve with static linting such as Ruff's or flake8's [as pointed out by the maintainer](https://github.com/deppen8/pandas-vet/issues/74#issuecomment-545930912).

The workaround

> A quick hack (that could be optional for example) would be to check if pandas is imported in the file. Sure, it won't work for a lot of cases, but for a lot of cases it also will work.

could lead to false negatives instead.

Wdyt?

---

_Comment by @edgarrmondragon on 2022-12-15 00:28_

> @edgarrmondragon Just fyi I had opened following ticket with pandas vet bit ago, that don't know if worth considering (again would double check if true, but wanted to bring up just in case) [deppen8/pandas-vet#113](https://github.com/deppen8/pandas-vet/issues/113)

Yeah, makes sense to recommend `to_numpy()` here :+1:

---

_Marked ready for review by @edgarrmondragon on 2022-12-15 01:17_

---

_Comment by @charliermarsh on 2022-12-15 01:23_

@edgarrmondragon - Just double-confirming, all set for review here?

---

_Comment by @edgarrmondragon on 2022-12-15 05:59_

> @edgarrmondragon - Just double-confirming, all set for review here?

Yup, all set 🙂

---

_Comment by @charliermarsh on 2022-12-15 17:10_

Great, will review and hopefully merge today :)

---

_Merged by @charliermarsh on 2022-12-15 19:01_

---

_Closed by @charliermarsh on 2022-12-15 19:01_

---

_Branch deleted on 2022-12-15 19:33_

---

_Comment by @andersk on 2022-12-18 04:48_

Any reason we renamed these codes from the upstream `PDnnn` to `PDVnnn`?

---

_Comment by @charliermarsh on 2022-12-18 05:35_

I’ve been advocating the use of more specific prefixes for plugins, to avoid collisions in the present and future. For example, flake8-tidy-imports uses the “I” prefix, which is maybe fine if you have to explicitly instead the plugin to activate it, but causes collisions in the Ruff model. So I’d generally been biasing towards three-letter prefixes for all plugins. 

It’s a questionable choice and maybe one that merits case-by-case consideration, would love your opinion. In this case, PD might be confusing if we implemented another Pandas-related plugin.

We could also consider adding redirects from PD to PDV like we do for some other codes (e.g., U to UP).

---

_Comment by @andersk on 2022-12-18 06:10_

Hmm. It seems to me that the main benefit of the `ABC123` system is compatibility with the upstream codes. Yeah, we’ll need to deviate occasionally when there are collisions. But if we’re also going to deviate on account of _hypothetical_ collisions (even for plugins following the [upstream recommendation](https://flake8.pycqa.org/en/latest/plugin-development/registering-plugins.html) of 2 or 3 characters), then maybe we’d be better off with a different naming system designed for humans instead of [military radio dials](https://youtu.be/tyOLJ99ysdU?t=440)?

This has sort of been in the back of my mind—something more like ESLint’s system where e.g. `eslint-plugin-unicorn` provides errors like `unicorn/no-array-for-each` would reduce the need to constantly refer back to the documentation or keep [comments next to every error code](https://github.com/zulip/zulip/blob/d4d285fc43b9ede7556f30878e28d8f851289bf2/pyproject.toml#L102-L136) to help remember what they mean.

---

_Comment by @charliermarsh on 2022-12-18 16:55_

Yeah that's a very reasonable argument. Perhaps the right approach for now is to only deviate if it will cause a conflict with an existing plugin _or_ knowingly cause a conflict with another known plugin that we might reasonably implement. I can revert to PD for this specific plugin.

And yes -- I do want to redo the error code system (ideally in a way that's compatible with the existing system and thus with Flake8), and I think ESLint is probably the right model to follow (though I also want to retain unique IDs for each code like Pylint, which is necessary for compatibility anyway). I wrote a bit about it here: https://github.com/charliermarsh/ruff/issues/967.

This will probably come with some more flexibility around `noqa` directives... like ESLint's ignore-next-line behavior.

---

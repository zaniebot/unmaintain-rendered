```yaml
number: 7732
title: Rework the documentation to incorporate the Ruff formatter
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/formatter-docs
created_at: 2023-10-01T03:10:11Z
updated_at: 2023-10-20T23:17:26Z
url: https://github.com/astral-sh/ruff/pull/7732
synced_at: 2026-01-12T02:32:41Z
```

# Rework the documentation to incorporate the Ruff formatter

---

_Pull request opened by @charliermarsh on 2023-10-01 03:10_

## Summary

This PR updates our documentation for the upcoming formatter release.

Broadly, the documentation is now structured as follows:

- Overview
- Tutorial
- Installing Ruff
- The Ruff Linter
    - Overview
    - `ruff check`
    - Rule selection
    - Error suppression
    - Exit codes
- The Ruff Formatter
    - Overview
    - `ruff format`
    - Philosophy
    - Configuration
    - Format suppression
    - Exit codes
    - Black compatibility
        - Known deviations
- Configuring Ruff
    - pyproject.toml
    - File discovery
    - Configuration discovery
    - CLI
    - Shell autocompletion
- Preview
- Rules
- Settings
- Integrations
    - `pre-commit`
    - VS Code
    - LSP
    - PyCharm
    - GitHub Actions
- FAQ
- Contributing

The major changes include:

- Removing the "Usage" section from the docs, and instead folding that information into "Integrations" and the new Linter and Formatter sections.
- Breaking up "Configuration" into "Configuring Ruff" (for generic configuration), and new Linter- and Formatter-specific sections.
- Updating all example configurations to use `[tool.ruff.lint]` and `[tool.ruff.format]`.

My suggestion is to pull and build the docs locally, and review by reading them in the browser rather than trying to parse all the code changes.

Closes https://github.com/astral-sh/ruff/issues/7235.

Closes https://github.com/astral-sh/ruff/issues/7647.


---

_Label `documentation` added by @charliermarsh on 2023-10-01 03:10_

---

_@charliermarsh reviewed on 2023-10-01 03:38_

---

_Review comment by @charliermarsh on `docs/formatter.md`:190 on 2023-10-01 03:38_

These aren't _necessarily_ conflicting with the formatter. The quote rules are probably consistent with it, assuming you have the same quote settings. And the trailing comma rules won't _conflict_ with the formatter, they'll just end up changing the resulting formatting.

---

_@charliermarsh reviewed on 2023-10-01 03:38_

---

_Review comment by @charliermarsh on `docs/formatter.md`:340 on 2023-10-01 03:38_

Using `<h4>` here ensures that this long list of intentional deviations is omitted from the table of contents on the left side of the page, which IMO is good. However, it also means they can't be permalinked, which IMO is bad...

---

_Comment by @github-actions[bot] on 2023-10-01 03:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `docs/configuration.md`:153 on 2023-10-01 15:50_

"those settings are used for across files" sounds strange

---

_Review comment by @konstin on `docs/faq.md`:3 on 2023-10-01 15:52_

Do we want "Ruff Linter" or "Ruff linter"?

---

_Review comment by @konstin on `docs/faq.md`:203 on 2023-10-01 15:53_

```suggestion
## Do I have to use Ruff's linter and formatter together?
```

---

_Review comment by @konstin on `docs/formatter.md`:153 on 2023-10-01 15:57_

```suggestion
]
# fmt: on
```

---

_Review comment by @konstin on `docs/formatter.md`:185 on 2023-10-01 15:59_

Can we add the codes here? Otherwise it will be hard to spot if you even have the rules active.

---

_Review comment by @konstin on `docs/formatter.md`:203 on 2023-10-01 16:01_

That is not the case atm:
```console
$ scratch.py
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
1 file reformatted
$ echo $?
0
```
For me, having files formatted is analogous to `all present violations were fixed automatically` and should exit 0.

---

_Review comment by @konstin on `docs/formatter.md`:236 on 2023-10-01 16:03_

We should add some words about how the black preview style will become the black stable style yearly and will do the same, even though not necessarily on the same timeline (right? i don't think we really talked about timelines for this, either way i think this should be done with a minor version bump)

---

_Review comment by @konstin on `docs/formatter.md`:340 on 2023-10-01 16:08_

Can you manually give them `id`? It's still not really accessible to users but at least it's possible if you know.

---

_Review comment by @konstin on `docs/linter.md`:41 on 2023-10-01 16:11_

```suggestion
Ruff would enable all rules with the `E` (pycodestyle) or `F` (Pyflakes) prefix, with the exception
```

---

_Review comment by @konstin on `docs/tutorial.md`:8 on 2023-10-01 16:15_

Would be so nice if pipx were part of the default python distribution

---

_@konstin approved on 2023-10-01 16:16_

:scroll: :scroll: :scroll:

---

_@zanieb reviewed on 2023-10-01 16:46_

---

_Review comment by @zanieb on `docs/faq.md`:3 on 2023-10-01 16:46_

I prefer "Ruff linter"

---

_Comment by @codspeed-hq[bot] on 2023-10-01 18:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/formatter-docs)

### Merging #7732 will **not alter performance**

<sub>Comparing <code>charlie/formatter-docs</code> (27a56f6) with <code>main</code> (fa556d1)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_@T-256 reviewed on 2023-10-02 21:12_

---

_Review comment by @T-256 on `docs/formatter.md`:128 on 2023-10-02 21:12_

```suggestion
# fmt: on
```

---

_@charliermarsh reviewed on 2023-10-02 21:16_

---

_Review comment by @charliermarsh on `docs/formatter.md`:128 on 2023-10-02 21:16_

Thx!

---

_Review comment by @charliermarsh on `docs/formatter.md`:203 on 2023-10-02 22:59_

Thanks, you're right.

---

_@charliermarsh reviewed on 2023-10-02 22:59_

---

_@dhirschfeld reviewed on 2023-10-03 01:46_

---

_Review comment by @dhirschfeld on `docs/formatter/black.md`:291 on 2023-10-03 01:46_

```suggestion
        d,
```

---

_Review comment by @charliermarsh on `docs/formatter/black.md`:291 on 2023-10-03 01:53_

Thanks!

---

_@charliermarsh reviewed on 2023-10-03 01:53_

---

_Review comment by @dhruvmanila on `README.md`:135 on 2023-10-05 05:01_

nit: `file_paths.txt` seems limited but I think it's not?
```suggestion
ruff check @ruff_args.txt          # Lint using an input file, treating its contents as newline-delimited command-line arguments.
```

Or, `cli_args`, `arguments`, etc.

---

_Review comment by @dhruvmanila on `README.md`:145 on 2023-10-05 05:01_

Same as above
```suggestion
ruff format @ruff_args.txt          # Format using an input file, treating its contents as newline-delimited command-line arguments.
```

---

_Review comment by @dhruvmanila on `docs/configuration.md`:154 on 2023-10-05 05:03_

typo
```suggestion
    analyzed files, and any relative paths in that configuration file (like `exclude` globs or
```

---

_Review comment by @dhruvmanila on `docs/configuration.md`:217 on 2023-10-05 05:05_

Do we want to recommend `extend-include` instead and wherever this is mentioned?

---

_Review comment by @dhruvmanila on `docs/faq.md`:354 on 2023-10-05 05:07_

Do you mind updating this as the PR has been merged?

Also, this is another section where we could recommend `extend-include` instead for including Jupyter Notebook files if we decide to do so.

---

_Review comment by @dhruvmanila on `docs/formatter.md`:13 on 2023-10-05 05:15_

Do we support `ruff format @ruff_args.txt` ? If so, we could include it here.

---

_Review comment by @dhruvmanila on `scripts/generate_mkdocs.py`:31 on 2023-10-05 05:20_

Do you think it's useful to have a dedicated "Reference" section which would contain the "Rules" and "Settings" sub-sections?

---

_@dhruvmanila reviewed on 2023-10-05 05:20_

---

_@charliermarsh reviewed on 2023-10-05 15:06_

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:31 on 2023-10-05 15:06_

I do, but I think it can be done separately.

---

_@charliermarsh reviewed on 2023-10-05 15:52_

---

_Review comment by @charliermarsh on `docs/formatter.md`:340 on 2023-10-05 15:52_

I decided to move them to their own page.

---

_Review comment by @MichaReiser on `docs/configuration.md`:194 on 2023-10-16 03:18_

```suggestion
Files that are passed to `ruff` directly are always analyzed, regardless of the above criteria.
```

Not sure what word to use to include formatting and linting

---

_Review comment by @MichaReiser on `docs/configuration.md`:199 on 2023-10-16 03:18_

```suggestion
Ruff has built-in support for [Jupyter Notebooks](https://jupyter.org/).
```

---

_Review comment by @MichaReiser on `docs/faq.md`:205 on 2023-10-16 03:20_

```suggestion
Nope! Ruff's linter and formatter can be used independently of one another -- you can use
```

---

_Review comment by @MichaReiser on `docs/formatter.md`:1 on 2023-10-16 03:21_

We should use consistent casing for "formatter"

```suggestion
# The Ruff formatter
```

---

_Review comment by @MichaReiser on `docs/formatter.md`:24 on 2023-10-16 03:22_

```suggestion
The goal of the Ruff formatter is _not_ to innovate on code style, but rather, to innovate on
```

---

_Review comment by @MichaReiser on `docs/formatter.md`:82 on 2023-10-16 03:23_

```suggestion
Like Black, the Ruff formatter does _not_ support extensive code style configuration; however,
```

---

_Review comment by @MichaReiser on `docs/formatter.md`:101 on 2023-10-16 03:23_

```suggestion
For example, to configure the formatter to use single quotes, a line width of 100, and
```

---

_@MichaReiser approved on 2023-10-16 07:12_

---

_Comment by @MichaReiser on 2023-10-18 00:58_

We should add an example for how to exclude certain files from linting / formatting only https://github.com/astral-sh/ruff/pull/8000

---

_@charliermarsh reviewed on 2023-10-18 01:31_

---

_Review comment by @charliermarsh on `docs/formatter.md`:1 on 2023-10-18 01:31_

I think the page titles (H1) should be title-case.

---

_@MichaReiser reviewed on 2023-10-19 01:55_

---

_Review comment by @MichaReiser on `README.md`:246 on 2023-10-19 01:55_

I'm about to change this back to `auto`

---

_@MichaReiser reviewed on 2023-10-19 01:56_

---

_Review comment by @MichaReiser on `docs/configuration.md`:72 on 2023-10-19 01:56_

```suggestion
line-ending = "auto"
```

---

_Review comment by @MichaReiser on `README.md`:218 on 2023-10-20 01:01_

```suggestion
line-length = 88
indent-width = 4
```

---

_Review comment by @MichaReiser on `docs/configuration.md`:42 on 2023-10-20 01:02_

```suggestion
indent-width = 4
```

---

_Review comment by @MichaReiser on `docs/configuration.md`:42 on 2023-10-20 01:02_

```suggestion
line-width = 88
```

---

_Review comment by @MichaReiser on `docs/configuration.md`:179 on 2023-10-20 01:02_

```suggestion
# ...but use a different line width.
line-width = 100
```

---

_Review comment by @MichaReiser on `docs/formatter.md`:110 on 2023-10-20 01:03_

```suggestion
line-width = 100

[tool.ruff.format]
quote-style = "single"
indent-style = "tab"
```

---

_Review comment by @MichaReiser on `docs/tutorial.md`:138 on 2023-10-20 01:05_

```suggestion
# Decrease the maximum line width to 79.
line-width = 79
```

---

_@MichaReiser reviewed on 2023-10-20 01:05_

---

_@MichaReiser reviewed on 2023-10-20 01:30_

---

_Review comment by @MichaReiser on `docs/formatter.md`:207 on 2023-10-20 01:30_

Our [internal configuration document](https://www.notion.so/astral-sh/Configuration-c291a149e50f438083221a65468a736f?pvs=4#972f1a2e4ce14549b899adce6563b931) mentions more conflicting rules and isort options. Are there reasons why you excluded some of the rules and options from here?

---

_@charliermarsh reviewed on 2023-10-20 21:27_

---

_Review comment by @charliermarsh on `docs/formatter.md`:207 on 2023-10-20 21:27_

I'll add them, but it's the same confusion as in your PR that enables the warnings -- what constitutes a "conflict"?

---

_Review comment by @charliermarsh on `docs/formatter.md`:207 on 2023-10-20 22:44_

I also added `ISC001` and `Q003`, were those intentionally omitted?

---

_@charliermarsh reviewed on 2023-10-20 22:44_

---

_Comment by @charliermarsh on 2023-10-20 22:45_

Made all changes except I haven't done the `length` to `width` tweaks yet, since that hasn't merged.

---

_Merged by @charliermarsh on 2023-10-20 23:08_

---

_Closed by @charliermarsh on 2023-10-20 23:08_

---

_Branch deleted on 2023-10-20 23:08_

---

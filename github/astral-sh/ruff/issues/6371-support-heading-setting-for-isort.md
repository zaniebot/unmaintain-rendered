---
number: 6371
title: Support heading setting for isort
type: issue
state: open
author: applied-mathematician
labels:
  - isort
  - accepted
assignees: []
created_at: 2023-08-06T11:02:27Z
updated_at: 2025-12-24T17:00:15Z
url: https://github.com/astral-sh/ruff/issues/6371
synced_at: 2026-01-07T13:12:15-06:00
---

# Support heading setting for isort

---

_Issue opened by @applied-mathematician on 2023-08-06 11:02_

The `isort` settings `import_heading_{section_name}` lets you specify heading comments for the sections, each defaulting to None.

I use these headings with all of my team projects so not a second is wasted trying to figure out where a package comes from. This is especially helpful when you have only a couple imports, hence not all sections. Developers new to a team have to figure out whether a package name they don't recognize is internal or third-party. There are even standard libraries that experienced developers often don't know, e.g. `ast`. This time savings translates to tasks like skimming your modules to gather a precise list of third-party dependencies.

---

_Label `isort` added by @charliermarsh on 2023-08-06 14:35_

---

_Referenced in [astral-sh/ruff#6190](../../astral-sh/ruff/issues/6190.md) on 2023-08-17 16:33_

---

_Comment by @kinoute on 2023-09-11 16:43_

Interested by this as well.

---

_Referenced in [instructlab/instructlab#739](../../instructlab/instructlab/issues/739.md) on 2024-03-25 06:29_

---

_Referenced in [astral-sh/ruff#465](../../astral-sh/ruff/issues/465.md) on 2024-08-29 21:58_

---

_Comment by @Alex-ley-scrub on 2024-09-14 23:38_

@charliermarsh I know you said this on the isort meta issue https://github.com/astral-sh/ruff/issues/6190#issuecomment-1664881638:
> I'm open to contributions for additional isort options, but we likely won't prioritize implementing them ourselves

I'd personally love this isort heading setting to be implemented since it would allow me to migrate us over to ruff isort from our current isort configuration. We use it for the same reason as @applied-mathematician 

I saw that this one was **not** labelled as a https://github.com/astral-sh/ruff/labels/good%20first%20issue - I was trying to gauge how difficult it would be to implement. I've been learning Rust off and on for the last 2 years and I've written some with pyO3 to speed up our python codebase but I'm definitely not (yet) as proficient as I am in other languages.

Knowing at present very little about the ruff codebase and the isort implementation makes it difficult to estimate the time/effort needed and wether this would be a good candidate for my first contribution or not.

I've had a read of [CONTRIBUTING.md](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md) and I've had a look around the [isort implementation folder](https://github.com/astral-sh/ruff/tree/main/crates/ruff_linter/src/rules/isort). I am guessing that this a point at which the insertion of the import section header (comment) could naively occur [ruff_linter/src/rules/isort/mod.rs#L201-L208](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/isort/mod.rs#L201-L208). i.e. we could just insert the import section header when we are adding a newline before the section (as per the code comment `// Add a blank line between every section`) but of course that doesn't handle a lot of edge cases like if the import section header was already present, how do we detect that and how does this import section header interact with other comment lines present and other potentially conflicting settings such as `no-lines-before` etc.

Would it be possible for you to outline how you think it could/should be implemented when you have time please? And if I think I can do it then I'll give it a go, if not perhaps someone else who reads your outline will feel confident to do so - or I can come back to it at a later date.

---

_Comment by @Alex-ley-scrub on 2024-10-28 00:07_

Hey @charliermarsh / @MichaReiser - I've recently been committing to https://github.com/getgrit/gritql/commits?author=Alex-ley-scrub and getting familiar with another AST / parsing heavy rust repo. It is really nice to write rust in a more mature and structured project (as opposed to just writing hotpath python stuff in rust with pyO3).

I think I'm ready to give this isort header issue an attempt now. I'm going to start with this approach unless you guys have some better ideas:
1.  add a new field to the `Settings` struct - i.e. `import_heading: FxHashMap<ImportSection, String>` or `BTreeMap<ImportSection, String>` for the isort section here: https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/isort/settings.rs#L47-L76
2. inside of `annotate_imports()` and/or `normalize_imports()` remove any exact matches for header strings that were defined in `import_heading` (as we will add them back later in the correct places): https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/isort/mod.rs#L81-L90 or just filter them out of `ImportCommentSet.atop` and `ImportFromCommentSet.atop` inside of `categorize_imports()` etc. https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/isort/mod.rs#L172
3. try the naive insertion mentioned in my last message https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/isort/mod.rs#L201-L208 `// Add a blank line between every section` but handling correctly the `no_lines_before` setting etc. - I guess I could do 2 and 3 inside of `format::format_import()` / `format::format_import_from()` potentially: https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/isort/mod.rs#L229-L255
4. write a bunch of tests and try to think of all the edge case and adapt the code as necessary (i.e. add fixtures in `crates/ruff_linter/resources/test/fixtures/isort/` and add those to the test functions such as `no_sections()` and add new test functions as required to test new settings combinations)

--------

1. 
```rust
#[derive(Debug, Clone, CacheKey)]
#[allow(clippy::struct_excessive_bools)]
pub struct Settings {
    ...
    pub no_lines_before: FxHashSet<ImportSection>,
    pub import_heading: FxHashMap<ImportSection, String>,
    ...
}
```
usage:
```toml
[tool.ruff.lint.isort]
force-sort-within-sections = false
required-imports = ["from __future__ import annotations"]
# https://github.com/astral-sh/ruff/issues/6371
import-heading.future = "Future imports (must occur at the beginning of the file):"
import-heading.standard-library = "Standard library imports:"
import-heading.third-party = "Third party imports:"
import-heading.first-party = "Local imports:"
extra-standard-library = ["pathlib", "packaging", "glob", "urllib"]
known-first-party = ["src"]
known-third-party = ["wandb", "pandas", "numpy"]
no-lines-before = ["local-folder"]
section-order = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder",
]
```

---

_Comment by @MichaReiser on 2024-10-28 07:00_

Thanks @Alex-ley-scrub for your interest. 


> > @charliermarsh I know you said this on the isort meta issue https://github.com/astral-sh/ruff/issues/6190#issuecomment-1664881638:
>
> I'm open to contributions for additional isort options, but we likely won't prioritize implementing them ourselves

We've since changed our stand and are less likely to accept new isort settings. I'm not familiar with isort myself so I'm unlikely the right person to make this decision but I would wait until we hear from @charliermarsh 

---

_Label `needs-decision` added by @MichaReiser on 2024-10-28 07:00_

---

_Referenced in [foundation-model-stack/fms-model-optimizer#25](../../foundation-model-stack/fms-model-optimizer/issues/25.md) on 2024-12-10 13:55_

---

_Comment by @bmitc on 2025-03-17 14:04_

> We've since changed our stand and are less likely to accept new isort settings. I'm not familiar with isort myself so I'm unlikely the right person to make this decision but I would wait until we hear from @charliermarsh.

Why is that?

I'm not sure I follow the point of bundling isort behavior into Ruff if all the features aren't going to be supported, especially a nice one. It's strange to try and replace tooling but not support all the features of the existing tooling.

---

_Comment by @charliermarsh on 2025-12-24 16:31_

Gonna remove the `needs-decision` label -- this looks reasonable to support.

---

_Label `needs-decision` removed by @charliermarsh on 2025-12-24 16:31_

---

_Label `accepted` added by @charliermarsh on 2025-12-24 16:31_

---

_Comment by @MichaReiser on 2025-12-24 17:00_

Not saying we shouldn't do this, but this setting might feel weird if we decide to integrate isort into the formatter because it sets a new precedence that the formatter inserts content (other than `,` or whitespace). 



---

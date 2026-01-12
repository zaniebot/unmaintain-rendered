```yaml
number: 1014
title: Relative import management
type: issue
state: open
author: noirbizarre
labels:
  - plugin
assignees: []
created_at: 2022-12-03T19:51:10Z
updated_at: 2025-03-15T23:35:38Z
url: https://github.com/astral-sh/ruff/issues/1014
synced_at: 2026-01-12T15:54:40Z
```

# Relative import management

---

_@noirbizarre_

This topic has always been a bit painful to manage with `isort` or `absolufy-imports` and given I am migrating all my project to `ruff` I would love to be able to tune the relative imports ahndling:
- basic case (the one I use everyday): force relative imports for siblings and children only (with auto-fix support) (ie. `..` starting imports)
- allow parent imports on a package basis: mark a package to be allowed as a relative-root reference by children packages/modules
- default: prevent relative

Is it something acceptable ? 

**Note:** I am motivated enough to contribute but not yet skilled enough in Rust to make this happen soon

---

_Comment by @charliermarsh on 2022-12-03 21:44_

Yeah, I think so... Right now, we support the "forbid relative imports of parent modules" rule from `flake8-tidy-imports`. But adding autofix to that rule would probably mean converting relative to absolute imports (basically points 2 and 3 on your list: prevent relative imports, rewrite them as absolute). So in addition, we'd need a new rule that's basically the inverse: prevent absolute imports in certain cases (with autofix).

Does that sound right?

Is there any tool or combination of tools you use today to enforce this?


---

_Label `question` added by @charliermarsh on 2022-12-03 22:01_

---

_Comment by @noirbizarre on 2022-12-05 20:55_

I think the autofix should be both ways for case 1:
- rewrite relative to absolute imports for the forbidden cases
- rewrite absolute to relative imports for siblings and children packages

For this case I am using `absolufy-import` with `--never` option but it is far from perfect and I have a lot of manual fixes to handle.

For case 3: This one should be the default: forbid relative imports and rewrite them as absolute as autofix. This is basically the deault behavior of `absolufy-imports` 

Case 2 is bonus to me but I have some cases of child-to-parent dependencies I want to keep relative. The idea it to flag a package as relative-root for its children using `by-package`  Ruff config syntax and then autofix should:
- Apply the same autofix as case 1 for siblings
- rewrite absolute imports to relative for children referencing this flagged parent (this is the point differing from case 1)
- keep/rewrite imports as absolute for those not matching this case

I took a look at `flake8-tidy-imports`: it is very close to the expected behavior but:
- cannot enforce relative/sibling imports
- does not autofix

---

_Label `question` removed by @charliermarsh on 2022-12-31 18:16_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:16_

---

_Comment by @sbrugman on 2023-02-16 08:09_

Note that `flake8-tidy-imports` now has partial autofix

---

_Comment by @lsorber on 2023-02-24 09:49_

Which cases are not covered by the `flake8-tidy-imports` autofix @sbrugman?

---

_Comment by @noirbizarre on 2023-04-20 10:28_

I submitted a PR for my missing case as well as a matching one on `flake8-tidy-import`. I will adapt the PR depending on the review and feedback on the `flake8-tidy-import` repository.

---

_Comment by @pawamoy on 2024-02-05 14:12_

Sorry if this is not the right place to ask: the docs (https://docs.astral.sh/ruff/rules/relative-imports/) say that

> Fix is sometimes available.

When is that autofix available exactly? I'm looking at automatically converting relative imports to absolute ones. I have the relevant rule enabled:

```toml
[flake8-tidy-imports]
ban-relative-imports = "all"
```

Is such an autofix planned/accepted in Ruff?

---

_Comment by @hynek on 2025-03-13 09:40_

Since this has been quiet here lately 😇: I, too, would love for an option to TID252 that would rewrite sibling imports to relative.

I.e. within a module `foo.bar.baz`:

```
from foo.bar.qux import xyz
```

becomes

```
from .qux import xyz
```

Given grouping rules, this has a great effect on readability since it's clear what's coming from the current subpackage (very useful with VSA).

---

_Comment by @noirbizarre on 2025-03-14 22:19_

I am still willing to contribute this, when I opened this issue I was initially intending to add a TID/I253 on both project, and I did submit a working pull request on both:
- https://github.com/adamchainz/flake8-tidy-imports/pull/441 for the `flake8-tidy-import` part, but the maintainer didn't seem OK with it and never answered me on what I should do to have this accepted, or even if it was a possibility
- #3986 which was its working ruff counterpart, but was closed because I was waiting for feedback on the `flake-tidy-import` counterpart, and the [closure comment](https://github.com/astral-sh/ruff/pull/3986#issuecomment-1771178725) seems to imply it would be merged only if the upstream is

It seems this approach is now out of the scope (I am not willing to insist on `flake8-tidy-import`) but given a TID253 rule has already been added there without an upstream counterpart, maybe having a TID254 for relative siblings is now acceptable.

I would gladly resubmit an updated version of the PR if this is OK for `ruff` maintainers, but I need a confirmation before that to avoid submitting a new PR that wouldn't be accepted, and perhaps some guidance on the form it should take:
- a new `ban-relative-imports` value `relative-siblings` acting like `parents` but force siblings as relative
- a new boolean setting `force-relative-siblings` which requires `ban-relative-imports=parents`

In the process, I would gladly add any missing autofixes if any

---

_Comment by @noirbizarre on 2025-03-15 23:35_

I submitted a new pull request https://github.com/astral-sh/ruff/pull/16772 based on the second approach (a new `TID254` opt-in rule to be enabled with a new `relative-sibling-imports` setting)

---

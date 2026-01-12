```yaml
number: 2777
title: Automatically linkify option references in rule documentation
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: option-references
created_at: 2023-02-11T19:01:49Z
updated_at: 2023-02-12T18:19:12Z
url: https://github.com/astral-sh/ruff/pull/2777
synced_at: 2026-01-12T04:52:01Z
```

# Automatically linkify option references in rule documentation

---

_Pull request opened by @not-my-profile on 2023-02-11 19:01_

Previously the rule documentation referenced configuration options
via full https:// URLs, which was bad for several reasons:

* changing the website would mean you'd have to change all URLs
* the links didn't work when building mkdocs locally
* the URLs showed up in the `ruff rule` output
* broken references weren't detected by our CI

This commit solves all of these problems by post-processing the
Markdown, recognizing sections such as:

    ## Options

    * `flake8-tidy-imports.ban-relative-imports`

`cargo dev generate-all` will automatically linkify such references
and panic if the referenced option doesn't exist.
Note that the option can also be linked in the other Markdown sections
via e.g. [`flake8-tidy-imports.ban-relative-imports`] since
the post-processing code generates a CommonMark link definition.

Resolves #2766.

---

_Review comment by @sbrugman on `crates/ruff_dev/src/generate_docs.rs`:68 on 2023-02-11 19:17_

Nit: Can we link to the mkdocs?

---

_@sbrugman reviewed on 2023-02-11 19:17_

---

_Comment by @sbrugman on 2023-02-11 19:19_

Elegant solution, love it

---

_@not-my-profile reviewed on 2023-02-12 05:35_

---

_Review comment by @not-my-profile on `crates/ruff_dev/src/generate_docs.rs`:68 on 2023-02-12 05:35_

Sure :) Changed it to `../../settings#{anchor}` ... which *should* work ... I am not certain since I don't know how to build mkdocs locally, see #2802.

---

_Marked ready for review by @not-my-profile on 2023-02-12 05:35_

---

_@charliermarsh reviewed on 2023-02-12 16:54_

---

_Review comment by @charliermarsh on `docs/rules/unused-variable.md`:37 on 2023-02-12 16:54_

Should this be an absolute link to GitHub? What happens when the user runs `ruff rule ...`? Do we show the relative link?

---

_@not-my-profile reviewed on 2023-02-12 17:01_

---

_Review comment by @not-my-profile on `docs/rules/unused-variable.md`:37 on 2023-02-12 17:01_

I initially linked these to the GitHub README but @sbrugman suggested to instead link the mkdocs page ... which I think makes sense, since it's weird if https://beta.ruff.rs/docs/ suddenly links you to the GitHub README.

I guess the only problem is that if you now follow these links in the GitHub README these links are broken ... which could be fixed by having absolute links to https://beta.ruff.rs/docs/settings/ ... but I think it would actually be better to stop listing the rules and configuration options in the README and have them solely on the documentation website. I'd also remove the `docs/rules/*.md` files from the master branch and only generate them for `mkdocs` ... having to run `cargo dev generate-all` all the time is annoying ... but it's your call ... I can also just change it to an absolute ruff.rs URL for now.

> What happens when the user runs ruff rule ...? Do we show the relative link?

This isn't part of the `ruff rule` output at all ... since it's only added by generate-docs for the `.md` files.

---

_Review comment by @charliermarsh on `docs/rules/unused-variable.md`:37 on 2023-02-12 18:18_

Ah right, this makes sense. Even if we leave the rules and configuration options in the README, we could just remove the checked-in versions and have the README links point to the versions in the docs?

---

_@charliermarsh reviewed on 2023-02-12 18:18_

---

_@charliermarsh reviewed on 2023-02-12 18:19_

---

_Review comment by @charliermarsh on `docs/rules/unused-variable.md`:37 on 2023-02-12 18:19_

We can follow-up though if we want to do that -- this makes sense for now.

---

_Merged by @charliermarsh on 2023-02-12 18:19_

---

_Closed by @charliermarsh on 2023-02-12 18:19_

---

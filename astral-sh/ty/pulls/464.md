```yaml
number: 464
title: Add ToC and troubleshooting guidance to detailed README
type: pull_request
state: merged
author: brainwane
labels: []
assignees: []
merged: true
base: main
head: install-venv
created_at: 2025-05-20T18:03:20Z
updated_at: 2025-05-21T19:56:07Z
url: https://github.com/astral-sh/ty/pull/464
synced_at: 2026-01-10T02:34:10Z
```

# Add ToC and troubleshooting guidance to detailed README

---

_Pull request opened by @brainwane on 2025-05-20 18:03_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add troubleshooting guidance and a table of contents to ty detailed README and installation instructions. This improves documentation navigability and, by incorporating advice from @AlexWaygood, helps prevent or remediate an error I stumbled into while testing ty at the PyCon US 2025 sprints.

## Test Plan

I previewed the Markdown file on GitHub and manually checked the formatting and hyperlinks.

---

_@AlexWaygood reviewed on 2025-05-20 18:05_

---

_Review comment by @AlexWaygood on `docs/README.md`:65 on 2025-05-20 18:05_

maybe "the standard-libary `venv` module"?

---

_Comment by @MichaReiser on 2025-05-20 18:06_

Thank you. I'm not sure that we want a toc. Doesn't github render a toc in the side bar (we recently removed the toc from ruff)

---

_@AlexWaygood reviewed on 2025-05-20 18:09_

---

_Review comment by @AlexWaygood on `docs/README.md`:20 on 2025-05-20 18:09_

sooo... I'm not too sure about the exact details but I _think_ we would also look at a `.ignore` file even if your project doesn't use git for source control. @MichaReiser is our expert on configuring ty!

---

_@brainwane reviewed on 2025-05-20 18:22_

---

_Review comment by @brainwane on `docs/README.md`:65 on 2025-05-20 18:22_

Thanks. Fixed.

---

_@brainwane reviewed on 2025-05-20 18:22_

---

_Review comment by @brainwane on `docs/README.md`:20 on 2025-05-20 18:22_

Reworded; happy to reword again once I have clearer facts.

---

_@AlexWaygood reviewed on 2025-05-20 18:24_

---

_Review comment by @AlexWaygood on `docs/README.md`:65 on 2025-05-20 18:24_

thank you!

---

_Comment by @brainwane on 2025-05-20 18:27_

> Thank you. I'm not sure that we want a toc. Doesn't github render a toc in the side bar (we recently removed the toc from ruff)

@MichaReiser, in my view (Firefox on a laptop), GitHub does not render a table of contents in the sidebar for [this document](https://github.com/astral-sh/ty/tree/main/docs) even when [I directly navigate to `README.md`](https://github.com/astral-sh/ty/blob/main/docs/README.md). As a newcomer I found I wanted a table of contents to give me a quick overview of the high-level section headings, especially since GitHub's displays of second- and third-level headings look really similar to me.

---

_Comment by @brainwane on 2025-05-20 18:40_

![ty-toc](https://github.com/user-attachments/assets/478bdfc0-2e61-475e-9daa-a3ebdc3b2115)

Thanks @AlexWaygood for pointing out the "show a table of contents" button that I had completely missed. In my opinion this is so tiny and hard to notice that I'm in favor of showing a one-line ToC of the top-level headings at the top of Markdown documents, but of course I'm happy to defer to Astral's documentation style guide on this point!

---

_@AlexWaygood approved on 2025-05-20 18:45_

This LGTM. Thank you!

---

_@AlexWaygood reviewed on 2025-05-20 18:46_

---

_Review comment by @AlexWaygood on `docs/README.md`:12 on 2025-05-20 18:46_

```suggestion
## Getting started

```

---

_Review requested from @zanieb by @AlexWaygood on 2025-05-20 18:48_

---

_Comment by @zanieb on 2025-05-20 18:51_

We're talking about this synchronously, but I briefly considered adding a ToC before noticing the GitHub does supply one. I don't really have strong feelings here, but generally here was my thought process:

- If we have a ToC, I probably want it to be automatically rendered
- GitHub has a ToC, which covers part of the immediate use-case
- We'll publish these as a real site eventually, with a real generated ToC

Overall, I don't mind adding it because the GitHub one stinks but I don't really want to invest in automatic rendering, so it's sort of a question if it's likely to go stale before we switch to MkDocs?

---

_@zanieb reviewed on 2025-05-20 18:52_

---

_Review comment by @zanieb on `docs/README.md`:57 on 2025-05-20 18:52_

I'm not sure if this belongs in the "Installation" section.

---

_@brainwane reviewed on 2025-05-20 20:46_

---

_Review comment by @brainwane on `docs/README.md`:57 on 2025-05-20 20:46_

@zanieb Indeed; I have now changed that header appropriately to "Installation and basic usage".

---

_@zanieb reviewed on 2025-05-21 01:52_

---

_Review comment by @zanieb on `docs/README.md`:15 on 2025-05-21 01:52_

Sorry to nitpick, but I'd rather leave this as just "Installation" and have a separate "Usage" or "Running ty" section. Is there a reason you'd prefer to combine them?

---

_@brainwane reviewed on 2025-05-21 02:24_

---

_Review comment by @brainwane on `docs/README.md`:15 on 2025-05-21 02:24_

Right now, the section in this document on running ty is only a few sentences, so I thought it was perfectly fine to combine it in this section for now. Sounds like you have a strong preference to instead have it be a top-level header; do you intend to add any more guidance for users in that section, or have all of that guidance be in `cli.md`?

---

_Comment by @brainwane on 2025-05-21 02:30_

Does Astral have a documentation style guide? If not, I recommend you create one, or find a public style guide you like and standardize on it, to clarify expectations for contributors.

I'm unlikely to make time for further revisions to this pull request after tomorrow (I'm at the PyCon US sprints till the end of the day tomorrow). Maintainers can of course revise this to your liking yourselves and merge it as you like.

---

_Comment by @zanieb on 2025-05-21 15:09_

The only style guide we have today is https://github.com/astral-sh/uv/blob/main/STYLE.md

---

_Comment by @zanieb on 2025-05-21 15:10_

(and, unfortunately, I don't think it's done very much to improve the quality of documentation pull requests in uv)

---

_@brainwane reviewed on 2025-05-21 19:48_

---

_Review comment by @brainwane on `docs/README.md`:15 on 2025-05-21 19:48_

Changed.

---

_Merged by @AlexWaygood on 2025-05-21 19:54_

---

_Closed by @AlexWaygood on 2025-05-21 19:54_

---

_Branch deleted on 2025-05-21 19:56_

---

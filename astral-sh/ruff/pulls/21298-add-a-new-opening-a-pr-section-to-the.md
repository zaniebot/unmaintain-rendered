```yaml
number: 21298
title: "Add a new \"Opening a PR\" section to the contribution guide"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/contributing
created_at: 2025-11-06T16:07:05Z
updated_at: 2025-11-10T14:12:34Z
url: https://github.com/astral-sh/ruff/pull/21298
synced_at: 2026-01-12T15:57:20Z
```

# Add a new "Opening a PR" section to the contribution guide

---

_@ntBre_

Summary
--

This PR adds a new section to CONTRIBUTING.md describing the expected contents of the PR summary and test plan, using the ecosystem report, and communicating the status of a PR.

This seemed like a pretty good place to insert this in the document, at the end of the advice on preparing actual code changes, but I'm certainly open to other suggestions about both the content and placement.

Test Plan
--

Future PRs :)


---

_Label `documentation` added by @ntBre on 2025-11-06 16:07_

---

_Review requested from @amyreese by @ntBre on 2025-11-06 16:07_

---

_Review requested from @MichaReiser by @ntBre on 2025-11-06 16:07_

---

_Review comment by @amyreese on `CONTRIBUTING.md`:300 on 2025-11-06 16:10_

Would it be worth adding something around keeping things brief of "succinct" as a way to discourage long or overly-fluffed summaries?

---

_@amyreese approved on 2025-11-06 16:12_

---

_@ntBre reviewed on 2025-11-06 17:16_

---

_Review comment by @ntBre on `CONTRIBUTING.md`:300 on 2025-11-06 17:16_

Yeah, I think that's a good idea. I'll think more about how to work it in.

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:298 on 2025-11-07 14:23_

This sounds a bit intimidating (even when not using AI). I think I'd frame it around AI being useful, but a successful AI generated contribution is reviewed and the summary is editoralized before submitting the PR.

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:316 on 2025-11-07 14:25_

```suggestion
After opening the PR, an ecosystem report will be run as part of CI. This shows a diff of linter
```

I'd omit this sentence. It's sort of implied by the previous sections

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:319 on 2025-11-07 14:27_

Rather than making this mandatory, I'd formalize this more around how them review and discussing the ecosystem changes helps us being more effective at reviewing their PR.

---

_@MichaReiser reviewed on 2025-11-07 14:28_

Thanks for working on this. This is great. I've a few smaller suggestions around pharsing.

---

_Review requested from @MichaReiser by @ntBre on 2025-11-07 18:10_

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:323 on 2025-11-10 09:32_

I think I'd remove this paragraph as it mostly restates what you already wrote in the previous paragraph and the *feel free to ask*... seems generic and non specific to this section

---

_@MichaReiser approved on 2025-11-10 09:33_

---

_Merged by @ntBre on 2025-11-10 14:12_

---

_Closed by @ntBre on 2025-11-10 14:12_

---

_Branch deleted on 2025-11-10 14:12_

---

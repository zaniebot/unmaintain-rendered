```yaml
number: 21884
title: Document range suppressions, reorganize suppression docs
type: pull_request
state: merged
author: amyreese
labels:
  - documentation
  - suppression
assignees: []
merged: true
base: main
head: amy/document-suppressions
created_at: 2025-12-10T02:41:49Z
updated_at: 2025-12-11T19:16:37Z
url: https://github.com/astral-sh/ruff/pull/21884
synced_at: 2026-01-12T15:57:36Z
```

# Document range suppressions, reorganize suppression docs

---

_@amyreese_

- **Reorganize suppression documentation, document range suppressions**
- **Note preview mode requirement**

Issue #21874, #3711



---

_Label `documentation` added by @amyreese on 2025-12-10 02:42_

---

_Label `suppression` added by @amyreese on 2025-12-10 02:42_

---

_Review requested from @ntBre by @amyreese on 2025-12-10 03:10_

---

_Review requested from @MichaReiser by @amyreese on 2025-12-10 03:10_

---

_Review comment by @MichaReiser on `docs/linter.md`:405 on 2025-12-10 16:01_

I think we allow trailing commas

---

_@MichaReiser approved on 2025-12-10 16:02_

This is great. 

It's a bit unfortunate that we can't land this before you added the new rule. We could consider omitting the mentions or sections mentioning the rule for now and add that later if you want to land this sooner.

Can you add a screenshot to how the new documentation looks in the browser? I'm mainly interested in seeing the navigation bar

---

_Comment by @amyreese on 2025-12-10 16:28_

<img width="2474" height="2598" alt="Screenshot 2025-12-10 at 08-27-15 The Ruff Linter Ruff" src="https://github.com/user-attachments/assets/2c0b4ad9-69c5-418a-a444-b27b58aad100" />


---

_@amyreese reviewed on 2025-12-10 16:30_

---

_Review comment by @amyreese on `docs/linter.md`:405 on 2025-12-10 16:30_

Good point

---

_Review comment by @ntBre on `docs/linter.md`:386 on 2025-12-10 16:32_

```suggestion
It is strongly suggested to use explicit range suppressions, in order to prevent
```

---

_Review comment by @ntBre on `docs/linter.md`:391 on 2025-12-10 16:34_

```suggestion
Range suppressions cannot be used to enable or select rules that aren't already
```

---

_Review comment by @ntBre on `docs/linter.md`:446 on 2025-12-10 16:36_

I think I very slightly preferred calling them `suppression comments`, but I'm happy either way.

---

_Review comment by @ntBre on `docs/linter.md`:441 on 2025-12-10 16:37_

Do you think it's worth emphasizing `--extend-select` here explicitly in the text? Since it's otherwise easy to use `--select` accidentally and delete all your comments.

---

_@ntBre approved on 2025-12-10 16:38_

Nice!

---

_Comment by @MichaReiser on 2025-12-10 16:38_

The three-layer deep repetition of Suppressions in the navigation isn't great but I'm not sure if I like omitting it from `Inline`, `File`, `Range`

---

_Comment by @amyreese on 2025-12-10 16:45_

> The three-layer deep repetition of Suppressions in the navigation isn't great but I'm not sure if I like omitting it from `Inline`, `File`, `Range`

How about renaming the "Suppression Comments" section to "Comment Directives" instead?

---

_Comment by @MichaReiser on 2025-12-10 16:50_

> How about renaming the "Suppression Comments" section to "Comment Directives" instead?

I think I'd lean towards just calling it `Comments` and then use `file-level`, `line-level`, `block-level` as the subheadings similar to pyright https://microsoft.github.io/pyright/#/comments

The reason why I wouldn't use Directives is because it introduces an entirely new term that I don't think we use anywhere else (we sometimes use the term pragma comments but not sure if only on isuses or also in the documentation)

---

_@amyreese reviewed on 2025-12-10 18:13_

---

_Review comment by @amyreese on `docs/linter.md`:446 on 2025-12-10 18:13_

I mostly dropped the "comments" part so that the header wouldn't wrap in the sidebar, but I can put it back 😅 

Edit: oh, and it also had better consistency with the title above it, "Detecting unused suppressions"

---

_Comment by @amyreese on 2025-12-10 18:20_

Updated headers and typos, and clarified ruff flag usage, moving the full commands into separate boxes.

---

_Comment by @amyreese on 2025-12-10 18:21_

<img width="2474" height="2598" alt="Screenshot 2025-12-10 at 10-21-36 The Ruff Linter Ruff" src="https://github.com/user-attachments/assets/42eb42ad-d4a9-4167-995d-b8f29ef0a11d" />


---

_Comment by @amyreese on 2025-12-11 01:15_

> It's a bit unfortunate that we can't land this before you added the new rule. We could consider omitting the mentions or sections mentioning the rule for now and add that later if you want to land this sooner.

Would it be enough to pull out and land this commit from #21908 to define the RUF103 and RUF104 rules, even if they were never triggered, or just defer this part of the docs until that other PR is landed? 

---

_Comment by @MichaReiser on 2025-12-11 08:31_

> Would it be enough to pull out and land this commit from https://github.com/astral-sh/ruff/pull/21908 to define the RUF103 and RUF104 rules, even if they were never triggered, or just defer this part of the docs until that other PR is landed?

It's a bit misleading to have rules that simply do nothing. It's also fine to wait for the rule PR to merge. It's very unlikely that you get merge conflicts on this branch

---

_Merged by @amyreese on 2025-12-11 19:16_

---

_Closed by @amyreese on 2025-12-11 19:16_

---

_Branch deleted on 2025-12-11 19:16_

---

```yaml
number: 16035
title: Fix reference definition labels for backtick-quoted shortcut links
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-links-without-targets
created_at: 2025-02-08T01:33:46Z
updated_at: 2025-02-10T16:15:24Z
url: https://github.com/astral-sh/ruff/pull/16035
synced_at: 2026-01-10T19:57:22Z
```

# Fix reference definition labels for backtick-quoted shortcut links

---

_Pull request opened by @InSyncWithFoo on 2025-02-08 01:33_

## Summary

Resolves #16010.

The changes boil down to something like this:

```diff
-/// The [FastAPI documentation] recommends the use of [`typing.Annotated`]
+/// The [FastAPI documentation] recommends the use of [`typing.Annotated`][typing-annotated]

-/// [typing.Annotated]: https://docs.python.org/3/library/typing.html#typing.Annotated
+/// [typing-annotated]: https://docs.python.org/3/library/typing.html#typing.Annotated
```

## Test Plan

Mkdocs:

![](https://github.com/user-attachments/assets/a2e6bf22-56fa-4b2c-9500-1c1256c5a218)

GitHub:

> ## Why is this bad?
> The [FastAPI documentation] recommends the use of [`typing.Annotated`][typing-annotated]
> 
> ...
>
> [FastAPI documentation]: https://fastapi.tiangolo.com/tutorial/query-params-str-validations/?h=annotated#advantages-of-annotated
> [typing-annotated]: https://docs.python.org/3/library/typing.html#typing.Annotated

[CommonMark dingus](https://spec.commonmark.org/dingus/?text=%23%23%20Why%20is%20this%20bad%3F%0AThe%20%5BFastAPI%20documentation%5D%20recommends%20the%20use%20of%20%5B%60typing.Annotated%60%5D%5Btyping-annotated%5D%0A%0A...%0A%0A%5BFastAPI%20documentation%5D%3A%20https%3A%2F%2Ffastapi.tiangolo.com%2Ftutorial%2Fquery-params-str-validations%2F%3Fh%3Dannotated%23advantages-of-annotated%0A%5Btyping-annotated%5D%3A%20https%3A%2F%2Fdocs.python.org%2F3%2Flibrary%2Ftyping.html%23typing.Annotated):

```html
<h2>Why is this bad?</h2>
<p>The <a href="https://fastapi.tiangolo.com/tutorial/query-params-str-validations/?h=annotated#advantages-of-annotated">FastAPI documentation</a> recommends the use of <a href="https://docs.python.org/3/library/typing.html#typing.Annotated"><code>typing.Annotated</code></a></p>
<p>...</p>
```

---

_Comment by @InSyncWithFoo on 2025-02-08 01:36_

That Mkdocs recognizes links like ```[`typing.Annotated`]``` in the first place is probably because it uses [@Python-Markdown/markdown](https://github.com/Python-Markdown/markdown), an implementation of the original Markdown rather than CommonMark or GFM.

---

I <em>think</em> that's all of them. More importantly, there should be a test somewhere so that such a link can't possibly be accidentally introduced again. Where would I even put that?

---

_Comment by @github-actions[bot] on 2025-02-08 01:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Daverball on 2025-02-08 08:45_

> I _think_ that's all of them. More importantly, there should be a test somewhere so that such a link can't possibly be accidentally introduced again. Where would I even put that?

Possibly here? https://github.com/astral-sh/ruff/blob/main/scripts/check_docs_formatted.py

That's where all of the other custom docs checks appear to happen. Although the main purpose appears to be ensuring that the examples are formatted correctly, there are a couple of unrelated checks as well, like ensuring the required sections for rule docs are present.

---

_@Daverball reviewed on 2025-02-08 09:02_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/ruff/rules/unsafe_markup_use.rs`:73 on 2025-02-08 09:02_

This is getting rid of the link in the references section (which is rendered), so I don't think we want to remove this. You could omit the explicit url portion of the reference, since it's technically redundant, although given that the labels in  the references section are sometimes changed to a more verbose, descriptive, style, I used an explicit link, so changing the label later on is easy. I don't think i have seen any shortcut links be used in other references sections.

---

_@InSyncWithFoo reviewed on 2025-02-08 17:15_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unsafe_markup_use.rs`:73 on 2025-02-08 17:15_

The same link has also been referenced 4 times right above. I suppose I should remove them instead of this one.

See also [Wikipedia's guide on duplicate links](https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style/Linking#Duplicate_and_repeat_links).

---

_Comment by @InSyncWithFoo on 2025-02-08 17:28_

> Possibly here? https://github.com/astral-sh/ruff/blob/main/scripts/check_docs_formatted.py

Section headers and code blocks are easy to check for, but inline links aren't. I imagine that would take quite some manual parsing/regexes on top of a list of every option known to Ruff.

---

_Comment by @MichaReiser on 2025-02-08 17:50_

Would you mind updating your test plan and showing the rendered documentation for a page before/after your change (doesn't have to be for all of them, just pick one ;))



---

_Label `documentation` added by @MichaReiser on 2025-02-08 17:50_

---

_@MichaReiser approved on 2025-02-10 08:54_

It would be great if we'd have some linting or at least, guidance in the contribution guidelines. I otherwise worry, that we'll introduce incompatible links in the future. But that's something we can do separately. 

Thank you!

---

_Merged by @MichaReiser on 2025-02-10 08:54_

---

_Closed by @MichaReiser on 2025-02-10 08:54_

---

_Branch deleted on 2025-02-10 16:15_

---

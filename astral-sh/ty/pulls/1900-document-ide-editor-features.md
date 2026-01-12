```yaml
number: 1900
title: Document IDE / editor features
type: pull_request
state: merged
author: sharkdp
labels:
  - documentation
assignees: []
merged: true
base: main
head: david/lsp-features
created_at: 2025-12-15T19:28:28Z
updated_at: 2025-12-16T14:11:12Z
url: https://github.com/astral-sh/ty/pull/1900
synced_at: 2026-01-12T15:54:28Z
```

# Document IDE / editor features

---

_@sharkdp_

**[Rendered view](https://shark.fish/ty-lsp-features/features/editor/)**

Document the supported IDE / editor / LSP features.

I tried to group them into categorizes because a list that goes over every single feature feels too detailed for me.

The whole document is hand-written, but the "## Feature reference" section is entirely Claude-generated, so I would appreciate if someone with more LSP background could quickly check / edit this.

---

_Label `documentation` added by @sharkdp on 2025-12-15 19:28_

---

_Review comment by @AlexWaygood on `docs/features/editor.md`:23 on 2025-12-15 19:34_

```suggestion
    ty supports both the "pull" and "push" diagnostic models. Most modern editors will use the "pull" model
```

---

_@sharkdp reviewed on 2025-12-15 19:35_

---

_Review comment by @sharkdp on `docs/features/language-server.md`:67 on 2025-12-15 19:35_

Should I mention https://github.com/astral-sh/ruff/pull/21968 here?

---

_Review comment by @sharkdp on `docs/features/screenshots/code-completion.png`:1 on 2025-12-15 19:36_

I can add dark-mode versions of these screenshots ~~before I merge this~~ if we think that's important.

---

_@sharkdp reviewed on 2025-12-15 19:36_

---

_Review comment by @AlexWaygood on `docs/features/editor.md`:82 on 2025-12-15 19:36_

and cmd-clicked for go-to definition on specific types inside the inlay!

---

_@sharkdp reviewed on 2025-12-15 19:37_

---

_Review comment by @sharkdp on `docs/features/language-server.md`:95 on 2025-12-15 19:37_

As mentioned in the description, this part is Claude-generated. Would be great if someone can quickly review/edit it. I'm also happy to remove it.

---

_@sharkdp reviewed on 2025-12-15 19:38_

---

_Review comment by @sharkdp on `docs/features/editor.md`:1 on 2025-12-15 19:38_

See [here](https://shark.fish/ty-lsp-features/features/editor) for a rendered version of this file

---

_@AlexWaygood approved on 2025-12-15 19:40_

(Didn't look at https://github.com/astral-sh/ty/pull/1900/files#r2620603645 because I'm not an expert on our server implementation)

---

_Review comment by @sharkdp on `docs/features/screenshots/inlay-hints.png`:1 on 2025-12-15 19:41_

Agree with Aria here: Let's find something with a more interesting hover type and docstring.

(I wanted to show inlay hints and hover type in a single screenshot)

---

_@sharkdp reviewed on 2025-12-15 19:41_

---

_@sharkdp reviewed on 2025-12-15 19:42_

---

_Review comment by @sharkdp on `docs/features/editor.md`:25 on 2025-12-15 19:42_

@MichaReiser Is this info box useful? And more importantly, is it correct?

---

_Review comment by @AlexWaygood on `docs/features/screenshots/inlay-hints.png`:1 on 2025-12-15 19:56_

This might be a good example from my typeshed-stats project? You can see us understanding the format of the parameters list in the docstring, applying syntax highlighting to the codeblock, and you can see the nontrivial inlay hint:


https://github.com/user-attachments/assets/d0b0ca76-62b6-4865-bd37-c60f8bbb8eb4



---

_@AlexWaygood reviewed on 2025-12-15 19:56_

---

_@zanieb reviewed on 2025-12-15 20:10_

---

_Review comment by @zanieb on `mkdocs.yml`:97 on 2025-12-15 20:10_

I wonder if this should be "Language server"?

I think since we say "type checker and language server" in the branding we probably want a "language server" section?

---

_@zanieb reviewed on 2025-12-15 20:10_

---

_Review comment by @zanieb on `docs/features/language-server.md`:6 on 2025-12-15 20:10_

I wonder if we should move that editors document into "Installation" under an "Editors" heading instead

---

_Comment by @zanieb on 2025-12-15 21:43_

The sooner we can merge this and iterate on the content the better imo.

---

_Merged by @sharkdp on 2025-12-15 21:53_

---

_Closed by @sharkdp on 2025-12-15 21:53_

---

_Branch deleted on 2025-12-15 21:53_

---

_Comment by @sharkdp on 2025-12-15 21:53_

> The sooner we can merge this and iterate on the content the better imo.

Done. I'll work on the remaining comments here tomorrow, unless someone else gets to them first.

---

_@dhruvmanila reviewed on 2025-12-16 06:08_

---

_Review comment by @dhruvmanila on `docs/features/language-server.md`:118 on 2025-12-16 06:08_

I think this would also be part of Ruff but it's not currently implemented, tracking issue is https://github.com/astral-sh/ruff/issues/16829.

---

_@sharkdp reviewed on 2025-12-16 13:23_

---

_Review comment by @sharkdp on `docs/features/language-server.md`:67 on 2025-12-16 13:23_

@Gankra would you like that to be included? What would a corresponding line in this list look like?

---

_@sharkdp reviewed on 2025-12-16 14:11_

---

_Review comment by @sharkdp on `docs/features/language-server.md`:6 on 2025-12-16 14:11_

I think this is now resolved.

---

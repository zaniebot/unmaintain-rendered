```yaml
number: 1912
title: "Add \"highlights\" to the README"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/highlights
created_at: 2025-12-16T01:34:56Z
updated_at: 2025-12-16T13:45:53Z
url: https://github.com/astral-sh/ty/pull/1912
synced_at: 2026-01-12T15:54:28Z
```

# Add "highlights" to the README

---

_@zanieb_

[Rendered](https://github.com/astral-sh/ty/tree/zb/highlights?tab=readme-ov-file#highlights)

Following consensus, I'll copy them into the `index.md` page of the documentation and add links.

---

_Label `documentation` added by @zanieb on 2025-12-16 01:34_

---

_@zanieb reviewed on 2025-12-16 01:39_

---

_Review comment by @zanieb on `README.md`:12 on 2025-12-16 01:39_

I'm not that into this one, probably another way to frame it or other features to highlight, e.g., the gradual guarantee

---

_Marked ready for review by @zanieb on 2025-12-16 03:24_

---

_Review comment by @AlexWaygood on `README.md`:19 on 2025-12-16 12:05_

```suggestion
- 🔩 Advanced typing features like first-class intersection types, advanced type narrowing, and
    type-driven reachability analysis
```

(similar to my comment at https://github.com/astral-sh/astral-sh/pull/128#discussion_r2622963481)

---

_Review comment by @AlexWaygood on `README.md`:15 on 2025-12-16 12:07_

```suggestion
- ⌨️ Language server with code navigation, completions, code actions, auto-import, inlay hints, on-hover help, etc.
```

---

_@AlexWaygood approved on 2025-12-16 12:08_

I don't love the emojis haha 😆

but this makes our README consistent with uv/ruff, and that's more important

---

_Review comment by @MichaReiser on `README.md`:12 on 2025-12-16 12:12_

🩺 or 🔍

---

_Review comment by @MichaReiser on `README.md`:14 on 2025-12-16 12:13_

🌱

---

_Review comment by @MichaReiser on `README.md`:15 on 2025-12-16 12:13_

🧠 or 💡

---

_Review comment by @MichaReiser on `README.md`:16 on 2025-12-16 12:14_

🔄

---

_Review comment by @MichaReiser on `README.md`:18 on 2025-12-16 12:14_

🧩 or 🔬

---

_@MichaReiser approved on 2025-12-16 12:14_

The emojis are indeed a bit random. I made some differently random suggestions for what to use instead. No strong opinions.

---

_Review comment by @sharkdp on `README.md`:12 on 2025-12-16 12:18_

I would remove yet another reference to Rust here.

```suggestion
- 📎 Comprehensive and helpful diagnostics
```

---

_Review comment by @sharkdp on `README.md`:17 on 2025-12-16 12:21_

Move this one above "Fine-grained incremental analysis" maybe? To introduce the editor integrations before talking about them again.

---

_@sharkdp approved on 2025-12-16 12:23_

Thank you!

I also really dislike the emojis. They look so 2015. If we really need consistency with ruff and uv, I'm voting for removing them from ruff and uv's READMEs instead of adding them here.

---

_@zanieb reviewed on 2025-12-16 12:53_

---

_Review comment by @zanieb on `README.md`:12 on 2025-12-16 12:53_

I started with that but the blurb felt too short and I was trying to match the blog post. I don't have strong feelings, but is there more we can say?

---

_@zanieb reviewed on 2025-12-16 12:54_

---

_Review comment by @zanieb on `README.md`:17 on 2025-12-16 12:54_

mm I tried to introduce it the other way around, e.g.

- Language server
- Incrementally in the language server
- Integrations for the language server

---

_@sharkdp reviewed on 2025-12-16 13:02_

---

_Review comment by @sharkdp on `README.md`:12 on 2025-12-16 13:02_

It sounds good to me, and is barely shorter than the line above(?).

Maybe something like "Comprehensive diagnostics with rich contextual information" or more emotive: "Beautiful, comprehensive and helpful diagnostics"?

---

_Review comment by @zanieb on `README.md`:12 on 2025-12-16 13:28_

Thanks!

---

_@zanieb reviewed on 2025-12-16 13:28_

---

_Merged by @zanieb on 2025-12-16 13:45_

---

_Closed by @zanieb on 2025-12-16 13:45_

---

_Branch deleted on 2025-12-16 13:45_

---

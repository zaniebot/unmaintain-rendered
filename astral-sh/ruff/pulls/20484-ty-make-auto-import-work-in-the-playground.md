```yaml
number: 20484
title: "[ty] Make auto-import work in the playground"
type: pull_request
state: merged
author: BurntSushi
labels:
  - playground
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/playground-new-completion-data
created_at: 2025-09-19T15:01:47Z
updated_at: 2025-09-19T18:35:52Z
url: https://github.com/astral-sh/ruff/pull/20484
synced_at: 2026-01-12T15:57:03Z
```

# [ty] Make auto-import work in the playground

---

_@BurntSushi_

It turned out that we weren't quite funneling the new completion data
all the way through.

I followed the docs for [`CompletionItem`] for the Monaco editor. It's
similar, but not identical, to the LSP protocol specification.

[`CompletionItem`]: https://microsoft.github.io/monaco-editor/typedoc/interfaces/languages.CompletionItem.html


---

_Review requested from @carljm by @BurntSushi on 2025-09-19 15:01_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-09-19 15:01_

---

_Review requested from @sharkdp by @BurntSushi on 2025-09-19 15:01_

---

_Review requested from @dcreager by @BurntSushi on 2025-09-19 15:01_

---

_Review request for @dcreager removed by @BurntSushi on 2025-09-19 15:01_

---

_Review request for @carljm removed by @BurntSushi on 2025-09-19 15:01_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-09-19 15:01_

---

_Label `playground` added by @BurntSushi on 2025-09-19 15:02_

---

_Label `server` added by @BurntSushi on 2025-09-19 15:02_

---

_Label `ty` added by @BurntSushi on 2025-09-19 15:02_

---

_Comment by @github-actions[bot] on 2025-09-19 15:03_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @BurntSushi on 2025-09-19 15:05_

Demo:


https://github.com/user-attachments/assets/f259f2bc-3fb7-4111-97d2-8c965d2eb20c



---

_Comment by @github-actions[bot] on 2025-09-19 15:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:332 on 2025-09-19 15:27_

I think you can write this as
```suggestion
          completion.insert_text ?? completion.name
```

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:339 on 2025-09-19 15:28_

and this as
```suggestion
							completion.additional_text_edits?.map((edit: TextEdit) => ({
                range: tyRangeToMonacoRange(edit.range),
                text: edit.new_text,
              })),
```

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:316 on 2025-09-19 15:32_

```suggestion
    const suggestions: CompletionItem[] = completions.map((completion, i) => ({
```

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:27 on 2025-09-19 15:33_

```suggestion
```

And then insert on line 39

import CompletionItem = languages.CompletionItem;


---

_@MichaReiser approved on 2025-09-19 15:33_

Thank you

---

_Merged by @BurntSushi on 2025-09-19 18:35_

---

_Closed by @BurntSushi on 2025-09-19 18:35_

---

_Branch deleted on 2025-09-19 18:35_

---

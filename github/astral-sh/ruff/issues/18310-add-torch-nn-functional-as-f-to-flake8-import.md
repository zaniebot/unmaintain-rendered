---
number: 18310
title: "Add `torch.nn.functional as F` to flake8-import-conventions aliases"
type: issue
state: open
author: IlanCosman
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-05-26T04:56:12Z
updated_at: 2025-09-05T15:49:04Z
url: https://github.com/astral-sh/ruff/issues/18310
synced_at: 2026-01-07T13:12:16-06:00
---

# Add `torch.nn.functional as F` to flake8-import-conventions aliases

---

_Issue opened by @IlanCosman on 2025-05-26 04:56_

### Summary

| Import style                                     | Search regex                                | Number of files (links are slightly borked)                                                     |
| ------------------------------------------------ | ------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `from torch.nn import functional`                | `/from torch\.nn import functional$/`       | [1.6k](https://github.com/search?q=%2Ffrom+torch%5C.nn+import+functional%24%2F&type=code)       |
| `from torch.nn import functional as <SOMETHING>` | `/from torch\.nn import functional as .*$/` | [162k](https://github.com/search?q=%2Ffrom+torch%5C.nn+import+functional+as+.*%24%2F&type=code) |
| `from torch.nn import functional as F`           | `/from torch\.nn import functional as F$/`  | [129k](https://github.com/search?q=%2Ffrom+torch%5C.nn+import+functional+as+F%24%2F&type=code)  |
| `import torch.nn.functional as <SOMETHING>`      | `/import torch\.nn\.functional as .*$/`     | [1.4M](https://github.com/search?q=%2Fimport+torch%5C.nn%5C.functional+as+.*%24%2F&type=code)   |
| `import torch.nn.functional as F`                | `/import torch\.nn\.functional as F$/`      | [1.2M](https://github.com/search?q=%2Fimport+torch%5C.nn%5C.functional+as+F%24%2F&type=code)    |

In total `1.329 / 1.562` ~= 85% of the `torch.nn.functional` imports are `as F`.

### Considerations

- Afaict `F` is nowhere explicitly recommended/mentioned by the PyTorch documentation, though it appears in every code example on https://docs.pytorch.org/docs/stable/nn.functional.html

#### The `F` alias violates rule N812, lowercase-imported-as-non-lowercase.

- This is unfortunate, but I think it's case where practicality beats purity, since the `F` alias is so ubiquitous in the PyTorch ecosystem.
- I think it's worth making some sort of special case for this in rule N812
  - Quick and dirty option: add `functional` to the default `ignore-names` in `[tool.ruff.lint.pep8-naming]`
    - This has the slight disadvantage of allowing all import of `functional`, regardless of whether they are from PyTorch or not, to be importable with uppercase names.
  - Cleaner option: some sort of particular exception for `torch.nn.functional`? Not sure tbh


---

_Renamed from "Add `torch.nn.function as F` to flake8-import-conventions aliases" to "Add `torch.nn.functional as F` to flake8-import-conventions aliases" by @IlanCosman on 2025-05-26 04:56_

---

_Comment by @ntBre on 2025-05-29 20:20_

Just to make sure I understand, is the suggestion here to add this to the default value for [lint.flake8-import-conventions.aliases](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases)? I think pytorch users could already configure this with either the `aliases` or `extend-aliases` options. Similarly, I think you could get around the N812 violation by configuring [`lint.pep8-naming.ignore_names`](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names), as you said.

As an aside, it looks like you have to add `F` to `extend-ignore-names`, not `functional`: https://play.ruff.rs/80ffcd74-1b08-4c19-bea4-f973fcff9850

---

_Label `configuration` added by @ntBre on 2025-05-29 20:20_

---

_Label `needs-info` added by @MichaReiser on 2025-05-29 21:12_

---

_Comment by @IlanCosman on 2025-05-30 18:12_

> Just to make sure I understand, is the suggestion here to add this to the default value for [lint.flake8-import-conventions.aliases](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases)? I think pytorch users could already configure this with either the aliases or extend-aliases options. Similarly, I think you could get around the N812 violation by configuring [lint.pep8-naming.ignore_names](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names), as you said.

Yah, everything you mentioned is correct. I think it would be good for the standard name to be enforced by default, and for it to not be a violation of N812. 

> As an aside, it looks like you have to add F to extend-ignore-names, not functional: https://play.ruff.rs/80ffcd74-1b08-4c19-bea4-f973fcff9850

Apparently both work, thanks for showing me that :)

---

Maybe the most reasonable way to do this is to just say that anything defined in import-conventions.aliases automatically passes N812. If a name is the standard, as defined by import-conventions.aliases, then rules should just accept that that's an allowed name.

---

_Label `needs-info` removed by @ntBre on 2025-06-02 16:28_

---

_Label `needs-decision` added by @ntBre on 2025-06-02 16:28_

---

_Referenced in [astral-sh/ruff#19394](../../astral-sh/ruff/issues/19394.md) on 2025-07-17 13:52_

---

_Comment by @guillaume-alliander on 2025-07-18 07:43_

> Maybe the most reasonable way to do this is to just say that anything defined in import-conventions.aliases automatically passes N812.

I would even go one step further: anything explicitly defined in in import-conventions.aliases / import-convention.extend-aliases should pass *all* N rules.

---

_Comment by @andrea-gasparini on 2025-09-04 10:04_

> Just to make sure I understand, is the suggestion here to add this to the default value for [lint.flake8-import-conventions.aliases](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases)? I think pytorch users could already configure this with either the `aliases` or `extend-aliases` options. 

Not sure I understood if the suggested change to `flake8-import-conventions.extend-aliases` is actually supposed to get around the N812 rule (doesn't look like it: https://play.ruff.rs/3cd93b53-1ab0-4aa8-a916-af3c64afcad3) or if the only workaround for now is to edit the [pep8-naming.ignore_names](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names).

---

_Comment by @ntBre on 2025-09-05 15:49_

Yeah, I think `ignore-names` is the key for now. The `aliases` came up in a couple of related suggestions to also add this as a default, known alias and to respect `aliases` in `N812` automatically, but neither of those are implemented today.

---

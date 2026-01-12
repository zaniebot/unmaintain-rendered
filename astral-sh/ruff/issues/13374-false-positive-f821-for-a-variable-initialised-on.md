```yaml
number: 13374
title: False positive F821 for a variable initialised on the first line of %%timeit (Jupyter notebook)
type: issue
state: closed
author: vitawasalreadytaken
labels:
  - question
  - notebook
assignees: []
created_at: 2024-09-17T08:25:58Z
updated_at: 2024-09-17T15:32:53Z
url: https://github.com/astral-sh/ruff/issues/13374
synced_at: 2026-01-12T15:54:53Z
```

# False positive F821 for a variable initialised on the first line of %%timeit (Jupyter notebook)

---

_@vitawasalreadytaken_

Hi, I've searched for "timeit F821", seen #7908 and understand that ruff should ignore magic cells. Strangely though, I'm getting F821 Undefined name on a magic cell where there is a variable defined on the first (initialisation) line and used on the subsequent lines.

The cell is
```
%%timeit test_tokenizer = CustomLemmaTokenizer()
test_tokenizer(
    "some string..."
)
```

The output is
```
% ruff check my_notebook.ipynb
my_notebook.ipynb:cell 21:2:1: F821 Undefined name `test_tokenizer`
  |
1 | %%timeit test_tokenizer = CustomLemmaTokenizer()
2 | test_tokenizer(
  | ^^^^^^^^^^^^^^ F821
3 |     "some string..."
4 | )
  |
```

I'd expect ruff to either ignore the cell entirely (as claimed in the linked issue) or understand that the first line of a `%%timeit` statement is an initialisation line which can define variables.

ruff 0.6.5, default settings.

---

_Label `question` added by @MichaReiser on 2024-09-17 08:31_

---

_Label `notebook` added by @MichaReiser on 2024-09-17 08:31_

---

_Comment by @MichaReiser on 2024-09-17 08:31_

> I'd expect ruff to either ignore the cell entirely (as claimed in the linked issue) or understand that the first line of a %%timeit statement is an initialisation line which can define variables.

Looking at the example I get the impression that this is exactly what Ruff is doing. It ignores the `%%timeit`, but that means that it won't be aware of the assignment to `test_tokenizer` which is why the usage of `test_tokenizer` on the next line gets flagged. 

---

_Comment by @vitawasalreadytaken on 2024-09-17 09:30_

This is a magic **cell** though – if ruff ignores the first line, it should ignore the whole cell IMO. Otherwise it doesn't make much sense to just ignore the first line but report errors in the rest of the cell (which depends on the first line).

---

_Comment by @charliermarsh on 2024-09-17 14:11_

This is covered in https://github.com/astral-sh/ruff/issues/10454 and a few other issues. We don't parse Python content within magic Jupyter magics. I think ignoring the rest of the cell would cause even more problems, since any downstream cells that rely on declarations below the magic would _also_ be missed.

---

_Closed by @charliermarsh on 2024-09-17 14:12_

---

_Comment by @vitawasalreadytaken on 2024-09-17 15:32_

I respect the decision to close this, and I'm already ignoring this error with a noqa comment.

Just wanted to point out that, FYI, ignoring a `%%timeit` **block** (note the double percent sign) is safe because variables defined therein do not leave the scope of that cell 🙂

Of course this isn't very important because these blocks are only used for ad-hoc, interactive benchmarking.

<img width="682" alt="Screenshot 2024-09-17 at 5 29 30 PM" src="https://github.com/user-attachments/assets/5cd5e6b8-e15b-487f-82c0-0e20aee24aad">


---

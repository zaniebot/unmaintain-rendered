```yaml
number: 11396
title: Incorrect character counting for non-ASCII characters
type: issue
state: closed
author: kotet
labels:
  - question
assignees: []
created_at: 2024-05-13T02:47:29Z
updated_at: 2024-05-14T07:58:35Z
url: https://github.com/astral-sh/ruff/issues/11396
synced_at: 2026-01-10T11:09:53Z
```

# Incorrect character counting for non-ASCII characters

---

_Issue opened by @kotet on 2024-05-13 02:47_

I am a Japanese speaker and am working on a project where Japanese is used as part of the code.
I have gotten different output from black and ruff for the line length.

black:

```python
s = "テスト用コードです。改行を除いて87文字です" + " (This is a test code. 87 characters w/o line breaks)"

t = "this is a test code. 87 characters" + " w/o line breaks and non-ascii characters"

```

ruff:

```bash
$ ruff format test.py
```

```python
s = (
    "テスト用コードです。改行を除いて87文字です"
    + " (This is a test code. 87 characters w/o line breaks)"
)

t = "this is a test code. 87 characters" + " w/o line breaks and non-ascii characters"

```

ruff version: `ruff 0.4.4`

---

_Comment by @zanieb on 2024-05-13 02:50_

Hi! I believe this is a [known deviation](https://docs.astral.sh/ruff/formatter/black/#line-width-vs-line-length) from Black.

> Ruff uses the Unicode width of a line to determine if a line fits. Black uses Unicode width for strings, and character width for all other tokens. Ruff also uses Unicode width for identifiers and comments.

Related https://github.com/astral-sh/ruff/pull/3714 and https://github.com/psf/black/pull/3445

---

_Comment by @kotet on 2024-05-13 03:00_

If all tokens are counted in unicode width, line splitting should not occur since `s = "テスト用コードです。改行を除いて87文字です" + " (This is a test code. 87 characters w/o line breaks)"` is ~87~86 characters long.

---

_Comment by @kotet on 2024-05-13 03:22_

These two lines have the same number of characters (88 + newline):

```python
u = "this is test"  # comment 🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍

u = "this is test" + "🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍"
```

However, the formatting results are different:

```python
u = "this is test"  # comment 🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍

u = (
    "this is test"
    + "🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍"
)
```

---

_Comment by @kotet on 2024-05-13 04:11_

`ruff check`result:
```bash
$ ruff check test.py 
test.py:1:60: E501 Line too long (146 > 88)
test.py:3:56: E501 Line too long (153 > 88)
Found 2 errors.
```

It seems that the character count will not work correctly in some situations.

---

_Comment by @153957 on 2024-05-13 05:27_

I believe ruff does not format (wrap) long trailing comments if associated with code on the same line. Probably because in this case it does not know if the comment is about the variable name or the value. Using ASCII characters in your example also does not cause ruff formatting to rewrap:

```
u = 'this is test'  # comment comment commentcommentcomment commentcommentcomment commentcommentcommentcommentcommentcommentcommentcommentcommentcommentcommentcommentcomment
```


---

_Comment by @kotet on 2024-05-13 06:52_

Sorry, my example are misguided! I created a more appropriate reproduction code.
All four lines below have the same number of characters (48).

```python
"🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍! + !🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍"
"🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍" + "🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍"
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa! + !aaaaaaaaaaa"
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" + "aaaaaaaaaaa"
```

pyproject.toml:

```toml
[tool.ruff]
[tool.ruff.lint]
select = [
    "E",
    "F",
    "W",
]
```

lint result:

```bash
$ ruff check test.py 
test.py:1:48: E501 Line too long (89 > 88)
test.py:2:48: E501 Line too long (89 > 88)
Found 2 errors.
```

format result:

```python
"🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍! + !🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍"

(
    "🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍"
    + "🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍"
)
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa! + !aaaaaaaaaaa"
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" + "aaaaaaaaaaa"
```



---

_Comment by @charliermarsh on 2024-05-13 17:04_

To be clear, we do _not_ use character count. We use character _width_.

---

_Comment by @kotet on 2024-05-14 02:42_

Oh, I see! I misunderstood "character width" to mean the number of bytes. I understand this works as intended.

---

_Closed by @kotet on 2024-05-14 02:42_

---

_Label `question` added by @dhruvmanila on 2024-05-14 07:58_

---

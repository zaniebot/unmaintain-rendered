```yaml
number: 3825
title: Line length incorrect for non-ascii text
type: issue
state: closed
author: snstanton
labels:
  - needs-decision
assignees: []
created_at: 2023-03-30T18:40:49Z
updated_at: 2024-07-18T06:10:45Z
url: https://github.com/astral-sh/ruff/issues/3825
synced_at: 2026-01-10T11:09:46Z
```

# Line length incorrect for non-ascii text

---

_Issue opened by @snstanton on 2023-03-30 18:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
version: ruff 0.0.260

The line length calculation for non-ascii characters is incorrect:

```
x = '12345678901234567890123456789012345678901234567890123456789012345678901234'
y = 'これは長い列です90これは長い列です90これは長い列です90これは長い列です90これは長い列です90これは長い列です90これは長い列です90これは長'
```

Both lines are under the character limit, but ruff flags the second line as too long, reporting the number of bytes rather than the number of characters.  This changed recently.  0.254 accepted this file, but 0.260 rejects it:
```
ruff bug.py
bug.py:2:81: E501 Line too long (140 > 80 characters)
```

pyproject.toml:
```
[tool.ruff]
line-length = 80

select = ["E"]

[tool.ruff.flake8-quotes]
inline-quotes = "single"
```

---

_Renamed from "Line length calculation does not work on " to "Line length incorrect for non-ascii text" by @snstanton on 2023-03-30 18:41_

---

_Comment by @charliermarsh on 2023-03-30 18:47_

It's not quite the number of bytes (or, at least, it _shouldn't_ be). Instead, it's the unicode width (or, at least, it _should_ be). The relevant PR is here: https://github.com/charliermarsh/ruff/pull/3714.


---

_Comment by @Yasu-umi on 2023-04-05 01:48_

Hi!  https://github.com/charliermarsh/ruff/pull/3714 is finished, so this issue is already solved ?
I added the following case to the `crates/ruff/resources/test/fixtures/pycodestyle/E501.py` in current main branch, and running `cargo run -p ruff_cli -- check crates/ruff/resources/test/fixtures/pycodestyle/E501.py --no-cache --select E501`, but I didn't get expected result.
```python
# Ok length=84
class JapaneseOk:
    """
    このドキュメントはテスト用のダミーです。十分に長い文章を用意しても大丈夫なことを確認してください。このくらいの長さではまだエラーになりません、
    """


# Error length=91
class JapaneseError:
    """
    このドキュメントはテスト用のダミーです。十分に長い文章を用意しても大丈夫なことを確認してください。このくらいの長さではまだエラーになりません、もーっともっと伸ばすとエラーになります。
    """
```

---

_Comment by @charliermarsh on 2023-04-05 01:53_

It _should_ be, yeah. I'll \cc @MichaReiser who may know better!

---

_Comment by @MichaReiser on 2023-04-05 12:32_

We don't report either of the two examples because both strings contain no whitespace, meaning there's no reasonable position where the user can break the text over multiple lines. 

https://github.com/charliermarsh/ruff/blob/46bcb1f725999540a315bc6fd44f455ec3cc38a9/crates/ruff/src/rules/pycodestyle/helpers.rs#L32-L36

Ruff reports both lines when adding whitespace somewhere inside the text


```python
# Ok length=84
class JapaneseOk:
    """
    このドキュメントは テスト用のダミーです。十分に長い文章を用意しても大丈夫なことを確認してください。このくらいの長さではまだエラーになりません、
    """


# Error length=91
class JapaneseError:
    """
    このドキュメントは テスト用のダミーです。十分に長い文章を用意しても大丈夫なことを確認してください。このくらいの長さではまだエラーになりません、もーっともっと伸ばすとエラーになります。
    """
```

Ruff reports both lines because both exceed the unicode width of 88:
* The first line has a width of 120 characters
* The second line has a width of 152 characters

---

_Comment by @Yasu-umi on 2023-04-06 03:22_

thanks for detailed explanation !

> because both strings contain no whitespace

I misunderstood about this.

> unicode width

I also misunderstood about this. (I assumed ruff uses &str.chars().count().)

Do you have any plan for move to grapheme cluster count ? (ex: [use unicode_segmentation](https://stackoverflow.com/questions/51818497/how-do-i-count-unique-grapheme-clusters-in-a-string-in-rust))
Of course, this counting is more expensive than &str.len(). but, is important (& easy for understanding err) for people use non-ascii chars language. please consider this!

---

_Comment by @MichaReiser on 2023-04-06 07:07_

> I also misunderstood about this. (I assumed ruff uses &str.chars().count().)

Ruff did use `chars().count()` but we changed it in #3714 to align with black. Pycodestyle does use `chars().count()` because [their reasoning](https://github.com/PyCQA/pycodestyle/pull/345) is that they don't want to add the complexity to their tool and PEP8 recommends using ASCII only.

[This commit](https://github.com/psf/black/commit/ef6e079901d53a42dfae4ab10b081ce7a73a47b5) in the black repository has a good explanation why they (and Ruff) choose unicode width over character or grapheme count.

---

_Comment by @eviljeff on 2023-05-05 10:46_

We have a case where ruff is reporting an over count line (112 vs our standard 88) but if we attempt to split it into two lines black auto-formats it back to one line because it'll fit on one line. 

Is there still some misalignment between ruff and black, or is there a correct "fix" to this (other than adding `#noqa: E501` to the end)?

```python
         query = '남포역립카페추천 ˇjjtat닷컴ˇ ≡제이제이♠♣ 남포역스파 남포역op남포역유흥≡남포역안마남포역오피 ♠♣'
```

---

_Comment by @MichaReiser on 2023-05-05 15:22_

Oh, I only now realized that Black only changed the behavior for the preview style but not for the "stable" style. You can see this in the [changes.md](https://github.com/psf/black/blob/main/CHANGES.md#2330). Black also only changed the behavior for string literals only, not for all characters. 

@eviljeff could you try using Black's preview style to see if that fixes the issue? I wonder if we should add a configuration setting to allow users to pick their preferred "width" measurement unit. 

---

_Comment by @eviljeff on 2023-05-05 16:53_

> Oh, I only now realized that Black only changed the behavior for the preview style but not for the "stable" style. You can see this in the [changes.md](https://github.com/psf/black/blob/main/CHANGES.md#2330). Black also only changed the behavior for string literals only, not for all characters.
> 
> @eviljeff could you try using Black's preview style to see if that fixes the issue? 

It does fix that problem - `black --preview` splits the the string onto two lines which ruff accepts. 

(Although if we wanted to use that as a workaround it wouldn't be a solution today as `black --preview` - for some reason - is rewriting `"['categories']."` to `'[\'categories\'].'` which ruff then raises as `Q003` 🤷‍♂️.  I hope that's fixed in black before the change moves to stable)

> I wonder if we should add a configuration setting to allow users to pick their preferred "width" measurement unit.

Personally, I prefer measuring for the line length seen in a (modern) code editor that supports unicode, because a line limit is mostly about readability, but practically, aligning with black is the most efficient solution (unless black adds a similar setting - which I would guess is unlikely given the project ethos of minimal configuration).  

---

_Comment by @eviljeff on 2023-05-05 17:00_

> (Although if we wanted to use that as a workaround it wouldn't be a solution today as `black --preview` - for some reason - is rewriting `"['categories']."` to `'[\'categories\'].'` which ruff then raises as `Q003` 🤷‍♂️. I hope that's fixed in black before the change moves to stable)

fwiw, looks like that bug(?) has been reported already as https://github.com/psf/black/issues/3643 (the example I included was also a multi-line string)

---

_Comment by @sgryjp on 2023-05-16 15:54_

@MichaReiser I think that the new behavior (using unicode-width instead of byte-count) should be an opt-in feature because Black's preview style is also an opt-in feature until Jan 2024... it's too far.

Note that, for the Black's preview style, "there are no guarantees around the stability of the output" according to their [stability policy](https://black.readthedocs.io/en/stable/the_black_code_style/index.html#stability-policy). I don't think we should use it for production code.

---

_Label `question` added by @charliermarsh on 2023-06-16 02:25_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:16_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:16_

---

_Comment by @MichaReiser on 2023-12-07 23:43_

Black's preview style is about to be released and it includes the change to measure strings using the unicode width rather the character length. Ruff's formatter already uses unicode width.

We also updated our documentation to improve the explanation that it isn't the character limit when using emoji or east asian characters. 



---

_Closed by @MichaReiser on 2023-12-07 23:43_

---

_Comment by @jackjyq on 2024-07-18 06:08_

Even ruff uses Unicode width to measure line length, the editor font might not respect that, which appears as an in-consistency line length in ruff's auto-formatting or E501 reporting.

To resolve this, your favorite editor shall employ a carefully designed font. 

For example [Sarasa-Gothic](https://github.com/be5invis/Sarasa-Gothic)

![image](https://github.com/user-attachments/assets/477bd54a-81e1-4a7b-a012-928bb6d1bcf0)


---

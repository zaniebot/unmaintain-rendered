```yaml
number: 11041
title: "ruff format doesn't respect # fmt: skip comment for line-length"
type: issue
state: closed
author: riZZZhik
labels: []
assignees: []
created_at: 2024-04-19T13:51:15Z
updated_at: 2024-04-20T16:22:42Z
url: https://github.com/astral-sh/ruff/issues/11041
synced_at: 2026-01-12T15:54:50Z
```

# ruff format doesn't respect # fmt: skip comment for line-length

---

_@riZZZhik_

I am running ruff 0.3.7.

line-length is modified and E501 is ignored for file with issue

```toml
[tool.ruff]
line-length = 100

[tool.ruff.lint.extend-per-file-ignores]
"tests/units/data/*" = ["E501"]
```

But `ruff format` command doesn't respect `# fmt: skip` comment after line longer than line-length

```shell
❯ ruff format --diff
--- tests/units/data/test_texts.py
+++ tests/units/data/test_texts.py
@@ -43,7 +43,25 @@
    TextItem(
        "<speak>    Привет    как дела   ? </speak>",
         ParsedItem(
-            ["п", "р'", "и", "в'", "е+", "т", "\t", "к", "а+", "к", "\t", "д'", "и", "л", "а+", "\t", "?"],  # fmt: skip
+            [
+                "п",
+                "р'",
+                "и",
+                "в'",
+                "е+",
+                "т",
+                "\t",
+                "к",
+                "а+",
+                "к",
+                "\t",
+                "д'",
+                "и",
+                "л",
+                "а+",
+                "\t",
+                "?",
+            ],  # fmt: skip
         ),
     ),
     TextItem(

1 file would be reformatted, 40 files already formatted
```

Code is:

```python
[
    ...
    TextItem(
        "<speak>    Привет    как дела   ? </speak>",
        ParsedItem(
            ["п", "р'", "и", "в'", "е+", "т", "\t", "к", "а+", "к", "\t", "д'", "и", "л", "а+", "\t", "?"],  # fmt: skip
        ),
    ),
    ...
]
```

---

_Comment by @MichaReiser on 2024-04-19 13:57_

Ruff only supports `fmt: skip` at the statement level

```python
[
    ...
    TextItem(
        "<speak>    Привет    как дела   ? </speak>",
        ParsedItem(
            ["п", "р'", "и", "в'", "е+", "т", "\t", "к", "а+", "к", "\t", "д'", "и", "л", "а+", "\t", "?"],
        ),
    ),
    ...
]  # fmt: skip
```

You can enable [`RUF028`](https://docs.astral.sh/ruff/rules/invalid-formatter-suppression-comment/) to get warned about incompatible formatter suppression comments.

---

_Closed by @MichaReiser on 2024-04-19 13:57_

---

_Comment by @riZZZhik on 2024-04-19 13:58_

Understood, thanks!

Strange that all RUF checks are enabled yet it doesn't warn me about this on ruff check

---

_Comment by @MichaReiser on 2024-04-19 14:44_

Hmm that's strange. It get's correctly flagged in the playground https://play.ruff.rs/6c0cc2fc-7028-4ffa-a8ef-43ca4b0ee24f



---

_Comment by @riZZZhik on 2024-04-20 15:36_

@MichaReiser Can you check this code https://pastebin.com/1ZSKYSae ?

---

_Comment by @riZZZhik on 2024-04-20 15:37_

Also, I was unable to open your link to the playground, it leaves me with a blank screen.

---

_Comment by @charliermarsh on 2024-04-20 15:41_

Works fine for me in Chrome, perhaps an issue with your browser:

![Screenshot 2024-04-20 at 11 41 14 AM](https://github.com/astral-sh/ruff/assets/1309177/7b35e642-0889-47b5-94a3-6b111e7c7e85)


---

_Comment by @MichaReiser on 2024-04-20 15:42_

@riZZZhik what browser are you using?

I tried your code and Ruff correctly flags https://play.ruff.rs/249420c2-2a8e-497e-85bf-cf7722cbdabc

Oh, I think I know whats happening. Do you have `ruff.lint.preview = true` or `ruff.preview = true` in your configuration? We should stabalize this rule in the next minor release (CC: @snowsignal )

---

_Comment by @riZZZhik on 2024-04-20 16:22_

Yes, you are right, preview was disabled. After setting `ruff.preview = true` warning appeared.

---

I am from Russia and get request to https://api.astral-1ad.workers.dev/249420c2-2a8e-497e-85bf-cf7722cbdabc gives `ERR_TIMED_OUT`.
Worked fine when i enabled vpn, i guess provider blocks connection from here

<img width="1512" alt="Screenshot 2024-04-20 at 7 18 11 PM" src="https://github.com/astral-sh/ruff/assets/21087403/0e0058ae-565e-4b6e-a953-d9c4ac0a439f">


---

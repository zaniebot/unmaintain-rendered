```yaml
number: 13088
title: "PLW3301 (nested-min-max) Autofix inappropriately strips nested function call when the nested function call wasn't `max`"
type: issue
state: closed
author: AustinStarnes
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-08-25T01:39:43Z
updated_at: 2024-08-26T12:56:18Z
url: https://github.com/astral-sh/ruff/issues/13088
synced_at: 2026-01-12T15:54:52Z
```

# PLW3301 (nested-min-max) Autofix inappropriately strips nested function call when the nested function call wasn't `max`

---

_@AustinStarnes_

> Keywords used searching for existing issues: `"nested-min-max", "PLW3301"`

> Using Ruff VSCode extension `v2024.42.0`

<blockquote>
<details><summary>Click to expand <code>ruff.toml</code>:</summary>
<pre lang=toml>
line-length = 88
indent-width = 4
<br/>
[lint]
preview = true
select = ["ALL"]
ignore = [
    "ANN204", 
    "S110",
    "DOC201",
    "DOC501", 
    "D107", 
    "D105", 
    "SIM102", 
    "S404", 
    "PLW1514", 
    "ANN002", 
    "ANN003", 
    "PYI051", 
    "FIX002", 
    "ANN401", 
    "DOC402",
    "B008",
    "FAST002",
    "RET504"
]
</pre>
</details>
</blockquote>


<blockquote>
<details><summary>Click to expand VSCode User <code>settings.json</code>:</summary>
<pre lang=json>
 // ...
    "[python]": {
        "editor.formatOnSave": true,
        "editor.formatOnType": true,
        "editor.codeActionsOnSave": {
            "source.fixAll.ruff": "always",
        },
// ...
    "ruff.lint.args": [
        "--config",
        "/home/austin/.vscode/ruff.toml",
        "--ignore", "INP001,TRY003,EM101,EM102,BLE001,G004,N806,TD002,TD003",
        "--target-version", "py311",
        "--unfixable", "F401"
    ],
// ...
</pre>
</blockquote>

<blockquote>
<details><summary>Click to expand the real context in which I encountered the bug, in case I am accused of reporting an issue that isn't encountered in real-world use cases:</summary>
To get the width with which to print and right-justify a token produced by an LLM, I needed the max length of any decoded token string (plus two for bounding <code>▻</code> and <code>◅</code> characters that will be printed around the token).
<br/>
But, in cases where none of the tokens generated were at least 3 (two less than the length of the column label "token"), this width with which to print and right-justify would be too narrow. So, I wanted the max of not just the token string length (plus two), but the max of the token string length (plus two) and the length of the label "token".
<pre lang=python>
# ...
    token_string_label = "token"
    max_token_char_length = max(
        max(len(tokenizer.decode(tok)) + 2 for tok in generated_tokens[0]),
        len(token_string_label),
    )
    print(
        f"| {token_string_label.rjust(max_token_char_length)} "
        f"| {'token id':>8s} | {'log prob.':>9} | {'prob.':>6}",
    )
    for tok, score in zip(generated_tokens[0], transition_scores[0], strict=False):
        print(
            f"| {('▻' + tokenizer.decode(tok) + '◅').rjust(max_token_char_length)} "
            f"| {tok:8d} | {score.item():9.3f} | {torch.exp(score).item():.2%}",
        )
# ...
</pre>
</blockquote>

Minimal example:
```py
sentence = "Hello world, ruff is cool!"

# The expression triggering rule PLW3301 (nested-min-max):
max_word_len = max(
    max(len(word) for word in sentence.split(" ")),
    len("Done!"),
)
# >>> max_word_len
# 6

# What the autofix expression produced:
max_word_len = max(*(len(word) for word in sentence.split(" ")), *"Done!")
# >>> max_word_len = max(*(len(word) for word in sentence.split(" ")), *"Done!")
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: '>' not supported between instances of 'str' and 'int'

# If nested-min-max must be autofixed, I would imagine the result should be:
max_word_len = max(*(len(word) for word in sentence.split(" ")), len("Done!"))
# i.e., if an argument to the outer max() is not the result of a call to max(), then it
# should be left as is

```


I believe issue #5066 also demonstrated the bug, but the issue was closed because the reporter's usage of max wasn't valid either. The issue probably should not have been closed.


---

_Renamed from "PLW3301 (nested-min-max) Autofix inappropriately strips function call for outer max argument" to "PLW3301 (nested-min-max) Autofix inappropriately strips nested function call when the nested function call wasn't `max`" by @AustinStarnes on 2024-08-25 01:42_

---

_Label `bug` added by @AlexWaygood on 2024-08-25 09:16_

---

_Label `fixes` added by @AlexWaygood on 2024-08-25 09:16_

---

_Closed by @dhruvmanila on 2024-08-26 04:31_

---

_Closed by @dhruvmanila on 2024-08-26 04:31_

---

_Comment by @AustinStarnes on 2024-08-26 12:56_

ruff dev team fast like ruff 🚀 cheers!

---

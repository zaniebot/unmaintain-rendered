```yaml
number: 12150
title: "Remove use of deprecated `E999` from the `fuzz-parser` script"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: fuzzer-e999
created_at: 2024-07-02T13:08:42Z
updated_at: 2024-07-02T13:18:57Z
url: https://github.com/astral-sh/ruff/pull/12150
synced_at: 2026-01-10T21:56:00Z
```

# Remove use of deprecated `E999` from the `fuzz-parser` script

---

_Pull request opened by @AlexWaygood on 2024-07-02 13:08_

## Summary

Using the `E999` code is now deprecated, so we shouldn't use it in the `fuzz-parser` script anymore. I _think_ the most idiomatic way of asking Ruff to "please detect syntax errors, but do not report any other checks" is now `--config='lint.select=[]'`.

I also bumped the requirement pins in the `requirements.txt` file, as they hadn't been updated in a while.

## Test Plan

In a virtual environment with the requirements installed, I ran `python scripts/fuzz-parser/fuzz.py $(shuf -i 0-100000 -n 1000)` to verify that the script still succeeded with these changes (it did). I then deliberately broke the parser with the below diff, and reran the command, to check that the fuzzer still reports when Ruff fails to parse valid syntax (it does):

```diff
diff --git a/crates/ruff_python_parser/src/lexer.rs b/crates/ruff_python_parser/src/lexer.rs
index d407ab357..2996b657a 100644
--- a/crates/ruff_python_parser/src/lexer.rs
+++ b/crates/ruff_python_parser/src/lexer.rs
@@ -364,18 +364,14 @@ impl<'src> Lexer<'src> {
             '\'' | '"' => self.lex_string(c),
             '=' => {
                 if self.cursor.eat_char('=') {
-                    TokenKind::EqEqual
+                    TokenKind::Unknown
                 } else {
                     self.state = State::AfterEqual;
-                    return TokenKind::Equal;
+                    return TokenKind::Unknown;
                 }
             }
             '+' => {
-                if self.cursor.eat_char('=') {
-                    TokenKind::PlusEqual
-                } else {
-                    TokenKind::Plus
-                }
+                TokenKind::Unknown
             }
             '*' => {
                 if self.cursor.eat_char('=') {
```


---

_Label `ci` added by @AlexWaygood on 2024-07-02 13:08_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-07-02 13:08_

---

_@MichaReiser approved on 2024-07-02 13:14_

---

_Comment by @AlexWaygood on 2024-07-02 13:15_

fun fact: my initial attempt to break the parser (not the diff I posted above) did indeed break the parser, but the outcome was that the script started using 100% of available memory on my MacBook, meaning everything completely froze and I had to force restart

---

_Merged by @AlexWaygood on 2024-07-02 13:18_

---

_Closed by @AlexWaygood on 2024-07-02 13:18_

---

_Branch deleted on 2024-07-02 13:18_

---

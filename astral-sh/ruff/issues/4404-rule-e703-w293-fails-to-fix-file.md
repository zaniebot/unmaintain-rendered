```yaml
number: 4404
title: "Rule `E703, W293` fails to fix file"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-05-12T21:17:37Z
updated_at: 2023-06-20T02:57:26Z
url: https://github.com/astral-sh/ruff/issues/4404
synced_at: 2026-01-12T15:54:44Z
```

# Rule `E703, W293` fails to fix file

---

_@qarmin_

Ruff https://github.com/charliermarsh/ruff/commit/f5be3d8e5b2e3f3a0c5890075b552371f4061023

Command - `ruff --fix` with config with all rules enabled

File 
```
if True:
    x = 1; \
   
```
shows error
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_11381303964.py`, the rule codes E703, W293, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_11381303964.py:1:1: D100 Missing docstring in public module
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_11381303964.py:1:1: INP001 File `Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_11381303964.py` is part of an implicit namespace package. Add an `__init__.py`.
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_11381303964.py:2:10: E703 Statement ends with an unnecessary semicolon
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_11381303964.py:3:1: W293 Blank line contains whitespace
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_11381303964.py:3:4: W292 No newline at end of file
```

EDIT - file - [aa.py.zip](https://github.com/charliermarsh/ruff/files/11504523/aa.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-05-12 21:36_

---

_Comment by @evanrittenhouse on 2023-05-17 03:30_

Feel free to assign this to me, I can take a look!

---

_Assigned to @evanrittenhouse by @charliermarsh on 2023-05-17 03:58_

---

_Comment by @evanrittenhouse on 2023-05-18 00:16_

Hmm, I can't seem to replicate this. I copied the Markdown file from the description and ran `ruff test.py --fix`, which seemed to successfully fix the problem. Are there any other examples of this that you've come across?

---

_Comment by @charliermarsh on 2023-05-18 00:44_

Me neither -- gonna close for now...

---

_Closed by @charliermarsh on 2023-05-18 00:44_

---

_Comment by @qarmin on 2023-05-18 05:05_

I added reproduction file.

Looks that pasting code to github changes some bytes, so proably I should always provide example file
```
Real content
0000000 6669 5420 7572 3a65 200a 2020 7820 3d20
0000010 3120 203b 0a5c 2020 0020               
0000019

After copying from github
0000000 6669 5420 7572 3a65 200a 2020 7820 3d20
0000010 3120 203b 0a5c 2020 0a20               
000001a
```

---

_Reopened by @charliermarsh on 2023-05-18 12:15_

---

_Comment by @evanrittenhouse on 2023-05-20 19:08_

Hmm. Even with the new file, it seems to work properly...
```
ruff/crates/ruff on  4404_fix_e703_w293 [ ?1 *2 ] via Python v3.11.3 via 🦀 v1.69.0
❯ ruff ~/Downloads/aa.py --fix
Found 1 error (1 fixed, 0 remaining).
```

---

_Comment by @qarmin on 2023-05-20 19:37_

```
ruff aa.py --fix --select ALL
```
have still problem(W293 is not enabled by default)
strange that 
```
ruff aa.py --fix --select E703,W293
```
not have any problems


---

_Comment by @evanrittenhouse on 2023-06-08 03:27_

Sorry for the delay - work has been insane and have had to step back from the project for a bit. Just now getting to this. 

I think I've isolated the problem. Basically, running `--select ALL` yields four errors before the autofix throws:
```
❯ cargo run -p ruff_cli ./aa.py --fix --select ALL --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff ./aa.py --fix --select ALL --no-cache`
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
error: Autofix introduced a syntax error in `aa.py` with rule codes E703, W293: unexpected EOF while parsing at byte offset 21
---
if True:
    x = 1 \

---
aa.py:1:1: D100 Missing docstring in public module
aa.py:2:10: E703 Statement ends with an unnecessary semicolon
aa.py:3:1: W293 Blank line contains whitespace
aa.py:3:4: W292 No newline at end of file
Found 4 errors.
```

I tested permutations of the various errors; it looks like the problem isn't with `E703` at all, but rather with `D100`. Some logs:
```
ruff on  4404_fix_e703_w293 via Python v3.11.3 via 🦀 v1.70.0
❯ cargo run -p ruff_cli ./aa.py --fix --select E703,D100 --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff ./aa.py --fix --select E703,D100 --no-cache`
aa.py:1:1: D100 Missing docstring in public module
Found 2 errors (1 fixed, 1 remaining).

ruff on  4404_fix_e703_w293 [ !1 ] via Python v3.11.3 via 🦀 v1.70.0
❮ cargo run -p ruff_cli ./aa.py --fix --select W292,D100 --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff ./aa.py --fix --select W292,D100 --no-cache`
aa.py:1:1: D100 Missing docstring in public module
Found 2 errors (1 fixed, 1 remaining).

ruff on  4404_fix_e703_w293 via Python v3.11.3 via 🦀 v1.70.0
❮ cargo run -p ruff_cli ./aa.py --fix --select W293,D100 --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff ./aa.py --fix --select W293,D100 --no-cache`
error: Autofix introduced a syntax error in `aa.py` with rule codes W293: unexpected EOF while parsing at byte offset 22
---
if True:
    x = 1; \

---
aa.py:1:1: D100 Missing docstring in public module
aa.py:3:1: W293 Blank line contains whitespace
Found 2 errors.
```

So I think what's happening is that the interaction between `W293` and `D100` is actually the root cause. I'll work on a follow-up for now. 

I think what may (more a note to myself than anything) be happening is that `D100` checks for a docstring at the last line of code, so when `W293` deletes the blank line, we re-lint the file and `D100` is expecting the same range. Need to look into how Ruff re-lints though.

---

_Comment by @evanrittenhouse on 2023-06-08 03:30_

Relatedly, @charliermarsh I wonder if we should look into our error reporting to see why the correct pair wasn't outputted. From what I can tell, in this case we only output what rules we've [already fixed](https://github.com/charliermarsh/ruff/blob/main/crates/ruff/src/linter.rs#L449), so `D100` wouldn't show up. Maybe we should show the full rule selection (or selector, since `ALL` would get messy) and keep track of which rules we've checked already? Not sure of a best path forward there, not super familiar with `linter.rs` yet.

---

_Comment by @evanrittenhouse on 2023-06-09 04:36_

Looks like the actual content of the file ends up being:
```python
'if True:\n'
'    x = 1; \\\n'
'   '
```

I was able to replicate the issue by attempting to run (in a separate file): 
```python
if True:
    x = 1; \
```
Note the missing newline after the `\`.

I think that's the root cause of the issue here - stripping the blank line strips all whitespace, leaving the last `\` which throws the syntax error.

It also seems like the issue only throws on `LintSource::Ast/Imports` rules (even without any imports). It throws with `D212, D205, SIM103, T100, A001, TID251` (all AST-based, picked from random linters) and `I001/I002` (imports), but not with `TD002, T001` (both token-based), `E101` (physical lines), `E211` (logical lines), `N999` (filesystem).

Just posting my updates here for future reference - sorry if that's bad form or if that's supposed to go in the PR or something. Regardless, I'll take a look at why the AST rules fail with `W293` tomorrow.

Wondering if this is related to #4828

E: Actually, this may only be related to `AST/Import` lints since they're the only ones that ascribe any meaning to the AST

---

_Comment by @addisoncrump on 2023-06-09 04:40_

This is very likely related to/duplicated by #4828.

---

_Comment by @evanrittenhouse on 2023-06-13 19:32_

@charliermarsh Would actually like your thoughts on this, since it may require some deeper fixes. 

I've been hacking around to see what's going on. Basically, we take the initial file (note that the last line is 3 spaces):
```python
if True:
    x = 1; \
   
```
We then, according to the fix of `W293`, strip out the last line since it's all whitespace. This then leads to a `SyntaxError` due to the file ending with `x = 1; \` (essentially trying to continue the line with nothing to continue). 

I was originally looking at including `;` and `\` in the [existing code's](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs#L82) whitespace detection as "effective whitespace". Basically, if a `\` terminates the line and the next non-whitespace character is a `;`, we know that we're attempting to split lines on an empty statement and should remove them as well.

I then realized that the issue is due to the way that we perform autofixes. We fix a line, then loop over the new AST and attempt to [re-parse](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/linter.rs#L415) it. This second parse introduces the error due to the RCA above. The issue here is that the fix actually spans fixing the two lines simultaneously (e.g., removing `; \\n<space><space><space>` all in one go - removing the three spaces first won't work, as outlined above. 

I'd like your input on how to fix this. It seems like we have two options:
1. Decouple the rule from the concept of `Line` - seems hacky since it's just this edge case. This would make the rule's implementation unique against other rules, which doesn't seem good.
2. Move it to a different linter (maybe `LintSource::Token`?) since those rules [don't seem to throw](https://github.com/astral-sh/ruff/issues/4404#issuecomment-1583963725). I'm not sure if we'd like to do that or even if it would work, just an initial solution I thought about.

If you have a cleaner solution in mind, or can offer any advice, that'd be much appreciated. Thanks!

---

_Closed by @charliermarsh on 2023-06-20 02:57_

---

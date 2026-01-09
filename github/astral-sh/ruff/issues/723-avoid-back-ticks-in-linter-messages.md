---
number: 723
title: Avoid back-ticks in linter messages
type: issue
state: closed
author: peterjc
labels:
  - question
assignees: []
created_at: 2022-11-13T18:16:22Z
updated_at: 2022-11-15T10:52:41Z
url: https://github.com/astral-sh/ruff/issues/723
synced_at: 2026-01-07T13:12:14-06:00
---

# Avoid back-ticks in linter messages

---

_Issue opened by @peterjc on 2022-11-13 18:16_

e.g. Trying ruff 0.0.114 on the Biopython codebase as an alternative to flake8 (for which is almost unbelievably fast, sub-second even if I deliberately run ``rm -rf .ruff_cache/`` first. Nice!):

```    
$ ruff Bio --extend-ignore E501,B007,E741,F401,F841,D105,B009,B010,B011
Bio/PDB/MMCIF2Dict.py:120:41: E713 Test for membership should be `not in`
```

Attempting to copy and past that into my git commit message at the command line is complicated due to the back ticks (bash attempts to run the command ``not in``). Typically flake8 plugins use single quotes, where that is not a problem.

---

_Comment by @andersk on 2022-11-13 18:55_

Many characters remain special to Bash inside double quotes. The way to suppress their special meaning is by surrounding them with single quotes ([documentation](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)):

```bash
git commit -am 'Fix E713 Test for membership should be `not in`'
```

which would actually become harder if ruff used single quotes itself.

(That said, if it were my project, I might have opted for Unicode curly quotes: `Test for membership should be ‘not in’`, like GCC.)

---

_Comment by @peterjc on 2022-11-13 21:57_

I know there are workarounds, my point was with flake8 and pycodestyle I don't need one because it uses single quote chars:

https://github.com/PyCQA/pycodestyle/blob/2.9.1/pycodestyle.py#L1418

i.e. I see this as a minor but annoying change in potentially switching from flake8 to ruff.

(Unicode quotes would be a neat alternative, they probably would work near universally nowadays.)

As an aside, the real flake8 and pycodestyle seems to miss this particular example https://github.com/biopython/biopython/pull/4159 very strange (e.g. ``$ flake8 --isolated --select E713 Bio/PDB/MMCIF2Dict.py`` to rule out a configuration issue) - so that's an upside.

---

_Comment by @peterjc on 2022-11-14 13:40_

This looks easy to fix in ruff, a series of string literal changes - the specific examples is here:

https://github.com/charliermarsh/ruff/blob/v0.0.117/src/checks.rs#L1347

I've not worked with rust before, but could attempt a pull request if welcome?

---

_Comment by @charliermarsh on 2022-11-14 18:09_

Let me take a look at a few other tools and see how they handle this! I initially liked the use of backticks since they render correctly in Markdown and (to my mind) had become a standard for indicating code snippets. But it's not worth it to me if it's causing a bunch of inconveniences.


---

_Label `question` added by @charliermarsh on 2022-11-14 18:14_

---

_Comment by @andersk on 2022-11-14 20:54_

“A bunch of inconveniences” seems overblown. Rust errors use backticks ``` `` ```; the Rust team has put a lot of effort into error message ergonomics generally, and thought about this specific question at least enough to put it in their [style guide](https://rustc-dev-guide.rust-lang.org/diagnostics.html#diagnostic-structure). I didn’t find any evidence of complaints from people incorrectly pasting backticks from Rust errors into their shell.

ASCII quotes `''` or `""` would be confusable with real Python syntax, and they’d still be misinterpreted under naïve shell pasting. Unicode quotes `‘’` or `“”` would be pretty and unambiguous (in good enough fonts), but might require a bit more effort from ruff contributors to maintain consistency. I assume we can all agree that the hideous `` `' `` “pairs” emitted by some older tools merit no consideration.

---

_Comment by @charliermarsh on 2022-11-15 02:32_

Hmm yeah. While I totally understand the critique @peterjc, I think I'm going to leave this as-is for now. If we continue to hear complaints, I'll definitely reconsider!


---

_Closed by @charliermarsh on 2022-11-15 02:32_

---

_Comment by @peterjc on 2022-11-15 10:52_

That's fine, it is only a minor inconvenience to me. Thank you.

I can see this was a deliberate choice away from the Python tools' use of single quotes to the back-ticks favoured by the Rust team (quoting the style guide Anders linked to):

> When code or an identifier must appear in a message or label, it should be surrounded with backticks

The Rust team were probably swayed by the back-tick use for literals in markdown and RST.

---

_Referenced in [astral-sh/ruff#2889](../../astral-sh/ruff/pulls/2889.md) on 2023-02-14 07:01_

---

_Referenced in [astral-sh/ruff#21163](../../astral-sh/ruff/pulls/21163.md) on 2025-10-31 13:39_

---

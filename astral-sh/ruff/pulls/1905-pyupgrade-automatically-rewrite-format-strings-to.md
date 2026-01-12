```yaml
number: 1905
title: "[pyupgrade] Automatically rewrite format-strings to f-strings"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: fstrings
created_at: 2023-01-16T03:11:34Z
updated_at: 2023-01-17T04:06:40Z
url: https://github.com/astral-sh/ruff/pull/1905
synced_at: 2026-01-12T15:55:07Z
```

# [pyupgrade] Automatically rewrite format-strings to f-strings

---

_@colin99d_

Draft opened for visibility. This is a part of #827. This PR is currently affected by the \N{emoji} bug that we are searching for a fix for.

---

_Renamed from "Fstrings" to "Pyupgrade: Fstrings" by @colin99d on 2023-01-16 03:11_

---

_Comment by @colin99d on 2023-01-16 20:33_

@charliermarsh this pr is ready except for the questions below, so I will move it out of draft. Before this is merged we need to consider or resolve the following items:
1. The `/N{emoji}` issue from UP031 remains unsolved (whenever we solve it I will integrate the solution here.
2. F strings introduce double bracket syntax `{{}}`, which is throwing my regexs for a loop. Currently, the best solution I have is `\{(?P<name>[^\W0-9]\w*)?(?P<fmt>[^{]*?)}` (this does NOT work fully). I tried using `libcst_native` but it just gives a normal string, and not a formatted one. I believe we could use `fancy_regex` here; however, that adds a whole dependency for one small functionality. As always, we could just only check for this and not fix it.
3. Similar to my PR for UP031, there is a negative test case I disagree with. 
```
"{}" . format(x)
```
This is not supposed to be formatted because of its "weird syntax", but our AST parser ignores the spaces. This means ignoring this is more work for us, and results in a worse user experience.
4. Lastly (and this one is kinda embarrassing). I cannot for the life of me get the proper `.snap` file to generate for tests. I have never had an issue before, but it keeps suggesting that the snap just be an empty list, even though running ruff on those files produces a lot of changes. I am sure its something small I am missing.

---

_Marked ready for review by @colin99d on 2023-01-16 20:33_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:1870 on 2023-01-17 01:30_

(This was causing the fixtures to be a no-op per your question in the PR summary @colin99d.)

---

_@charliermarsh reviewed on 2023-01-17 01:30_

---

_Review comment by @charliermarsh on `src/rules/pyupgrade/rules/f_strings.rs`:89 on 2023-01-17 02:15_

@colin99d - Is this strictly necessary? (Do you know of any case where it would trigger?)

---

_@charliermarsh reviewed on 2023-01-17 02:15_

---

_Comment by @charliermarsh on 2023-01-17 02:52_

@colin99d - I'm fine to ignore the unicode case. What else needs solving here? The `{{}}` case?

---

_@colin99d reviewed on 2023-01-17 02:53_

---

_Review comment by @colin99d on `src/checkers/ast.rs`:1870 on 2023-01-17 02:53_

Thank you!!

---

_Comment by @colin99d on 2023-01-17 02:57_

That should be the only thing!

---

_Comment by @charliermarsh on 2023-01-17 02:59_

I have one idea I'd like to try, which may let us avoid regexes.

---

_Comment by @charliermarsh on 2023-01-17 03:00_

(As-is, what happens in those cases? We just ignore them?)

---

_Comment by @colin99d on 2023-01-17 03:19_

> (As-is, what happens in those cases? We just ignore them?)

Right now it is ignored in a way. For example: below rust thinks it needs three arguments but only finds 2, so it exits. If we are going to ignore these, we should probably implement some better logic for ignoring them.
`{}{{}}{}".format(escaped, y)`

---

_Comment by @charliermarsh on 2023-01-17 03:22_

I think we can leverage RustPython's actual string parser and forego regular expressions. Trying it now...

---

_Renamed from "Pyupgrade: Fstrings" to "Pyupgrade: automatically rewrite format-strings to f-strings" by @charliermarsh on 2023-01-17 03:42_

---

_Renamed from "Pyupgrade: automatically rewrite format-strings to f-strings" to "[pyupgrade] Automatically rewrite format-strings to f-strings" by @charliermarsh on 2023-01-17 03:42_

---

_Comment by @charliermarsh on 2023-01-17 03:43_

@colin99d - I was able to use RustPython's own parser, and I think it's properly handling all test-cases now.

This might be a useful approach to use in the other PR too? I haven't looked into it deeply, but generally, it's best to use the RustPython parsers where we can.


---

_Merged by @charliermarsh on 2023-01-17 04:06_

---

_Closed by @charliermarsh on 2023-01-17 04:06_

---

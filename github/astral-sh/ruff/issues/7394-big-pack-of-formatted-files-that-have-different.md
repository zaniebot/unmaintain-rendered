---
number: 7394
title: Big pack of formatted files that have different output than black
type: issue
state: closed
author: qarmin
labels:
  - formatter
assignees: []
created_at: 2023-09-14T20:11:28Z
updated_at: 2023-09-28T07:35:02Z
url: https://github.com/astral-sh/ruff/issues/7394
synced_at: 2026-01-07T13:12:15-06:00
---

# Big pack of formatted files that have different output than black

---

_Issue opened by @qarmin on 2023-09-14 20:11_

Ruff 0.0.289

Pack of files, which contains differences between black/ruff - https://github.com/qarmin/Automated-Fuzzer/releases/download/test/Broken_Black_Formatting_0_0_289.zip


This files I took directly from pypi packages, without any changes/minimization

I tested 1,500,000 python files and I found 7535 different files, so that means that only 0.5 % formats differently.

This zip file contains 3 type of files - file formatted by black, file formatted by ruff, diff between them

E.g.
12330_black.py
```
def somar(valor1, valor2):
    resultado = valor1 + valor2
    return (
        "A Soma: "
        "{valor1} + {valor2} = {resultado}"
        "".format(valor1=valor1, valor2=valor2, resultado=resultado)
    )
```
12330_ruff.py
```
def somar(valor1, valor2):
    resultado = valor1 + valor2
    return "A Soma: " "{valor1} + {valor2} = {resultado}" "".format(
        valor1=valor1, valor2=valor2, resultado=resultado
    )
```
12330_diff.py
```
--- /home/rafal/test/DOWNLOADED/broken_files/12330_black.py	2023-09-14 21:57:26.352892470 +0200
+++ /home/rafal/test/DOWNLOADED/broken_files/12330_ruff.py	2023-09-14 21:57:33.788715213 +0200
@@ -1,7 +1,5 @@
 def somar(valor1, valor2):
     resultado = valor1 + valor2
-    return (
-        "A Soma: "
-        "{valor1} + {valor2} = {resultado}"
-        "".format(valor1=valor1, valor2=valor2, resultado=resultado)
+    return "A Soma: " "{valor1} + {valor2} = {resultado}" "".format(
+        valor1=valor1, valor2=valor2, resultado=resultado
     )
```

I compared `ruff format .` with `black .`.
Is there a way to remove automatically known incompatibilities?

To find differences, first I formatted file with ruff, next I formatted them with black and from output I took list of files that were formatted second them to report them here.

---

_Renamed from "Bick pack of formatted files that have different output than black" to "Big pack of formatted files that have different output than black" by @qarmin on 2023-09-15 03:37_

---

_Label `formatter` added by @MichaReiser on 2023-09-15 05:37_

---

_Comment by @charliermarsh on 2023-09-15 20:43_

Thank you for doing this! It's really cool to see the compatibility statistics at-scale. I don't think there will be an easy way to deduplicate these with known fixes, but I'm gonna spend a few minutes skimming through and triaging where I can.

It would be nice to re-run this after a few releases to see how the metrics evolve.

---

_Comment by @charliermarsh on 2023-09-15 20:44_

A surprisingly large number of these are https://github.com/astral-sh/ruff/issues/7318.

---

_Referenced in [astral-sh/ruff#7420](../../astral-sh/ruff/issues/7420.md) on 2023-09-15 20:53_

---

_Referenced in [astral-sh/ruff#7421](../../astral-sh/ruff/issues/7421.md) on 2023-09-15 20:57_

---

_Comment by @charliermarsh on 2023-09-15 21:03_

The most common thing I'm seeing are cases like:

```python
self._http_client = CdmHttpClient(
    self._STANDARDS_ENDPOINT
)  # type: CdmHttpClient
```

Where Ruff collapses this as:

```python
self._http_client = CdmHttpClient(self._STANDARDS_ENDPOINT)  # type: CdmHttpClient
```

Which I believe is intentional.

---

_Referenced in [astral-sh/ruff#7422](../../astral-sh/ruff/issues/7422.md) on 2023-09-15 21:11_

---

_Referenced in [astral-sh/ruff#7423](../../astral-sh/ruff/issues/7423.md) on 2023-09-15 21:15_

---

_Comment by @T-256 on 2023-09-15 21:41_

Hello, first of all, a special thanks for your hard works on this part of Ruff @MichaReiser.
There is an opinion in my mind, that could we know the reason why the specific format style picked? Just like linter rules and their auto-fixes. I didn't go deep on codebases, just saw few PRs, is it even possible?

I think categorize formatting styles, can help improve investigating on solutions for issues like current issue.
For example, a separated metadata file for formatted ranges of files and its style rule name:
```toml
# test.py_meta.toml
lines_range = "1..5"
reasons = ["await expression parentheses"]
```

> Is there a way to remove automatically known incompatibilities?

By these metadata we can skip known incompatibilities.

---

_Referenced in [astral-sh/ruff#7431](../../astral-sh/ruff/issues/7431.md) on 2023-09-16 03:17_

---

_Comment by @MichaReiser on 2023-09-16 14:34_

Hy @T-256 

Ruff's formatter follows Black's style guide and is an opinionated formatter. It intentionally provides very few settings. That means it doesn't provide options to opt-in to individual formatting styles.

---

_Comment by @qarmin on 2023-09-17 15:09_

0.0.290 version with 1875 differences - [0_0_290.zip](https://github.com/astral-sh/ruff/files/12643094/0_0_290.zip)

Diff file with changes in each source file -  [a.txt.zip](https://github.com/astral-sh/ruff/files/12643097/a.txt.zip)


---

_Comment by @charliermarsh on 2023-09-17 15:11_

@qarmin - Does that mean the differences were reduced from 7535 files to 1875 files? Or is this a different set?

---

_Comment by @qarmin on 2023-09-17 15:25_

My new set of python files, now use 4.5 million of files from pypi instead 1.5 million like before, so it is quite strange that number of problem is few times lower.

Looks that I messed something with script to gather results so until I fix them, numbers of problems should not be compared to each other, because each time different set of files was used

---

_Comment by @qarmin on 2023-09-18 12:14_

0.0.290 update 2 - 155996 files (since this moment, numbers can be compared to each other)

This gives ~3% different files than black - 4.5 million python files used to test

Files(ruff, black, diff) - https://github.com/qarmin/Automated-Fuzzer/releases/download/test/Broken_Black_Formatting_0_0_290.zip (1GB zipped)
Diff of all files - https://github.com/qarmin/Automated-Fuzzer/releases/download/test/Broken_Black_Formatting_0_0_290_one_file.zip (30MB zipped)

---

_Comment by @qarmin on 2023-09-28 07:35_

Since number of allowed incompatibilities increases(https://github.com/astral-sh/ruff/issues/7323), I don't see the point in automatically generating such a large number of differences between programs, because there is no method to force 100% compatibility with black so each example requires manual analysis

---

_Closed by @qarmin on 2023-09-28 07:35_

---

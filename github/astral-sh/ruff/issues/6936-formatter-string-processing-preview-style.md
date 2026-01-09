---
number: 6936
title: "Formatter: `string_processing` preview style"
type: issue
state: open
author: MichaReiser
labels:
  - formatter
  - style
assignees: []
created_at: 2023-08-28T08:46:54Z
updated_at: 2025-12-23T20:31:41Z
url: https://github.com/astral-sh/ruff/issues/6936
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: `string_processing` preview style

---

_Issue opened by @MichaReiser on 2023-08-28 08:46_

Add support for Black's improved [string processing](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-string-processing).

> Black will split long string literals and merge short ones. Parentheses are used where appropriate. When split, parts of f-strings that don’t need formatting are converted to plain strings. User-made splits are respected when they do not exceed the line length limit. Line continuation backslashes are converted into parenthesized strings. Unnecessary parentheses are stripped. The stability and status of this feature is tracked in [this issue](https://github.com/psf/black/issues/2188).

## Examples

```python
"a somewhat long string that doesn't fit the line length. " "Testing to see what black does keep it a bit longer and longer",
"a somewhat long string that doesn't fit the line length. " \
    "Testing to see what black does keep it a bit longer and longer",

if "a somewhat long string that doesn't fit the line length. " + "Testing to see what black does keep it a bit longer and longer longer":
    pass

"a somewhat long string that doesn't fit the line length. " + "Testing to see what black does keep it a bit longer and longer longer longer longer longer longer longer longer longer longer longer"

("a somewhat long string that doesn't fit the line length. " + "Testing to see what black does keep it a bit longer and longer longer longer longer longer longer longer longer longer longer longer")

"a somewhat long string that doesn't fit the line length. " + ["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", "bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb", "Ccccccccccccccccccccccccccccccccccccccc cccccccccccccccccccccccccccccccccccccccccccccccccccccc"]

call(
    "a somewhat long string that doesn't fit the line length. "
    "Testing to see what black does keep it a bit longer and longer",
    "a second argument",
    3,
    4,
)


call(
    "a somewhat long string that doesn't fit the line length. Testing to see what black does keep "
    "it a bit longer and longer",
    "a second argument",
    3,
    4,
)

call(
    "a somewhat long string that doesn't fit the line length. "
    "shorter",
    "a second argument",
    3,
    4,
)


L1 = ["The is a short string", "This is a really long string that can't possibly be expected to fit all together on one line. Also it is inside a list literal, so it's expected to be wrapped in parens when spliting to avoid implicit str concatenation.", short_call("arg", {"key": "value"}), "This is another really really (not really) long string that also can't be expected to fit on one line and is, like the other string, inside a list literal.", ("parens should be stripped for short string in list")]
```

```diff
--- /home/micha/.config/JetBrains/PyCharmCE2023.2/scratches/scratch.py  2023-08-28 09:15:01.476535+00:00
+++ /home/micha/.config/JetBrains/PyCharmCE2023.2/scratches/scratch.py  2023-08-28 09:15:02.026782+00:00
@@ -1,17 +1,36 @@
-"a somewhat long string that doesn't fit the line length. " "Testing to see what black does keep it a bit longer and longer",
-"a somewhat long string that doesn't fit the line length. " \
-    "Testing to see what black does keep it a bit longer and longer",
+"a somewhat long string that doesn't fit the line length. "
+"Testing to see what black does keep it a bit longer and longer",
+"a somewhat long string that doesn't fit the line length. "
+"Testing to see what black does keep it a bit longer and longer",
 
-if "a somewhat long string that doesn't fit the line length. " + "Testing to see what black does keep it a bit longer and longer longer":
+if (
+    "a somewhat long string that doesn't fit the line length. "
+    + "Testing to see what black does keep it a bit longer and longer longer"
+):
     pass
 
-"a somewhat long string that doesn't fit the line length. " + "Testing to see what black does keep it a bit longer and longer longer longer longer longer longer longer longer longer longer longer"
+(
+    "a somewhat long string that doesn't fit the line length. "
+    + "Testing to see what black does keep it a bit longer and longer longer longer"
+    " longer longer longer longer longer longer longer longer"
+)
 
-("a somewhat long string that doesn't fit the line length. " + "Testing to see what black does keep it a bit longer and longer longer longer longer longer longer longer longer longer longer longer")
+(
+    "a somewhat long string that doesn't fit the line length. "
+    + "Testing to see what black does keep it a bit longer and longer longer longer"
+    " longer longer longer longer longer longer longer longer"
+)
 
-"a somewhat long string that doesn't fit the line length. " + ["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", "bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb", "Ccccccccccccccccccccccccccccccccccccccc cccccccccccccccccccccccccccccccccccccccccccccccccccccc"]
+"a somewhat long string that doesn't fit the line length. " + [
+    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
+    "bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb",
+    (
+        "Ccccccccccccccccccccccccccccccccccccccc"
+        " cccccccccccccccccccccccccccccccccccccccccccccccccccccc"
+    ),
+]
 
 call(
     "a somewhat long string that doesn't fit the line length. "
     "Testing to see what black does keep it a bit longer and longer",
     "a second argument",
@@ -19,22 +38,35 @@
     4,
 )
 
 
 call(
-    "a somewhat long string that doesn't fit the line length. Testing to see what black does keep "
-    "it a bit longer and longer",
+    "a somewhat long string that doesn't fit the line length. Testing to see what black"
+    " does keep it a bit longer and longer",
     "a second argument",
     3,
     4,
 )
 
 call(
-    "a somewhat long string that doesn't fit the line length. "
-    "shorter",
+    "a somewhat long string that doesn't fit the line length. shorter",
     "a second argument",
     3,
     4,
 )
 
 
-L1 = ["The is a short string", "This is a really long string that can't possibly be expected to fit all together on one line. Also it is inside a list literal, so it's expected to be wrapped in parens when spliting to avoid implicit str concatenation.", short_call("arg", {"key": "value"}), "This is another really really (not really) long string that also can't be expected to fit on one line and is, like the other string, inside a list literal.", ("parens should be stripped for short string in list")]
\ No newline at end of file
+L1 = [
+    "The is a short string",
+    (
+        "This is a really long string that can't possibly be expected to fit all"
+        " together on one line. Also it is inside a list literal, so it's expected to"
+        " be wrapped in parens when spliting to avoid implicit str concatenation."
+    ),
+    short_call("arg", {"key": "value"}),
+    (
+        "This is another really really (not really) long string that also can't be"
+        " expected to fit on one line and is, like the other string, inside a list"
+        " literal."
+    ),
+    "parens should be stripped for short string in list",
+]
```

Observations: 
* Binary operators break before the strings
* The formatting only applies to implicit concatenated strings, but not strings joined with the `+` operator


The strings must be parenthesized in some contexts, see https://github.com/psf/black/pull/3162/files

---

_Referenced in [astral-sh/ruff#6935](../../astral-sh/ruff/issues/6935.md) on 2023-08-28 08:46_

---

_Label `formatter` added by @MichaReiser on 2023-08-28 08:48_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-08-28 08:48_

---

_Comment by @MichaReiser on 2023-08-28 08:52_

My first reaction is that this is similar to Prettier's/Rome's JSX formatting and that we need to use `BestFitting` with three variants:

1. Join all string parts into a single string to make it fit
2. Keep the same string parts as in the source. Test if all lines fit 
3. Use `fill` [see prototype](https://github.com/astral-sh/ruff/blob/f5f7ddf1ba02bc3134bb1257c49b0e83114e6997/crates/ruff_python_formatter/src/lib.rs#L267-L320) to fit as many words as possible on the line. 

The main concern with this is performance:

* We should avoid normalizing the same strings multiple times. This is already one of the most expensive functions today
* We use `BestFitting` for the expression `BestFit` layout. Using `BestFitting for strings would result in a nested BestFitting layout with exponential complexity. Can we remove the need to use `BestFit` for Strings altogether? 


---

_Referenced in [astral-sh/ruff#7052](../../astral-sh/ruff/issues/7052.md) on 2023-09-01 20:37_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-07 13:02_

---

_Referenced in [astral-sh/ruff#7237](../../astral-sh/ruff/issues/7237.md) on 2023-09-08 08:06_

---

_Label `preview` added by @MichaReiser on 2023-09-15 09:50_

---

_Referenced in [DetachHead/pytest-robotframework#106](../../DetachHead/pytest-robotframework/pulls/106.md) on 2023-10-01 01:13_

---

_Unassigned @MichaReiser by @MichaReiser on 2023-10-27 02:07_

---

_Referenced in [Toufool/AutoSplit#259](../../Toufool/AutoSplit/pulls/259.md) on 2023-10-27 14:01_

---

_Referenced in [encode/httpx#2901](../../encode/httpx/pulls/2901.md) on 2023-10-31 11:04_

---

_Referenced in [astral-sh/ruff#8678](../../astral-sh/ruff/issues/8678.md) on 2023-11-28 09:55_

---

_Renamed from "Improved string processing" to "Improved string processing (`string_processing`)" by @MichaReiser on 2023-11-29 00:31_

---

_Renamed from "Improved string processing (`string_processing`)" to "Formatter: `string_processing` preview style" by @MichaReiser on 2023-11-29 03:10_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 03:13_

---

_Comment by @KotlinIsland on 2023-12-05 02:46_

Black no longer wraps implicitly concatenated strings used as func args in parens, due to [this comment](https://github.com/psf/black/issues/2188#issuecomment-1489015964).
There is an open issue to re-enable it: https://github.com/psf/black/issues/3955

---

_Referenced in [tefra/xsdata#894](../../tefra/xsdata/issues/894.md) on 2023-12-26 07:44_

---

_Referenced in [astral-sh/ruff#9634](../../astral-sh/ruff/issues/9634.md) on 2024-01-24 18:32_

---

_Comment by @DetachHead on 2024-02-02 22:27_

fyi in case anyone was confused and thought this feature was removed in black version 24.1 like i did, the string processing now has to be enabled with `--enable-unstable-feature=string_processing` in addition to the `--preview` flag.

---

_Comment by @MichaReiser on 2024-02-23 21:30_

Black considers to not move forward with the `string_processing` preview style ([issue](https://github.com/psf/black/issues/4208))

---

_Referenced in [scikit-learn/scikit-learn#28802](../../scikit-learn/scikit-learn/pulls/28802.md) on 2024-04-10 13:27_

---

_Referenced in [astral-sh/ruff#12057](../../astral-sh/ruff/issues/12057.md) on 2024-06-26 23:48_

---

_Comment by @KotlinIsland on 2024-06-27 05:47_

A simpler aspect of this larger problem is joining strings that get put onto the same line. It constantly annoys me and would love to see it resolved:
```py
# Before
(
    ""
    ""
)
# After
("" "")
# Desired
("") 
# OR
""
```

- from #12057

---

_Comment by @MichaReiser on 2024-06-27 05:51_

@KotlinIsland we agree. A solution for this is described in https://github.com/astral-sh/ruff/issues/9457

---

_Comment by @raayu83 on 2024-09-24 13:25_

Would love to see this feature coming!
When auto creating new Django migrations for models with long help text, I always get files with very long strings on a single line.

---

_Referenced in [app-sre/github-mirror#186](../../app-sre/github-mirror/pulls/186.md) on 2024-11-29 07:02_

---

_Referenced in [CDCgov/cfa-ring-vax-widget#43](../../CDCgov/cfa-ring-vax-widget/pulls/43.md) on 2024-12-18 22:46_

---

_Referenced in [cucumber/gherkin#347](../../cucumber/gherkin/pulls/347.md) on 2025-01-05 22:33_

---

_Referenced in [astral-sh/ruff-vscode#357](../../astral-sh/ruff-vscode/issues/357.md) on 2025-01-17 03:12_

---

_Comment by @hypnopump on 2025-01-17 13:22_

+1 want this

---

_Comment by @MichaReiser on 2025-01-17 13:44_

Ruff now automatically joins implicit concatenated strings and I consider it as unlikely that we'll implement automatic splitting of string literals, at least in the near future. 

---

_Comment by @carschandler on 2025-01-17 14:15_

Is there a suggested method for formatting long string literals, i.e. using docstrings and relying on text editor to format those? 

---

_Label `preview` removed by @MichaReiser on 2025-01-17 14:20_

---

_Label `style` added by @MichaReiser on 2025-01-17 14:20_

---

_Comment by @dukkee on 2025-02-18 20:08_

+1 want this



---

_Comment by @ArnoGW1 on 2025-03-04 09:02_

> Ruff now automatically joins implicit concatenated strings and I consider it as unlikely that we'll implement automatic splitting of string literals, at least in the near future.

Just mentioning that [clang-format's method](https://clang.llvm.org/docs/ClangFormatStyleOptions.html#breakstringliterals) has always been satisfying in my experience. Is it a problem of having to think about what the expected result should be?

---

_Referenced in [cpplint/cpplint#332](../../cpplint/cpplint/pulls/332.md) on 2025-03-08 19:13_

---

_Referenced in [AllenInstitute/aibs-informatics-aws-utils#26](../../AllenInstitute/aibs-informatics-aws-utils/pulls/26.md) on 2025-03-11 20:46_

---

_Referenced in [scikit-learn/scikit-learn#31015](../../scikit-learn/scikit-learn/pulls/31015.md) on 2025-03-25 16:08_

---

_Referenced in [scikit-learn/scikit-learn#31214](../../scikit-learn/scikit-learn/pulls/31214.md) on 2025-04-16 19:31_

---

_Comment by @k4lizen on 2025-05-08 18:36_

re: the autopep8 solution

It seems it also tries to break up strings, and is not the best at it:

![Image](https://github.com/user-attachments/assets/a463af37-49f2-4d5f-9e08-321487cd12b6)

---

_Comment by @giampaolo on 2025-05-09 12:08_

FWIW, `string_processing` is the only reason why I'm still using black. It's just too convenient to automatically split long lines on save.

---

_Referenced in [pwndbg/pwndbg#2966](../../pwndbg/pwndbg/pulls/2966.md) on 2025-05-09 16:38_

---

_Comment by @solace-rcampbell on 2025-05-15 17:08_

Manually splitting and reflowing strings (when you have to edit them later) is one of the worst parts of formatting code.  It would be a super useful feature.

---

_Comment by @ArnoGW1 on 2025-05-19 12:33_

I guess that there are some exceptions where you'd want the string to be unbroken, like URLs, but the case where we want the string to be broken up is _much_ more common. Writing docs, especially, is pretty tedious by default if you want them to stay within bounds, and ruff could really help with that.

---

_Comment by @brki on 2025-12-23 20:31_

I'd be happy with an interactive option. I'm imagining something like:
```
[tool.ruff]
fix-interactive = ["line-length"]
```

Potentially non-safe `fix-interactive` changes would need user interaction to apply a suggested fix, or not.

```
 call(
-    "a very long string that doesn't fit the line length. bla bla bla bla bla bla"
+    (
+        "a very long string that doesn't fit the line "
+        "length. bla bla bla bla bla bla"
+    )
 )

(A)ccept / (S)kip [S]:
```

---

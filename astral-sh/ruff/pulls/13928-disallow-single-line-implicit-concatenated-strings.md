```yaml
number: 13928
title: Disallow single-line implicit concatenated strings
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
  - style
assignees: []
merged: true
base: main
head: micha/enforce-multiline-implicit-concatenated-strings
created_at: 2024-10-25T15:42:42Z
updated_at: 2024-11-03T11:49:27Z
url: https://github.com/astral-sh/ruff/pull/13928
synced_at: 2026-01-12T15:55:46Z
```

# Disallow single-line implicit concatenated strings

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/8272 

The formatter is incompatible with `ISC001` because it can introduce new single-line implicit concatenated strings if all parts fit on the line. The new implicit concatenated string formatting style already addresses this incompatibility for regular strings and most f-string but not for triple quoted strings and raw strings. 

This PR proposes to change the formatter style to **never** collapse the parts of an implicit concatenated string onto a single line unless it has been formatted on a single line by the author. 

```python
(
	r"aaaaaaa" "bbbbbbbbbbbb"
)
```

Is preserved as is because the author wrote the implicit concatenated string on a single line and it fits. The source i already incompatible with ISC001. 

```python
(
	r"aaaaaaa"
	"bbbbbbbbbbbb"
)
```

The current style collapses this string to match the example above. The style proposed in this PR preserves the multiline formatting in this case because there's a line break between the two parts. 

## Preview gating

It's technically not required to gate this change behind preview because it doesn't change the formatting of any existing code. But it fits nicely into the other implicit concatenated work that we've been doing and the new implicit concatenated string formatting also largely mitigates that this leads to slightly less consistent formatting (because it's up to the author whether the string remains multiline or not)

## Test Plan

Added tests


---

_Label `formatter` added by @MichaReiser on 2024-10-25 15:42_

---

_Label `style` added by @MichaReiser on 2024-10-25 15:42_

---

_@MichaReiser reviewed on 2024-10-25 15:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_long_strings__regression.py.snap`:947 on 2024-10-25 15:43_

The diff here is slightly confusing. We should double check that this change is correct.

---

_Comment by @github-actions[bot] on 2024-10-25 15:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+18 -6 lines in 4 files in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+11 -3 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/styling/mathtext/latex_bessel.py#L21'>examples/styling/mathtext/latex_bessel.py~L21</a>
```diff
     width=700,
     height=500,
     title=(
-        r"Bessel functions of the first kind: $$J_\alpha(x) = \sum_{m=0}^{\infty}" r"\frac{(-1)^m}{m!\:\Gamma(m+\alpha+1)} \left(\frac{x}{2}\right)^{2m+\alpha}$$"
+        r"Bessel functions of the first kind: $$J_\alpha(x) = \sum_{m=0}^{\infty}"
+        r"\frac{(-1)^m}{m!\:\Gamma(m+\alpha+1)} \left(\frac{x}{2}\right)^{2m+\alpha}$$"
     ),
 )
 p.x_range.range_padding = 0
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/color.py#L126'>src/bokeh/core/property/color.py~L126</a>
```diff
             Regex(r"^#[0-9a-fA-F]{4}$"),
             Regex(r"^#[0-9a-fA-F]{6}$"),
             Regex(r"^#[0-9a-fA-F]{8}$"),
-            Regex(r"^rgba\(((25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*," r"\s*?){2}(25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*," r"\s*([01]\.?\d*?)\)"),
-            Regex(r"^rgb\(((25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*," r"\s*?){2}(25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*?\)"),
+            Regex(
+                r"^rgba\(((25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*,"
+                r"\s*?){2}(25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*,"
+                r"\s*([01]\.?\d*?)\)"
+            ),
+            Regex(
+                r"^rgb\(((25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*,"
+                r"\s*?){2}(25[0-5]|2[0-4]\d|1\d{1,2}|\d\d?)\s*?\)"
+            ),
             Tuple(Byte, Byte, Byte),
             Tuple(Byte, Byte, Byte, Percent),
             RGB,
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/a3f14bfa373c6fb4e3470a1f3bd8fed1657e09e1/pandas/tests/tools/test_to_datetime.py#L1346'>pandas/tests/tools/test_to_datetime.py~L1346</a>
```diff
         assert res is NaT
 
         msg = "|".join([
-            r'^time data "a" doesn\'t match format "%H:%M:%S". ' f"{PARSING_ERR_MSG}$",
+            r'^time data "a" doesn\'t match format "%H:%M:%S". '
+            f"{PARSING_ERR_MSG}$",
             r'^Given date string "a" not likely a datetime$',
             r'^unconverted data remains when parsing with format "%H:%M:%S": "9". '
             f"{PARSING_ERR_MSG}$",
```
<a href='https://github.com/pandas-dev/pandas/blob/a3f14bfa373c6fb4e3470a1f3bd8fed1657e09e1/pandas/tests/tools/test_to_datetime.py#L1396'>pandas/tests/tools/test_to_datetime.py~L1396</a>
```diff
 
         msg = "|".join([
             r'^Given date string "a" not likely a datetime$',
-            r'^time data "a" doesn\'t match format "%H:%M:%S". ' f"{PARSING_ERR_MSG}$",
+            r'^time data "a" doesn\'t match format "%H:%M:%S". '
+            f"{PARSING_ERR_MSG}$",
             r'^unconverted data remains when parsing with format "%H:%M:%S": "9". '
             f"{PARSING_ERR_MSG}$",
             r"^second must be in 0..59: 00:01:99$",
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/e625dac30d750424cb81166890d0c945372047fc/rotkehlchen/exchanges/coinbase.py#L68'>rotkehlchen/exchanges/coinbase.py~L68</a>
```diff
 LEGACY_RE: re.Pattern = re.compile(r"^[\w]+$")
 NEW_RE: re.Pattern = re.compile(r"^organizations/[\w-]+/apiKeys/[\w-]+$")
 PRIVATE_KEY_RE: re.Pattern = re.compile(
-    r"^-----BEGIN EC PRIVATE KEY-----\n" r"[\w+/=\n]+" r"-----END EC PRIVATE KEY-----\n?$",
+    r"^-----BEGIN EC PRIVATE KEY-----\n"
+    r"[\w+/=\n]+"
+    r"-----END EC PRIVATE KEY-----\n?$",
     re.MULTILINE,
 )
 
```

</p>
</details>




---

_Label `preview` added by @MichaReiser on 2024-11-01 16:44_

---

_Marked ready for review by @MichaReiser on 2024-11-01 17:05_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-11-01 17:05_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-01 17:05_

---

_@charliermarsh approved on 2024-11-02 17:21_

I think this is a smart change. We should document it in the Black deviations though.

---

_Comment by @MichaReiser on 2024-11-02 17:33_

> I think this is a smart change. We should document it in the Black deviations though.

Thanks for the feedback. I plan to document the deviations as part of promoting the new style guide.

---

_Merged by @MichaReiser on 2024-11-03 11:49_

---

_Closed by @MichaReiser on 2024-11-03 11:49_

---

_Branch deleted on 2024-11-03 11:49_

---

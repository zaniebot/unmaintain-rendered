---
number: 7050
title: Formatter removes parentheses around call chaining
type: issue
state: closed
author: cnpryer
labels:
  - bug
  - formatter
  - help wanted
assignees: []
created_at: 2023-09-01T19:51:37Z
updated_at: 2023-09-04T09:57:06Z
url: https://github.com/astral-sh/ruff/issues/7050
synced_at: 2026-01-10T01:22:46Z
---

# Formatter removes parentheses around call chaining

---

_Issue opened by @cnpryer on 2023-09-01 19:51_

Version: ruff 0.0.287

Line-length: 88

Related: #5343

My team does a lot of exploratory scripting with Pandas (most are analysts, not developers), so often some analysts will chain together Pandas methods for SQL-like operations. 

Note that the removed parentheses still results in valid Python. I actually prefer this change since the expression is already wrapped by `pd.concat` and shouldn't confuse the original author or subsequent readers. The removed parentheses, IMO, are redundant (see `(...).groupby(..)`).

I figured this kind of code is probably common outside the current ecosystem checks (and in Python's data analysis world), especially with Pandas users.

The following is an example of a diff snippet from one of our repositories:

Input, formatted with `black` (23.7.0):
```py
merged = pd.concat(
    [
        df1_aaaaaaaaaaaa[
            df1_aaaaaaaaaaaa["a"].isin(["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"])
        ],
        (
            df1_aaaaaaaaaaaa.loc[
                df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                [
                    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                ],
            ]
            .merge(
                df2_aaaaaaaaaaaa[["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"]],
                on=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"],
                how="a",
                validate="a",
            )
            .drop(columns=["a"])
            .merge(
                df3_aaaaaaaaaaaa[
                    [
                        "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                        "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                        "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                    ]
                ],
                on=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"],
                how="a",
                validate="a",
            )
        )
        .groupby(
            [
                "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
            ]
        )
        .sum()
        .reset_index(),
    ]
)
```

`ruff format` diff:

```diff
diff --git a/scratch.py b/scratch.py
index d2ee558..2e4900c 100644
--- a/scratch.py
+++ b/scratch.py
@@ -3,35 +3,33 @@ merged = pd.concat(
         df1_aaaaaaaaaaaa[
             df1_aaaaaaaaaaaa["a"].isin(["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"])
         ],
-        (
-            df1_aaaaaaaaaaaa.loc[
-                df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
+        df1_aaaaaaaaaaaa.loc[
+            df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
+            [
+                "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
+                "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
+                "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
+                "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
+            ],
+        ]
+        .merge(
+            df2_aaaaaaaaaaaa[["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"]],
+            on=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"],
+            how="a",
+            validate="a",
+        )
+        .drop(columns=["a"])
+        .merge(
+            df3_aaaaaaaaaaaa[
                 [
                     "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                     "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
                     "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
-                    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
-                ],
-            ]
-            .merge(
-                df2_aaaaaaaaaaaa[["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"]],
-                on=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"],
-                how="a",
-                validate="a",
-            )
-            .drop(columns=["a"])
-            .merge(
-                df3_aaaaaaaaaaaa[
-                    [
-                        "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
-                        "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
-                        "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
-                    ]
-                ],
-                on=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"],
-                how="a",
-                validate="a",
-            )
+                ]
+            ],
+            on=["aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"],
+            how="a",
+            validate="a",
         )
         .groupby(
             [
```

Playground: https://play.ruff.rs/11aa202b-fee6-4a8f-bc61-330acffeca9c
 
More to come :)

---

_Comment by @cnpryer on 2023-09-01 20:11_

Maybe related  #6933

---

_Label `formatter` added by @charliermarsh on 2023-09-01 22:59_

---

_Comment by @charliermarsh on 2023-09-01 23:03_

Interesting, it looks like Black will preserve parentheses around the first part of a call chain, no matter how many components are included in the parentheses ([example](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AHbAIddAD2IimZxl1N_Wlji3aLIjlAOoWjeEBM_F5fqvT7g4nY6QiR8pVSCOPgD44-SaGL5PbocOIP6NQZMPSK8SFhNN06cxWVwWuDWlI2WmXhzZ7K8kZ-dHMf0jpKCvDhl1OHtqjoKznQG_SDlibf4R52k--a-wiJtLYTwr-EbZE3gsW-s2c1DE7yAAAAATCpVExpiO78AAaMB3AMAAFUmeQ-xxGf7AgAAAAAEWVo=)). Not obvious to me whether we want to preserve this or not.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-01 23:22_

---

_Comment by @cnpryer on 2023-09-02 01:25_

Hmm maybe I'm misunderstanding your example. It seems to preserve with Ruff ([playground](https://play.ruff.rs/69cc0ba9-5297-457b-9237-6cad4fb95738)). But I can boil my original example down as:

Within some parenthesized context you can get the following behavior:

I can get both Ruff and Black to preserve the parenthesized dataframe/series expression with one method call
```py
# Input
[(df1_aaaaaaaaaaaa.loc[df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaa"]).groupby()]

# Black
[(df1_aaaaaaaaaaaa.loc[df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaa"]).groupby()]

# Ruff
[(df1_aaaaaaaaaaaa.loc[df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaa"]).groupby()]
```
[Black Playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADEAHJdAD2IimZxl1N_Wg09_Mey5PsMtNn9Q6umuCzSUzwf_xQd5wBrHnIToNE5csx6izz-p51nsvX2pNoX_C2U6GwdfTdy6bXdnBo8C94mhSvC4tREmnHIU6hAFjXoY2mx42LcsbjplghG4Ah1laNQWC5P4bciAAAAAMVBzr2NpjZDAAGOAcUBAADG_N6ascRn-wIAAAAABFla)

[Ruff Playground](https://play.ruff.rs/673711f4-b01c-4c15-824b-c9ea379e85ce) 


When you add a second method call Ruff will strip the parentheses
```py
# Input
[(df1_aaaaaaaaaaaa.loc[df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaa"]).groupby().sum()]

# Black
[(df1_aaaaaaaaaaaa.loc[df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaa"]).groupby().sum()]

# Ruff
[df1_aaaaaaaaaaaa.loc[df1_aaaaaaaaaaaa["a"] == "aaaaaaaaaaaaaaaaa"].groupby().sum()]
```
[Black Playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADKAHZdAD2IimZxl1N_Wg09_Mey5PsMtNn9Q6umuCzSUzwf_xQd5wBrHnIToNE5csx6izz-p51nsvX2pNoX_C2U6JY-jJPfrvKp7Cmykxku0BYbgJh59Z0ELUvky7RbB9sd9hOptbxjnem1pVGeur475-sTsmLMFbKLs3gAAAD0r_EWF5I19QABkgHLAQAAFUkVDrHEZ_sCAAAAAARZWg==)

[Ruff Playground](https://play.ruff.rs/0b11c753-444f-47ec-bddf-f53576c670a1)

What I want to play with would be examples requiring us to retain order of operations for dataframe/series operations.

---

_Comment by @cnpryer on 2023-09-02 02:00_

And even simpler the same behavior occurs with:

```py
[((d)).call()]
```

Which is a good example of what you're pointing out.

---

_Comment by @cnpryer on 2023-09-02 02:09_

Oh wow, you don't need the parenthesized context either.

```py
((d)).call()
```

[Black Playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ABxAEddAD2IimZxl1N_WhTiKqpnadz_5hp9I0OGBK0lLz9B9g-V1SRhOBoqQVAADHPeRclIIfPZjWfTs5KGaV0ox6v-dbeOLz16ZTgAAABWvJqIZRMzwwABY3JfnV3QH7bzfQEAAAAABFla)

[Ruff Playground](https://play.ruff.rs/aba7d33f-613d-4798-9b65-cd1300c2d4cd)

---

_Comment by @MichaReiser on 2023-09-02 07:58_

Ruff should preserve expression-parentheses except in clause headers (like `if (True)`). Not sure why it isn't in this case. Thanks for narrowing down the example.

---

_Label `bug` added by @MichaReiser on 2023-09-02 07:58_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-02 07:58_

---

_Label `help wanted` added by @MichaReiser on 2023-09-02 07:58_

---

_Comment by @MichaReiser on 2023-09-02 08:11_

@charliermarsh are you working on this or is it up for grabs?

---

_Comment by @charliermarsh on 2023-09-02 09:39_

I was working on this but didn’t get to PR yet. I believe it’s specific to call chain formatting.

---

_Comment by @charliermarsh on 2023-09-02 09:43_

If I don’t find time to fix today, I’ll unassign myself :)

---

_Comment by @MichaReiser on 2023-09-02 10:50_

sounds good. @konstin has context on this as well.

---

_Comment by @charliermarsh on 2023-09-02 11:25_

For reference, using [this](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AHlAIJdAD2IimZxl1N_WhQxSjX4ufjgaLutdoZKjVr0Bl2fBAPjS2mA72tc4H7tPYw2oE32qgRx7UjuwjJJlklXhcQ0KZf_NKLwJ94RbHHxTIqGSF1efU_DgvdjEWuT2N2t90oKwmqrnyKwFJpARhO4q0h8p7t7ywVqM8TSXAUF0CvN5ogwjKgAAAD67Py4NzoEOQABngHmAwAA4yoLKLHEZ_sCAAAAAARZWg==) to demonstrate Black's behavior are parenthesized "heads" on call chains.

---

_Comment by @charliermarsh on 2023-09-02 23:12_

I made some progress on this at [`charlie/parens`](https://github.com/astral-sh/ruff/compare/main...charlie/parens) but haven't fully figured it out. I thought this could be solved by changing `expr_attribute.rs` alone but I'm now thinking we may need to change the fluent style detection code too. If anyone beats me to this they're welcome to take it over (\cc @konstin as a good contender :)). I'll re-assign if I find more time for it.

---

_Unassigned @charliermarsh by @charliermarsh on 2023-09-02 23:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 18:45_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-09-03 18:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 20:34_

---

_Referenced in [astral-sh/ruff#7109](../../astral-sh/ruff/pulls/7109.md) on 2023-09-03 20:41_

---

_Closed by @charliermarsh on 2023-09-04 09:57_

---

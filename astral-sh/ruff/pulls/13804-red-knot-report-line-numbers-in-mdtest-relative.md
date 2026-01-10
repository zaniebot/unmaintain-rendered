```yaml
number: 13804
title: "[red-knot] Report line numbers in mdtest relative to the markdown file, not the test snippet"
type: pull_request
state: merged
author: pilleye
labels:
  - ty
assignees: []
merged: true
base: main
head: pilleye/md-relative-line-number
created_at: 2024-10-18T05:02:23Z
updated_at: 2024-10-22T13:24:27Z
url: https://github.com/astral-sh/ruff/pull/13804
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Report line numbers in mdtest relative to the markdown file, not the test snippet

---

_Pull request opened by @pilleye on 2024-10-18 05:02_

Summary: Adds absolute line numbers to mdtest (fixes #13798)

Test Plan: 

https://github.com/user-attachments/assets/63f84e34-da00-4146-a22b-e17322d5d687


---

_Review requested from @carljm by @pilleye on 2024-10-18 05:02_

---

_Review requested from @MichaReiser by @pilleye on 2024-10-18 05:02_

---

_Review requested from @AlexWaygood by @pilleye on 2024-10-18 05:02_

---

_Comment by @github-actions[bot] on 2024-10-18 05:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `red-knot` added by @MichaReiser on 2024-10-18 05:54_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:250 on 2024-10-18 05:57_

Aren't the line numbers off after a code block (that could span multiple lines)? 

I think a better approach is to track the `offset` (`TextSize`) and increment it in `self.scan` and here. Then use `LineIndex` during rendering to get the line number. 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:11 on 2024-10-18 06:01_

Nit: I'm inclined to use a regular `Vec` here and instead sort it before returning it in `run_test`. Vec's are just overall simpler data structure (e.g. for iterating). I then would add the `md_offset` to the `FailuresByLine` data structure (do we still need `line_number` or can we replace it with the `md_offset`?)

---

_@MichaReiser reviewed on 2024-10-18 06:01_

---

_Comment by @MichaReiser on 2024-10-18 06:03_

Thanks, this is great. I suspect that the line numbers may get out of sync during parsing if the error isn't in the file's first code block.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:45 on 2024-10-18 12:55_

nit: just because you _can_ do arbitrary unpacking in Rust doesn't mean you _should_ ;)

Here, I'd find this more readable:

```suggestion
            for (path, by_line) in failures {
                let AbsoluteLineNumberPath {
                    path,
                    line_number: absolute_line_number,
                } = path;
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:55 on 2024-10-18 13:07_

The indentation should come before `absolute_line_info` here, so that the per-line diagnostics are indented below the "heading", which is the name of the embedded file

```suggestion
                        println!("    {absolute_line_info} {line_info} {failure}");
```

This means that the output in the terminal looks is beautifully organised like this if you have a test which has multiple Python snippets in the same Markdown test, and failures in more than one of those code blocks:

![image](https://github.com/user-attachments/assets/a12dfbab-aed4-4a46-b861-78946ee9a790)

I checked, and I'm still able to double-click on the `exception_control_flow.md:157` if I run the test in VSCode, and it takes me straight to the line, even if it's indented.

---

Separately, I'm wondering if we need the relative line number to be reported at all, or if we could just make do with the relative line number? It feels like it just adds noise. Maybe we could just do this (diff relative to your PR branch):

```diff
diff --git a/crates/red_knot_test/src/lib.rs b/crates/red_knot_test/src/lib.rs
index 691a71528..e31fad615 100644
--- a/crates/red_knot_test/src/lib.rs
+++ b/crates/red_knot_test/src/lib.rs
@@ -50,9 +50,9 @@ pub fn run(path: &PathBuf, title: &str) {
                             "{}:{}",
                             title,
                             absolute_line_number.saturating_add(line_number.get())
-                        );
-                        let line_info = format!("line {line_number}:").cyan();
-                        println!("{absolute_line_info}    {line_info} {failure}");
+                        )
+                        .cyan();
+                        println!("    {absolute_line_info} {failure}");
                     }
                 }
```

---

_@AlexWaygood reviewed on 2024-10-18 13:07_

Thanks, this is fantastic! A couple more comments on top of Micha's :-)

---

_@MichaReiser reviewed on 2024-10-18 13:19_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:55 on 2024-10-18 13:19_

I find the many line numbers and file names slightly confusing... Especially because they're next to each other. 

But it's hard to come up with a sane grouping. The grouping by file seems reasonable but is also somewhat confusing with diagnostics that then have line numbers absolute to the entire file.

And do we still need to show both line numbers? Aren't the md file line numbers sufficient?

```
/src/single_except.py
	exception/control_flow.md:157 unmatched assertion: revealed: list[int]
	exception/control_flow.md:159 unmatched assertion: revealed: list[int]
```

---

_@AlexWaygood reviewed on 2024-10-18 13:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:55 on 2024-10-18 13:20_

> And do we still need to show both line numbers? Aren't the md file line numbers sufficient?

I already asked that; I agree with you. Take a look at the final suggested diff in my comment above :-)

---

_@MichaReiser reviewed on 2024-10-18 13:27_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:55 on 2024-10-18 13:27_

Oh shoot. Reading is hard. 

---

_@carljm reviewed on 2024-10-18 18:32_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:55 on 2024-10-18 18:32_

I also agree with removing the old embedded-relative line number entirely, in favor of just the new absolute line number.

I would favor removing the grouping by embedded-file altogether in the output, and just having flat output of one failure per line, per test. It's weird to me to have it grouped by embedded file but then the line numbers be relative to the markdown file. If you find the given line in the markdown file, it'll be obvious what embedded file you are in. And so many of the embedded file names are just `/src/test.py` anyway, it feels like mostly noise.

---

_@AlexWaygood reviewed on 2024-10-18 18:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:55 on 2024-10-18 18:33_

> I would favor removing the grouping by embedded-file altogether in the output, and just having flat output of one failure per line, per test. It's weird to me to have it grouped by embedded file but then the line numbers be relative to the markdown file. If you find the given line in the markdown file, it'll be obvious what embedded file you are in. And so many of the embedded file names are just `/src/test.py` anyway, it feels like mostly noise.

That rationale makes sense to me!

---

_@carljm reviewed on 2024-10-18 18:34_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:11 on 2024-10-18 18:34_

If we remove the grouping by embedded-file path from the output altogether, as I suggested below, that also suggests simplifying this data structure.

---

_@pilleye reviewed on 2024-10-21 05:25_

---

_Review comment by @pilleye on `crates/red_knot_test/src/parser.rs`:250 on 2024-10-21 05:25_

Used a mix of the two, using offset within the entire Markdown file to find the line that the backticks of a code block start on. Then, we just add the line onto the line number reported from the linter itself.

---

_Comment by @pilleye on 2024-10-21 05:39_

Think I mostly cleaned up the comments, would appreciate another review. I'm now returning a `Vec<FailuresByLine>`, where the paths that are checked are sorted by their starting position, so errors should occur from top-to-bottom.

---

_Comment by @pilleye on 2024-10-21 06:00_


https://github.com/user-attachments/assets/9cd21d21-f4e2-4f0d-8212-5375a312b7fd



---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:35 on 2024-10-21 08:16_

nit: I think the blank line in between the title and the line-by-line details really helps readability here; I'd keep it, personally 😄

```suggestion
            println!("\n{}\n", test.name().bold().underline());
```

---

_@AlexWaygood reviewed on 2024-10-21 08:16_

---

_@AlexWaygood reviewed on 2024-10-21 08:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:57 on 2024-10-21 08:21_

here you use `saturating_add`, but in the `increment_offset` method in `parser.rs` you use `checked_add` and return an error if the addition would overflow. I think either is honestly defensible for this kind of thing, but could you possibly say why you used one in one place and the other in the other place?

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:301 on 2024-10-21 08:30_

We should move this into `scan` to avoid repeating the same code

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:336 on 2024-10-21 08:31_

I suggest just storing the embedded file and computing the line number when needed. It has the advantage that the test framework doesn't need to compute the `LineIndex` if there are no errors. Computing the `LineIndex` isn't expensive, but it can add up. Querying the line index is `O(log(n))`, so that's not entirely free as well.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:108 on 2024-10-21 08:34_

I don't think calling `shrink_to_fit` gives us much here. `shrink_to_fit` is mainly useful if you have a long-living `Vec` to avoid wasting memory. The failures are consumed right away (and if not, let the caller handle it) and then dropped. 

---

_@MichaReiser reviewed on 2024-10-21 08:38_

I would still suggest to store offsets to avoid computing the markdown `LineIndex` unnecessarily. 

We also discussed removing the grouping by test file and I see that you kept it. What's your motivation for keeping the grouping?

---

_Comment by @AlexWaygood on 2024-10-21 10:20_

> We also discussed removing the grouping by test file and I see that you kept it. What's your motivation for keeping the grouping?

@MichaReiser I think this feedback was mostly addressed. The errors are no longer grouped according to the _embedded Python file inside a single Markdown test_. Here's what it currently looks like on this PR branch if some tests fail:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/ca27b86f-3db7-4357-86e0-2e1eb660c7a7)

</details>

And here's what the same failures looked like on this PR branch prior to https://github.com/astral-sh/ruff/pull/13804/commits/5d20b5c3cca2e56a866301204884e3c71aae81f8

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/9171275a-f2af-4583-8b2b-2cc6b45e6da1)

</details>

So the `/src/test.py`, `/src/branches_unify_to_non_union_type.py`, `/src/union_type_inferred.py` and `/src/test.py` headings are now gone. (These all told you the name of the _embedded Python file_ inside a specific Markdown test.) However, there's still an unnecessary line break in between these two lines in the test output:
- ```/exception/control_flow.md:53 unexpected error: [revealed-type] "Revealed type is `Literal[2]`"```
- ```/exception/control_flow.md:74 unmatched assertion: revealed: bytes```
 
The errors before the line break occur in one embedded Python code block, and the errors after the line break occur in a different Python code block. I think this line break is unnecessary; @pilleye could you possibly get rid of it?

For the record, this is the diff I made to `crates/red_knot_python_semantic` in order to get these screenshots:

<details>
<summary>Diff to trigger test failures</summary>

````diff
diff --git a/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md b/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md
index f7cf500d0..792d9ed69 100644
--- a/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md
@@ -46,11 +46,11 @@ x = 1
 try:
     reveal_type(x)  # revealed: Literal[1]
     x = could_raise_returns_str()
-    reveal_type(x)  # revealed: str
+    reveal_type(x)  # revealed: int
 except:
     reveal_type(x)  # revealed: Literal[1] | str
     x = 2
-    reveal_type(x)  # revealed: Literal[2]
+    reveal_type(x)
 
 reveal_type(x)  # revealed: str | Literal[2]
 ```
@@ -71,7 +71,7 @@ try:
 except:
     x = could_raise_returns_str()
 
-reveal_type(x)  # revealed: str
+reveal_type(x)  # revealed: bytes
 ```
 
 ## A non-bare `except`
@@ -92,7 +92,7 @@ def could_raise_returns_str() -> str:
 x = 1
 
 try:
-    reveal_type(x)  # revealed: Literal[1]
+    reveal_type(x)  # revealed: str
     x = could_raise_returns_str()
     reveal_type(x)  # revealed: str
 except TypeError:
diff --git a/crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md b/crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md
index 44d701d1f..b44572c43 100644
--- a/crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md
@@ -6,7 +6,7 @@
 try:
     x
 except* BaseException as e:
-    reveal_type(e)  # revealed: BaseExceptionGroup
+    reveal_type(e)  # revealed: int
 ```
````

</details>

The failures are still grouped according to the name of the Markdown test inside the Markdown file, but I find that very helpful: it means you can have some idea of what kinds of tests are failing without even having to jump and look at the specific assertions that are failing.

---

_Comment by @pilleye on 2024-10-21 13:47_

> The errors before the line break occur in one embedded Python code block, and the errors after the line break occur in a different Python code block. I think this line break is unnecessary; @pilleye could you possibly get rid of it?

<img width="1470" alt="Screenshot 2024-10-21 at 9 46 50 AM" src="https://github.com/user-attachments/assets/7bdcc0ee-1a9a-4558-a17d-9ff01123de68">


---

_@AlexWaygood approved on 2024-10-21 14:22_

This LGTM now. Thanks again! @MichaReiser, anything more from you?

---

_Comment by @MichaReiser on 2024-10-21 16:42_

I'll re-review tomorrow morning

---

_Review requested from @MichaReiser by @MichaReiser on 2024-10-21 16:42_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:60 on 2024-10-21 18:27_

nit: this doesn't store a line number anymore, maybe `PathWithOffset`?

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:85 on 2024-10-21 18:31_

This may be a holdover from a previous version of the PR, but I don't think this `with_capacity` really makes sense anymore. This vector isn't going to have a number of entries based on the number of embedded files, it'll have a number of entries based on the number of lines with failures on them. In the ideal scenario (where arguably we care the most about speed) it'll be an empty vec. So I think it would be better to optimistically not allocate:
```suggestion
    let mut failures = Vec::default();
```

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:87 on 2024-10-21 18:32_

naming nit: I don't think `contextual_path` is very clear, I'd name the struct `PathWithOffset` (as mentioned above) and name this `path_with_offset`.

---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:60 on 2024-10-21 18:37_

I think we could avoid eagerly copying the entire `lines` vec by instead just adding a `starting_line_number` or `line_offset` field to `FailuresByLine`, and adding the offset to each line number in `FailuresByLine::iter` instead.

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:146 on 2024-10-21 18:38_

```suggestion
    /// The offset of the backticks beginning the code block within the markdown file
```

---

_@carljm approved on 2024-10-21 18:40_

The behavior here looks good to me; thanks @pilleye!!

Some comments on the code; maybe wait to implement them until @MichaReiser has a look, in case he has a different idea on any of this, since he's done some recent work on mdtest perf.

---

_Comment by @MichaReiser on 2024-10-21 19:52_

I can help tomorrow morning with merging my latest changes into this PR.

---

_@MichaReiser approved on 2024-10-22 07:38_

---

_Merged by @MichaReiser on 2024-10-22 07:42_

---

_Closed by @MichaReiser on 2024-10-22 07:42_

---

_Branch deleted on 2024-10-22 13:24_

---

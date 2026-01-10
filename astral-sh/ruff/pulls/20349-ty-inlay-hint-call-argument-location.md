```yaml
number: 20349
title: "[ty] Inlay hint call argument location"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: inlay-hint-call-argument-location
created_at: 2025-09-11T12:17:49Z
updated_at: 2025-12-27T00:21:02Z
url: https://github.com/astral-sh/ruff/pull/20349
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Inlay hint call argument location

---

_Pull request opened by @MatthewMckee4 on 2025-09-11 12:17_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds location to the inlay hint label part for call arguments. This allows "goto" on the inlay hint.

[inlay-hint-call-argument-location.webm](https://github.com/user-attachments/assets/9458c4ce-8cd1-4edb-bac6-4379ae913075)

This has not been added to the playground as Monaco doesn't support it.

## Test Plan

updated a test in `ty_server`. But other than that im not sure the best way to approach tests for this.


---

_Comment by @github-actions[bot] on 2025-09-11 12:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-09-11 12:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @MatthewMckee4 on 2025-09-11 12:26_

Getting this error in playground. It seems like i have broken the goto as well as the inlay hint location still not working. will look into this more

```bash
Uncaught Error: can't access property "monaco", this is undefined

mapNavigationTarget@http://localhost:5173/src/Editor/Editor.tsx:464:13
mapNavigationTargets@http://localhost:5173/src/Editor/Editor.tsx:495:22
provideDefinition@http://localhost:5173/src/Editor/Editor.tsx:391:21
```

---

_Renamed from "Inlay hint call argument location" to "[ty] Inlay hint call argument location" by @AlexWaygood on 2025-09-11 12:41_

---

_Label `server` added by @AlexWaygood on 2025-09-11 12:41_

---

_Label `ty` added by @AlexWaygood on 2025-09-11 12:41_

---

_Comment by @dhruvmanila on 2025-09-13 04:09_

> Getting this error in playground. It seems like i have broken the goto as well as the inlay hint location still not working. will look into this more

I'd be happy to keep the playground changes in a separate PR to get the LSP changes in.

---

_Comment by @MatthewMckee4 on 2025-09-13 17:24_

I fixed this error, was a small issue, I fixed in the "Fix Playground" commit

---

_Comment by @MichaReiser on 2025-09-15 12:45_

> I fixed this error, was a small issue, I fixed in the "Fix Playground" commit

Nice. Does that mean this PR is now ready for review or is there more to do?

---

_Comment by @MatthewMckee4 on 2025-09-15 13:55_

I am just looking over it for the final time now, will make it ready for review today

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:933 on 2025-09-15 14:02_

I could clean this up more by using `Option<FileRange>`, but this requires a lot more cloning

---

_@MatthewMckee4 reviewed on 2025-09-15 14:02_

---

_Marked ready for review by @MatthewMckee4 on 2025-09-15 14:07_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-09-15 14:07_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-09-15 14:07_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-09-15 14:07_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-09-15 14:07_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-09-15 14:07_

---

_@MatthewMckee4 reviewed on 2025-09-15 14:08_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:933 on 2025-09-15 14:08_

Yeah I'm not too sure about this. I'm not sure if the cloning of a `File` is "too" expensive. This would simplify the code a bit but not too much really

---

_Comment by @MichaReiser on 2025-09-16 07:29_

@Gankra is this a PR you could take a look at? If not, that's fine. It might just take me a few days before I get to it because I'm still catching up with notifications after my PTO (and have some other time sensitive things that I need to do first)

---

_@Gankra reviewed on 2025-09-18 00:28_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:933 on 2025-09-18 00:28_

Cloning this kind of File is essentially free, aiui.

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:822 on 2025-09-18 00:38_

```suggestion
        // TODO: lambda functions
```

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:795 on 2025-09-18 00:40_

Hm is anything added by this being an Option?

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/ide_support.rs`:985 on 2025-09-18 00:52_

I think the body of the and_then can be `offsets.get(&param.name()?.to_string())`

---

_@Gankra approved on 2025-09-18 01:09_

The basic approach seems reasonable! I was going to complain that this approach doesn't properly play with stub-mapping and the goto-definition vs goto-declaration distinction, but after looking at the fields provided by the spec it seems like it really does only let you pick one "target" for the inlay hint.

That said the implementation you've chosen implements goto-declaration semantics which is not the default for a cmd+click, which prefers goto-definition. I think it would be fine to have that be a followup, as this is already seemingly strictly better than what pylance does!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-09-18 06:42_

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-18 06:49_

---

_Review request for @carljm removed by @carljm on 2025-09-19 15:50_

---

_@MatthewMckee4 reviewed on 2025-09-19 15:51_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:795 on 2025-09-19 15:51_

No not really, removed the option now

---

_Review requested from @Gankra by @MichaReiser on 2025-09-22 10:00_

---

_Comment by @MatthewMckee4 on 2025-09-22 20:20_

hmm there seems to be something quite wrong about the location for the parameters (in playground), and i think there's an issue with my video above

---

_Comment by @MatthewMckee4 on 2025-09-22 20:41_

working fine in vscode, just need to look into the playground not working

---

_Comment by @MatthewMckee4 on 2025-09-22 21:05_

really have no clue why it isnt working

---

_@MichaReiser reviewed on 2025-09-23 08:41_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:933 on 2025-09-23 08:41_

`FileRange` is even marked as `Copy` (it's as expensive as copying a u128)

---

_Review comment by @MichaReiser on `crates/ty_wasm/src/lib.rs`:936 on 2025-09-23 08:43_

Nit: We could make this a method on `target` (`target.full_file_range`)

---

_@MatthewMckee4 reviewed on 2025-09-23 08:48_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:933 on 2025-09-23 08:48_

Okay I'll update this code then

---

_@MichaReiser reviewed on 2025-09-23 09:27_

> really have no clue why it isnt working

Do you mean that the inlay's aren't clickable. I somewhat suspect that this isn't implemented in monaco. 

I tried to find an inlay example but couldn't find any (the TypeScript playground also doesn't use inlays). It's quiet possible that monaco doesn't support clickable inlays?

---

_@MichaReiser reviewed on 2025-09-23 16:40_

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:439 on 2025-09-23 16:40_

Given that this doesn't work in Monaco (it seems) I'm inclined to not set location and instead at a comment saying that. As of today (date), location isn't supported by Monaco which is why the field is ommited in the playground.

---

_@MatthewMckee4 reviewed on 2025-09-23 16:43_

---

_Review comment by @MatthewMckee4 on `playground/ty/src/Editor/Editor.tsx`:439 on 2025-09-23 16:43_

yeah that makes sense, will update. Thanks

---

_@MatthewMckee4 reviewed on 2025-09-23 16:56_

---

_Review comment by @MatthewMckee4 on `playground/ty/src/Editor/Editor.tsx`:439 on 2025-09-23 16:56_

I think it would be good to keep all of the updates to `ty_wasm`, just incase in the future monaco supports this, what do you think?

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:439 on 2025-09-24 07:54_

Yeah, I think that's fine. I also don't expect the ty_wasm code to add too much overhead. It only maps a few locations

---

_@MichaReiser reviewed on 2025-09-24 07:54_

---

_Comment by @MichaReiser on 2025-09-24 07:54_

@Gankra would you mind taking another look at this PR (The playground changes look good to me)

---

_Comment by @MatthewMckee4 on 2025-10-21 16:46_

I understand this isn't important for the beta but just making sure this doesn't get lost.

---

_Comment by @MichaReiser on 2025-10-21 16:58_

Thanks. I think it got lost 😓 I'll take another look tomorrow and another ping to @Gankra 

---

_Comment by @MatthewMckee4 on 2025-10-21 21:55_

Thank you

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 06:39_

I find the byte offsets here hard to understand. This seems to be a perfect use case for a diagnostic with two spans:

* The first span points to the source (the `y` in the call)
* The second span points to the target (the `y` in the function declaration)

I think that will make it much easier to read those snapshots


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:873 on 2025-10-22 06:41_

Do we need to use a `FileRange` here? How does `parameter_label_offset` get away with using `TextRange`? Does it use the `Definition.file`?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:938 on 2025-10-22 06:44_

Given that `CallSignatureDetails` stores the function's definition: Can't we compute the definition to parameter offset mapping on demand rather than storing it on `CallSignatureDetails`? What I mean by that: We could have a method `get_parameter_range(self, db, name: &str) -> Option<FileRange>` that calls `parsed_module` ad-hoc, searches the parameter, and returns it. 

Or we can build this map closer to where we use it rather than building it for every `CallSignatureDetails` even when it's never used.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:1143 on 2025-10-22 06:45_

Given that we already map the parameters. Could we look up the parameter range by param_index instead of by name?

---

_@MichaReiser reviewed on 2025-10-22 06:47_

---

_@MatthewMckee4 reviewed on 2025-10-22 10:37_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:1143 on 2025-10-22 10:37_

The issue i was having was with bound methods with `self`. If i had a list then the index would be one off.

---

_@MatthewMckee4 reviewed on 2025-10-22 10:38_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:873 on 2025-10-22 10:38_

Yes we need the `File` and `TextRange` for `NavigationTarget`

---

_@MatthewMckee4 reviewed on 2025-10-22 10:40_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 10:40_

Yes, good point

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:938 on 2025-10-22 10:41_

Yeah, fair point

---

_@MatthewMckee4 reviewed on 2025-10-22 10:41_

---

_@MatthewMckee4 reviewed on 2025-10-22 10:48_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 10:48_

Ah the one hard part about this was that when i annotate with a span
```rs
source.annotate(Annotation::primary(
                Span::from(self.source.file()).with_range(self.source.range()),
            ));
```

I dont get the actual inlay hints in it, so we just point to a "random" part in the file

---

_@MichaReiser reviewed on 2025-10-22 11:02_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:873 on 2025-10-22 11:02_

Fair, but can't we use `definition.file` instead of repeating the `File` for each parameter (which I think is always the same?)

---

_@MichaReiser reviewed on 2025-10-22 11:03_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 11:03_

> I dont get the actual inlay hints in it, so we just point to a "random" part in the file

Can you elaborate on this? Why would we get random parts in the file?

---

_@MatthewMckee4 reviewed on 2025-10-22 11:05_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:873 on 2025-10-22 11:05_

Yeah, i had that before. Changed in https://github.com/astral-sh/ruff/pull/20349/commits/20364f173946173210255be731b938b0bf71bdea. The code is a lot cleaner like this, since definition is an `Option` in `CallSignatureDetails`.

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 11:12_

Yes sorry.Our extra inlay hint labels that we add in the output `[a=]`. We want to be able to add a `source` section in the diagnostics section of the snapshots. But when we try to use the original file to show the source of the inlay hint goto `a` is not in the file. So we cant really show a source, unless there is another way to do this for an arbitrary `String`.

---

_@MatthewMckee4 reviewed on 2025-10-22 11:12_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 11:17_

Sorry if this is hard to read, this is the new snapshots from when i try to include the source. We see thee `^` points to random places where the inlay hint label "would" be.
```
   18    25 │   |                                      ^
   19    26 │ 3 | foo(1, 'pos', 3.14, False, e=42)
   20    27 │ 4 | foo(1, 'pos', 3.14, e=42, f='custom')
   21    28 │   |
   22       │-info: For inlay hint label 'd' at 115..116
         29 │+info: Source
         30 │+ --> main.py:3:25
         31 │+  |
         32 │+2 | def foo(a: int, b: str, /, c: float, d: bool = True, *, e: int, f: str = 'default'): pass
         33 │+3 | foo(1, 'pos', 3.14, False, e=42)
         34 │+  |                         ^
         35 │+4 | foo(1, 'pos', 3.14, e=42, f='custom')
         36 │+  |
   23    37 │
   24    38 │ info[inlay-hint-location]: Inlay Hint Target
   25    39 │  --> main.py:2:28
   26    40 │   |
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   28    42 │   |                            ^
   29    43 │ 3 | foo(1, 'pos', 3.14, False, e=42)
   30    44 │ 4 | foo(1, 'pos', 3.14, e=42, f='custom')
   31    45 │   |
   32       │-info: For inlay hint label 'c' at 146..147
         46 │+info: Source
         47 │+ --> main.py:4:23
         48 │+  |
         49 │+2 | def foo(a: int, b: str, /, c: float, d: bool = True, *, e: int, f: str = 'default'): pass
         50 │+3 | foo(1, 'pos', 3.14, False, e=42)
         51 │+4 | foo(1, 'pos', 3.14, e=42, f='custom')
         52 │+  |                       ^
         53 │+  |
```

---

_@MatthewMckee4 reviewed on 2025-10-22 11:17_

---

_@MichaReiser reviewed on 2025-10-22 12:24_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 12:24_

What about something like this

```patch
Index: crates/ty_ide/src/inlay_hints.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_ide/src/inlay_hints.rs b/crates/ty_ide/src/inlay_hints.rs
--- a/crates/ty_ide/src/inlay_hints.rs	(revision 1522cbd742f0e725a1d4725ecfcd465a4b50f7b1)
+++ b/crates/ty_ide/src/inlay_hints.rs	(date 1761135752041)
@@ -435,17 +435,10 @@
                 for part in hint.label.parts() {
                     hint_str.push_str(part.text());
 
-                    let label_length = part.text().len();
-
                     if let Some(target) = part.target() {
-                        let label_range = TextRange::new(
-                            TextSize::try_from(end_position).unwrap(),
-                            TextSize::try_from(end_position + label_length).unwrap(),
-                        );
-
                         diagnostics.push(InlayHintLocationDiagnostic::new(
                             part.text().to_string(),
-                            label_range,
+                            FileRange::new(self.file, TextRange::at(hint.position, TextSize::ZERO)),
                             target,
                         ));
                     }
@@ -555,16 +548,19 @@
         a.y[: Literal[3]] = 3
 
         ---------------------------------------------
-        info[inlay-hint-location]: Inlay Hint Target
+        info[inlay-hint-location]: Inlay Hint part y
          --> main.py:3:24
           |
         2 | class A:
         3 |     def __init__(self, y):
-          |                        ^
+          |                        - points here
         4 |         self.x = 1
         5 |         self.y = y
+        6 |
+        7 | a = A(2)
+          |       ^
+        8 | a.y = 3
           |
-        info: For inlay hint label 'y' at 112..113
         ");
     }
 
@@ -1132,7 +1128,7 @@
          6 |     return y
          7 |
          8 | def baz(a: int, b: str, c: bool): pass
-           |         ^
+           |     ^
          9 |
         10 | baz(foo(5), bar(bar('test')), True)
            |
@@ -1140,11 +1136,11 @@
 
         info[inlay-hint-location]: Inlay Hint Target
          --> main.py:2:9
-          |
-        2 | def foo(x: int) -> int:
+           |
+         2 | def foo(x: int) -> int:
           |         ^
-        3 |     return x * 2
-          |
+         3 |     return x * 2
+           |
         info: For inlay hint label 'x' at 133..134
 
         info[inlay-hint-location]: Inlay Hint Target
@@ -1153,7 +1149,7 @@
          6 |     return y
          7 |
          8 | def baz(a: int, b: str, c: bool): pass
-           |                 ^
+           |             ^
          9 |
         10 | baz(foo(5), bar(bar('test')), True)
            |
@@ -1161,24 +1157,24 @@
 
         info[inlay-hint-location]: Inlay Hint Target
          --> main.py:5:9
-          |
-        3 |     return x * 2
-        4 |
-        5 | def bar(y: str) -> str:
+           |
+         3 |     return x * 2
+         4 |
+         5 | def bar(y: str) -> str:
           |         ^
-        6 |     return y
-          |
+         6 |     return y
+           |
         info: For inlay hint label 'y' at 149..150
 
         info[inlay-hint-location]: Inlay Hint Target
          --> main.py:5:9
-          |
-        3 |     return x * 2
-        4 |
-        5 | def bar(y: str) -> str:
+           |
+         3 |     return x * 2
+         4 |
+         5 | def bar(y: str) -> str:
           |         ^
-        6 |     return y
-          |
+         6 |     return y
+           |
         info: For inlay hint label 'y' at 157..158
 
         info[inlay-hint-location]: Inlay Hint Target
@@ -1400,24 +1396,24 @@
         ---------------------------------------------
         info[inlay-hint-location]: Inlay Hint Target
          --> main.py:5:9
-          |
-        4 | @overload
-        5 | def foo(x: int) -> str: ...
+           |
+         4 | @overload
+         5 | def foo(x: int) -> str: ...
           |         ^
-        6 | @overload
-        7 | def foo(x: str) -> int: ...
-          |
+         6 | @overload
+         7 | def foo(x: str) -> int: ...
+           |
         info: For inlay hint label 'x' at 136..137
 
         info[inlay-hint-location]: Inlay Hint Target
          --> main.py:5:9
-          |
-        4 | @overload
-        5 | def foo(x: int) -> str: ...
+           |
+         4 | @overload
+         5 | def foo(x: int) -> str: ...
           |         ^
-        6 | @overload
-        7 | def foo(x: str) -> int: ...
-          |
+         6 | @overload
+         7 | def foo(x: str) -> int: ...
+           |
         info: For inlay hint label 'x' at 148..149
         ");
     }
@@ -1558,7 +1554,7 @@
                 "Inlay Hint Target".to_string(),
             );
             main.annotate(Annotation::primary(
-                Span::from(self.target.file()).with_range(self.target.range()),
+                    Span::from(self.target.file()).with_range(self.target.range()),
             ));
             main.info(format!(
                 "For inlay hint label '{}' at {:?}",
```

The alternative is that you:
* iterate over all hints and collect all parts
* Once you have the final buffer, write a new file to the in-memory file system
* Now create the diagnostics for all parts



---

_@MatthewMckee4 reviewed on 2025-10-22 12:33_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:567 on 2025-10-22 12:33_

That looks good, updated now

---

_Comment by @MichaReiser on 2025-10-22 12:48_

Let me know when this is ready for re-review (you can request a review)

---

_@MatthewMckee4 reviewed on 2025-10-22 13:00_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/ide_support.rs`:938 on 2025-10-22 13:00_

added this now

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-10-22 13:01_

---

_Merged by @MichaReiser on 2025-11-17 10:33_

---

_Closed by @MichaReiser on 2025-11-17 10:33_

---

_Branch deleted on 2025-12-27 00:21_

---

```yaml
number: 8835
title: New AST nodes for f-string elements
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/fstring-element
created_at: 2023-11-24T23:39:34Z
updated_at: 2023-12-07T16:28:08Z
url: https://github.com/astral-sh/ruff/pull/8835
synced_at: 2026-01-12T15:55:27Z
```

# New AST nodes for f-string elements

---

_@dhruvmanila_

Rebase of #6365 authored by @davidszotten.

## Summary

This PR updates the AST structure for an f-string elements.

The main **motivation** behind this change is to have a dedicated node for the string part of an f-string. Previously, the existing `ExprStringLiteral` node was used for this purpose which isn't exactly correct. The `ExprStringLiteral` node should include the quotes as well in the range but the f-string literal element doesn't include the quote as it's a specific part within an f-string. For example,

```python
f"foo {x}"
# ^^^^
# This is the literal part of an f-string
```

The introduction of `FStringElement` enum is helpful which represent either the literal part or the expression part of an f-string.

### Rule Updates

This means that there'll be two nodes representing a string depending on the context. One for a normal string literal while the other is a string literal within an f-string. The AST checker is updated to accommodate this change. The rules which work on string literal are updated to check on the literal part of f-string as well.

#### Notes

1. The `Expr::is_literal_expr` method would check for `ExprStringLiteral` and return true if so. But now that we don't represent the literal part of an f-string using that node, this improves the method's behavior and confines to the actual expression. We do have the `FStringElement::is_literal` method.
2. We avoid checking if we're in a f-string context before adding to `string_type_definitions` because the f-string literal is now a dedicated node and not part of `Expr`.
3. Annotations cannot use f-string so we avoid changing any rules which work on annotation and checks for `ExprStringLiteral`.

## Test Plan

- All references of `Expr::StringLiteral` were checked to see if any of the rules require updating to account for the f-string literal element node.
- New test cases are added for rules which check against the literal part of an f-string.
- Check the ecosystem results and ensure it remains unchanged.

## Performance

There's a performance penalty in the parser. The reason for this remains unknown as it seems that the generated assembly code is now different for the `__reduce154` function. The reduce function body is just popping the `ParenthesizedExpr` on top of the stack and pushing it with the new location.

- The size of `FStringElement` enum is the same as `Expr` which is what it replaces in `FString::format_spec`
- The size of `FStringExpressionElement` is the same as `ExprFormattedValue` which is what it replaces

I tried reducing the `Expr` enum from 80 bytes to 72 bytes but it hardly resulted in any performance gain. The difference can be seen here:
- Original profile: https://share.firefox.dev/3Taa7ES
- Profile after boxing some node fields: https://share.firefox.dev/3GsNXpD

### Backtracking

I tried backtracking the changes to see if any of the isolated change produced this regression. The problem here is that the overall change is so small that there's only a single checkpoint where I can backtrack and that checkpoint results in the same regression. This checkpoint is to revert using `Expr` to the `FString::format_spec` field. After this point, the change would revert back to the original implementation.

## Review process

The review process is similar to #7927. The first set of commits update the node structure, parser, and related AST files. Then, further commits update the linter and formatter part to account for the AST change.

---

_Comment by @dhruvmanila on 2023-11-24 23:39_

Current dependencies on/for this PR:
* main
  * **PR #8062** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8062?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8063** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8063?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8064** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8064?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7927** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7927?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #8826** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8826?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #8828** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8828?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #8835** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8835?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
                * **PR #9016** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9016?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8835?utm_source=stack-comment).

---

_Comment by @codspeed-hq[bot] on 2023-11-24 23:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/fstring-element)

### Merging #8835 will **degrade performances by 6.65%**

<sub>Comparing <code>dhruv/fstring-element</code> (b6be853) with <code>main</code> (af88ffc)</sub>



### Summary

`❌ 6` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/fstring-element)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/fstring-element` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/ctypeslib.py]` | 16.1 ms | 17.2 ms | -6.65% |
| ❌ | `parser[numpy/ctypeslib.py]` | 11.1 ms | 11.8 ms | -5.86% |
| ❌ | `parser[pydantic/types.py]` | 24.8 ms | 26.3 ms | -5.59% |
| ❌ | `parser[numpy/globals.py]` | 1.1 ms | 1.1 ms | -4.97% |
| ❌ | `parser[large/dataset.py]` | 64.8 ms | 68.7 ms | -5.7% |
| ❌ | `parser[unicode/pypinyin.py]` | 3.9 ms | 4.1 ms | -5.58% |


---

_Comment by @github-actions[bot] on 2023-11-24 23:54_

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

_Comment by @dhruvmanila on 2023-11-25 00:41_

Ugh, I need to revisit this later. I think I messed up the git history somehow :/

---

_Label `internal` added by @dhruvmanila on 2023-11-25 00:41_

---

_Closed by @dhruvmanila on 2023-11-27 22:04_

---

_Reopened by @dhruvmanila on 2023-11-27 22:04_

---

_@MichaReiser reviewed on 2023-12-04 00:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:182 on 2023-12-04 00:31_

What's the motivation for boxing some fields? it seems unrelated to the PR itself. It makes it more difficult for me to judge if the drop regression is due to the new f-string node layout or the other boxing changes. Would you mind splitting this commit into its own PR?

---

_Comment by @MichaReiser on 2023-12-04 02:55_

Comparing the generated llvm instructions between main and this PR

```diff
< define internal fastcc void @_ZN18ruff_python_parser6python12__parse__Top11__reduce15417hd406a4d6cb8441daE(ptr noalias nocapture noundef align 8 dereferenceable(24) %__symbols) unnamed_addr #2 personality ptr @rust_eh_personality {
---
> define internal fastcc void @_ZN18ruff_python_parser6python12__parse__Top11__reduce15417h429ba5e9ae4663f4E(ptr noalias nocapture noundef align 8 dereferenceable(24) %__symbols) unnamed_addr #2 personality ptr @rust_eh_personality {
6,10c6,12
<   %_10.sroa.0 = alloca [192 x i8], align 8
<   tail call void @llvm.experimental.noalias.scope.decl(metadata !21616)
<   call void @llvm.lifetime.start.p0(i64 208, ptr nonnull %_2.i), !noalias !21619
<   tail call void @llvm.experimental.noalias.scope.decl(metadata !21621)
<   tail call void @llvm.experimental.noalias.scope.decl(metadata !21624)
---
>   %__nt = alloca %"ruff_python_ast::nodes::ParenthesizedExpr", align 8
>   %_11.sroa.4 = alloca [23 x i32], align 4
>   %_10.sroa.4 = alloca [23 x i32], align 4
>   tail call void @llvm.experimental.noalias.scope.decl(metadata !21750)
>   call void @llvm.lifetime.start.p0(i64 208, ptr nonnull %_2.i), !noalias !21753
>   tail call void @llvm.experimental.noalias.scope.decl(metadata !21755)
>   tail call void @llvm.experimental.noalias.scope.decl(metadata !21758)
12c14
<   %_2.i.i = load i64, ptr %0, align 8, !alias.scope !21626, !noalias !21627, !noundef !14
---
>   %_2.i.i = load i64, ptr %0, align 8, !alias.scope !21760, !noalias !21761, !noundef !14
14c16
<   br i1 %1, label %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.thread.i", label %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.i"
---
>   br i1 %1, label %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.thread.i", label %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.i"
16,18c18,19
< "_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.thread.i": ; preds = %start
<   %2 = getelementptr inbounds %"core::option::Option<(ruff_text_size::size::TextSize, python::__parse__Top::__Symbol, ruff_text_size::size::TextSize)>", ptr %_2.i, i64 0, i32 1
<   store i8 104, ptr %2, align 8, !alias.scope !21621, !noalias !21628
---
> "_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.thread.i": ; preds = %start
>   store i32 137, ptr %_2.i, align 8, !alias.scope !21755, !noalias !21762
21,26c22,27
< "_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.i": ; preds = %start
<   %3 = add i64 %_2.i.i, -1
<   store i64 %3, ptr %0, align 8, !alias.scope !21626, !noalias !21627
<   %4 = getelementptr inbounds { ptr, i64 }, ptr %__symbols, i64 0, i32 1
<   %5 = load i64, ptr %4, align 8, !noalias !14, !noundef !14
<   %_3.i.i = icmp ult i64 %3, %5
---
> "_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.i": ; preds = %start
>   %2 = add i64 %_2.i.i, -1
>   store i64 %2, ptr %0, align 8, !alias.scope !21760, !noalias !21761
>   %3 = getelementptr inbounds { ptr, i64 }, ptr %__symbols, i64 0, i32 1
>   %4 = load i64, ptr %3, align 8, !noalias !14, !noundef !14
>   %_3.i.i = icmp ult i64 %2, %4
29,34c30,34
<   %src.i.i = getelementptr inbounds { %"python::__parse__Top::__Symbol", i32, i32 }, ptr %self1.i.i, i64 %3
<   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 8 dereferenceable(208) %_2.i, ptr noundef nonnull align 8 dereferenceable(208) %src.i.i, i64 208, i1 false), !noalias !21628
<   %.phi.trans.insert.i = getelementptr inbounds %"core::option::Option<(ruff_text_size::size::TextSize, python::__parse__Top::__Symbol, ruff_text_size::size::TextSize)>", ptr %_2.i, i64 0, i32 1
<   %.pre.i = load i8, ptr %.phi.trans.insert.i, align 8, !range !2745, !noalias !21619
<   %6 = icmp eq i8 %.pre.i, 17
<   br i1 %6, label %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$4push17h6b55ac9fa3bd2a2aE.exit", label %bb2.i
---
>   %src.i.i = getelementptr inbounds { %"python::__parse__Top::__Symbol", i32, i32 }, ptr %self1.i.i, i64 %2
>   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 8 dereferenceable(208) %_2.i, ptr noundef nonnull align 8 dereferenceable(208) %src.i.i, i64 208, i1 false), !noalias !21762
>   %.pr.i = load i32, ptr %_2.i, align 8, !noalias !21753
>   %cond.i = icmp eq i32 %.pr.i, 47
>   br i1 %cond.i, label %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$4push17h6f744e2ae0097c52E.exit", label %bb2.i
36,37c36,37
< bb2.i:                                            ; preds = %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.i", %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.thread.i"
<   %7 = phi i8 [ 104, %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.thread.i" ], [ %.pre.i, %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.i" ]
---
> bb2.i:                                            ; preds = %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.i", %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.thread.i"
>   %5 = phi i32 [ 137, %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.thread.i" ], [ %.pr.i, %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.i" ]
40c40
<           to label %unreachable.i unwind label %cleanup.i, !noalias !21619
---
>           to label %unreachable.i unwind label %cleanup.i, !noalias !21753
43c43
<   %8 = landingpad { ptr, i32 }
---
>   %6 = landingpad { ptr, i32 }
45,46c45,46
<   %9 = icmp eq i8 %7, 104
<   br i1 %9, label %bb6.i, label %bb2.i3.i
---
>   %7 = icmp eq i32 %5, 137
>   br i1 %7, label %bb6.i, label %bb2.i2.i
48c48
< bb2.i3.i:                                         ; preds = %cleanup.i
---
> bb2.i2.i:                                         ; preds = %cleanup.i
50,51c50,51
<   invoke fastcc void @"_ZN4core3ptr71drop_in_place$LT$ruff_python_parser..python..__parse__Top..__Symbol$GT$17h7ca4ec50464f112bE"(ptr noalias noundef nonnull align 8 dereferenceable(200) %_2.i)
<           to label %bb6.i unwind label %terminate.i, !noalias !21619
---
>   invoke fastcc void @"_ZN4core3ptr71drop_in_place$LT$ruff_python_parser..python..__parse__Top..__Symbol$GT$17ha84e15138d5fb157E"(ptr noalias noundef nonnull align 8 dereferenceable(200) %_2.i)
>           to label %bb6.i unwind label %terminate.i, !noalias !21753
56,57c56,57
< terminate.i:                                      ; preds = %bb2.i3.i
<   %10 = landingpad { ptr, i32 }
---
> terminate.i:                                      ; preds = %bb2.i2.i
>   %8 = landingpad { ptr, i32 }
60c60
<   call void @_ZN4core9panicking16panic_in_cleanup17he7753e109d98c84aE() #34, !noalias !21619
---
>   call void @_ZN4core9panicking16panic_in_cleanup17he7753e109d98c84aE() #34, !noalias !21753
63,64c63,64
< bb6.i:                                            ; preds = %bb2.i3.i, %cleanup.i
<   resume { ptr, i32 } %8
---
> bb6.i:                                            ; preds = %bb2.i2.i, %cleanup.i
>   resume { ptr, i32 } %6
66,77c66,83
< "_ZN5alloc3vec16Vec$LT$T$C$A$GT$4push17h6b55ac9fa3bd2a2aE.exit": ; preds = %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h34c3d6d4499edab0E.exit.i"
<   %11 = getelementptr inbounds { %"python::__parse__Top::__Symbol", i32, i32 }, ptr %_2.i, i64 0, i32 1
<   %__l.i = load i32, ptr %11, align 8, !noalias !21619, !noundef !14
<   %12 = getelementptr inbounds { %"python::__parse__Top::__Symbol", i32, i32 }, ptr %_2.i, i64 0, i32 2
<   %__r.i = load i32, ptr %12, align 4, !noalias !21619, !noundef !14
<   call void @llvm.lifetime.start.p0(i64 192, ptr nonnull %_10.sroa.0)
<   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 8 dereferenceable(88) %_10.sroa.0, ptr noundef nonnull align 8 dereferenceable(88) %src.i.i, i64 88, i1 false)
<   call void @llvm.lifetime.end.p0(i64 208, ptr nonnull %_2.i), !noalias !21619
<   tail call void @llvm.experimental.noalias.scope.decl(metadata !21629)
<   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 8 dereferenceable(192) %src.i.i, ptr noundef nonnull align 8 dereferenceable(192) %_10.sroa.0, i64 192, i1 false), !noalias !21629
<   %_10.sroa.4.0.end.i.sroa_idx = getelementptr inbounds i8, ptr %src.i.i, i64 192
<   store i8 17, ptr %_10.sroa.4.0.end.i.sroa_idx, align 8, !noalias !21629
---
> "_ZN5alloc3vec16Vec$LT$T$C$A$GT$4push17h6f744e2ae0097c52E.exit": ; preds = %"_ZN5alloc3vec16Vec$LT$T$C$A$GT$3pop17h91ca183f74ada0f2E.exit.i"
>   %9 = getelementptr inbounds { %"python::__parse__Top::__Symbol", i32, i32 }, ptr %_2.i, i64 0, i32 1
>   %__l.i = load i32, ptr %9, align 8, !noalias !21753, !noundef !14
>   %10 = getelementptr inbounds { %"python::__parse__Top::__Symbol", i32, i32 }, ptr %_2.i, i64 0, i32 2
>   %__r.i = load i32, ptr %10, align 4, !noalias !21753, !noundef !14
>   %11 = getelementptr inbounds %"python::__parse__Top::__Symbol::Variant15", ptr %_2.i, i64 0, i32 1
>   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 8 dereferenceable(88) %__nt, ptr noundef nonnull align 8 dereferenceable(88) %11, i64 88, i1 false)
>   call void @llvm.lifetime.end.p0(i64 208, ptr nonnull %_2.i), !noalias !21753
>   call void @llvm.lifetime.start.p0(i64 92, ptr nonnull %_10.sroa.4)
>   call void @llvm.lifetime.start.p0(i64 92, ptr nonnull %_11.sroa.4)
>   %_11.sroa.4.8.sroa_idx = getelementptr inbounds i8, ptr %_11.sroa.4, i64 4
>   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 4 dereferenceable(88) %_11.sroa.4.8.sroa_idx, ptr noundef nonnull align 8 dereferenceable(88) %__nt, i64 88, i1 false)
>   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 4 dereferenceable(92) %_10.sroa.4, ptr noundef nonnull align 4 dereferenceable(92) %_11.sroa.4, i64 92, i1 false)
>   call void @llvm.lifetime.end.p0(i64 92, ptr nonnull %_11.sroa.4)
>   tail call void @llvm.experimental.noalias.scope.decl(metadata !21763)
>   store i32 47, ptr %src.i.i, align 8, !noalias !21763
>   %_10.sroa.4.0.end.i.sroa_idx = getelementptr inbounds i8, ptr %src.i.i, i64 4
>   call void @llvm.memcpy.p0.p0.i64(ptr noundef nonnull align 4 dereferenceable(92) %_10.sroa.4.0.end.i.sroa_idx, ptr noundef nonnull align 4 dereferenceable(92) %_10.sroa.4, i64 92, i1 false), !noalias !21763
79c85
<   store i32 %__l.i, ptr %_10.sroa.6.0.end.i.sroa_idx, align 8, !noalias !21629
---
>   store i32 %__l.i, ptr %_10.sroa.6.0.end.i.sroa_idx, align 8, !noalias !21763
81,83c87,89
<   store i32 %__r.i, ptr %_10.sroa.7.0.end.i.sroa_idx, align 4, !noalias !21629
<   store i64 %_2.i.i, ptr %0, align 8, !alias.scope !21629, !noalias !21632
<   call void @llvm.lifetime.end.p0(i64 192, ptr nonnull %_10.sroa.0)
---
>   store i32 %__r.i, ptr %_10.sroa.7.0.end.i.sroa_idx, align 4, !noalias !21763
>   store i64 %_2.i.i, ptr %0, align 8, !alias.scope !21763, !noalias !21766
>   call void @llvm.lifetime.end.p0(i64 92, ptr nonnull %_10.sroa.4)
(venv) 
```

I did compare the generated assembly and the new assembly indeed is longer for reduce154 than it used to.

```diff
8c8
<  cbz     x8, LBB835_3
---
>  cbz     x8, LBB839_3
28,53c28,70
<  ldrb    w19, [sp, #384]
<  cmp     w19, #17
<  b.ne    LBB835_4
<  ldr     w10, [sp, #392]
<  ldr     w11, [sp, #396]
<  ldr     x12, [x9, #80]
<  str     x12, [sp, #80]
<  ldp     q0, q1, [x9, #32]
<  stp     q0, q1, [sp, #32]
<  ldr     q2, [x9, #64]
<  str     q2, [sp, #64]
<  ldp     q3, q4, [sp, #144]
<  ldp     q6, q5, [sp, #112]
<  stp     q5, q3, [x9, #128]
<  ldr     q3, [sp, #176]
<  stp     q4, q3, [x9, #160]
<  ldp     q3, q4, [sp, #80]
<  stp     q2, q3, [x9, #64]
<  ldp     q2, q3, [x9]
<  stp     q2, q3, [sp]
<  stp     q4, q6, [x9, #96]
<  stp     q2, q3, [x9]
<  stp     q0, q1, [x9, #32]
<  mov     w12, #17
<  strb    w12, [x9, #192]
<  stp     w10, w11, [x9, #200]
---
>  ldr     w19, [sp, #192]
>  cmp     w19, #47
>  b.ne    LBB839_4
>  add     x10, sp, #192
>  ldr     w11, [sp, #392]
>  ldr     w12, [sp, #396]
>  ldur    q0, [x10, #40]
>  ldur    q1, [x10, #56]
>  stp     q0, q1, [sp, #128]
>  ldur    q2, [x10, #72]
>  str     q2, [sp, #160]
>  ldr     x13, [sp, #280]
>  str     x13, [sp, #176]
>  ldur    q3, [x10, #8]
>  ldur    q4, [x10, #24]
>  stp     q3, q4, [sp, #96]
>  stur    x13, [x10, #84]
>  stur    q2, [x10, #68]
>  stur    q1, [x10, #52]
>  stur    q0, [x10, #36]
>  stur    q4, [x10, #20]
>  stur    q3, [x10, #4]
>  ldur    q0, [x10, #76]
>  stur    q0, [sp, #76]
>  ldr     q2, [sp, #256]
>  ldp     q3, q0, [sp, #224]
>  stp     q0, q2, [sp, #48]
>  ldp     q0, q1, [sp, #192]
>  str     q0, [sp]
>  stp     q1, q3, [sp, #16]
>  mov     w10, #47
>  str     w10, [x9]
>  ldp     q0, q1, [sp, #32]
>  stur    q0, [x9, #36]
>  stur    q1, [x9, #52]
>  ldr     q0, [sp, #64]
>  stur    q0, [x9, #68]
>  ldur    q0, [sp, #76]
>  str     q0, [x9, #80]
>  ldp     q0, q1, [sp]
>  stur    q0, [x9, #4]
>  stur    q1, [x9, #20]
>  stp     w11, w12, [x9, #200]
60,64c77,81
< LBB835_3:
<  mov     w8, #104
<  mov     w19, #104
<  strb    w8, [sp, #384]
< LBB835_4:
---
> LBB839_3:
>  mov     w8, #137
>  mov     w19, #137
>  str     w8, [sp, #192]
> LBB839_4:
67c84
< LBB835_6:
---
> LBB839_6:
69,70c86,87
<  cmp     w19, #104
<  b.eq    LBB835_8
---
>  cmp     w19, #137
>  b.eq    LBB839_8
72,73c89,90
<  bl      __ZN4core3ptr71drop_in_place$LT$ruff_python_parser..python..__parse__Top..__Symbol$GT$17h7ca4ec50464f112bE
< LBB835_8:
---
>  bl      __ZN4core3ptr71drop_in_place$LT$ruff_python_parser..python..__parse__Top..__Symbol$GT$17ha84e15138d5fb157E
> LBB839_8:
76c93
< LBB835_9:
---
> LBB839_9:

```

It seems that the new version uses `ldr` instead of `ldrb` on ARM which loads the entire 4 bytes instead of just 8bits into memory. I'm no good at reading assembly...

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3951 on 2023-12-04 03:31_

Nit: Can we move this above the size_assertions?

---

_@MichaReiser reviewed on 2023-12-04 03:31_

---

_Comment by @MichaReiser on 2023-12-04 04:15_

Some findings from reading through the `python.rs` changes:

* There are now 104 Symbol variants compared to 101 before (one for `Option<FStringFormatSpec>`, one for `FStringFormatSpec`, and one for `FStringElement`)
* The Symbol size remains unchanged at 200 bytes (My hope was that it changed)

---

_@dhruvmanila reviewed on 2023-12-04 14:53_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:182 on 2023-12-04 14:53_

Ugh, sorry. That was just me figuring out if decreasing the size would revert the regression -- it doesn't, I'll revert the change.

---

_Marked ready for review by @dhruvmanila on 2023-12-04 21:04_

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-12-04 21:04_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-05 01:40_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1011 on 2023-12-05 01:51_

Nit: Not necessarily better, it just feels a bit more explicit to me but I don't have a clear preference.


```suggestion
            for element in value.elements().filter_map(|element| element.as_literal()) {
                if checker.enabled(Rule::HardcodedBindAllInterfaces) {
```

But you then need to access the `value` and `range` through `element`. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1017 on 2023-12-05 01:53_

Would it make sense to have a `analyze_string_like` function that accepts a `string_like` value (new `StringLike` enum that is either a `FStringLiteral` or `StringExpressionLiteral`) to ensure we don't forget to the rules to `FStringLiteral`, regular strings, and potentially bytes? Or is it only a subset of string rules that need to run for fstring literals?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1017 on 2023-12-05 01:55_

Is it correct that this rule now triggers for `f"0.0.0.0{expression}something_else"` or should we enforce that the fstring only has a single element that exactly matches a certain value (same for other rules)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:46 on 2023-12-05 01:59_

Using `AnyNodeRef` for `node` is unfortunate because it is now unclear what kind of nodes the rule accepts. I recommend renaming `node` to `string_like` to make it more explicit and/or consider introducing a `StringLike` enum that covers `FStringExpression`, `ByteLiteral`, and `StringLiteral` if this is something that we need in multiple places.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:59 on 2023-12-05 02:17_

I got rather confused by the `parts` method here. Like what's the difference between a `FStringPart` and an `FStringElement`, they even have the same members (although `FStringPart::Literal` takes a `StringLiteral` and `Element::Literal` an `FStringElementLiteral`). 

It clicked when opening the PR and looking up the definition, although I wonder if we can improve the naming (unrelated to this PR). 

A not fully formed idea:

* `ExprStringLiteral` -> `ExprString`
* `ExprStringLiteral::parts` rename to `literals` (it returns the individal string literals of which the string expression is composed)
* `ExprFString::parts` rename to `literals`
* `FStringPart` rename to `ExprFStringLiteral` 
* `FStringPart::Literal` rename to `FSTringPart::String`

Same for bytes. 

We may also want to change the `parts` and `parts_mut` methods to return a `&[]` instead of an iterator. 

The part I feel uncertain about is if it is okay to name an `f"abcd"` a literal, but I think it is. For example, the object notation `{a: "bcd"}` in JavaScript is a `ObjectLiteral` and it contains dynamic content as well. 

I don't suggest making these changes as part of this PR. It's just something I dripped over when reviewing and thought I would raise.


Edit: Even the Python spec names them literals: A formatted string literal 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/f_string_missing_placeholders.rs`:55 on 2023-12-05 02:21_

Nit: We could add these as a method on `fstring` itself: `has_placeholders()` or similar if we want.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/tryceratops/rules/raise_vanilla_args.rs`:109 on 2023-12-05 02:23_

Nit:
```suggestion
                    ast::FStringPart::FString(f_string) => {
                        for literal in f_string.elements.iter().filter_map(|element| element.as_literal()) {
                            if literal.value.chars().any(char::is_whitespace) {
                                return true;
                            }
                        }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor/transformer.rs`:738 on 2023-12-05 02:40_

It feels inconsistent to me that we have visitors for `string_literal` and `byte_literals` but that we aren't visiting `Literal` elements here. I think we should either have visitors for both or neither. The same applies to `preorder` and the regular visitor

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor/preorder.rs`:529 on 2023-12-05 02:42_

I think you can call here into `element.visit_preorder(visitor)` considering that `FStringExpressionElement` implements `AstNode`

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/tests/preorder.rs`:226 on 2023-12-05 02:45_

Can you add overrides for the new visitor functions or rewrite the test to use `enter_node` and `exit_node`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/f_string_expression_element.rs`:8 on 2023-12-05 02:48_

I'm not sure if it's worth adding these implementations considering that they won't work. I rather have `fstring_expression_element.format()` to not compile than fail at runtime. 

Should we exclude them from the generate script. We can still add them when they become useful.

---

_@MichaReiser approved on 2023-12-05 02:53_

Thanks for driving this long standing PR forward. I like what I see in the parser diffs. 

The majority of comments are nits, but there's the question whather we should visit the `FStringLiteralElement` in visitors and if some lint rules should check if it si a single element fstring to avoid false positives. 

I would prefer for @charliermarsh to also review the linter changes, considering that he has more context than I.

---

_@dhruvmanila reviewed on 2023-12-05 14:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1017 on 2023-12-05 14:19_

It always triggered for such cases, we're just maintaining the behavior. The reason is that it could be useful for cases like `f"0.0.0.0{port}/path"`: https://play.ruff.rs/7b6e80ea-5858-4983-877c-1669a5cd538d

---

_@dhruvmanila reviewed on 2023-12-05 14:21_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:59 on 2023-12-05 14:21_

Thanks! I'll explore this in a follow-up PR.

---

_@dhruvmanila reviewed on 2023-12-05 14:23_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyflakes/rules/f_string_missing_placeholders.rs`:55 on 2023-12-05 14:23_

I thought so but currently it's only being used in a single place.

---

_@dhruvmanila reviewed on 2023-12-05 14:42_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1011 on 2023-12-05 14:42_

Yeah, I thought about this. I don't really like the formatting so I think it's worth updating as per your suggestion.

---

_@dhruvmanila reviewed on 2023-12-05 15:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor/transformer.rs`:738 on 2023-12-05 15:02_

I see your point. Wouldn't `visit_f_string_element` cover visiting the literal element of an f-string? This is similar to what we have with `visit_match_case` and `visit_pattern`.

Currently, we've a visitor for an `FStringElement` which is what you're highlighting here. I'd prefer to keep the literal visitors because they help in the formatter comment placements. Is what you're suggesting to replace `visit_f_string_element` with `visit_f_string_literal_element` and `visit_f_string_expression_element`? So, the hierarchy would be:

```bash
FString # visit_f_string
 |- FStringLiteralElement # visit_f_string_literal_element
 |- FStringExpressionElement # visit_f_string_expression_element
```

---

_@dhruvmanila reviewed on 2023-12-05 15:14_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor/preorder.rs`:529 on 2023-12-05 15:14_

Ah yes, thanks for noticing that.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:46 on 2023-12-05 20:34_

Yeah, I think this might be a good idea (https://github.com/astral-sh/ruff/pull/9016).

---

_@dhruvmanila reviewed on 2023-12-05 20:34_

---

_@MichaReiser reviewed on 2023-12-06 00:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor/transformer.rs`:738 on 2023-12-06 00:30_

> I see your point. Wouldn't visit_f_string_element cover visiting the literal element of an f-string? This is similar to what we have with visit_match_case and visit_pattern.

That makes sense. Thank's for explaining. 



---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1017 on 2023-12-06 17:56_

Refer: https://github.com/astral-sh/ruff/pull/9016

---

_@dhruvmanila reviewed on 2023-12-06 17:56_

---

_@dhruvmanila reviewed on 2023-12-06 21:54_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:818 on 2023-12-06 21:54_

The `in_f_string` check isn't required now that there's a dedicated node for the literal part of f-string and it isn't the same as `ExprStringLiteral`.

---

_@charliermarsh reviewed on 2023-12-07 00:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:818 on 2023-12-07 00:36_

Smart.

---

_@charliermarsh reviewed on 2023-12-07 00:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1010 on 2023-12-07 00:40_

So what happens here if we have, like `func(f"/tmp" f"/bad")`, and the rule is matching against `"/tmp/bad"`? I guess that would now be a false negative? Or how do we handle concatenations here?

---

_@charliermarsh reviewed on 2023-12-07 00:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1010 on 2023-12-07 00:44_

It looks like we already wouldn't flag that on `main`, though I'm curious if it should be considered a bug. E.g., for `S104`, should we flag `f"0.0" f".0.0"`?

---

_@charliermarsh approved on 2023-12-07 00:44_

This looks great.

---

_@dhruvmanila reviewed on 2023-12-07 01:10_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1010 on 2023-12-07 01:10_

This is a good point and yes it will be a false negative now. It's not just `f"/tmp" f"/bad"` but also `"/tmp" f"/bad"` (string and f-string)

We can introduce a method which iterates over the concatenated literal parts of an f-string. For example,

```python
"foo" f"bar {x} baz" "end"
```

The above code would return two strings: `"foobar "` and `" bazend"`. Here, I don't see a way to avoid string allocation here.

---

_@dhruvmanila reviewed on 2023-12-07 16:27_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1010 on 2023-12-07 16:27_

I'm moving ahead with this limitation for now but we can revisit if it proves to be a problem. My hunch is this shouldn't be but you never know 🤷‍♂️ 

---

_Merged by @dhruvmanila on 2023-12-07 16:28_

---

_Closed by @dhruvmanila on 2023-12-07 16:28_

---

_Branch deleted on 2023-12-07 16:28_

---

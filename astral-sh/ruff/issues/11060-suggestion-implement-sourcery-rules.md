---
number: 11060
title: "Suggestion: Implement Sourcery rules"
type: issue
state: open
author: diceroll123
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2024-04-20T23:26:46Z
updated_at: 2025-11-26T06:51:38Z
url: https://github.com/astral-sh/ruff/issues/11060
synced_at: 2026-01-10T01:22:50Z
---

# Suggestion: Implement Sourcery rules

---

_Issue opened by @diceroll123 on 2024-04-20 23:26_

[Sourcery](https://github.com/sourcery-ai/sourcery) is a closed-source, paid program with refactoring/suggestion rules, some of which are covered by existing rules within Ruff.

Since Sourcery is closed-source, the rules would need to be clean room implementations.

Below, I've enumerated all current Sourcery rules with any overlap with Ruff `0.4.1`, for Astral's consideration.

***Note:*** There is of course the possibility that some unimplemented rules below are equivalent to different unimplemented rules from different libraries, such as Pylint.

I expect that any implementations made would fall under `RUF` (if there is no unimplemented alternative from another set of rules)?

---

- [x] ~~[`assign-if-exp`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/assign-if-exp/)~~ (`SIM108`)
- [x] ~~[`aug-assign`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/aug-assign/)~~ (`PLR6104`)
- [x] ~~[`avoid-builtin-shadow`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/avoid-builtin-shadow/)~~ (`A001`)
- [x] ~~[`aware-datetime-for-utc`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/aware-datetime-for-utc/)~~ (`DTZ003` *implemented but no autofix in Ruff*)
- [ ] [`bin-op-identity`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/bin-op-identity/)
- [x] ~~[`boolean-if-exp-identity`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/boolean-if-exp-identity/)~~ (`SIM210`)
- [x] ~~[`break-or-continue-outside-loop`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/break-or-continue-outside-loop/)~~ (`F701`)
- [ ] [`chain-compares`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/chain-compares/)
- [ ] [`class-extract-method`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/class-extract-method/)
- [x] ~~[`class-method-first-arg-name`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/class-method-first-arg-name/)~~ (`N804`)
- [x] ~~[`collection-builtin-to-comprehension`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/collection-builtin-to-comprehension/)~~ (`C400`, `C401`, `C402`)
- [x] ~~[`collection-into-set`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/collection-into-set/)~~ (`PLR6201`)
- [x] ~~[`collection-to-bool`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/collection-to-bool/)~~ (`F634`)
- [ ] [`compare-via-equals`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/compare-via-equals/)
- [x] ~~[`comprehension-to-generator`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/comprehension-to-generator/)~~ (`C419`)
- [ ] [`convert-any-to-in`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/convert-any-to-in/)
- [x] ~~[`convert-to-enumerate`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/convert-to-enumerate/)~~ (`SIM113`)
- [ ] [`dataframe-append-to-concat`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/dataframe-append-to-concat/)
- [x] ~~[`de-morgan`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/de-morgan/)~~ (`SIM208`)
- [x] ~~[`default-get`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/default-get/)~~ (`SIM401`)
- [x] ~~[`default-mutable-arg`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/default-mutable-arg/)~~ (`B006`)
- [ ] [`del-comprehension`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/del-comprehension/)
- [ ] [`dict-assign-update-to-union`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/dict-assign-update-to-union/)
- [ ] [`dict-comprehension`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/dict-comprehension/)
- [x] ~~[`dict-literal`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/dict-literal/)~~ (`C408`)
- [x] ~~[`do-not-use-bare-except`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/do-not-use-bare-except/)~~ (`E722`)
- [ ] [`dont-import-test-modules`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/dont-import-test-modules/)
- [x] ~~[`ensure-file-closed`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/ensure-file-closed/)~~ (`SIM115`)
- [x] ~~[`equality-identity`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/equality-identity/)~~ (`PLR0133` + `PLR0124`)
- [ ] [`extract-duplicate-method`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/extract-duplicate-method/)
- [ ] [`extract-method`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/extract-method/)
- [ ] [`flatten-nested-try`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/flatten-nested-try/)
- [x] ~~[`flip-comparison`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/flip-comparison/)~~ (`SIM300`)
- [x] ~~[`for-append-to-extend`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/for-append-to-extend/)~~ (`PERF401`)
- [ ] [`for-index-replacement`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/for-index-replacement/)
- [x] ~~[`for-index-underscore`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/for-index-underscore/)~~ (`B004`)
- [ ] [`guard`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/guard/)
- [ ] [`hoist-if-from-if`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/hoist-if-from-if/)
- [ ] [`hoist-loop-from-if`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/hoist-loop-from-if/)
- [ ] [`hoist-repeated-if-condition`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/hoist-repeated-if-condition/)
- [ ] [`hoist-similar-statement-from-if`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/hoist-similar-statement-from-if/)
- [ ] [`hoist-statement-from-if`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/hoist-statement-from-if/)
- [ ] [`hoist-statement-from-loop`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/hoist-statement-from-loop/)
- [x] ~~[`identity-comprehension`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/identity-comprehension/)~~ (`C416`)
- [x] ~~[`inline-immediately-returned-variable`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/inline-immediately-returned-variable/)~~ (`RET504`)
- [ ] [`inline-immediately-yielded-variable`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/inline-immediately-yielded-variable/)
- [ ] [`inline-variable`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/inline-variable/)
- [x] ~~[`instance-method-first-arg-name`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/instance-method-first-arg-name/)~~ (`N805`)
- [ ] [`introduce-default-else`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/introduce-default-else/)
- [ ] [`invert-any-all`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/invert-any-all/)
- [ ] [`invert-any-all-body`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/invert-any-all-body/)
- [x] ~~[`last-if-guard`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/last-if-guard/)~~ (`RET505`)
- [ ] [`lift-duplicated-conditional`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/lift-duplicated-conditional/)
- [ ] [`lift-return-into-if`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/lift-return-into-if/)
- [x] ~~[`list-comprehension`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/list-comprehension/)~~ (`PERF401`)
- [x] ~~[`list-literal`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/list-literal/)~~ (`C408`)
- [ ] [`low-code-quality`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/low-code-quality/)
- [ ] [`max-min-default`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/max-min-default/) (similar to `SIM108` but not quite the same)
- [ ] [`merge-assign-and-aug-assign`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-assign-and-aug-assign/)
- [x] ~~[`merge-comparisons`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-comparisons/)~~ (`PLR1714`)
- [ ] [`merge-dict-assign`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-dict-assign/)
- [x] ~~[`merge-duplicate-blocks`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-duplicate-blocks/)~~ (`SIM114`)
- [x] ~~[`merge-else-if-into-elif`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-else-if-into-elif/)~~ (`PLR5501`)
- [ ] [`merge-except-handler`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-except-handler/)
- [x] ~~[`merge-isinstance`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-isinstance/)~~ (`PLR1701` + `SIM101`)
- [x] ~~[`merge-list-append`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-list-append/)~~ (`FURB113`)
- [ ] [`merge-list-appends-into-extend`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-list-appends-into-extend/)
- [ ] [`merge-list-extend`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-list-extend/)
- [x] ~~[`merge-nested-ifs`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-nested-ifs/)~~ (`SIM102`)
- [ ] [`merge-repeated-ifs`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-repeated-ifs/)
- [ ] [`merge-set-add`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/merge-set-add/)
- [ ] [`method_chaining`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/method_chaining/)
- [ ] [`min-max-identity`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/min-max-identity/) (partially covered by `PLR2004`)
- [x] ~~[`missing-dict-items`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/missing-dict-items/)~~ (`PLE1141`)
- [ ] [`move-assign`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/move-assign/)
- [ ] [`move-assign-in-block`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/move-assign-in-block/)
- [ ] [`no-conditionals-in-tests`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/no-conditionals-in-tests/)
- [ ] [`no-loop-in-tests`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/no-loop-in-tests/)
- [ ] [`non-equal-comparison`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/non-equal-comparison/)
- [x] ~~[`none-compare`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/none-compare/)~~ (`E711`)
- [x] ~~[`or-if-exp-identity`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/or-if-exp-identity/)~~ (`FURB110`)
- [x] ~~[`pandas-avoid-inplace`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/pandas-avoid-inplace/)~~ (`PD002`)
- [x] ~~[`path-read`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/path-read/)~~ (`FURB101`)
- [x] ~~[`raise-from-previous-error`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/raise-from-previous-error/)~~ (`B904`)
- [x] ~~[`raise-specific-error`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/raise-specific-error/)~~ (`TRY002`)
- [ ] [`reintroduce-else`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/reintroduce-else/)
- [ ] [`remove-assert-true`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-assert-true/)
- [x] ~~[`remove-dict-items`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-dict-items/)~~ (`PERF102` + `SIM118`)
- [x] ~~[`remove-dict-keys`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-dict-keys/)~~ (`SIM118`)
- [ ] [`remove-duplicate-dict-key`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-duplicate-dict-key/)
- [x] ~~[`remove-duplicate-set-key`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-duplicate-set-key/)~~ (`B033`)
- [ ] [`remove-empty-nested-block`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-empty-nested-block/)
- [x] ~~[`remove-none-from-default-get`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-none-from-default-get/)~~ (`SIM910`)
- [ ] [`remove-pass-body`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-pass-body/)
- [ ] [`remove-pass-elif`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-pass-elif/)
- [ ] [`remove-redundant-boolean`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-boolean/)
- [ ] [`remove-redundant-condition`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-condition/)
- [ ] [`remove-redundant-constructor-in-dict-union`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-constructor-in-dict-union/)
- [ ] [`remove-redundant-continue`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-continue/)
- [ ] [`remove-redundant-except-handler`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-except-handler/)
- [ ] [`remove-redundant-exception`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-exception/)
- [x] ~~[`remove-redundant-fstring`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-fstring/)~~ (`F541`)
- [ ] [`remove-redundant-if`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-if/)
- [ ] [`remove-redundant-pass`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-pass/)
- [ ] [`remove-redundant-path-exists`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-path-exists/)
- [ ] [`remove-redundant-slice-index`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-redundant-slice-index/)
- [x] ~~[`remove-str-from-fstring`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-str-from-fstring/)~~ (`RUF010`)
- [ ] [`remove-str-from-print`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-str-from-print/)
- [ ] [`remove-unit-step-from-range`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-unit-step-from-range/)
- [ ] [`remove-unnecessary-cast`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-unnecessary-cast/)
- [x] ~~[`remove-unnecessary-else`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-unnecessary-else/)~~ (`RET505`)
- [ ] [`remove-unreachable-code`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-unreachable-code/)
- [ ] [`remove-unused-enumerate`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-unused-enumerate/)
- [x] ~~[`remove-zero-from-range`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/remove-zero-from-range/)~~ (`PIE808`)
- [ ] [`replace-apply-with-method-call`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/replace-apply-with-method-call/)
- [ ] [`replace-apply-with-numpy-operation`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/replace-apply-with-numpy-operation/)
- [x] ~~[`replace-dict-items-with-values`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/replace-dict-items-with-values/)~~ (`PERF102`)
- [x] ~~[`replace-interpolation-with-fstring`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/replace-interpolation-with-fstring/)~~ (`UP031`)
- [x] ~~[`return-or-yield-outside-function`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/return-or-yield-outside-function/)~~ (`F704` + `F706`)
- [x] ~~[`set-comprehension`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/set-comprehension/)~~ (sort-of implemented with `FURB142`)
- [x] ~~[`simplify-boolean-comparison`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-boolean-comparison/)~~ (`E712`)
- [ ] [`simplify-constant-sum`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-constant-sum/)
- [ ] [`simplify-dictionary-update`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-dictionary-update/)
- [ ] [`simplify-division`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-division/)
- [ ] [`simplify-empty-collection-comparison`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-empty-collection-comparison/) (partially covered by `PLC1901`)
- [ ] [`simplify-fstring-formatting`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-fstring-formatting/)
- [ ] [`simplify-generator`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-generator/)
- [ ] [`simplify-len-comparison`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-len-comparison/)
- [ ] [`simplify-negative-index`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-negative-index/)
- [ ] [`simplify-numeric-comparison`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-numeric-comparison/)
- [x] ~~[`simplify-single-exception-tuple`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-single-exception-tuple/)~~ (`B013`)
- [ ] [`simplify-str-len-comparison`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-str-len-comparison/)
- [ ] [`simplify-substring-search`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/simplify-substring-search/)
- [ ] [`skip-sorted-list-construction`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/skip-sorted-list-construction/)
- [ ] [`split-or-ifs`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/split-or-ifs/)
- [ ] [`square-identity`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/square-identity/)
- [ ] [`str-prefix-suffix`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/str-prefix-suffix/)
- [ ] [`sum-comprehension`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/sum-comprehension/)
- [ ] [`swap-if-else-branches`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/swap-if-else-branches/)
- [ ] [`swap-if-expression`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/swap-if-expression/)
- [ ] [`swap-nested-ifs`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/swap-nested-ifs/)
- [ ] [`swap-variable`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/swap-variable/)
- [x] ~~[`switch`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/switch/)~~ (`SIM114`)
- [x] ~~[`ternary-to-if-expression`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/ternary-to-if-expression/)~~ (`RUF021`)
- [x] ~~[`tuple-literal`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/tuple-literal/)~~ (`C408`)
- [x] ~~[`unwrap-iterable-construction`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/unwrap-iterable-construction/)~~ (`C409`)
- [ ] [`use-any`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-any/)
- [ ] [`use-assigned-variable`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-assigned-variable/)
- [x] ~~[`use-contextlib-suppress`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-contextlib-suppress/)~~ (`SIM105`)
- [ ] [`use-count`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-count/)
- [x] ~~[`use-datetime-now-not-today`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-datetime-now-not-today/)~~ (`DT002`)
- [ ] [`use-dict-items`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-dict-items/)
- [ ] [`use-dictionary-union`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-dictionary-union/)
- [x] ~~[`use-file-iterator`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-file-iterator/)~~ (`FURB129`)
- [ ] [`use-fstring-for-concatenation`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-fstring-for-concatenation/)
- [x] ~~[`use-fstring-for-formatting`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-fstring-for-formatting/)~~ (`UP032`)
- [ ] [`use-getitem-for-re-match-groups`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-getitem-for-re-match-groups/)
- [ ] [`use-isna`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-isna/)
- [ ] [`use-itertools-product`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-itertools-product/)
- [ ] [`use-join`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-join/)
- [ ] [`use-len`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-len/)
- [ ] [`use-named-expression`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-named-expression/)
- [ ] [`use-next`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-next/)
- [ ] [`use-or-for-fallback`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-or-for-fallback/)
- [ ] [`use-string-remove-affix`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-string-remove-affix/)
- [ ] [`use_iloc`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use_iloc/)
- [x] ~~[`useless-else-on-loop`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/useless-else-on-loop/)~~ (`PLW0120`)
- [ ] [`while-guard-to-condition`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/while-guard-to-condition/)
- [ ] [`while-to-for`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/while-to-for/)
- [x] ~~[`yield-from`](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/yield-from/)~~ (`UP028`)

---

_Comment by @MithicSpirit on 2024-04-20 23:33_

Do we know what license the Sourcery documentation is distributed under? There might be legal issues with basing implementations on it if it is sufficiently restrictive. Although, it might be fine regardless due to [*Google v. Oracle*](https://en.wikipedia.org/wiki/Google_LLC_v._Oracle_America,_Inc.). I am not a lawyer; this is not legal advice.

---

_Comment by @diceroll123 on 2024-04-20 23:41_

Also not a lawyer, but my intention here is the (surprisingly not the software development methodology of) [clean room design](https://en.wikipedia.org/wiki/Clean_room_design).

After all, this issue's a proposal, and I would say nobody has the copyright over an idea of a lint rule, especially if we can't see the source to replicate it.

In any case, I leave it up to the team to decide!

---

_Comment by @rogelho-junior on 2024-10-29 18:23_

Hello @diceroll123 is this list updated with Ruff 0.7.x new rules? Asking that just to know if we need to check, thanks

---

_Comment by @diceroll123 on 2024-10-29 22:47_

@rogelho-junior-byx I have not once updated the list since making this issue, would like to hear back from Astral folks before anyone spends time on it tbh 🫠 

---

_Label `plugin` added by @dhruvmanila on 2024-10-30 03:19_

---

_Label `needs-decision` added by @dhruvmanila on 2024-10-30 03:19_

---

_Referenced in [astral-sh/ruff#19518](../../astral-sh/ruff/pulls/19518.md) on 2025-07-24 00:59_

---

_Comment by @Avasam on 2025-11-26 06:45_

Since this already mentions a possible rule for using itertools.product rather than nested loops, I won't create a new issue.

I am interested in something like [use-itertools-product](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-itertools-product/) after seeing a PR with 6 nested loops.

I'd like to see:
```py
for k in range(4):
    for j in range(5):
        for i in range(3):
            rec = array_of_structs[k][j][i]
            for n in range(4):
                for m in range(5):
                    for l in range(3):
                        f = float_tuple[n][m][l]
                        assert rec.array_of_double[n][m][l] == f * rec.id
```

rewritten as

```py
from itertools import product

for k, j, i in product(range(4), range(5), range(3)):
    rec = array_of_structs[k][j][i]
    for n, m, l in product(range(4), range(5), range(3)):
        f = float_tuple[n][m][l]
        assert rec.array_of_double[n][m][l] == f * rec.id
```

---

---
number: 970
title: Implement Pylint
type: issue
state: open
author: charliermarsh
labels:
  - plugin
assignees: []
created_at: 2022-11-30T04:52:53Z
updated_at: 2025-12-01T20:28:22Z
url: https://github.com/astral-sh/ruff/issues/970
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement Pylint

---

_Issue opened by @charliermarsh on 2022-11-30 04:52_

This is the parent issue for tracking parity with Pylint. Below, we've enumerated all Pylint rules.

Rules that are checked off have been implemented in Ruff already (either as a Pylint rule, e.g., `PLE0237`, or by way of overlap with another linter like Pyflakes, as with `missing-format-string-key`).

Rules that are crossed out have been removed some consideration. (In most cases, crossed-out rules represent Pylint specific rules, e.g., rules related to Pylint configuration.)

At time of writing, many of the remaining rules require type inference and/or multi-file analysis, and aren't ready to be implemented in Ruff. (See: https://github.com/astral-sh/ruff/issues/970#issuecomment-1565594417 for an enumeration.) If you're looking to start work on a specific rule, I'd suggest commenting in the issue, to get some input on whether Ruff is capable of supporting it at present.

For guidance on getting started, see the [Contributing](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#example-adding-a-new-lint-rule) documentation.

> *Note*: Don't implement rules that are part of Pylint extension until #1774 is completed. Don't implement rules that mainly target Python 2.

# Error

- [ ] `abstract-class-instantiated` / `E0110`
- [ ] `access-member-before-definition` / `E0203`
- [x] `assigning-non-slot` / `E0237` (`PLE0237`)
- [ ] `assignment-from-no-return` / `E1111`
- [ ] `assignment-from-none` / `E1128`
- [x] `await-outside-async` / `E1142` (`PLE1142`)
- [x] ~`bad-configuration-section` / `E0014`~
- [ ] `bad-except-order` / `E0701`
- [ ] `bad-exception-cause` / `E0705`
- [x] `bad-format-character` / `E1300` (`PLE1300`)
- [x] ~`bad-plugin-value` / `E0013`~
- [ ] `bad-reversed-sequence` / `E0111`
- [x] `bad-str-strip-call` / `E1310` (`PLE1310`)
- [x] `bad-string-format-type` / `E1307` (`PLE1307`)
- [ ] ~`bad-super-call` / `E1003`~ (mainly targets Python 2)
- [x] `bidirectional-unicode` / `E2502` (`PLE2502`)
- [x] ~`broken-collections-callable` / `E6005`~
- [x] ~`broken-noreturn` / `E6004`~
- [ ] `catching-non-exception` / `E0712`
- [ ] `class-variable-slots-conflict` / `E0242`
- [x] `continue-in-finally` / `E0116` (`PLE0116`)
- [x] `dict-iter-missing-items` / `E1141`
- [x] ~`duplicate-argument-name` / `E0108`~
- [x] `duplicate-bases` / `E0241` (`PLE0241`)
- [x] `format-needs-mapping` / `E1303` (`F502`)
- [x] `function-redefined` / `E0102` (`F811`)
- [ ] `import-error` / `E0401`
- [ ] `inconsistent-mro` / `E0240`
- [ ] `inherit-non-class` / `E0239`
- [x] `init-is-generator` / `E0100` (`PLE0100`)
- [x] `invalid-all-format` / `E0605` (`PLE0605`)
- [x] `invalid-all-object` / `E0604` (`PLE0604`)
- [x] `invalid-bool-returned` / `E0304` (`PLE0304`)
- [x] `invalid-bytes-returned` / `E0308` (`PLE0308`)
- [x] `invalid-character-backspace` / `E2510` (`PLE2510`)
- [ ] `invalid-character-carriage-return` / `E2511`
- [x] `invalid-character-esc` / `E2513` (`PLE2513`)
- [x] `invalid-character-nul` / `E2514` (`PLE2514`)
- [x] `invalid-character-sub` / `E2512` (`PLE2512`)
- [x] `invalid-character-zero-width-space` / `E2515` (`PLE2515`)
- [ ] `invalid-class-object` / `E0243`
- [ ] `invalid-enum-extension` / `E0244`
- [x] `invalid-envvar-value` / `E1507`
- [ ] `invalid-format-returned` / `E0311`
- [ ] `invalid-getnewargs-ex-returned` / `E0313`
- [ ] `invalid-getnewargs-returned` / `E0312`
- [x] `invalid-hash-returned` / `E0309` (`PLE0309`)
- [x] `invalid-index-returned` / `E0305` (`PLE0305`)
- [ ] `invalid-length-hint-returned` / `E0310`
- [x] `invalid-length-returned` / `E0303` (`PLE0303`)
- [ ] `invalid-metaclass` / `E1139`
- [ ] `invalid-repr-returned` / `E0306`
- [ ] `invalid-sequence-index` / `E1126`
- [ ] `invalid-slice-index` / `E1127`
- [ ] `invalid-slice-step` / `E1144`
- [ ] `invalid-slots` / `E0238`
- [ ] `invalid-slots-object` / `E0236`
- [ ] `invalid-star-assignment-target` / `E0113`
- [x] `invalid-str-returned` / `E0307`
- [ ] `invalid-unary-operand-type` / `E1130`
- [ ] `invalid-unicode-codec` / `E2501`
- [ ] `logging-format-truncated` / `E1201`
- [x] `logging-too-few-args` / `E1206` (`PLE1206`)
- [x] `logging-too-many-args` / `E1205` (`PLE1205`)
- [ ] `logging-unsupported-format` / `E1200`
- [ ] `method-hidden` / `E0202`
- [x] `misplaced-bare-raise` / `E0704` (`PLE0704`)
- [ ] `misplaced-format-function` / `E0119`
- [x] `missing-format-string-key` / `E1304`  (`F524`)
- [ ] `missing-kwoa` / `E1125`
- [x] `mixed-format-string` / `E1302` (`F506`)
- [ ] `modified-iterating-dict` / `E4702`
- [x] `modified-iterating-set` / `E4703` (`PLE4703`)
- [ ] `no-member` / `E1101`
- [ ] `no-method-argument` / `E0211`
- [ ] `no-name-in-module` / `E0611`
- [x] `no-self-argument` / `E0213` (`N805`)
- [ ] `no-value-for-parameter` / `E1120`
- [ ] `non-iterator-returned` / `E0301`
- [x] `nonexistent-operator` / `E0107` (`B002`)
- [x] `nonlocal-and-global` / `E0115` (`PLE0115`)
- [x] `nonlocal-without-binding` / `E0117` (`PLE0117`)
- [ ] `not-a-mapping` / `E1134`
- [ ] `not-an-iterable` / `E1133`
- [ ] `not-async-context-manager` / `E1701`
- [ ] `not-callable` / `E1102`
- [ ] `not-context-manager` / `E1129`
- [x] `not-in-loop` / `E0103` (`F701`, `F702`)
- [x] `notimplemented-raised` / `E0711` (`F901`)
- [x] `potential-index-error` / `E0643` (`PLE0643`)
- [ ] `raising-bad-type` / `E0702`
- [ ] `raising-non-exception` / `E0710`
- [ ] `redundant-keyword-arg` / `E1124`
- [x] `relative-beyond-top-level` / `E0402` (`TID252`)
- [x] `repeated-keyword` / `E1132` (`PLE1132`)
- [x] ~`return-arg-in-generator` / `E0106`~
- [x] `return-in-init` / `E0101` (`PLE0101`)
- [x] `return-outside-function` / `E0104` (`F706`)
- [x] `singledispatch-method` / `E1519` (`PLE1519`)
- [x] `singledispatchmethod-function` / `E1520` (`PLE1520`)
- [x] `star-needs-assignment-target` / `E0114` (syntax error in preview)
- [x] `syntax-error` / `E0001` (always enabled)
- [x] `too-few-format-args` / `E1306` (`F524`)
- [x] `too-many-format-args` / `E1305` (`F522`)
- [ ] `too-many-function-args` / `E1121`
- [x] `too-many-star-expressions` / `E0112` (`F622`)
- [x] `truncated-format-string` / `E1301` (`F501`)
- [x] `undefined-all-variable` / `E0603` (`F822`)
- [x] `undefined-variable` / `E0602` (`F821`)
- [ ] `unexpected-keyword-arg` / `E1123`
- [x] `unexpected-special-method-signature` / `E0302` (`PLE0302`)
- [ ] `unhashable-member` / `E1143`
- [ ] `unpacking-non-sequence` / `E0633`
- [ ] `unsubscriptable-object` / `E1136`
- [ ] `unsupported-assignment-operation` / `E1137`
- [ ] `unsupported-binary-operation` / `E1131`
- [ ] `unsupported-delete-operation` / `E1138`
- [ ] `unsupported-membership-test` / `E1135`
- [x] `used-before-assignment` / `E0601` (`F821`)
- [x] `used-prior-global-declaration` / `E0118` (`PLE0118`)
- [x] `yield-inside-async-function` / `E1700` (`PLE1700`)
- [x] `yield-outside-function` / `E0105` (`F704`)

# Warning

- [ ] `abstract-method` / `W0223`
- [x] `anomalous-backslash-in-string` / `W1401` (`W605`)
- [ ] `anomalous-unicode-escape-in-string` / `W1402`
- [ ] `arguments-differ` / `W0221`
- [ ] `arguments-out-of-order` / `W1114`
- [ ] `arguments-renamed` / `W0237`
- [x] `assert-on-string-literal` / `W0129` (`PLW0129`)
- [x] `assert-on-tuple` / `W0199` (`F631`)
- [ ] `attribute-defined-outside-init` / `W0201`
- [ ] `bad-builtin` / `W0141`
- [ ] `bad-chained-comparison` / `W3601`
- [x] `bad-dunder-name` / `W3201` (`PLW3201`)
- [x] `bad-format-string` / `W1302` (`F521`)
- [ ] `bad-format-string-key` / `W1300`
- [x] `bad-indentation` / `W0311` (`E111`)
- [x] `bad-open-mode` / `W1501` (`PLW1501`)
- [x] `bad-staticmethod-argument` / `W0211` (`PLW0211`)
- [ ] `bad-thread-instantiation` / `W1506`
- [x] `bare-except` / `W0702` (`E722`)
- [x] `binary-op-exception` / `W0711` (`PLW0711`)
- [x] ~`boolean-datetime` / `W1502`~
- [x] `broad-exception-caught` / `W0718` (`BLE001`)
- [x] `broad-exception-raised` / `W0719` (`TRY002`)
- [x] `cell-var-from-loop` / `W0640` (`B023`)
- [ ] `comparison-with-callable` / `W0143`
- [ ] `confusing-with-statement` / `W0124`
- [x] `consider-ternary-expression` / `W0160` (`SIM108`)
- [x] `dangerous-default-value` / `W0102` (`B006`)
- [ ] `deprecated-argument` / `W4903`
- [ ] `deprecated-class` / `W4904`
- [ ] `deprecated-decorator` / `W4905`
- [ ] `deprecated-method` / `W4902`
- [ ] `deprecated-module` / `W4901`
- [ ] `deprecated-typing-alias` / `W6001`
- [ ] `differing-param-doc` / `W9017`
- [ ] `differing-type-doc` / `W9018`
- [x] `duplicate-except` / `W0705` (`B014`)
- [x] `duplicate-key` / `W0109` (`F601`)
- [ ] `duplicate-string-formatting-argument` / `W1308`
- [x] `duplicate-value` / `W0130` (`B033`)
- [x] `eq-without-hash` / `W1641` (`PLW1641`)
- [x] `eval-used` / `W0123` (`S307`)
- [x] `exec-used` / `W0122` (`S102`)
- [x] `expression-not-assigned` / `W0106` (`B018`)
- [x] `f-string-without-interpolation` / `W1309` (`F541`)
- [x] `fixme` / `W0511` (`FIX001`, `FIX002`, `FIX003`, `FIX004`)
- [x] `forgotten-debug-statement` / `W1515` (`T100 `)
- [x] `format-combined-specification` / `W1305` (`F525`)
- [x] `format-string-without-interpolation` / `W1310` (`F541`)
- [x] `global-at-module-level` / `W0604` (`PLW0604`)
- [x] `global-statement` / `W0603` (`PLW0603`)
- [x] `global-variable-not-assigned` / `W0602` (`PLW0602`)
- [ ] `global-variable-undefined` / `W0601`
- [x] `implicit-str-concat` / `W1404` (`ISC001`)
- [x] `import-self` / `W0406` (`PLW0406`)
- [x] `inconsistent-quotes` / `W1405` (`Q000`)
- [x] `invalid-envvar-default` / `W1508` (`PLW1508`)
- [ ] `invalid-format-index` / `W1307`
- [ ] `invalid-overridden-method` / `W0236`
- [ ] `isinstance-second-argument-not-valid-type` / `W1116`
- [x] `keyword-arg-before-vararg` / `W1113` (`B026`)
- [x] `logging-format-interpolation` / `W1202` (`G001`)
- [x] `logging-fstring-interpolation` / `W1203` (`G004`)
- [x] `logging-not-lazy` / `W1201` (`G002`)
- [x] `lost-exception` / `W0150` (`B012`)
- [x] `method-cache-max-size-none` / `W1518` (`B019`)
- [x] `misplaced-future` / `W0410` (`F404`)
- [ ] `missing-any-param-doc` / `W9021`
- [x] `missing-format-argument-key` / `W1303` (`F524`)
- [ ] `missing-format-attribute` / `W1306`
- [ ] `missing-param-doc` / `W9015`
- [ ] `missing-parentheses-for-call-in-test` / `W0126`
- [ ] `missing-raises-doc` / `W9006`
- [ ] ~`missing-return-doc` / `W9011`~ (Pylint extension)
- [ ] `missing-return-type-doc` / `W9012`
- [ ] `missing-timeout` / `W3101`
- [ ] `missing-type-doc` / `W9016`
- [ ] `missing-yield-doc` / `W9013`
- [ ] `missing-yield-type-doc` / `W9014`
- [ ] `modified-iterating-list` / `W4701`
- [ ] `multiple-constructor-doc` / `W9005`
- [x] `named-expr-without-context` / `W0131` (`PLW0131`)
- [x] `nan-comparison` / `W0177` (`PLW0177`)
- [x] `nested-min-max` / `W3301` (`PLW3301`)
- [x] `non-ascii-file-name` / `W2402` (`N999`)
- [ ] `non-parent-init-called` / `W0233`
- [ ] `non-str-assignment-to-dunder-name` / `W1115`
- [ ] `overlapping-except` / `W0714`
- [ ] `overridden-final-method` / `W0239`
- [x] `pointless-exception-statement` / `W0133` (`PLW0133`)
- [x] `pointless-statement` / `W0104` (`B018`)
- [ ] `pointless-string-statement` / `W0105`
- [ ] `possibly-unused-variable` / `W0641`
- [ ] `preferred-module` / `W0407`
- [x] `protected-access` / `W0212` (`SLF001`)
- [x] `raise-missing-from` / `W0707` (`B904`)
- [ ] `raising-format-tuple` / `W0715`
- [ ] `redeclared-assigned-name` / `W0128`
- [x] `redefined-builtin` / `W0622` (`A001`)
- [x] `redefined-loop-name` / `W2901` (`PLW2901`)
- [ ] `redefined-outer-name` / `W0621`
- [x] `redefined-slots-in-subclass` / `W0244`
- [ ] `redundant-returns-doc` / `W9008`
- [x] `redundant-u-string-prefix` / `W1406` (`UP025`)
- [ ] `redundant-unittest-assert` / `W1503`
- [ ] `redundant-yields-doc` / `W9010`
- [x] `reimported` / `W0404` (`F811`)
- [x] `self-assigning-variable` / `W0127` (`PLW0127`)
- [x] `self-cls-assignment` / `W0642`
- [x] `shallow-copy-environ` / `W1507`
- [ ] `signature-differs` / `W0222`
- [ ] `subclassed-final-class` / `W0240`
- [x] `subprocess-popen-preexec-fn` / `W1509` (`PLW1509`)
- [x] `subprocess-run-check` / `W1510` (`PLW1510`)
- [ ] `super-init-not-called` / `W0231`
- [x] `super-without-brackets` / `W0245` (`PLW0245`)
- [ ] `too-many-try-statements` / `W0717`
- [x] `try-except-raise` / `W0706` (`TRY302`)
- [ ] `unbalanced-dict-unpacking` / `W0644`
- [ ] `unbalanced-tuple-unpacking` / `W0632`
- [ ] `undefined-loop-variable` / `W0631`
- [x] ~`unknown-option-value` / `W0012`~
- [x] `unnecessary-ellipsis` / `W2301` (`PIE790)
- [x] `unnecessary-lambda` / `W0108` (`PLW0108`)
- [x] `unnecessary-pass` / `W0107` (`PIE790`)
- [x] `unnecessary-semicolon` / `W0301` (`E703`)
- [ ] `unreachable` / `W0101`
- [x] `unspecified-encoding` / `W1514` (`PLW1514`)
- [x] `unused-argument` / `W0613` (`ARG001`)
- [x] `unused-format-string-argument` / `W1304` (`F507`)
- [x] `unused-format-string-key` / `W1301` (`F504`)
- [x] `unused-import` / `W0611` (`F401`)
- [ ] `unused-private-member` / `W0238`
- [x] `unused-variable` / `W0612` (`F841`)
- [ ] `unused-wildcard-import` / `W0614`
- [x] `useless-else-on-loop` / `W0120` (`PLW0120`)
- [ ] `useless-param-doc` / `W9019`
- [ ] `useless-parent-delegation` / `W0246`
- [ ] `useless-type-doc` / `W9020`
- [x] `useless-with-lock` / `W2101` (`PLW2101`)
- [ ] `using-constant-test` / `W0125`
- [x] ~`using-f-string-in-unsupported-version` / `W2601`~
- [ ] `using-final-decorator-in-unsupported-version` / `W2602`
- [ ] `while-used` / `W0149`
- [x] `wildcard-import` / `W0401` (`F403`)
- [ ] `wrong-exception-operation` / `W0716`

# Convention

- [x] `bad-classmethod-argument` / `C0202` (`N804)
- [x] `bad-docstring-quotes` / `C0198` (`Q002`)
- [ ] `bad-file-encoding` / `C2503`
- [ ] `bad-mcs-classmethod-argument` / `C0204`
- [ ] `bad-mcs-method-argument` / `C0203`
- [x] `compare-to-empty-string` / `C1901` (`PLC1901`)
- [ ] `compare-to-zero` / `C2001`
- [x] `consider-iterating-dictionary` / `C0201` (`SIM118`)
- [x] `consider-using-any-or-all` / `C0501` (`SIM110`, `SIM111`)
- [x] `consider-using-dict-items` / `C0206` (`PLC0206`)
- [ ] `consider-using-enumerate` / `C0200`
- [ ] `consider-using-f-string` / `C0209`
- [ ] `dict-init-mutate` / `C3401`
- [ ] `disallowed-name` / `C0104`
- [x] `docstring-first-line-empty` / `C0199` (`D210`)
- [x] `empty-docstring` / `C0112` (`D419`)
- [x] `import-outside-toplevel` / `C0415` (`PLC0415`)
- [x] `import-private-name` / `C2701` (`PLC2701`)
- [ ] `invalid-characters-in-docstring` / `C0403`
- [x] `invalid-name` / `C0103` (`N815`)
- [x] `line-too-long` / `C0301` (`E501`)
- [x] `misplaced-comparison-constant` / `C2201` (`SIM300`)
- [x] `missing-class-docstring` / `C0115` (`D101`)
- [x] `missing-final-newline` / `C0304` (`W292`)
- [x] `missing-function-docstring` / `C0116` (`D103`)
- [x] `missing-module-docstring` / `C0114` (`D100`)
- [ ] `mixed-line-endings` / `C0327`
- [x] `multiple-imports` / `C0410` (`E401`)
- [x] `multiple-statements` / `C0321` (`E701`, `E702`)
- [x] `non-ascii-module-import` / `C2403` (`PLC2403`)
- [x] `non-ascii-name` / `C2401` (`PLC2401`)
- [x] `single-string-used-for-slots` / `C0205` (`PLC0205`)
- [x] `singleton-comparison` / `C0121` (`E711 `, `E712`)
- [ ] `superfluous-parens` / `C0325`
- [ ] ~`too-many-lines` / `C0302`~ (not compatible with the formatter)
- [x] `trailing-newlines` / `C0305` (`W391`)
- [x] `trailing-whitespace` / `C0303` (`W291`)
- [x] `typevar-double-variance` / `C0131` (`PLC0131`)
- [x] `typevar-name-incorrect-variance` / `C0105` (`PLC0105`)
- [x] `typevar-name-mismatch` / `C0132` (`PLC0132`)
- [ ] `unexpected-line-ending-format` / `C0328`
- [x] `ungrouped-imports` / `C0412` (`I001`)
- [x] `unidiomatic-typecheck` / `C0123` (`E721`)
- [x] `unnecessary-direct-lambda-call` / `C3002` (`PLC3002`)
- [x] `unnecessary-dunder-call` / `C2801` (`PLC2801`)
- [x] `unnecessary-lambda-assignment` / `C3001` (`E731`)
- [x] `unneeded-not` / `C0113` (`SIM208`)
- [ ] `use-implicit-booleaness-not-comparison` / `C1803`
- [x] `use-implicit-booleaness-not-len` / `C1802` (`PLC1802`)
- [x] `use-maxsplit-arg` / `C0207` (`PLC0207`)
- [x] `use-sequence-for-iteration` / `C0208` (`PLC0208`)
- [x] `useless-import-alias` / `C0414` (`PLC0414`)
- [x] `wrong-import-order` / `C0411` (`I001`)
- [x] `wrong-import-position` / `C0413` (`E402 `)
- [ ] `wrong-spelling-in-comment` / `C0401`
- [ ] `wrong-spelling-in-docstring` / `C0402`

# Refactor

- [ ] `chained-comparison` / `R1716`
- [x] `comparison-of-constants` / `R0133` (`PLR0133`)
- [x] `comparison-with-itself` / `R0124` (`PLR0124`)
- [ ] `condition-evals-to-constant` / `R1727`
- [ ] ~`confusing-consecutive-elif` / `R5601`~
- [x] `consider-alternative-union-syntax` / `R6003` (`UP007`)
- [x] `consider-merging-isinstance` / `R1701` (`SIM101`)
- [ ] `consider-swap-variables` / `R1712`
- [x] `consider-using-alias` / `R6002` (`UP006`)
- [ ] `consider-using-assignment-expr` / `R6103`
- [x] `consider-using-augmented-assign` / `R6104` (`PLR6104`)
- [x] `consider-using-dict-comprehension` / `R1717` (`C402`)
- [x] `consider-using-from-import` / `R0402` (`PLR0402`)
- [x] `consider-using-generator` / `R1728` (`C417`)
- [x] `consider-using-get` / `R1715` (`SIM401`)
- [x] `consider-using-in` / `R1714` (`PLR1714`)
- [ ] `consider-using-join` / `R1713`
- [x] `consider-using-min-builtin` / `R1730` (`PLR1730`)
- [x] `consider-using-max-builtin` / `R1731` (`PLR1730`)
- [ ] `consider-using-namedtuple-or-dataclass` / `R6101`
- [x] `consider-using-set-comprehension` / `R1718` (`C401`)
- [x] `consider-using-sys-exit` / `R1722` (`PLR1722`)
- [x] ~`consider-using-ternary` / `R1706` (`PLR1706 `)~
- [ ] `consider-using-tuple` / `R6102`
- [x] `consider-using-with` / `R1732` (`SIM115`)
- [ ] `cyclic-import` / `R0401`
- [ ] `duplicate-code` / `R0801`
- [x] `else-if-used` / `R5501` (`PLR5501`)
- [x] `empty-comment` / `R2044` (`PLR2044`)
- [x] `inconsistent-return-statements` / `R1710` (`RET501`, `RET502`)
- [x] `literal-comparison` / `R0123` (`F632`)
- [x] `magic-value-comparison` / `R2004` (`PLR2004`)
- [x] `no-classmethod-decorator` / `R0202` (`PLR0202`)
- [x] `no-else-break` / `R1723` (`RET508`)
- [x] `no-else-continue` / `R1724` (`RET507`)
- [x] `no-else-raise` / `R1720` (`RET506`)
- [x] `no-else-return` / `R1705` (`RET505`)
- [x] `no-self-use` / `R6301` (`PLR6301`)
- [x] `no-staticmethod-decorator` / `R0203` (`PLR0203`)
- [x] `property-with-parameters` / `R0206` (`PLR0206`)
- [x] `redefined-argument-from-local` / `R1704` (`PLR1704`)
- [ ] `redefined-variable-type` / `R0204`
- [ ] `simplifiable-condition` / `R1726`
- [x] `simplifiable-if-expression` / `R1719` (`SIM210`, `SIM211`)
- [x] `simplifiable-if-statement` / `R1703` (`SIM108`)
- [ ] `simplify-boolean-expression` / `R1709`
- [x] `stop-iteration-return` / `R1708`
- [x] `super-with-arguments` / `R1725` (`UP008`)
- [x] `too-complex` / `R1260` (`C901`)
- [ ] `too-few-public-methods` / `R0903`
- [ ] `too-many-ancestors` / `R0901` (requires multifile analysis)
- [x] `too-many-arguments` / `R0913` (`PLR0913`)
- [x] `too-many-boolean-expressions` / `R0916` (`PLR0916`)
- [x] `too-many-positional-arguments` /  `R0917` (`PLR0917`)
- [x] `too-many-branches` / `R0912` (`PLR0912`)
- [ ] ~`too-many-instance-attributes` / `R0902`~ (requires #1774)
- [x] `too-many-locals` / `R0914` (`PLR0914`)
- [x] `too-many-nested-blocks` / `R1702` (`PLR1702`)
- [x] `too-many-public-methods` / `R0904` (`PLR0904`)
- [x] `too-many-return-statements` / `R0911` (`PLR0911`)
- [x] `too-many-statements` / `R0915` (`PLR0915`)
- [x] `trailing-comma-tuple` / `R1707` (`COM818`)
- [x] `unnecessary-comprehension` / `R1721` (`C416`)
- [x] `use-a-generator` / `R1729` (`C419`)
- [x] `unnecessary-dict-index-lookup` / `R1733` (`PLR1733`)
- [x] `use-list-literal` / `R1734` (`C405`)
- [x] `use-dict-literal` / `R1735` (`C406`)
- [x] `unnecessary-list-index-lookup` / `R1736` (`PLR1736`)
- [x] `use-yield-from` / `R1737` (`UP028`)
- [x] `use-set-for-membership` / `R6201` (`PLR6201`)
- [x] `useless-object-inheritance` / `R0205` (`UP004`)
- [x] ~`useless-option-value` / `R0022`~
- [x] `useless-return` / `R1711` (`PLR1711`)

---

_Label `rule` added by @charliermarsh on 2022-11-30 04:52_

---

_Referenced in [astral-sh/ruff#689](../../astral-sh/ruff/issues/689.md) on 2022-11-30 04:53_

---

_Comment by @charliermarsh on 2022-11-30 05:26_

Need to figure out what the right "check code" approach is here. We could just start to put these under `RUF`. Or we could do `PYE` for "Pylint Error", and so on. Or we could use `PYL` for all of them. In either case, though, we have to allow four-digit codes, as opposed to our current three-digit codes.


---

_Comment by @JonathanPlasse on 2022-11-30 06:28_

I vote for `PYE` as it would keep the same categories and the migration would be easier.

---

_Referenced in [astral-sh/ruff#972](../../astral-sh/ruff/pulls/972.md) on 2022-11-30 15:16_

---

_Comment by @harupy on 2022-12-01 03:49_

I'll work on `unneeded-not` this weekend.

---

_Comment by @relsunkaev on 2022-12-01 07:27_

I would suggest `PL` as the pylint prefix (`PLE` -> `P`y`L`int `E`rror). I think `PYE` might be a bit ambiguous with `PY`flakes, `PY`docstyle, etc.

---

_Comment by @MartinBernstorff on 2022-12-01 09:33_

Would absolutely love to see this happen. I know flake8 intentionally decided not to implement e.g. `unexpected-keyword-arg` because it required looking outside of the current file for imported functions. 

I expect you've already considered this, and really hope this won't stop ruff from implementing it, but just wanted to raise it.

---

_Comment by @charliermarsh on 2022-12-01 15:22_

@relsunkaev :+1: Went with `PLE`! The first code is checked in as `PLE1142` (matching `E1142` from Pylint).

---

_Comment by @charliermarsh on 2022-12-01 15:23_

@MartinBernstorff - Definitely. The multi-file stuff will come, just not as quickly.


---

_Referenced in [astral-sh/ruff#1005](../../astral-sh/ruff/pulls/1005.md) on 2022-12-03 04:49_

---

_Referenced in [astral-sh/ruff#1008](../../astral-sh/ruff/pulls/1008.md) on 2022-12-03 06:18_

---

_Referenced in [astral-sh/ruff#1009](../../astral-sh/ruff/pulls/1009.md) on 2022-12-03 08:49_

---

_Referenced in [astral-sh/ruff#1018](../../astral-sh/ruff/pulls/1018.md) on 2022-12-04 03:35_

---

_Referenced in [astral-sh/ruff#1023](../../astral-sh/ruff/pulls/1023.md) on 2022-12-04 04:52_

---

_Referenced in [astral-sh/ruff#1025](../../astral-sh/ruff/pulls/1025.md) on 2022-12-04 07:49_

---

_Referenced in [astral-sh/ruff#1031](../../astral-sh/ruff/pulls/1031.md) on 2022-12-04 12:29_

---

_Comment by @hmc-cs-mdrissi on 2022-12-08 02:41_

Not sure if it should be separate issue, but looking at missing-function-docstring rule (C0116), pylint's version of it is different. It only requires that a function has docstring some in class hierarchy and does not require over-ridden methods to have a docstring. As an example,

```python
class Foo:
  def validate(self):
    """Doc."""
    ...

class Bar(Foo):
  def validate(self):
    ...
```

gives no errors in pylint even though second validate is missing docstring being pylint allows inheriting one. This is very useful when implementing interface and the docstring would really be same for all things that subclass.

---

_Label `rule` removed by @charliermarsh on 2022-12-31 18:18_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:18_

---

_Referenced in [astral-sh/ruff#1543](../../astral-sh/ruff/issues/1543.md) on 2023-01-02 01:26_

---

_Comment by @charliermarsh on 2023-01-05 18:53_

I'd like to get `pointless-statement`, `unnecessary-ellipsis`, and `unnecessary-pass` in.

---

_Comment by @charliermarsh on 2023-01-05 19:20_

Relevant source for defining a `pointless-statement`: https://github.com/PyCQA/pylint/blob/56121344f6c9c223d8e4477c0cdb58c46c840b4b/pylint/checkers/base/basic_checker.py#L447

---

_Comment by @thejcannon on 2023-01-12 16:08_

A while ago I set out to find all the pylint codes make redundant by type checking.

Unfortunately that code got lost to the sands of time.

You might consider skipping those as type checking tools are very sophisticated, if you can do the effort of compiling that list

---

_Comment by @thejcannon on 2023-01-12 16:14_

I've also thought very long and hard about the fact that pylint appears very sophisticated when it comes to types because it builds on `astroid` which works very hard to infer types based on code (pre-type-annotations)

Ideally ruff (or any modern linter) is type-aware to be as helpful as possible, but that requires either:
1. A Rust-implemented type checkers for Python. Short of a large company writing and maintaining this I doubt it'll keep up with the other big dogs
2. Having the type information be seeded to ruff. Aside from the overhead and protocol, this actually seems feasible.

---

_Referenced in [astral-sh/ruff#1828](../../astral-sh/ruff/pulls/1828.md) on 2023-01-12 18:48_

---

_Comment by @dae on 2023-01-12 22:32_

For what it's worth, the type awareness of pylint is the killer feature that keeps us using it in our project despite how painfully slow it is. Our first-party code is almost fully typed, and mypy is generally good at catching typing issues. We've found multiple instances where pylint caught an issue that mypy missed however - usually in third-party code with missing or incomplete type definitions, but occasionally also in our own code when mypy doesn't notice (eg due to the presence of `__getattr__`: https://github.com/python/mypy/issues/6251). 

---

_Comment by @thejcannon on 2023-01-12 22:41_

Out of curiosity, have you tried pyright? It claims to have inference support for codebases that aren't fully typed.

---

_Comment by @dae on 2023-01-12 22:49_

Mypy has a check_untyped_defs which sounds equivalent, and we use that. We currently rely on mypy's no_strict_optional which pyright has no equivalent to, so pyright is not currently usable on our codebase.

---

_Referenced in [astral-sh/ruff#1841](../../astral-sh/ruff/pulls/1841.md) on 2023-01-13 01:19_

---

_Comment by @alonme on 2023-01-21 15:54_

Started to look at `import-error / E0401`,

From what i can tell - ruff doesn't currently use any "context" about the python environment, right?
In order to add this rule  - we need to use something like `pkgutil` - which can be slow, or implement import resolution in ruff.

@charliermarsh  Is the project open to any of these options currently?

---

_Comment by @charliermarsh on 2023-01-21 18:17_

We have _some_ support for import resolution, but not enough to power `import-error` right now.

What we _do_ support is (1) a `src` list, which is used for first-party resolution, similar to `isort` and `reorder-python-imports` (so it detects module-level first-party imports, but doesn't do "full" resolution), (2) a `namespace-packages` list, for namespace packages, and (3) we do crawl the filesystem to find all `__init__.py` files to power some checks.

Does `import-error` need to be able to lookup third-party packages?


---

_Comment by @martimlobao on 2023-01-21 19:07_

> Does `import-error` need to be able to lookup third-party packages?

FWIW this is one of the biggest reasons we use pylint at all, despite obviously being much slower to do than just first party code. 

---

_Comment by @alonme on 2023-01-21 22:15_

Any plans on when and how to implement more import resolution features?

WDYT about creating an issue to gather rules that will need these capabilities?

---

_Referenced in [jupyter-server/jupyter_server#1186](../../jupyter-server/jupyter_server/issues/1186.md) on 2023-01-23 16:14_

---

_Referenced in [astral-sh/ruff#2110](../../astral-sh/ruff/issues/2110.md) on 2023-01-23 17:21_

---

_Comment by @charliermarsh on 2023-01-26 04:27_

I just went through and mapped all the checked-off rules to the relevant codes within Ruff. I'm guessing we implement more than the 86 that are checked off (at time of writing), but it's my best guess based on a single pass through the list.

---

_Referenced in [astral-sh/ruff#2186](../../astral-sh/ruff/issues/2186.md) on 2023-01-26 04:27_

---

_Referenced in [Bouni/python-luxtronik#79](../../Bouni/python-luxtronik/pulls/79.md) on 2023-01-26 09:58_

---

_Referenced in [astral-sh/ruff#2241](../../astral-sh/ruff/pulls/2241.md) on 2023-01-26 23:54_

---

_Referenced in [astral-sh/ruff#2262](../../astral-sh/ruff/issues/2262.md) on 2023-01-27 15:58_

---

_Referenced in [astral-sh/ruff#2308](../../astral-sh/ruff/pulls/2308.md) on 2023-01-28 19:06_

---

_Comment by @justinchuby on 2023-01-30 15:05_

It’d be really nice if ruff can understand the inline pylint ignores to make migration very easy. 

---

_Comment by @spaceone on 2023-01-30 15:06_

> It’d be really nice if ruff can understand the inline pylint ignores to make migration very easy.

this is covered in https://github.com/charliermarsh/ruff/issues/1203

---

_Referenced in [astral-sh/ruff#2414](../../astral-sh/ruff/issues/2414.md) on 2023-01-31 20:59_

---

_Referenced in [astral-sh/ruff#2445](../../astral-sh/ruff/pulls/2445.md) on 2023-02-02 06:29_

---

_Referenced in [astral-sh/ruff#2550](../../astral-sh/ruff/pulls/2550.md) on 2023-02-03 19:05_

---

_Referenced in [astral-sh/ruff#2564](../../astral-sh/ruff/pulls/2564.md) on 2023-02-04 11:31_

---

_Comment by @colin99d on 2023-02-04 18:39_

We can probbably mark `bad-configuration-section` as complete, it has to do with a pylint refactor

---

_Referenced in [astral-sh/ruff#2570](../../astral-sh/ruff/pulls/2570.md) on 2023-02-04 18:40_

---

_Comment by @Pierre-Sassoulas on 2023-02-04 18:45_

useless-option-value too, it's a warning for an issue in the pylint configuration.

---

_Comment by @colin99d on 2023-02-04 18:56_

`bad-plugin-value` is also for pylint plugins that cannot be loaded, which we do not need.

---

_Comment by @charliermarsh on 2023-02-04 19:29_

Thank you! Going to strike them out.

---

_Referenced in [astral-sh/ruff#2572](../../astral-sh/ruff/pulls/2572.md) on 2023-02-04 22:32_

---

_Comment by @tomecki on 2023-02-05 15:05_

What's the policy with duplicate logic being checked by multiple linters? E.g. [too-many-format-args](https://pylint.pycqa.org/en/latest/user_guide/messages/error/too-many-format-args.html) seems functionally equivalent to [F524](https://flake8.pycqa.org/en/latest/user/error-codes.html) which is already implemented:

https://github.com/charliermarsh/ruff/blob/41900316186ae65b588ec0fda64c21bfee4b9956/src/rules/pyflakes/rules/strings.rs#L191

---

_Comment by @charliermarsh on 2023-02-05 15:10_

We tend towards including the Flake8 variant over the Pylint variant. We're working on enabling rule aliasing, so that we'll be able to map multiple codes to the same underlying rule (details TBD).


---

_Referenced in [astral-sh/ruff#2589](../../astral-sh/ruff/pulls/2589.md) on 2023-02-05 20:45_

---

_Comment by @colin99d on 2023-02-07 01:28_

@charliermarsh do we have anyway of knowing the minor version of python we are on? `broken-collections-callable` should only be run on python 3.9.0 and 3.9.1, and I don't know of a way to detect only those two sub-versions.

---

_Referenced in [astral-sh/ruff#2618](../../astral-sh/ruff/pulls/2618.md) on 2023-02-07 02:29_

---

_Comment by @charliermarsh on 2023-02-07 02:36_

@colin99d - Hmm -- not really. I think we should just skip that rule.

---

_Comment by @DanielNoord on 2023-02-07 08:10_

Just my two cents, but from working on `pylint` for some time we have seen the definite need to specify the expected python version, preferably with the config option `py-version` which is slowly becoming a standard within the python ecosystem. It might be nice to support such an option as well.

---

_Referenced in [OpenBB-finance/OpenBB#4160](../../OpenBB-finance/OpenBB/pulls/4160.md) on 2023-02-07 13:44_

---

_Comment by @colin99d on 2023-02-08 00:34_

@charliermarsh, do we have any way of knowing that the name token data is a dict, particularly if the item is defined in a different file.
```
data = {'Paris': 2_165_423, 'New York City': 8_804_190, 'Tokyo': 13_988_129}
for city, population in data:  # [dict-iter-missing-items]
    print(f"{city} has population {population}.")
```

---

_Comment by @charliermarsh on 2023-02-08 03:08_

@colin99d - No, unlike Pylint we don't have that capability right now.

---

_Referenced in [astral-sh/ruff#2716](../../astral-sh/ruff/pulls/2716.md) on 2023-02-10 09:59_

---

_Comment by @AvnerCohen on 2023-02-16 06:52_

@charliermarsh Would love to understand the T-shirt size effort for `E0611` (before I take the time to dive into the code base for contribution) and how likely it is to be part of ruff.

I guess the simplest example would be:
```
from requests import aaa
```

Would pass on ruff or flake8, on pylint, i'd get:
```
E0611: No name 'aaa' in module 'requests' (no-name-in-module)
```





---

_Comment by @charliermarsh on 2023-02-16 17:53_

@AvnerCohen - I would call that a "Large". Ruff operates under a single-file model right now, so each file is analyzed in isolated (though we do some traversals upfront to infer the package hierarchy etc.). To power something like that, we need to move to a model in which we do some analysis of the available members in each module as a first pass, then enforce lint rules as a second pass. It's within scope, but it's a significant feature more-so than a single rule, if that makes sense :)


---

_Referenced in [astral-sh/ruff#2914](../../astral-sh/ruff/issues/2914.md) on 2023-02-16 17:59_

---

_Comment by @charliermarsh on 2023-02-16 17:59_

Related to #2914 -- I've posted some more commentary in there.

---

_Referenced in [astral-sh/ruff#2964](../../astral-sh/ruff/issues/2964.md) on 2023-02-16 18:19_

---

_Referenced in [seeing-things/track#264](../../seeing-things/track/issues/264.md) on 2023-02-18 06:55_

---

_Referenced in [astral-sh/ruff#3007](../../astral-sh/ruff/pulls/3007.md) on 2023-02-18 12:31_

---

_Comment by @sbrugman on 2023-02-18 12:46_

The pylint rules implemented by PyCharm: https://github.com/charliermarsh/ruff/issues/2972

---

_Referenced in [astral-sh/ruff#2972](../../astral-sh/ruff/issues/2972.md) on 2023-02-18 19:05_

---

_Comment by @matthewlloyd on 2023-02-19 19:08_

> The pylint rules implemented by PyCharm: #2972

Opened a new issue for PyCharm inspections covered and not covered by PyLint here, since #2972 may soon be closed: https://github.com/charliermarsh/ruff/issues/3040

---

_Referenced in [astral-sh/ruff#3084](../../astral-sh/ruff/pulls/3084.md) on 2023-02-21 05:02_

---

_Referenced in [astral-sh/ruff#3116](../../astral-sh/ruff/pulls/3116.md) on 2023-02-22 08:46_

---

_Referenced in [astral-sh/ruff#3227](../../astral-sh/ruff/pulls/3227.md) on 2023-02-25 22:13_

---

_Referenced in [astral-sh/ruff#3231](../../astral-sh/ruff/pulls/3231.md) on 2023-02-26 02:47_

---

_Comment by @chanman3388 on 2023-02-26 04:07_

I was looking at maybe implementing `E0203` along with some others that are of the same ilk, but I noticed that, at least in my noddy testing and reading around, we don't really seem to keep track of these neither per scope. Would the sensible thing be to push a new `Binding` onto the `bindings` member of the `Checker` in `checkers/ast.rs`? If so, what sort of `Binding`? `Binding::Binding`?

---

_Comment by @r3m0t on 2023-02-26 16:56_

I'll do `invalid-character-*`

---

_Comment by @sanmai-NL on 2023-02-27 16:09_

@charliermarsh What is the suggested approach to configure a project so as to have Ruff subsume Pylint, where both implement the same checks? Right now, we have lot of duplicate warnings.

---

_Referenced in [astral-sh/ruff#3268](../../astral-sh/ruff/pulls/3268.md) on 2023-02-28 07:45_

---

_Comment by @Kircheneer on 2023-02-28 07:57_

W0123 is an overlap of PHG001, so I guess that could be ticked off?
Same goes for W0122 with S102. 👍🏻 

---

_Referenced in [conda/conda#12445](../../conda/conda/issues/12445.md) on 2023-03-06 17:35_

---

_Comment by @mdbernard on 2023-03-07 02:04_

For convenience, this page in the Pylint documentation lists all of the Pylint rules, with links to descriptions and examples of each: https://pylint.pycqa.org/en/latest/user_guide/messages/messages_overview.html.

---

_Referenced in [ml6team/fondant#12](../../ml6team/fondant/pulls/12.md) on 2023-03-09 09:26_

---

_Referenced in [astral-sh/ruff#3449](../../astral-sh/ruff/pulls/3449.md) on 2023-03-11 17:33_

---

_Referenced in [astral-sh/ruff#3467](../../astral-sh/ruff/pulls/3467.md) on 2023-03-12 17:49_

---

_Referenced in [astral-sh/ruff#3541](../../astral-sh/ruff/pulls/3541.md) on 2023-03-15 16:39_

---

_Comment by @latonis on 2023-03-19 16:17_

@charliermarsh , I've began work on implementing  `duplicate-argument-name` / `E0108`, however, the parser throws an error when attempting to parse the test Python file. Does this mean it can be closed off as already implemented or is there something I can alter elsewhere? :) 

```
cargo run -p ruff_cli -- check crates/ruff/resources/test/fixtures/pylint/duplicate_argument_name.py --no-cache --select PLE0108 
   Compiling ruff v0.0.257 (/home/jacoblatonis/Projects/ruff/crates/ruff)
   Compiling ruff_cli v0.0.257 (/home/jacoblatonis/Projects/ruff/crates/ruff_cli)
warning: `ruff` (lib) generated 3 warnings
    Finished dev [unoptimized + debuginfo] target(s) in 21.66s
     Running `target/debug/ruff check crates/ruff/resources/test/fixtures/pylint/duplicate_argument_name.py --no-cache --select PLE0108`
error: Failed to parse crates/ruff/resources/test/fixtures/pylint/duplicate_argument_name.py: duplicate argument 'first_name' in function definition at line 1 column 39
```


---

_Referenced in [astral-sh/ruff#3610](../../astral-sh/ruff/pulls/3610.md) on 2023-03-19 18:44_

---

_Comment by @charliermarsh on 2023-03-19 19:13_

@latonis - Ah yeah, we can't support that one right now, since it's not valid Python. It's sort of debatable whether Ruff _should_ support that.

---

_Comment by @thejcannon on 2023-03-19 19:15_

Type checkers ought to flag that as invalid syntax. Or running the code under tests.

Maybe at the most ruff flags it can't operate because there's a syntax error at <location> and well there's your failure

---

_Comment by @charliermarsh on 2023-03-19 19:18_

:thumbsup: Yeah Ruff would flag that there's a syntax error in that file, and would _probably_ tell you where / what it is but I haven't tested that specific case.

---

_Referenced in [astral-sh/ruff#3243](../../astral-sh/ruff/pulls/3243.md) on 2023-03-19 22:00_

---

_Referenced in [astral-sh/ruff#3639](../../astral-sh/ruff/pulls/3639.md) on 2023-03-21 02:08_

---

_Referenced in [astral-sh/ruff#3662](../../astral-sh/ruff/pulls/3662.md) on 2023-03-22 02:52_

---

_Referenced in [osbuild/osbuild#1264](../../osbuild/osbuild/pulls/1264.md) on 2023-03-24 16:06_

---

_Referenced in [bottlesdevs/Bottles#2772](../../bottlesdevs/Bottles/pulls/2772.md) on 2023-03-25 08:44_

---

_Referenced in [EleutherAI/elk#154](../../EleutherAI/elk/pulls/154.md) on 2023-03-25 08:49_

---

_Referenced in [week-password/wlss-backend#5](../../week-password/wlss-backend/issues/5.md) on 2023-04-01 22:09_

---

_Referenced in [astral-sh/ruff#3880](../../astral-sh/ruff/pulls/3880.md) on 2023-04-05 00:26_

---

_Referenced in [rfguimaraes/template-sandbox#7](../../rfguimaraes/template-sandbox/issues/7.md) on 2023-04-07 08:34_

---

_Comment by @itamarst on 2023-04-17 13:51_

The table above suggests `ruff` implements `cell-var-from-loop`, but it doesn't show up in the documentation or on the command-line with `ruff check`. Is it implemented under a different name, or was that a mistake and it's not actually implemented?

---

_Comment by @itamarst on 2023-04-17 13:56_

Ah, `cell-var-from-loop` is equivalent to `B023`. For ease of switching for `pylint` users, then, might be handy to alias `B023` as `PLW0640`, if aliasing is supported.

---

_Comment by @rjarry on 2023-04-18 12:07_

Hey folks, Thanks a lot for this awesome project!

I have scrubbed over the list of pylint checks and it looks like (at least) `no-name-in-module` / `E0611` would require leveraging the new `ImportMap` feature.

Also, producing a proper implementation of `no-member` / `E1101` seems significantly more involved.

Are there any plans to work on these? I wish I could help but I am a bit clueless about rust...

---

_Comment by @chanman3388 on 2023-04-18 12:46_

> Hey folks, Thanks a lot for this awesome project!
> 
> I have scrubbed over the list of pylint checks and it looks like (at least) `no-name-in-module` / `E0611` would require leveraging the new `ImportMap` feature.
> 
> Also, producing a proper implementation of `no-member` / `E1101` seems significantly more involved.
> 
> Are there any plans to work on these? I wish I could help but I am a bit clueless about rust...

It's something I would like to do after finishing the work on detecting cyclic imports. The `ImportMap` only shows what a module imports, not exports, but I suppose a similar type of approach would be needed whereby after traversing a module, I guess we could have an`ExportMap` if you will of the names available.

---

_Comment by @rjarry on 2023-04-18 15:06_

Chris Chan, Apr 18, 2023 at 14:46:
> It's something I would like to do after finishing the work on detecting cyclic imports. The `ImportMap` only shows what a module imports, not exports, but I suppose a similar type of approach would be needed whereby after traversing a module, I guess we could have an `ExportMap` if you will of the names available.

Awesome.

I guess it also requires to determine where these imports should be resolved, check if there is a virtualenv, that sort of thing.

How far do you intend to go in code inspection to be able to do type checking? Do you plan on introspecting compiled extensions? How about type stubs (`.pyi`)?

Pylint relies on astroid to do such things. And I remember having to patch it to support compiled extensions: https://github.com/pylint-dev/astroid/commit/2da60a08de6f146f5dff78db3d01bee10ed375dc. Which (by the way) was easily broken a bit later on: https://github.com/pylint-dev/pylint/issues/7399.

I will gladly help if I can.

---

_Comment by @chanman3388 on 2023-04-18 15:54_

@rjarry I have to admit, I wasn't planning on doing it, I don't know what the wider plans are for ruff, I do know that ruff uses RustPython's parser, and to be honest, I don't know how t RustPython works, but if has an analogue to .pyc files, maybe we could. But it would be something I would need to learn/understand in order to do so.

---

_Comment by @youknowone on 2023-04-20 15:51_

Can RustPython make a very small interpreter enough to import names?
The import rules of python are very complicated and depending on comparably bigger library `_imp`, `_frozen_importlib` etc


---

_Comment by @hmc-cs-mdrissi on 2023-04-21 01:53_

Performance hit from using real interpreter for just imports can be pretty high especially as some python files run logic at module level (metaclasses/init_subclass/any global code). There are major libraries like tensorflow where just importing them takes several seconds.

---

_Comment by @sanmai-NL on 2023-04-21 06:29_

Can we and/or PyLint instead use CPython audit events to record imports? https://docs.python.org/3/library/audit_events.html

---

_Comment by @gandhis1 on 2023-04-21 12:42_

Linters don't run code. Any runtime construct is likely not useful here.

---

_Comment by @sanmai-NL on 2023-04-21 13:56_

Why do you think that is necessarily true? Have you read https://github.com/charliermarsh/ruff/issues/970#issuecomment-1516569335?

---

_Comment by @hmc-cs-mdrissi on 2023-04-21 17:37_

Some linters run code. Various others do not for multiple reasons. In ruff’s case one major core feature is performance. Running real imports can be very slow and for some files could immediately make ruff orders of magnitude slower. Python files are allowed to run whatever code they want at import time and some libraries do non trivial initialization when imported. It may also be mild security concern although most use cases if you lint a file you typically trust content in that file.

pyright is one static analysis tool that avoids almost all imports/runtime execution for those reasons. I think mypy is the same. Some tools do choose to accept trade offs of runtime imports and use them. Given ruff’s focus on performance I don’t think it makes sense for ruff to do them.

---

_Referenced in [astral-sh/ruff#4075](../../astral-sh/ruff/pulls/4075.md) on 2023-04-24 04:51_

---

_Comment by @dciborow on 2023-04-30 22:18_

I'm peaking at `subprocess-run-check` / `W1510`. It's not a particularly important check, but the fix is pretty simple, to just add a new parameter to the function call `check=True`. But it get's a little complicated to me if the function is formatted across more than one line.
If anyone has any suggestions of any other similar fixes where a parameter is added to a method that I could use as an example, I'd like to take a stab at this simple case.

---

_Comment by @chanman3388 on 2023-04-30 22:56_

> I'm peaking at `subprocess-run-check` / `W1510`. It's not a particularly important check, but the fix is pretty simple, to just add a new parameter to the function call `check=True`. But it get's a little complicated to me if the function is formatted across more than one line. If anyone has any suggestions of any other similar fixes where a parameter is added to a method that I could use as an example, I'd like to take a stab at this simple case.

I've not tried it, but it should be in the 'args' member of the `StmtKind::FunctionDef` struct
Another issue to consider is that a user might have imported subprocess under an alias, but you should test how pylint deals with it, or even look at the source for pylint to find out how it's done.

---

_Comment by @thejcannon on 2023-04-30 23:28_

Pylint uses astroid, so I wouldn't be surprised if it understands aliases pretty well.

That being said, don't let perfect be the enemy of good enough. It isn't something that is usually aliased, and reporting on the simple case is worth it to do now, and improve later.

I don't think @charliermarsh or any of the Astral folks have mentioned how type information (or any information not easily gleamable from an AST) will make it into ruff, so doing the easy thing is likely the first pass at any of these 

---

_Comment by @chanman3388 on 2023-04-30 23:38_

> Pylint uses astroid, so I wouldn't be surprised if it understands aliases pretty well.
> 
> That being said, don't let perfect be the enemy of good enough. It isn't something that is usually aliased, and reporting on the simple case is worth it to do now, and improve later.
> 
> I don't think @charliermarsh or any of the Astral folks have mentioned how type information (or any information not easily gleamable from an AST) will make it into ruff, so doing the easy thing is likely the first pass at any of these

This is true, but we could track those aliases considering the `Import/ImportFrom` `StmtKind` structs have that information, we already track imports per module as it is.

---

_Comment by @chanman3388 on 2023-04-30 23:56_

https://github.com/search?q=repo%3Apylint-dev%2Fpylint%20subprocess-run-check&type=code
So...pylint just does the basic check.

---

_Comment by @charliermarsh on 2023-04-30 23:56_

@chanman3388 - Thankfully we do handle alias tracking and it's largely abstracted away in end-user code. If you grep around for calls to `resolve_call_path`, this returns the absolute path for an expression, so it handles aliased imports, import-froms, etc. This is what we tend to use across Ruff to check (e.g.) "Is this expression a call to `subprocess.check_call`?" (Note that this _doesn't_ handle code like `import subprocess; foo = subprocess; foo.check_call`, we treat those as independent bindings right now; but it _does_ handle code like `import subprocess as foo; foo.check_call`.)

---

_Comment by @charliermarsh on 2023-05-01 00:01_

@dciborow - Sounds like you're mostly wondering how to construct the proper fix, even in the event that the function arguments are split over multiple lines, etc. -- is that right? We have `crates/ruff/src/autofix/actions.rs#remove_argument` which is _similar_. You could write an `add_or_replace_argument` helper with a similar structure.

In general, I think you want to find the last argument in the call, and then insert a comma (if there's no trailing comma), and then insert the argument -- unless the argument is already present in the call, in which case you just want to replace its value. This will require some mix of looking at the AST (to see if the argument is present) and looking the token stream (to see if there's a trailing comma). The method I mentioned above does some of that kind of stuff, as an example.


---

_Comment by @dciborow on 2023-05-01 15:23_

@charliermarsh , thanks!

I had been working on adding auto-fixes to our vscode-pylint plugin, (this one got more [complicated](https://github.com/microsoft/vscode-pylint/pull/323) then my easy regex fixer can handle) but was recommending to check out Ruff, and find that it runs a lot faster inside of VS Code. I also like that I can use rust to auto-fix things outside of VSCode. So, while I have never written Rust before, excited to see what I can figure out. 

---

_Comment by @tusharsadhwani on 2023-05-02 04:07_

A lot of good, useful Pylint lints require at least some type inference to be useful (even if it's as rudimentary as backtracking through assignments), and that's probably the thing that I'm interested in the most from Ruff.

If such an inference system starts building up, it might give the project momentum to finish through the pylint port. It'd make auto fixes much easier as well.

---

_Referenced in [openedx/edx-platform#32174](../../openedx/edx-platform/issues/32174.md) on 2023-05-03 17:02_

---

_Comment by @chanman3388 on 2023-05-05 00:08_

`E0102` seems to be already covered by `pyflakes F811`?

---

_Referenced in [ansible/ansible-navigator#1518](../../ansible/ansible-navigator/pulls/1518.md) on 2023-05-05 17:05_

---

_Referenced in [astral-sh/ruff#4200](../../astral-sh/ruff/pulls/4200.md) on 2023-05-06 20:33_

---

_Referenced in [astral-sh/ruff#2389](../../astral-sh/ruff/issues/2389.md) on 2023-05-07 12:24_

---

_Referenced in [zulip/zulip#25492](../../zulip/zulip/pulls/25492.md) on 2023-05-09 17:48_

---

_Comment by @animalnots on 2023-05-10 14:55_

no-value-for-parameter / `E1120` is what I expected to be implemented, any timelines on this or how I can enable it manually(within ruff)? 

Nevermind, It's not covered by Pyflakes rules so probably impossible to implement

---

_Comment by @tusharsadhwani on 2023-05-10 15:30_

it's not implemented right now. I'd assume it's fairly hard to implement, at least to the degree that pylint can detect.

---

_Referenced in [beamer-bridge/beamer#1339](../../beamer-bridge/beamer/issues/1339.md) on 2023-05-12 10:42_

---

_Comment by @Skylion007 on 2023-05-12 14:21_

Now that E1206, E1205 are implemented, seems like E1306, and E1305 would be good first issues if someone wants to take it up. Can we also tag this issue as good first issue @charliermarsh ? Lots of easy to implement rules here.

---

_Label `good first issue` added by @charliermarsh on 2023-05-12 14:25_

---

_Comment by @alonme on 2023-05-12 14:50_

i'll take a look at E1306 and E1305

---

_Comment by @charliermarsh on 2023-05-12 14:51_

Those specific rules might actually be covered by existing Pyflakes rules. I think `E1306` is the same as `F522`, and `E1305` is the same as `F524`?

---

_Comment by @alonme on 2023-05-12 14:57_

@charliermarsh yup - that looks right.
should we add anything to the code to make it clear that they also cover the pylint rules?

---

_Comment by @charliermarsh on 2023-05-12 14:57_

@alonme - For now, I'll just mark them as completed here and reference those tasks. In the future, we will mark them as aliases in the code, but there's no support for that right now.

---

_Comment by @alonme on 2023-05-12 15:42_

cool,
taking a look at `E0112`


EDIT:
oh well seems like that is also covered by `F622`

---

_Comment by @alonme on 2023-05-12 15:51_

and i am not sure if `E6004` is worth implementing as it is only relevant in python 3.7.0 and 3.7.1, and python 3.7 will be EOL in a month or so

---

_Comment by @charliermarsh on 2023-05-12 16:18_

Calling out a few that look like good-first-rules: `E1003`, `E0241`, `W0130`, `W0131`.

---

_Referenced in [oxsecurity/megalinter#1665](../../oxsecurity/megalinter/issues/1665.md) on 2023-05-13 09:44_

---

_Referenced in [astral-sh/ruff#4411](../../astral-sh/ruff/pulls/4411.md) on 2023-05-13 12:51_

---

_Referenced in [astral-sh/ruff#827](../../astral-sh/ruff/issues/827.md) on 2023-05-13 23:54_

---

_Referenced in [astral-sh/ruff#4459](../../astral-sh/ruff/pulls/4459.md) on 2023-05-17 00:25_

---

_Comment by @nunokaeru on 2023-05-17 00:26_

Unfortunately `E1003` is not as easy as it looked due to classes inheriting others.

So `PLE1003` will be on-hold until we get a way to track down the inherited parents.

---

_Comment by @tusharsadhwani on 2023-05-17 07:17_

@nm-remarkable Hey!

Can you maybe create a separate issue/discussion about all the pylint lints that seem hard to implement? I can collaborate and help create a knowledge base for other collaborators.

Or, we can talk over text or email if that suits better.

---

_Comment by @hoel-bagard on 2023-05-18 04:55_

I'll take a look at W0130 and W0131.

---

_Referenced in [astral-sh/ruff#4515](../../astral-sh/ruff/pulls/4515.md) on 2023-05-19 03:30_

---

_Referenced in [astral-sh/ruff#4531](../../astral-sh/ruff/pulls/4531.md) on 2023-05-19 15:23_

---

_Comment by @Skylion007 on 2023-05-19 15:28_

W0706 is already implemented as TRY302.

---

_Comment by @ghost on 2023-05-19 16:24_

Hey folks, I'm interested in taking on so of this work. I have a rough draft for PLE1128 already, how would I go about pushing that up for review?

---

_Comment by @tusharsadhwani on 2023-05-19 16:26_

@wiwa5606 creating a fork of this repository on your own account and then making a pull request should be good

---

_Comment by @chanman3388 on 2023-05-19 16:27_

> Hey folks, I'm interested in taking on so of this work. I have a rough draft for PLE1128 already, how would I go about pushing that up for review?

If you have it in a branch on your fork, then GitHub should give you the option to create a pull request. Normally you would do this from your fork. You can take a look at the merged issues higher up for what these look like. I hope that answers your question!

---

_Comment by @ghost on 2023-05-19 16:35_

Thanks, that helps! Raised https://github.com/charliermarsh/ruff/pull/4532

---

_Comment by @Skylion007 on 2023-05-19 17:48_

`W0707` is an alias for `TRY200`

---

_Comment by @Skylion007 on 2023-05-19 17:52_

`W1201`is also implemented as combination of several rules logging-format G

---

_Comment by @nunokaeru on 2023-05-19 19:54_

I am looking into `E1111`

---

_Comment by @DavideCanton on 2023-05-22 15:12_

I would really need W1203, in the next days maybe I can start working at it

---

_Comment by @Skylion007 on 2023-05-22 15:38_

@DavideCanton Isn't W1203 already implemented as one of the logging-format rules G?

---

_Comment by @DavideCanton on 2023-05-22 15:43_

@Skylion007 oh, I didn't know there was a flake8 plugin for that too :/ thanks for your help!


---

_Referenced in [astral-sh/ruff#4652](../../astral-sh/ruff/pulls/4652.md) on 2023-05-25 09:32_

---

_Comment by @antonagestam on 2023-05-27 13:13_

Some notes about various listed codes:

- `return-arg-in-generator`/`E0106` probably should not be implemented as it only applies to dead versions of Python (3.3 and earlier).
- `unknown-option-value` / `W0012` probably shouldn't be implemented either, as it's specific to pylint error codes.
- `using-f-string-in-unsupported-version` / `W2601` probably shouldn't be implemented as it only applies to dead versions of Python (3.5 and earlier).
- `boolean-datetime` / `W1502` also only applies to dead versions of Python (3.4 and earlier).
- `bad-indentation` / `W0311` is implemented by `E111` and the pylint example gives a true positive with this configuration:
  ```
  [tool.ruff]
  select = ["E111"]
  ```
- `compare-to-empty-string` / `C1901` has been renamed to `use-implicit-booleaness-not-comparison-to-string` / `C1804`.
- `compare-to-zero` / `C2001` has been renamed to `use-implicit-booleaness-not-comparison-to-zero` / `C1805`.
- `consider-using-f-string` / `C0209` is implemented by `UP031` + `UP032`.

(I combined the other comments into this one, sorry for noise).

---

_Comment by @antonagestam on 2023-05-27 13:39_

Note: `bad-indentation` / `W0311` is implemented by `E111` and the pylint example gives a true positive with this configuration:

```
[tool.ruff]
select = ["E111"]
```

---

_Comment by @antonagestam on 2023-05-27 13:44_

Note: `boolean-datetime` / `W1502` also only applies to dead versions of Python (3.4 and earlier).

---

_Comment by @antonagestam on 2023-05-27 15:51_

Note: `unknown-option-value` / `W0012` probably shouldn't be implemented either, as it's specific to pylint error codes.

---

_Comment by @antonagestam on 2023-05-27 16:55_

I've went through and evaluated all the rules listed here that aren't yet implemented (see the previous comment for some other findings), to try and find out which rules are redundant when using mypy. Mostly this was to find out what value Pylint brings that isn't already covered by the combination of ruff + mypy, but I think this could also be useful to ruff developers when prioritizing which of the rules listed to work on.

There was one rule that is partially implemented in mypy, because Pylint seems to have its own semantics for what to consider an abstract method:

- `abstract-method`

I found these rules to have an equivalent error in mypy:

- `abstract-class-instantiated`
- `arguments-differ`
- `assigning-non-slot`
- `assignment-from-no-return`
- `assignment-from-none`
- `bad-exception-cause`
- `bad-format-character`
- `bad-reversed-sequence`
- `bad-super-call`
- `bad-thread-instantiation`
- `catching-non-exception`
- `comparison-with-callable`
- `deprecated-class`
- `dict-iter-missing-items`
- `format-combined-specification`
- `global-variable-undefined`
- `import-error`
- `inconsistent-mro`
- `inherit-non-class`
- `init-is-generator`
- `invalid-class-object`
- `invalid-enum-extension`
- `invalid-envvar-value`
- `invalid-format-returned`
- `invalid-hash-returned`
- `invalid-metaclass`
- `invalid-overridden-method`
- `invalid-repr-returned`
- `invalid-sequence-index`
- `invalid-slice-index`
- `invalid-slots-object`
- `invalid-slots`
- `invalid-star-assignment-target`
- `invalid-str-returned`
- `invalid-unary-operand-type`
- `invalid-unicode-codec`
- `isinstance-second-argument-not-valid-type`
- `method-hidden`
- `misplaced-format-function`
- `missing-format-argument-key`
- `missing-format-attribute`
- `missing-kwoa`
- `missing-type-doc`
- `missing-yield-type-doc`
- `no-member`
- `no-value-for-parameter`
- `non-iterator-returned`
- `non-str-assignment-to-dunder-name`
- `nonlocal-and-global`
- `not-a-mapping`
- `not-an-iterable`
- `not-async-context-manager`
- `not-callable`
- `not-context-manager`
- `overridden-final-method`
- `raising-bad-type`
- `raising-non-exception`
- `redefined-variable-type`
- `redundant-keyword-arg`
- `relative-beyond-top-level`
- `self-cls-assignment`
- `signature-differs`
- `star-needs-assignment-target`
- `subclassed-final-class`
- `super-without-brackets`
- `too-many-function-args`
- `typevar-double-variance`
- `typevar-name-mismatch`
- `unbalanced-dict-unpacking`
- `unbalanced-tuple-unpacking`
- `unexpected-keyword-arg`
- `unhashable-member`
- `unpacking-non-sequence`
- `unsubscriptable-object`
- `unsupported-assignment-operation`
- `unsupported-binary-operation`
- `unsupported-delete-operation`
- `unsupported-membership-test`
- `used-before-assignment`
- `using-final-decorator-in-unsupported-version`
- `wrong-exception-operation`

I didn't evaluate these rules:

- `deprecated-argument`
- `invalid-character-carriage-return`
- `invalid-characters-in-docstring`

And I found these shouldn't be implemented for various reasons, see previous comment:

- `boolean-datetime`
- `return-arg-in-generator`
- `unknown-option-value`
- `using-f-string-in-unsupported-version`

I hope this is useful. Here's a repository with my categorizations, should someone be interested in the details: https://github.com/antonagestam/pylint-mypy-overlap

---

_Referenced in [pylint-dev/pylint#8579](../../pylint-dev/pylint/issues/8579.md) on 2023-05-27 17:21_

---

_Comment by @Skylion007 on 2023-05-28 14:45_

This PR also implements one of the rules for FixMe comments (W0511): https://github.com/charliermarsh/ruff/pull/4681

---

_Referenced in [astral-sh/ruff#4706](../../astral-sh/ruff/pulls/4706.md) on 2023-05-29 15:33_

---

_Comment by @hoel-bagard on 2023-05-29 16:35_

I'm looking into `unnecessary-ellipsis` ~~and `unnecessary-pass`~~.

Edit: `unnecessary-ellipsis` is partially covered by [ellipsis-in-non-empty-class-body](https://beta.ruff.rs/docs//rules/ellipsis-in-non-empty-class-body/)

---

_Comment by @tusharsadhwani on 2023-05-29 16:48_

@hoel-bagard ~those would most probably already be detected by flake8 lints.~

EDIT: seems not! My bad.

---

_Referenced in [Ulauncher/Ulauncher#1231](../../Ulauncher/Ulauncher/pulls/1231.md) on 2023-05-29 20:22_

---

_Comment by @pespinel on 2023-05-30 06:49_

> I'm looking into `unnecessary-ellipsis` and `unnecessary-pass`.

`unnecessary-pass`is detected with Flake8-pie (PIE):


PIE790 	[unnecessary-pass](https://beta.ruff.rs/docs/rules/unnecessary-pass/) 	Unnecessary pass statement

---

_Referenced in [astral-sh/ruff#4726](../../astral-sh/ruff/pulls/4726.md) on 2023-05-30 15:17_

---

_Referenced in [bdaiinstitute/spot_ros2#67](../../bdaiinstitute/spot_ros2/issues/67.md) on 2023-05-31 00:03_

---

_Comment by @Skylion007 on 2023-06-05 20:48_

W0130 is now also flake8-bugbear B033 as flake8-bugbear has implemented it too in their most recent release.

---

_Comment by @charliermarsh on 2023-06-05 21:04_

Oh nice. I will move it to bugbear and alias it.

---

_Referenced in [astral-sh/ruff#4957](../../astral-sh/ruff/pulls/4957.md) on 2023-06-08 11:21_

---

_Referenced in [home-assistant/core#94359](../../home-assistant/core/pulls/94359.md) on 2023-06-09 14:20_

---

_Comment by @Ryang20718 on 2023-06-10 05:12_

general question regarding direction of ruff: it seems a lot of these pylint errors are related to typechecking which may overlap with a type checker such as mypy or pyre. Wondering if Ruff plans to become a type checker or intend to stay a linter? (Mainly asking as pyre is rather slow and I fear AST type checking may also slow ruff down in the long run?)

---

_Comment by @Pierre-Sassoulas on 2023-06-10 06:46_

pylint is doing some limited type checking for historical reason (as it started 20 years ago when no type checkers were available). We're not implementing type checking in 2023 because mypy and pyright happened and we would not re-implement those checks again if we had too. I think they would bring no value if they were implemented in ruff -- and we're also considering removing them from pylint. But unfortunately I don't have an exhaustive list of them to give, I always use the same [``invalid-envvar-value``]( https://pylint.readthedocs.io/en/stable/user_guide/messages/error/invalid-envvar-value.html) as an example :)

---

_Comment by @hmc-cs-mdrissi on 2023-06-10 07:03_

Aww, I find those rules quite valuable for untyped/partially typed codebase. While I'd love to have all codebases be fully typed code I work on often was started before typing and is in partial state of typed. Or libraries we import are untyped and pylint can still type issues that type checker can't. I think python ecosystem for top 100ish libraries is like 30-40% typed today. Still long way to go there and I'm unsure less popular libraries are more/less typed.

If your codebase is fully typed then I would agree that mypy/pyright/etc are generally better choice then type lint rules in non type checker.

I'm mostly thinking of function argument/attribute defined/no-member rules though. Never seen invalid-envvar-value rule.

---

_Comment by @Pierre-Sassoulas on 2023-06-10 19:14_

Don't worry we're not throwing inference away any time soon :) Pylint checks like ``invalid-envvar-value`` though are checking the type of a particular argument of a particular function in the stdlib (i.e. it's already typed for everyone). mypy detect it generically at no cost for them and ruff would also detect it if if it become a type checker.

---

_Comment by @tjkuson on 2023-06-14 10:50_

`W0150` is implemented as `jump-statement-in-finally` (`B012`).

---

_Comment by @neilsh on 2023-06-16 17:33_

> We're not implementing type checking in 2023 because mypy and pyright happened and we would not re-implement those checks again if we had too. I think they would bring no value if they were implemented in ruff -- and we're also considering removing them from pylint.

It might make sense to move the rules which require type-checking to a separate section.

---

_Comment by @charliermarsh on 2023-06-16 17:54_

Yeah, I need to do a pass over this issue to make it more useful / actionable based on @antonagestam's excellent analysis.

---

_Comment by @tusharsadhwani on 2023-06-16 18:42_

@charliermarsh while pylint and mypy lints may have significant overlap, do note that mypy can only effectively catch these problems if the given code is properly type hinted. pylint however works with zero type hints.

Using both mypy and ruff for example, can probably cover the vast majority of pylint issues, but that would only work for users that have fully type checked code.

---

_Comment by @Igetin on 2023-06-16 22:25_

Mypy does infer types when the code isn’t type annotated, although not to the extent where it would output warnings like Pylint does without them.

I think that problem can be alleviated by running Mypy in [strict mode](https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-strict), and probably [disallowing dynamic typing](https://mypy.readthedocs.io/en/stable/command_line.html#disallow-dynamic-typing).

---

_Comment by @tusharsadhwani on 2023-06-17 04:20_

@Igetin The problem here is that any variables coming from function parameters will never be inferred or checked, for example. all mypy will say is that "the type is not known". mypy without type hints is really not that useful.

---

_Referenced in [astral-sh/ruff#5180](../../astral-sh/ruff/pulls/5180.md) on 2023-06-19 14:17_

---

_Comment by @tjkuson on 2023-06-19 15:59_

Rules

- `no-else-break` / `R1723`,
- `no-else-continue` / `R1724`,
- `no-else-raise` / `R1720`,
- and `no-else-return` / `R1705`

are all implemented as `RET508`, `RET507`, `RET506`, `RET505` respectively.

---

_Referenced in [astral-sh/ruff#5193](../../astral-sh/ruff/pulls/5193.md) on 2023-06-19 19:47_

---

_Referenced in [astral-sh/ruff#5399](../../astral-sh/ruff/pulls/5399.md) on 2023-06-27 17:27_

---

_Referenced in [astral-sh/ruff#5417](../../astral-sh/ruff/pulls/5417.md) on 2023-06-28 11:52_

---

_Referenced in [astral-sh/ruff#5501](../../astral-sh/ruff/pulls/5501.md) on 2023-07-04 13:40_

---

_Referenced in [cockpit-project/cockpit#19065](../../cockpit-project/cockpit/pulls/19065.md) on 2023-07-04 15:59_

---

_Referenced in [astral-sh/ruff#5517](../../astral-sh/ruff/pulls/5517.md) on 2023-07-04 23:18_

---

_Referenced in [astral-sh/ruff#5651](../../astral-sh/ruff/pulls/5651.md) on 2023-07-10 14:25_

---

_Referenced in [astral-sh/ruff#5685](../../astral-sh/ruff/pulls/5685.md) on 2023-07-11 12:42_

---

_Comment by @tjkuson on 2023-07-13 12:59_

[`broad-exception-raised` (`W0719`)](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/broad-exception-raised.html) has been implemented as [`raise-vanilla-class` (`TRY002`)](https://beta.ruff.rs/docs/rules/raise-vanilla-class/)

---

_Referenced in [delvtech/agent0#500](../../delvtech/agent0/pulls/500.md) on 2023-07-18 22:23_

---

_Referenced in [astral-sh/ruff#5900](../../astral-sh/ruff/pulls/5900.md) on 2023-07-19 20:45_

---

_Referenced in [astral-sh/ruff#5920](../../astral-sh/ruff/pulls/5920.md) on 2023-07-20 13:49_

---

_Referenced in [adfinis/timedctl#15](../../adfinis/timedctl/issues/15.md) on 2023-07-21 06:46_

---

_Referenced in [astral-sh/ruff#5955](../../astral-sh/ruff/pulls/5955.md) on 2023-07-21 21:15_

---

_Referenced in [astral-sh/ruff#5978](../../astral-sh/ruff/pulls/5978.md) on 2023-07-22 13:25_

---

_Referenced in [astral-sh/ruff#6015](../../astral-sh/ruff/pulls/6015.md) on 2023-07-23 12:05_

---

_Referenced in [astral-sh/ruff#6171](../../astral-sh/ruff/pulls/6171.md) on 2023-07-29 12:34_

---

_Comment by @LaBatata101 on 2023-08-05 01:57_

I'm implementing `PLE0243`

Edit: This rule cannot be implemented at the moment due to ruff not supporting a specific type of static analysis.

---

_Comment by @TomerBin on 2023-08-05 09:47_

It looks like W0107 is already implemented (derived from the flake8-pie linter). Can we check that one?

---

_Referenced in [astral-sh/ruff#6428](../../astral-sh/ruff/issues/6428.md) on 2023-08-08 15:41_

---

_Comment by @LaBatata101 on 2023-08-09 21:17_

I'm implementing `PLW3201`

---

_Referenced in [astral-sh/ruff#6486](../../astral-sh/ruff/pulls/6486.md) on 2023-08-10 21:20_

---

_Referenced in [astral-sh/ruff#6487](../../astral-sh/ruff/pulls/6487.md) on 2023-08-10 21:45_

---

_Comment by @LaBatata101 on 2023-08-10 22:07_

I'm implementing PLR0913

---

_Comment by @LaBatata101 on 2023-08-11 22:53_

I'm implementing `PLR6301`

---

_Referenced in [sandialabs/pyttb#210](../../sandialabs/pyttb/issues/210.md) on 2023-08-13 20:23_

---

_Comment by @cidrblock on 2023-08-14 17:12_

We are slowly disabling duplicate pylint checks based on the list above, in case you need it, this little script builds a list of plyint messages that can be disabled, extracted from the list above.   YMMV :)

https://gist.github.com/cidrblock/ec3412bacfeb34dbc2d334c1d53bef83

It produces something like this:
```
"C0103", # invalid-name / ruff N815
"C0105", # typevar-name-incorrect-variance / ruff PLC0105
"C0112", # empty-docstring / ruff D419
"C0113", # unneeded-not / ruff SIM208
<...>
```



---

_Comment by @lucaspar on 2023-08-14 19:53_

I ported the script of @cidrblock to run [interactively](https://trinket.io/embed/python3/89f2deda0e) without setting up a local env.

Edit: updated the code after the parsing broke.


---

_Comment by @charliermarsh on 2023-08-14 19:54_

That's really cool, thank you both!

---

_Referenced in [astral-sh/ruff#6574](../../astral-sh/ruff/pulls/6574.md) on 2023-08-14 22:10_

---

_Comment by @LaBatata101 on 2023-08-14 22:47_

I'm implementing `PLC2401`

---

_Comment by @qartik on 2023-08-16 15:08_

> [ ] `broad-exception-raised` / `W0719`

Isn't this covered by TRY002? doc: https://beta.ruff.rs/docs/rules/raise-vanilla-class/

---

_Referenced in [astral-sh/ruff#6633](../../astral-sh/ruff/pulls/6633.md) on 2023-08-16 22:18_

---

_Referenced in [astral-sh/ruff#6582](../../astral-sh/ruff/issues/6582.md) on 2023-08-19 06:56_

---

_Referenced in [astral-sh/ruff#6806](../../astral-sh/ruff/issues/6806.md) on 2023-08-25 21:44_

---

_Referenced in [astral-sh/ruff#6903](../../astral-sh/ruff/issues/6903.md) on 2023-08-27 01:05_

---

_Comment by @parafoxia on 2023-09-04 14:27_

Don't want to be "that guy" as I'm sure this list is a bit of a nightmare to maintain, but there are a few entries on this list which are claimed to be done, but don't have an assigned code, nor do they appear in the docs.

(I've been doing some initial investigation for my work in an aim to migrate from Pylint to Ruff, hence the combing lmao.)

Implemented rules missing codes:

* W1302
* W1300
* W0718
* W1308
* W1310
* R1714

I also compiled a list of rules with a code in the docs, but not in the issue body:

* E0302 (PLE0302)
* W0130 (B033)
* W1310 (potentially F541?)
* W3301 (PLW3301)
* W0107 (PIE790)
* R0913 (PLR0913)
* R1721 (C416)

Been following this for a while. Glad to see the good work continuing!

---

_Comment by @jaap3 on 2023-09-05 08:58_

Pylint has a number of configuration options that are not (yet) available in ruff.

In my case I want to add "good" dunder name (`__html__`), this is possible using the [`good-dunder-names`](https://pylint.readthedocs.io/en/latest/user_guide/configuration/all-options.html#good-dunder-names) option in pylint. I cannot find such an option for ruff, so either disabling the rule or `noqa`'ing each occurrence is the only option for now.

---

_Comment by @tjkuson on 2023-09-05 11:01_

@parafoxia R1714 is implemented as [PLR1714](https://beta.ruff.rs/docs/rules/repeated-equality-comparison/) in Ruff, though you might be right about the rest. 

---

_Comment by @parafoxia on 2023-09-05 12:25_

> @parafoxia R1714 is implemented as [PLR1714](https://beta.ruff.rs/docs/rules/repeated-equality-comparison-target/) in Ruff, though you might be right about the rest. 

Ah, thanks!

---

_Referenced in [astral-sh/ruff#7307](../../astral-sh/ruff/issues/7307.md) on 2023-09-12 15:23_

---

_Referenced in [ghga-de/microservice-repository-template#159](../../ghga-de/microservice-repository-template/pulls/159.md) on 2023-09-14 07:24_

---

_Referenced in [ghga-de/microservice-repository-template#161](../../ghga-de/microservice-repository-template/pulls/161.md) on 2023-09-21 11:54_

---

_Referenced in [astral-sh/ruff#7660](../../astral-sh/ruff/issues/7660.md) on 2023-09-25 20:52_

---

_Referenced in [coqui-ai/TTS#3004](../../coqui-ai/TTS/pulls/3004.md) on 2023-09-27 08:51_

---

_Comment by @akx on 2023-09-27 09:01_

Since there probably are pylintistas in this issue, linking my tool: https://github.com/akx/pylint-to-ruff (It actually reads this issue to figure out what's been implemented in Ruff...)

---

_Referenced in [astral-sh/ruff#7753](../../astral-sh/ruff/pulls/7753.md) on 2023-10-02 04:28_

---

_Comment by @danbi2990 on 2023-10-04 04:26_

Working on `simplify-boolean-expression / R1709`
but seems there's a wrong progress status in the original post.

R1706 is marked as done by SIM108 but they are somewhat different.

R1706
https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/consider-using-ternary.html
```python
# before
x, y = 1, 2
maximum = x >= y and x or y  # [consider-using-ternary]

# after
x, y = 1, 2
`maximum = x if x >= y else y`
```

SIM108
https://docs.astral.sh/ruff/rules/if-else-block-instead-of-if-exp/
```python
# before
if foo:
    bar = x
else:
    bar = y

# after
bar = x if foo else y
```

R1706 can be implemented in line with R1709, so have plan to do it together.
pylint source code implementing both together:
https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/refactoring/refactoring_checker.py#L1542

Please review and confirm it. Thank you.

---

_Comment by @charliermarsh on 2023-10-04 04:32_

@danbi2990 - Ah, that makes sense. I'm fine to have R1706 implemented separately in that case.

The only issue with R1709 is that it seems to have some overlap with https://docs.astral.sh/ruff/rules/expr-and-false/.

---

_Comment by @danbi2990 on 2023-10-04 09:06_

Ok, R1706 is in progress.

For R1709,
Below difference exists which seems trivial.
If you think it's unnecessary, only R1706 will be PRed.

```python
# before
'apple' and False or 'orange'

# after expr-and-false (SIM223)
False or 'orange'

# after R1709
'orange'
```


---

_Referenced in [astral-sh/ruff#7811](../../astral-sh/ruff/pulls/7811.md) on 2023-10-04 13:00_

---

_Comment by @Ttibsi on 2023-10-04 13:51_

Hi, is there any plans for implementing the [pylint extensions?](https://pylint.pycqa.org/en/latest/user_guide/checkers/extensions.html)

---

_Referenced in [digitalfabrik/integreat-cms#2442](../../digitalfabrik/integreat-cms/issues/2442.md) on 2023-10-09 11:50_

---

_Comment by @DetachHead on 2023-10-12 13:24_

> * [ ]  `reimported` / `W0404`

i think this is covered by [`redefined-while-unused` (`F811`)](https://docs.astral.sh/ruff/rules/redefined-while-unused/#redefined-while-unused-f811)

---

_Comment by @danbi2990 on 2023-10-13 07:28_

Working on `redefined-argument-from-local` / `R1704`

---

_Comment by @Avasam on 2023-10-13 17:50_

`unspecified-encoding` / `W1514` would also cover for [flake8-file-encoding](https://github.com/rayjolt/flake8-file-encoding)

---

_Comment by @codereport on 2023-10-13 18:34_

FYI, `UP034` does not implement `C0325` and should be unmarked as done. See: https://github.com/astral-sh/ruff/issues/2389#issuecomment-1762006600 (and the full thread) for more details

---

_Comment by @charliermarsh on 2023-10-13 19:19_

(I went ahead and unchecked it.)

---

_Referenced in [astral-sh/ruff#7953](../../astral-sh/ruff/pulls/7953.md) on 2023-10-13 19:19_

---

_Referenced in [astral-sh/ruff#7961](../../astral-sh/ruff/pulls/7961.md) on 2023-10-14 17:46_

---

_Referenced in [GenericMappingTools/pygmt#2741](../../GenericMappingTools/pygmt/issues/2741.md) on 2023-10-15 09:53_

---

_Referenced in [astral-sh/ruff#7973](../../astral-sh/ruff/pulls/7973.md) on 2023-10-16 03:51_

---

_Comment by @DetachHead on 2023-10-16 04:47_

i think `relative-beyond-top-level` is covered by [`TID252`](https://docs.astral.sh/ruff/rules/relative-imports/)

---

_Referenced in [astral-sh/ruff#7975](../../astral-sh/ruff/pulls/7975.md) on 2023-10-16 06:31_

---

_Referenced in [astral-sh/ruff#7999](../../astral-sh/ruff/pulls/7999.md) on 2023-10-17 01:51_

---

_Referenced in [pymodbus-dev/pymodbus#1829](../../pymodbus-dev/pymodbus/pulls/1829.md) on 2023-10-17 20:07_

---

_Referenced in [astral-sh/ruff#8036](../../astral-sh/ruff/pulls/8036.md) on 2023-10-18 04:19_

---

_Comment by @diceroll123 on 2023-10-18 04:24_

Looks like [`compare-to-zero` / `C2001`](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/compare-to-zero.html) (not implemented in Ruff) has been removed from pylint in favor of [`use-implicit-booleaness-not-comparison-to-zero` / `C1805`](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/use-implicit-booleaness-not-comparison-to-zero.html)

Nice and terse.

---

_Referenced in [astral-sh/ruff#8038](../../astral-sh/ruff/pulls/8038.md) on 2023-10-18 06:00_

---

_Referenced in [astral-sh/ruff#8056](../../astral-sh/ruff/pulls/8056.md) on 2023-10-18 22:36_

---

_Referenced in [astral-sh/ruff#8058](../../astral-sh/ruff/pulls/8058.md) on 2023-10-19 03:04_

---

_Referenced in [astral-sh/ruff#8059](../../astral-sh/ruff/pulls/8059.md) on 2023-10-19 04:21_

---

_Referenced in [astral-sh/ruff#8159](../../astral-sh/ruff/pulls/8159.md) on 2023-10-24 09:08_

---

_Referenced in [SINTEF/ci-cd#192](../../SINTEF/ci-cd/pulls/192.md) on 2023-10-25 07:16_

---

_Comment by @flavius on 2023-10-26 13:20_

How did you guys manage to get all the hype, with only 20% of the checkboxes?

---

_Comment by @tusharsadhwani on 2023-10-26 13:22_

@flavius in my experience there's two camps of people: flake8 and flake8 plugin users, and pylint users. Ruff currently successfully caters to the first, and a small number of second.

---

_Comment by @tjkuson on 2023-10-26 13:50_

> How did you guys manage to get all the hype, with only 20% of the checkboxes?

Among other things, Ruff with mypy covers a significant portion of the not yet implemented Pylint rules.

---

_Comment by @tusharsadhwani on 2023-10-26 13:53_

Mypy is a different thing altogether as it requires your and your dependencies' code to have correct type hints, ruff and pylint don't.

---

_Comment by @jmbowman on 2023-10-26 14:02_

There are some pretty good reasons to switch from pylint only to ruff + pylint, even with the current set of implemented checks: shorter overall linting duration, fast feedback loop on a large percentage of the most useful checks, some checks that pylint doesn't have, etc.  I wrote up the rationale in our case on https://github.com/openedx/edx-platform/issues/32174 ; we just started the switch so I don't have numbers yet, but it's looking pretty promising so far.

---

_Comment by @tjkuson on 2023-10-26 14:04_

> Mypy is a different thing altogether as it requires your and your dependencies' code to have correct type hints, ruff and pylint don't.

That's true! If a user both really wants the above missing Pylint rules and doesn't want to use and facilitate a type checker like mypy, then they probably shouldn't replace Pylint *entirely* with Ruff. I just think it's relevant because many of the missing rules are not implemented as

1.  they require a more sophisticated type inference system than Ruff (or flake8 for that matter), and
2. they were designed before the widespread acceptance of type checking and annotation, so implement checks that would now be considered the remit of a type checker cf. a linter (for example, `no-member`/`E1101`).

This, personally, is why I have been happy to replace Pylint with Ruff, despite Ruff missing many of Pylint's rules.

---

_Comment by @DetachHead on 2023-10-26 19:39_

Since mypy is about as slow as pylint, it would be nice if ruff replaced it too (#3893)

---

_Comment by @dpinol on 2023-10-26 20:11_

> Since mypy is about as slow as pylint, it would be nice if ruff replaced it too (#3893)

Have you tried dmypy https://mypy.readthedocs.io/en/stable/mypy_daemon.html?

---

_Comment by @DetachHead on 2023-10-26 21:11_

Yeah, but it's barely any faster in my experience 

---

_Comment by @flavius on 2023-10-27 06:27_

Maybe I sounded offensive, but my question was honest: how?

If I did this project, I would have gotten maybe 10 occasional users at this stage.

---

_Comment by @akx on 2023-10-27 06:40_

@flavius Yeah, your tone didn't really convey 😅 

"At this stage"? Ruff has been available for more than a year now (though admittedly the old versions are much, pun intended, ruffer than what we have now).

Also, developers do care about their DX, and Ruff delivers on that: even if the (Pylint or otherwise) rule coverage isn't 100%, or say, even 50%, getting practically instant results on large codebases is a much bigger win for most, which certainly drove (and drives) adoption.

There's also the fact that the Ruff team has one of the most inclusive and friendly attitudes towards contributions I've seen in OSS in a while; if a rule is missing, and it's not thoroughly silly, you can go ahead and implement it.

Then, of course, there's a bit of marketing and luck involved :)

---

_Comment by @ssokolow on 2023-10-27 06:40_

> Maybe I sounded offensive, but my question was honest: how?
> 
> If I did this project, I would have gotten maybe 10 occasional users at this stage.

As tusharsadhwani said, there are a lot of users who make heavy use of MyPy type annotations and don't feel much need for PyLint because of it.

(eg. These days, I use Python because Rust has no PyQt/PySide or Django or SQLAlchemy+Alembic, I do a "backend in Rust, frontend in Python, glued together with PyO3" configuration where feasible (eg. using Python as "QML for QWidget GUIs, with MyPy checking" in a Rust+PyQt app) , and it saddens me that, even in strict mode and with type annotations applied liberally, MyPy is nowhere near as strong a guarantee of code function as Rust would be.)

Thanks to ruff, and with PyLint turned off as *horrendously slow*, MyPy's sluggishness is the biggest drag on my Vim+ALE Python development experience.

---

_Comment by @parafoxia on 2023-10-27 08:18_

@DetachHead I've never used it as I've only really just heard of it myself, but Pylyzer is a Mypy and Pyright alternative written in Rust, so may be worth checking out if you're looking for something like that. I think it's still in alpha itself though, and is one of those projects that makes very bold subjective claims as objective fact, so make of that what you will I spose lmao.

---

_Comment by @Pierre-Sassoulas on 2023-10-27 08:55_

> Maybe I sounded offensive, but my question was honest: how?

ruff was not originally a replacement for pylint, black or mypy: it was a replacement for flake8. You can use pylint + ruff but a lot of flake8 users were not using pylint to begin with. ruff is faster than flake8, it had more feature than flake8 from the get go, it has autofix, and is more contributor friendly than flake8 (ruff has "``pyproject.toml`` support" listed as the 3rd selling point in 2023, because flake8's maintainers vehemently refuse(d?) to support it, and ban on sight those asking for it). ruff was optimized for speed but also for **adoption** : everything was done for an easy transition from flake8 to ruff.  If you can replace one open-source package by another open source package easily and it's 20 time faster, with feature parity + autofix + some, and with a friendly and reactive maintainer working full time on it, the migration is going to be an easy choice. 

---

_Comment by @NeilGirdhar on 2023-10-27 13:40_

For me, the reason I'm so excited about Ruff is:
* The maintainer friendliness that Pierre mentioned--they respond to issues in such a kind and open-minded way.  This is the biggest indicator to me that a project will have longevity and success because this is the kind of projects that attracts talented people to help out.
* The trajectory they're on.  I don't think it's "20% of the checkboxes"; it's like 50% at this point?  And it's all happened incredibly quickly.  So as a user, it's easy to imagine that they'll be at 90% pretty soon.  (I think we should also give credit to other linters like Pylint that have come up with lists of useful rules that Ruff now uses.  Also, I'm not sure if they share test cases for the rules too?)
* The fast feedback loop that Jeremy mentioned.  It makes development a lot faster and more painless.

(As for MyPy speed, what I do is run Ruff and PyRight really often, and then I run Pylint and MyPy before committing changes.)

---

_Comment by @allisonkarlitskaya on 2023-10-27 14:26_

I think the single most useful post in this PR is this one, from the pylint maintainer:

https://github.com/astral-sh/ruff/issues/970#issuecomment-1585522741
> pylint is doing some limited type checking for historical reason (as it started 20 years ago when no type checkers were available). We're not implementing type checking in 2023 because mypy and pyright happened and we would not re-implement those checks again if we had too. I think they would bring no value if they were implemented in ruff -- and we're also considering removing them from pylint. But unfortunately I don't have an exhaustive list of them to give, I always use the same [`invalid-envvar-value`](https://pylint.readthedocs.io/en/stable/user_guide/messages/error/invalid-envvar-value.html) as an example :)

It's been completely lost in the noise by now, but it's worth repeating.

The ruff FAQ also states pretty clearly:
> It's recommended that you use Ruff in conjunction with a type checker, like Mypy, Pyright, or Pyre, with Ruff providing faster feedback on lint violations and the type checker providing more detailed feedback on type errors.

https://docs.astral.sh/ruff/faq/#how-does-ruff-compare-to-mypy-or-pyright-or-pyre

I buy into this logic and I personally hope that ruff *won't* implement the entire set of pylint rules.

---

_Comment by @GriceTurrble on 2023-10-27 14:40_

Even with the low overall coverage of current Pylint rules, what Ruff has implemented already is still covering a majority of the issues I care about.

I think it's completely fair to scrutinize the options in this list that are better-covered by a different tool, like Mypy. You can't make this tool do everything, or it will become a bloated and unmaintainable mess within 5 years.

I would go as far as to suggest that some of the auto-fixing, import sorting, and code formatting could just be gutted: they have their own dedicated tools that cover the concerns. Ruff would be well-suited as a linter to replace other linters, while working well in concert with other tools like isort, black, and mypy.

From where things are today, I would say take the rules that have overlap with mypy, and just mark them wontfix. Other tools exist, will continue to exist, and are more focused on covering these concerns: Ruff does not have to reinvent everything.

---

_Comment by @zanieb on 2023-10-27 15:54_

Thanks for the support everyone <3

If someone wants to compile a list of unimplemented Pylint rules that are covered by mypy (preferably with references to the type checking rule that replaces it), I will update the list.

---

_Comment by @ssokolow on 2023-10-27 16:15_

> (As for MyPy speed, what I do is run Ruff and PyRight really often, and then I run Pylint and MyPy before committing changes.)

I run ruff and MyPy on save and it happens asynchronously... that doesn't change the fact that **I** block on MyPy frequently and whether I'm manually waiting for Vim to update the quickcheck pane after typing `:w` or a Yakuake tab to update after running some kind of pre-commit check script doesn't make a significant different to the fact that **I'm** blocking on MyPy being slow.

---

_Comment by @tjkuson on 2023-10-27 16:15_

> Thanks for the support everyone <3
> 
> If someone wants to compile a list of unimplemented Pylint rules that are covered by mypy (preferably with references to the type checking rule that replaces it), I will update the list.

There was [a comment from earlier this year](https://github.com/astral-sh/ruff/issues/970#issuecomment-1565594417) that listed some rules.

---

_Comment by @ssokolow on 2023-10-27 16:19_

> Even with the low overall coverage of current Pylint rules, what Ruff has implemented already is still covering a majority of the issues I care about.
> 
> I think it's completely fair to scrutinize the options in this list that are better-covered by a different tool, like Mypy. You can't make this tool do everything, or it will become a bloated and unmaintainable mess within 5 years.
> 
> I would go as far as to suggest that some of the auto-fixing, import sorting, and code formatting could just be gutted: they have their own dedicated tools that cover the concerns. Ruff would be well-suited as a linter to replace other linters, while working well in concert with other tools like isort, black, and mypy.
> 
> From where things are today, I would say take the rules that have overlap with mypy, and just mark them wontfix. Other tools exist, will continue to exist, and are more focused on covering these concerns: Ruff does not have to reinvent everything.

I replaced tools like autopep8 with ruff because it's faster. I haven't even considered benchmarking black because "By using Black, you agree to cede control over minutiae of hand-formatting." translates to "Our creation is not fit for purpose by design."

I'm one of those people who refused to use `rustfmt` until a mix of new configuration options and RFCing its defaults closer to my preferred style allowed me to stand my ground on the style I like. (Were you aware that `gofmt`'s `black`-like stance resulted in someone creating [goformat](https://github.com/mbenkmann/goformat)?)

Thankfully, the rustfmt devs are sane and my rustfmt config these days is just:

```toml
match_block_trailing_comma = true
use_field_init_shorthand = true
use_small_heuristics = "Max"
use_try_shorthand = true
```

...and the `use_small_heuristics = "Max"` is the only one of those I consider non-negotiable as a "my monitor is landscape-oriented, not portrait" measure.

I will agree that ruff doesn't need to reinvent everything though. That's why I'm keeping my eye on Pylyzer's progress.

---

_Comment by @harupy on 2023-10-28 01:38_

Working on `bad-open-mode / W1501`.

---

_Referenced in [astral-sh/ruff#8294](../../astral-sh/ruff/pulls/8294.md) on 2023-10-28 03:45_

---

_Referenced in [astral-sh/ruff#8298](../../astral-sh/ruff/pulls/8298.md) on 2023-10-28 08:34_

---

_Referenced in [getpelican/pelican#3216](../../getpelican/pelican/issues/3216.md) on 2023-10-28 11:40_

---

_Referenced in [astral-sh/ruff#8321](../../astral-sh/ruff/pulls/8321.md) on 2023-10-29 07:49_

---

_Comment by @diceroll123 on 2023-10-29 20:03_

`W1406` can be removed from the list, it's a duplicate of `UP025`

---

_Referenced in [astral-sh/ruff#8335](../../astral-sh/ruff/pulls/8335.md) on 2023-10-30 02:54_

---

_Referenced in [ouhaiorg/ha-gt06#1](../../ouhaiorg/ha-gt06/issues/1.md) on 2023-10-31 02:29_

---

_Referenced in [petrfaitl/homeassistant-dev#1](../../petrfaitl/homeassistant-dev/issues/1.md) on 2023-10-31 02:33_

---

_Referenced in [openedx/edx-platform#33559](../../openedx/edx-platform/pulls/33559.md) on 2023-11-01 15:20_

---

_Comment by @pythonweb2 on 2023-11-03 13:46_

`W0212` is implemented with `SLF001`

---

_Comment by @pythonweb2 on 2023-11-03 14:18_

The `broad-exception-raised` is caught by the `TRY002` rule

---

_Comment by @charliermarsh on 2023-11-03 14:57_

Thanks @pythonweb2, appreciate these, I'm updating the list...

---

_Comment by @harupy on 2023-11-04 05:54_

`consider-using-from-import / R0402` is already implemented.

---

_Comment by @harupy on 2023-11-04 08:02_

working on `nan-comparison`.

---

_Comment by @ashanbrown on 2023-11-12 15:47_

Noticed that [E1300](https://docs.astral.sh/ruff/rules/#pylint-pl) is implemented but not checked off at https://github.com/astral-sh/ruff/issues/970#issue-1469044346

---

_Referenced in [openfun/joanie#470](../../openfun/joanie/pulls/470.md) on 2023-11-15 10:35_

---

_Referenced in [vprusso/toqito#280](../../vprusso/toqito/pulls/280.md) on 2023-11-24 22:01_

---

_Referenced in [johannes-graner/ruff#1](../../johannes-graner/ruff/pulls/1.md) on 2023-11-26 18:33_

---

_Referenced in [astral-sh/ruff#8843](../../astral-sh/ruff/pulls/8843.md) on 2023-11-26 19:40_

---

_Referenced in [astral-sh/ruff#8903](../../astral-sh/ruff/pulls/8903.md) on 2023-11-29 06:20_

---

_Comment by @diceroll123 on 2023-12-01 15:32_

- `R0202`
- `R0203`
- `PLR1733`
- `PLR1736`

can be checked off, they were merged within the last day.

---

_Referenced in [astral-sh/ruff#8973](../../astral-sh/ruff/issues/8973.md) on 2023-12-02 21:53_

---

_Referenced in [astral-sh/ruff#9062](../../astral-sh/ruff/issues/9062.md) on 2023-12-09 10:14_

---

_Referenced in [astral-sh/ruff#9103](../../astral-sh/ruff/issues/9103.md) on 2023-12-12 06:19_

---

_Comment by @mdbernard on 2023-12-13 14:10_

Noting that E0100 was implemented [here](https://github.com/astral-sh/ruff/pull/2716) (under the name `yield-in-init`) but is not checked off

---

_Referenced in [astral-sh/ruff#9149](../../astral-sh/ruff/issues/9149.md) on 2023-12-15 11:58_

---

_Referenced in [coveragepy/coveragepy#1716](../../coveragepy/coveragepy/issues/1716.md) on 2023-12-15 17:13_

---

_Referenced in [astral-sh/ruff#9163](../../astral-sh/ruff/pulls/9163.md) on 2023-12-16 15:19_

---

_Referenced in [astral-sh/ruff#9166](../../astral-sh/ruff/pulls/9166.md) on 2023-12-16 22:46_

---

_Referenced in [astral-sh/ruff#9172](../../astral-sh/ruff/pulls/9172.md) on 2023-12-17 21:55_

---

_Referenced in [astral-sh/ruff#9174](../../astral-sh/ruff/pulls/9174.md) on 2023-12-18 03:49_

---

_Referenced in [jtc42/fastapi-hypermodel#43](../../jtc42/fastapi-hypermodel/pulls/43.md) on 2023-12-18 13:55_

---

_Comment by @sanmai-NL on 2023-12-20 12:56_

@charliermarsh What to do with e.g., source code style related checkers, that are covered when using Ruff as formatter? See e.g., https://pylint.pycqa.org/en/latest/faq.html#which-messages-should-i-disable-to-avoid-duplicates-if-i-use-other-popular-linters

---

_Comment by @sanmai-NL on 2023-12-20 13:02_

For other users' reference, here is a `pyproject.toml` section that enables only Pylint checkers that aren't as of now covered by Ruff (not using its formatting functionality yet). Note that this section is only valid if you run Pylint with `--enable-all-extensions`, which can't be configured here (https://github.com/pylint-dev/pylint/issues/8217).

```toml
[tool.pylint]
disable = ["all"]
enable = [
  "abstract-class-instantiated",
  "abstract-method",
  "access-member-before-definition",
  "anomalous-unicode-escape-in-string",
  "arguments-differ",
  "arguments-out-of-order",
  "arguments-renamed",
  "assigning-non-slot",
  "assignment-from-no-return",
  "assignment-from-none",
  "attribute-defined-outside-init",
  "bad-builtin",
  "bad-except-order",
  "bad-exception-cause",
  "bad-file-encoding",
  "bad-indentation",
  "bad-mcs-classmethod-argument",
  "bad-mcs-method-argument",
  "bad-reversed-sequence",
  "bad-staticmethod-argument",
  "bad-super-call",
  "bad-thread-instantiation",
  "catching-non-exception",
  "chained-comparison",
  "class-variable-slots-conflict",
  "compare-to-zero",
  "comparison-with-callable",
  "condition-evals-to-constant",
  "confusing-consecutive-elif",
  "confusing-with-statement",
  "consider-swap-variables",
  "consider-using-assignment-expr",
  "consider-using-augmented-assign",
  "consider-using-dict-items",
  "consider-using-enumerate",
  "consider-using-f-string",
  "consider-using-from-import",
  "consider-using-join",
  "consider-using-max-builtin",
  "consider-using-min-builtin",
  "consider-using-namedtuple-or-dataclass",
  "consider-using-tuple",
  "consider-using-with",
  "cyclic-import",
  "deprecated-argument",
  "deprecated-class",
  "deprecated-decorator",
  "deprecated-method",
  "deprecated-module",
  "deprecated-typing-alias",
  "dict-init-mutate",
  "dict-iter-missing-items",
  "differing-param-doc",
  "differing-type-doc",
  "disallowed-name",
  "duplicate-code",
  "empty-comment",
  "global-at-module-level",
  "global-variable-undefined",
  "import-error",
  "import-private-name",
  "inconsistent-mro",
  "inherit-non-class",
  "invalid-bool-returned",
  "invalid-bytes-returned",
  "invalid-character-carriage-return",
  "invalid-characters-in-docstring",
  "invalid-class-object",
  "invalid-enum-extension",
  "invalid-envvar-value",
  "invalid-format-index",
  "invalid-format-returned",
  "invalid-getnewargs-ex-returned",
  "invalid-getnewargs-returned",
  "invalid-hash-returned",
  "invalid-index-returned",
  "invalid-length-hint-returned",
  "invalid-length-returned",
  "invalid-metaclass",
  "invalid-overridden-method",
  "invalid-repr-returned",
  "invalid-sequence-index",
  "invalid-slice-index",
  "invalid-slice-step",
  "invalid-slots",
  "invalid-slots-object",
  "invalid-star-assignment-target",
  "invalid-str-returned",
  "invalid-unary-operand-type",
  "invalid-unicode-codec",
  "isinstance-second-argument-not-valid-type",
  "logging-format-truncate",
  "logging-unsupported-format",
  "method-cache-max-size-none",
  "method-hidden",
  "misplaced-format-function",
  "missing-any-param-doc",
  "missing-format-attribute",
  "missing-kwoa",
  "missing-param-doc",
  "missing-parentheses-for-call-in-test",
  "missing-raises-doc",
  "missing-return-doc",
  "missing-return-type-doc",
  "missing-timeout",
  "missing-type-doc",
  "missing-yield-doc",
  "missing-yield-type-doc",
  "mixed-line-endings",
  "modified-iterating-dict",
  "modified-iterating-list",
  "modified-iterating-set",
  "multiple-constructor-doc",
  "nan-comparison",
  "no-member",
  "no-name-in-module",
  "no-value-for-parameter",
  "non-iterator-returned",
  "non-parent-init-called",
  "non-str-assignment-to-dunder-name",
  "nonlocal-and-global",
  "not-a-mapping",
  "not-an-iterable",
  "not-async-context-manager",
  "not-callable",
  "not-context-manager",
  "overlapping-except",
  "overridden-final-method",
  "pointless-string-statement",
  "possibly-unused-variable",
  "potential-index-error",
  "preferred-module",
  "raising-bad-type",
  "raising-format-tuple",
  "raising-non-exception",
  "redeclared-assigned-name",
  "redefined-outer-name",
  "redefined-slots-in-subclass",
  "redefined-variable-type",
  "redundant-keyword-arg",
  "redundant-returns-doc",
  "redundant-u-string-prefix",
  "redundant-unittest-assert",
  "redundant-yields-doc",
  "self-cls-assignment",
  "shallow-copy-environ",
  "signature-differs",
  "simplifiable-condition",
  "simplifiable-if-expression",
  "simplifiable-if-statement",
  "simplify-boolean-expression",
  "singledispatch-method",
  "singledispatchmethod-function",
  "star-needs-assignment-target",
  "stop-iteration-return",
  "subclassed-final-class",
  "super-init-not-called",
  "super-without-brackets",
  "superfluous-parens",
  "too-few-public-methods",
  "too-many-ancestors",
  "too-many-function-args",
  "too-many-instance-attributes",
  "too-many-lines",
  "too-many-nested-blocks",
  "too-many-try-statements",
  "trailing-newlines",
  "trailing-whitespace",
  "unbalanced-dict-unpacking",
  "unbalanced-tuple-unpacking",
  "undefined-loop-variable",
  "unexpected-keyword-arg",
  "unexpected-line-ending-format",
  "unhashable-member",
  "unnecessary-dunder-call",
  "unnecessary-ellipsis",
  "unpacking-non-sequence",
  "unreachable",
  "unsubscriptable-object",
  "unsupported-assignment-operation",
  "unsupported-binary-operation",
  "unsupported-delete-operation",
  "unsupported-membership-test",
  "unused-private-member",
  "unused-wildcard-import",
  "use-implicit-booleaness-not-comparison",
  "use-implicit-booleaness-not-len",
  "use-maxsplit-arg",
  "used-before-assignment",
  "useless-param-doc",
  "useless-parent-delegation",
  "useless-type-doc",
  "using-constant-test",
  "using-final-decorator-in-unsupported-version",
  "while-used",
  "wrong-exception-operation",
  "wrong-spelling-in-comment",
  "wrong-spelling-in-docstring",
]
```

---

_Referenced in [astral-sh/ruff#9251](../../astral-sh/ruff/pulls/9251.md) on 2023-12-23 05:41_

---

_Referenced in [astral-sh/ruff#9257](../../astral-sh/ruff/pulls/9257.md) on 2023-12-23 07:05_

---

_Referenced in [astral-sh/ruff#9267](../../astral-sh/ruff/pulls/9267.md) on 2023-12-24 22:05_

---

_Referenced in [astral-sh/ruff#9268](../../astral-sh/ruff/pulls/9268.md) on 2023-12-24 22:55_

---

_Comment by @diceroll123 on 2023-12-26 20:23_

~~`W0641` (`possibly-unused-variable`) is covered by PyFlakes `F841` (`unused-variable`)~~

EDIT: nvm!

---

_Comment by @tusharsadhwani on 2023-12-26 21:02_

@diceroll123 `possibly-unused-variable` checks for the presence of calls like `locals()`, whereas `unused-variable` doesn't. Ref:

https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/unused-variable.html

https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/possibly-unused-variable.html


---

_Comment by @diceroll123 on 2023-12-26 21:04_

I stand corrected! Thanks @tusharsadhwani 

---

_Comment by @JonathanPlasse on 2023-12-27 11:51_

`simplifiable-if-expression` is already implemented with `SIM210` and `SIM211` and some rules like `E713`.
`simplifiable-if-statement` is already implemented with `SIM108`.
`simplifiable-condition` and `simplify-boolean-expression` could extend the behavior of `SIM222` and `SIM223`.

---

_Referenced in [astral-sh/ruff#9305](../../astral-sh/ruff/issues/9305.md) on 2023-12-31 12:40_

---

_Referenced in [astral-sh/ruff#9354](../../astral-sh/ruff/issues/9354.md) on 2024-01-05 09:28_

---

_Referenced in [astral-sh/ruff#9397](../../astral-sh/ruff/issues/9397.md) on 2024-01-05 10:11_

---

_Comment by @dpinol on 2024-01-11 09:48_

About pylint `unused-private-member / W0238`, I [suggest](https://github.com/pylint-dev/pylint/issues/9356) that it should actually deal about private members, and not about members with mangled names

---

_Comment by @arosu on 2024-01-11 20:40_

I'd be very interested in `undefined-loop-variable /  W0631` as it has caused a bug in some of my team's software. We are currently running pylint and only enabling that rule.

---

_Referenced in [astral-sh/ruff#9500](../../astral-sh/ruff/issues/9500.md) on 2024-01-13 00:09_

---

_Comment by @diceroll123 on 2024-01-14 01:22_

W0604 can be crossed off, implemented https://github.com/astral-sh/ruff/pull/8058


---

_Comment by @NeilGirdhar on 2024-01-14 02:18_

Is [method-cache-max-size-none](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/method-cache-max-size-none.html) almost [B019](https://docs.astral.sh/ruff/rules/cached-instance-method/)?   Should B019 be adjusted to match?

---

_Comment by @NeilGirdhar on 2024-01-14 03:29_

keyword-arg-before-vararg / W1113 is marked completed by B026, but I don't think these are the same.  B026 checks _calls_ whereas W1113 checks _definitions_.

I think we need a rule that catches:
```python
def f(x=1, *ar, **kw):
    pass
```
and maybe fixes it to:
```python
def f(*ar, x=1, **kw):
    pass
```

Should I create an issue for that?

---

_Comment by @jungwookim on 2024-01-15 03:33_

Is there anyone who work `no-member / E1101` in progress or any plans? Can I try to contribute `no-member / E1101` rule?

---

_Referenced in [astral-sh/ruff#9545](../../astral-sh/ruff/pulls/9545.md) on 2024-01-16 07:56_

---

_Comment by @tjkuson on 2024-01-16 17:54_

> Is there anyone who work `no-member / E1101` in progress or any plans? Can I try to contribute `no-member / E1101` rule?

Not a maintainer, but that rule might be difficult to implement at present as Ruff lacks [cross-file analysis](https://github.com/astral-sh/ruff/issues?q=is%3Aopen+is%3Aissue+label%3Amultifile-analysis) (and non-trivial type deduction).

---

_Comment by @jungwookim on 2024-01-17 01:44_

@tjkuson Thanks for you comments. I wasn't aware of it. Let me await for it

---

_Referenced in [c0fec0de/anytree#255](../../c0fec0de/anytree/pulls/255.md) on 2024-01-18 02:42_

---

_Referenced in [astral-sh/ruff#9601](../../astral-sh/ruff/pulls/9601.md) on 2024-01-21 22:34_

---

_Referenced in [astral-sh/ruff#9611](../../astral-sh/ruff/pulls/9611.md) on 2024-01-22 08:29_

---

_Comment by @heso on 2024-01-23 09:01_

Is it possible to add E0601/used-before-assignment?

---

_Comment by @dhruvmanila on 2024-01-23 10:02_

@heso Do you wish to add it to the list? If so, it's already there. Or, do you want to work on it?

---

_Comment by @heso on 2024-01-23 10:44_

> If so, it's already there

when i try to check simple example:
```
def foo():
    p = 0
    if p > 1:
        t = 1
    t += 1
```
ruff (0.1.14) doesnt match [incorrect behavior](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/used-before-assignment.html) for `t`. 

---

_Comment by @dhruvmanila on 2024-01-23 10:47_

Sorry if I wasn't clear but I meant the list in the issue description. The rule hasn't been implemented yet which is why you don't see it being flagged in your example.

---

_Referenced in [astral-sh/ruff#9623](../../astral-sh/ruff/pulls/9623.md) on 2024-01-23 11:09_

---

_Comment by @vilim on 2024-01-24 12:45_

The C406 rule for dict literals does not cover the `init_with_keyword`  [case](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/use-dict-literal.html#use-dict-literal-r1735) 

---

_Comment by @nedbat on 2024-01-24 21:45_

I came here looking for something to start on.  Perhaps the description of the issue could have some explanation? Even a link to the contributing docs that detail how to add a new rule would be good.  And the checklist at the top here could use an explanation: what does it mean to be checked off vs crossed out?

---

_Comment by @charliermarsh on 2024-01-24 21:49_

That's a nice idea -- I'll add some info. (To answer that specific question: checked off implies that the rule has been implemented, while crossed out implies that it _won't_ be implemented (typically because it's Pylint-specific in some way).)

---

_Comment by @tusharsadhwani on 2024-01-24 21:52_

@charliermarsh what was the reasoning behind crossing out `duplicate-argument-name`? that one doesn't make sense for me.

---

_Comment by @nedbat on 2024-01-24 21:53_

> checked off implies that the rule has been implemented, while crossed out implies that it won't be implemented (typically because it's Pylint-specific in some way)

That's what I would have guessed, but it'll be good to spell it out, esp on a "good first issue".  Also, the two different message identifiers for some rules could be explained.


---

_Comment by @charliermarsh on 2024-01-24 22:03_

@tusharsadhwani - IIRC, because it's already a syntax error.

---

_Referenced in [astral-sh/ruff#9640](../../astral-sh/ruff/pulls/9640.md) on 2024-01-25 10:25_

---

_Referenced in [astral-sh/ruff#9656](../../astral-sh/ruff/pulls/9656.md) on 2024-01-27 07:08_

---

_Comment by @Skylion007 on 2024-01-27 16:13_

PLR1703 is already implemented as https://github.com/MartinThoma/flake8-simplify#SIM210

---

_Comment by @diceroll123 on 2024-01-27 17:21_

> PLR1703 is already implemented as [MartinThoma/flake8-simplify#SIM210](https://github.com/MartinThoma/flake8-simplify#SIM210)

@Skylion007 Not quite, `SIM210` only runs on if-expressions (ternary), `PLR1703` is for if-statements.

`SIM108` is the closest thing we have to `PLR1703` at the moment, which turns the if-statement into a ternary. _which would then be handled by SIM210 I suppose_

However, I do believe `SIM210` covers `PLR1719` which has not yet been implemented.

---

_Referenced in [astral-sh/ruff#9513](../../astral-sh/ruff/pulls/9513.md) on 2024-01-28 21:22_

---

_Referenced in [vprusso/toqito#444](../../vprusso/toqito/issues/444.md) on 2024-01-30 21:20_

---

_Referenced in [DanielNoord/ruff#1](../../DanielNoord/ruff/pulls/1.md) on 2024-02-05 21:45_

---

_Referenced in [astral-sh/ruff#9845](../../astral-sh/ruff/pulls/9845.md) on 2024-02-05 21:55_

---

_Referenced in [astral-sh/ruff#9932](../../astral-sh/ruff/pulls/9932.md) on 2024-02-13 20:30_

---

_Referenced in [astral-sh/ruff#10002](../../astral-sh/ruff/pulls/10002.md) on 2024-02-15 15:32_

---

_Comment by @tibor-reiss on 2024-02-15 16:09_

Hi all, I had a go at R1730/R1731 since I realized this was covered by pylint but not ruff while running them against our code base. Any feedback appreciated on the PR!

---

_Referenced in [LLNL/Surfactant#139](../../LLNL/Surfactant/issues/139.md) on 2024-02-17 20:18_

---

_Comment by @tjkuson on 2024-02-19 21:26_

Working on chained-comparison / R1716

---

_Referenced in [jatkinson1000/archeryutils#77](../../jatkinson1000/archeryutils/issues/77.md) on 2024-02-21 10:30_

---

_Comment by @jku on 2024-02-21 13:37_

I can't find an implementation of `broad-exception-caught / W0718` -- am I misunderstanding things or is the checkmark on that a mistake?

---

_Comment by @lucaspar on 2024-02-21 14:39_

@jku `broad-exception-caught / W0718` might be a subset to `blind-except (BLE001)`. In which case, maybe we can add this code to the list?

Related:

+ #2212 
+ #1119 


---

_Referenced in [astral-sh/ruff#10028](../../astral-sh/ruff/pulls/10028.md) on 2024-02-22 22:16_

---

_Comment by @tjkuson on 2024-02-23 16:23_

> Working on chained-comparison / R1716

Holding off on this until #10028 is merged as it would be import to avoid conflicts between the rules. (Plus the way I wrote the initial implementation doesn't work well when implementing an autofix, so need to rewrite a lot of it when I have some more time.)

---

_Referenced in [astral-sh/ruff#10103](../../astral-sh/ruff/pulls/10103.md) on 2024-02-23 19:26_

---

_Comment by @tibor-reiss on 2024-02-23 19:28_

Implemented (PL)R5601 (confusing-consecutive-elif). Any feedback on the PR appreciated.

---

_Comment by @mdbernard on 2024-02-24 08:56_

working on modified-iterating-dict / E4702

---

_Referenced in [astral-sh/ruff#10146](../../astral-sh/ruff/pulls/10146.md) on 2024-02-27 16:55_

---

_Comment by @tibor-reiss on 2024-02-27 20:49_

Implemented (PL)R6101 (consider-using-namedtuple-or-dataclass). Any feedback on the PR appreciated.

---

_Comment by @tibor-reiss on 2024-02-27 20:51_

If I am not mistaken, (PL)E1507 (invalid-envvar-value) is missing a checkmark because it was implemented here: https://github.com/astral-sh/ruff/pull/3467

---

_Referenced in [astral-sh/ruff#6934](../../astral-sh/ruff/issues/6934.md) on 2024-02-28 06:29_

---

_Comment by @j-hil-nwm on 2024-02-28 14:10_

Is  `trailing-whitespace / C0303` not covered by [W291](https://docs.astral.sh/ruff/rules/trailing-whitespace/)?



---

_Comment by @kaddkaka on 2024-02-28 15:38_

What about configurations currently available in pylint.conf such as
* `docstring-min-length` (see #5860)
* `max-args`

supporting these kind of configurations will make a transition from pylint to ruff more smooth.

---

_Comment by @nkxxll on 2024-03-01 11:11_

tried bad super call was a little bit over my skill gonna work on bad-file-encoding this should be easy for the start sorry, have a nice one you all <3

---

_Comment by @chanman3388 on 2024-03-01 12:39_

> tried bad super call was a little bit over my skill gonna work on bad-file-encoding this should be easy for the start sorry, have a nice one you all <3

I might give it a go, you got a branch of it open?

---

_Comment by @nkxxll on 2024-03-01 14:39_

jep @chanman3388 but there is barley something working (I am also not sure if this was even the right approach). But it could save you some boiler plate code. Branch is open give it a try https://github.com/nkxxll/ruff/tree/bad-super-call

---

_Comment by @chanman3388 on 2024-03-01 14:46_

> jep @chanman3388 but there is barley something working (I am also not sure if this was even the right approach). But it could save you some boiler plate code. Branch is open give it a try https://github.com/nkxxll/ruff/tree/bad-super-call

Cheers man, I'll give it a go later, I haven't touched this in a while so I'll be a bit...rusty.
I'll see myself out.

---

_Referenced in [astral-sh/ruff#10193](../../astral-sh/ruff/pulls/10193.md) on 2024-03-02 02:10_

---

_Comment by @chanman3388 on 2024-03-02 02:11_

^ @nkxxll 

---

_Referenced in [digitalfabrik/integreat-cms#2679](../../digitalfabrik/integreat-cms/pulls/2679.md) on 2024-03-03 15:50_

---

_Comment by @chanman3388 on 2024-03-04 01:40_

@nkxxll Not sure this helps or anything, but I guess looking at your code mostly you weren't sure what type to check for? Also as it stood, I think your approach would've only caught the one `bad-super-call` per method, though I think it could've easily been modified to check for more. Is the approach I took clear or sensible? If you'd like some help I'd be happy to give it.

---

_Comment by @nkxxll on 2024-03-04 17:54_

> @nkxxll Not sure this helps or anything, but I guess looking at your code mostly you weren't sure what type to check for? Also as it stood, I think your approach would've only caught the one `bad-super-call` per method, though I think it could've easily been modified to check for more. Is the approach I took clear or sensible? If you'd like some help I'd be happy to give it.

Yes, you are totally right I had no idea which type I had to search...\
I'm gonna look at you PR again. I'll get back to you if I have any questions, thanks 😇😊.



---

_Referenced in [delvtech/agent0#1345](../../delvtech/agent0/issues/1345.md) on 2024-03-05 18:05_

---

_Comment by @chanman3388 on 2024-03-06 21:16_

Going to give a go at `class-variable-slots-conflict` (`E0242`)

---

_Referenced in [astral-sh/ruff#10262](../../astral-sh/ruff/pulls/10262.md) on 2024-03-07 01:45_

---

_Comment by @canassa on 2024-03-07 16:40_

PLR1706 was removed, I think you can remove it from the list

---

_Comment by @kaddkaka on 2024-03-07 17:21_

@canassa removed from pylint? That's great, I was so annoyed about that rule today! 

---

_Comment by @canassa on 2024-03-07 18:30_

@kaddkaka No, removed from Ruff 😆 

---

_Referenced in [astral-sh/ruff#10317](../../astral-sh/ruff/pulls/10317.md) on 2024-03-09 18:28_

---

_Comment by @nkxxll on 2024-03-11 08:55_

@chanman3388 if you want to have a little issue to wrap up you can finish the bad file encoding lint. I also have a branch for that would love to see how you solve it (though I hope my problem is only in the testing infrastructure). I personally have the feeling there is a hard skill issue on my side with Rust and Rust infra structure.

see the branch https://github.com/nkxxll/ruff/tree/bad-file-encoding

---

_Comment by @chanman3388 on 2024-03-11 11:56_

> @chanman3388 if you want to have a little issue to wrap up you can finish the bad file encoding lint. I also have a branch for that would love to see how you solve it (though I hope my problem is only in the testing infrastructure). I personally have the feeling there is a hard skill issue on my side with Rust and Rust infra structure.
> 
> see the branch https://github.com/nkxxll/ruff/tree/bad-file-encoding

Sure I'll take a look. What in particular to you feel you have an issue with? If you're on the ruff discord I'd be happy to discuss further.

---

_Comment by @nkxxll on 2024-03-11 12:08_

> Sure I'll take a look. What in particular to you feel you have an issue with? If you're on the ruff discord I'd be happy to discuss further.

Yes I am let's chat there...

---

_Referenced in [astral-sh/ruff#10377](../../astral-sh/ruff/pulls/10377.md) on 2024-03-13 05:19_

---

_Comment by @darko-techsee on 2024-03-13 13:33_

E0401 will really help from refactoring perspective. As soon as you move something in different file/dir it will start complaining. This will eliminate the runtime exceptions of wrong imports if by accident you forgot to fix them everywhere.

---

_Referenced in [astral-sh/ruff#10401](../../astral-sh/ruff/pulls/10401.md) on 2024-03-14 01:36_

---

_Referenced in [astral-sh/ruff#10407](../../astral-sh/ruff/pulls/10407.md) on 2024-03-14 05:36_

---

_Referenced in [astral-sh/ruff#10428](../../astral-sh/ruff/pulls/10428.md) on 2024-03-15 23:03_

---

_Referenced in [astral-sh/ruff#10431](../../astral-sh/ruff/pulls/10431.md) on 2024-03-16 20:34_

---

_Comment by @hikaru-kajita on 2024-03-19 07:47_

I will work on `modified-iterating-set` / `E4703`.

---

_Referenced in [astral-sh/ruff#10473](../../astral-sh/ruff/pulls/10473.md) on 2024-03-19 13:16_

---

_Referenced in [getappmap/appmap-python#293](../../getappmap/appmap-python/pulls/293.md) on 2024-03-20 10:02_

---

_Comment by @pjspereira on 2024-03-21 21:19_

Can the compatibility table include the source of the pylint check and whether it is optional? I am not sure of the best way to add that to the table, perhaps just at the end of the line.

When aligning ruff with an existing pylint rule set, or pylint defaults, this info would help to determine whether the corresponding ruff rule should be enabled.

For example, ruff supports magic-value-comparison, R2004 (PLR2004), which is great, but a relatively noisy check. However, R2004’s rule documentation indicates that it is enabled by the optional ‘magic-value’ checker, so you might not wish to enable this ruff check if you wish to align with pylint defaults.

There is more needed to ease migration, for example calling out configuration differences for the checks, but just highlighting optional ones would be a great start. I suppose the best level of compatibility would be to read pylintrc and write a ruff.toml, which would be significant effort, but might ease version upgrades as you implement new rules.

Of course, I am not trying to request major work here, just trying to identify the best strategy. I mapped out the rule origins once about a year ago and might be able to add.

---

_Comment by @charliermarsh on 2024-03-21 21:40_

@pjspereira - For what it's worth, we've stopped adding Pylint rules that come from Pylint extensions. In hindsight, we may not have added that rule if it were proposed today.

---

_Comment by @augustelalande on 2024-03-21 23:27_

E0102 is covered by F811

---

_Comment by @augustelalande on 2024-03-22 03:51_

C0303 is covered by W291

---

_Comment by @augustelalande on 2024-03-22 04:25_

R1719 is covered by SIM210 and SIM211

---

_Referenced in [astral-sh/ruff#10522](../../astral-sh/ruff/issues/10522.md) on 2024-03-23 09:35_

---

_Comment by @kaddkaka on 2024-03-23 09:47_

function-redefined rule in pylint `E0102` and pyflakes rule `F811` is *not* the same, see https://github.com/astral-sh/ruff/issues/10522 for details.

---

_Referenced in [astral-sh/ruff#10540](../../astral-sh/ruff/pulls/10540.md) on 2024-03-24 00:39_

---

_Comment by @augustelalande on 2024-03-24 04:26_

E0601 is covered by F821

---

_Referenced in [astral-sh/ruff#10572](../../astral-sh/ruff/issues/10572.md) on 2024-03-25 16:31_

---

_Referenced in [astral-sh/ruff#10629](../../astral-sh/ruff/issues/10629.md) on 2024-03-27 15:26_

---

_Referenced in [astral-sh/ruff#10781](../../astral-sh/ruff/pulls/10781.md) on 2024-04-04 22:57_

---

_Comment by @autinerd on 2024-04-06 18:23_

`broad-exception-caught / W0718` is checked, but it's missing that it is covered by `BLE001`

---

_Referenced in [astral-sh/ruff#10820](../../astral-sh/ruff/pulls/10820.md) on 2024-04-07 19:20_

---

_Comment by @tibor-reiss on 2024-04-07 20:07_

Hi all, I had a crack at W0601. Any feedback appreciated on the PR!

---

_Comment by @kaddkaka on 2024-04-08 10:24_

> E0102 is covered by F811

It's not. I already said this in my previous comment, but wanted to make sure you and @charliermarsh see this.

Should we consider a rule as implemented for cases that are not explicitly meant to be reimplementations (as pep8 > pylint as I'm guessing)?

---

_Comment by @DetachHead on 2024-04-12 06:23_

`consider-using-augmented-assign` / `R6104` can be marked as done (#9932)

---

_Comment by @xrmx on 2024-04-12 13:16_

FYI pylint 3.1.0 added more checks, there's at least: `R1737: Use 'yield from' directly instead of yielding each element one by one (use-yield-from)`

---

_Comment by @diceroll123 on 2024-04-13 05:44_

@xrmx Pylint's `R1737` is covered by PyUpgrade's `UP028`

---

_Referenced in [astral-sh/ruff#10944](../../astral-sh/ruff/pulls/10944.md) on 2024-04-15 05:49_

---

_Referenced in [astral-sh/ruff#10959](../../astral-sh/ruff/pulls/10959.md) on 2024-04-15 19:09_

---

_Referenced in [astral-sh/ruff#10961](../../astral-sh/ruff/pulls/10961.md) on 2024-04-15 20:34_

---

_Referenced in [astral-sh/ruff#10962](../../astral-sh/ruff/pulls/10962.md) on 2024-04-15 20:37_

---

_Referenced in [astral-sh/ruff#10963](../../astral-sh/ruff/pulls/10963.md) on 2024-04-15 20:39_

---

_Comment by @tibor-reiss on 2024-04-15 20:52_

I had a go at the invalid dunder rules (PLE0303, PLE0305, PLE0308, PLE0309). Any feedback is appreciated on the PRs.

Note: I could not figure out why the cache::tests::same_results::ruff_linter_fixtures test (and others) break. All is fine locally until commit 670d66f, but then with the next commit, c221035 (which added a new rule), these tests start to fail. Any ideas please?
Edit: solved, thanks! The issue was that we crossed 832 rules and we are adding more :)

Ps.: E0304 and E0307 are both already implemented.

---

_Referenced in [cloudera/hue#3703](../../cloudera/hue/pulls/3703.md) on 2024-04-17 18:13_

---

_Referenced in [astral-sh/ruff#10791](../../astral-sh/ruff/issues/10791.md) on 2024-04-22 20:41_

---

_Comment by @TomerBin on 2024-04-23 17:51_

R1732 is covered by SIM115

---

_Referenced in [astral-sh/ruff#11198](../../astral-sh/ruff/issues/11198.md) on 2024-04-29 06:35_

---

_Comment by @com6056 on 2024-05-02 00:13_

It would be awesome if ruff could support setting `--allow-global-unused-variables=no` 🎉  https://pylint.readthedocs.io/en/stable/user_guide/configuration/all-options.html#allow-global-unused-variables

---

_Referenced in [digitalfabrik/integreat-cms#2758](../../digitalfabrik/integreat-cms/pulls/2758.md) on 2024-05-08 11:15_

---

_Comment by @Mellowdv on 2024-05-09 13:59_

> It would be awesome if ruff could support setting `--allow-global-unused-variables=no` 🎉 https://pylint.readthedocs.io/en/stable/user_guide/configuration/all-options.html#allow-global-unused-variables

I would like to work on this, if nobody is on it. We would like to use ruff at my workplace but we also require this rule.

---

_Comment by @charliermarsh on 2024-05-09 14:29_

What rule does that setting impact?

---

_Comment by @Mellowdv on 2024-05-09 14:41_

This is essentially Unused Variable (F841) except at Module scope instead of Function scope the way we understand it. My idea was to basically handle it in deferred_scopes when doing other ScopeKind::Module rules.

Edit: I misunderstood ScopeKind::Module. I understand now that unused variables local to functions will be caught in ScopeKind::Module as well. Then no new rule is probably necessary. F841 could probably be modified to run at Module scope instead of Function scope when the allow-global-unsued-variables=no flag is set.

---

_Referenced in [canonical/pycloudlib#374](../../canonical/pycloudlib/pulls/374.md) on 2024-05-09 20:10_

---

_Comment by @DetachHead on 2024-05-15 13:38_

new rule in pylint 3.2.0: [`contextmanager-generator-missing-cleanup`](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/contextmanager-generator-missing-cleanup.html)

---

_Referenced in [element-hq/synapse#17187](../../element-hq/synapse/pulls/17187.md) on 2024-05-23 20:01_

---

_Referenced in [eth-easl/modyn#446](../../eth-easl/modyn/pulls/446.md) on 2024-05-27 11:13_

---

_Referenced in [astral-sh/ruff#11600](../../astral-sh/ruff/issues/11600.md) on 2024-05-30 03:37_

---

_Comment by @s-cork on 2024-05-30 13:32_

Just raising it here from #11600

> no-method-argument / E0211 (N805) 

Is (possibly) incorrectly marked as done ⬆️. But is not implemented as part of N805. 

https://britonad.github.io/pylint-errors/plerr/errors/classes/E0211.html

---

_Comment by @charliermarsh on 2024-05-30 17:26_

Thanks, updated!

---

_Referenced in [astral-sh/ruff#11633](../../astral-sh/ruff/issues/11633.md) on 2024-05-31 23:20_

---

_Comment by @mistercrunch on 2024-06-04 21:55_

I had the intuition that if I could disable all the rules stated here in my `.pylintrc` and that it should speed up pylint significantly, making it more bearable to live with - especially as a commit hook - and increasingly as more rules get tackled here. Well I wanted to share some results here in case someone is thinking the same. This is in Apache Superset that has a fair amount of python code. 

Before disabling all the rules above:
```
real    0m46.724s
user    1m24.896s
sys     0m6.318s
```
After:
```
real    0m42.501s
user    1m17.920s
sys     0m9.299s
```

So pylint works in mysterious ways, and disabling half+ the rules doesn't seem to help it much unfortunately ... No action needed here, just thought it'd be relevant to people visiting this issue.

---

_Comment by @Pierre-Sassoulas on 2024-06-04 22:16_

I'm not too surprised with your data @mistercrunch, the main cost in pylint is ast parsing and inference and if there's a single check that require inference we pay the full cost upfront. The checks that use inferences are the costly one and are harder to implement in ruff (so by disabling the rules ruff has, you're probably deactivating the fastest pylint checks). And it would make sense for the fundamentally slow checks in pylint like duplicate-code to be the one that will never ends up in ruff. 

---

_Referenced in [astral-sh/ruff#11808](../../astral-sh/ruff/pulls/11808.md) on 2024-06-09 01:27_

---

_Referenced in [astral-sh/ruff#8018](../../astral-sh/ruff/issues/8018.md) on 2024-06-09 14:37_

---

_Comment by @augustelalande on 2024-06-14 16:58_

C0206 was done #11688

---

_Referenced in [astral-sh/ruff#11917](../../astral-sh/ruff/issues/11917.md) on 2024-06-18 05:24_

---

_Comment by @alexcdennis on 2024-06-18 16:39_

> > Working on chained-comparison / R1716
> 
> Holding off on this until #10028 is merged as it would be import to avoid conflicts between the rules. (Plus the way I wrote the initial implementation doesn't work well when implementing an autofix, so need to rewrite a lot of it when I have some more time.)

I am interested in working on [R1716](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/chained-comparison.html), but it looks like it is blocked by #10028, which is blocked by #1774.

Is it worth unblocking this by starting with a smaller rule which does not conflict with #10028? Specifically, only consider monotonic chained comparisons (`a < b < c`) and ignore non-monotonic chained comparisons (`a < b > c`), since I assume that this is what people actually care about when they think about chained comparisons.

I'm not sure on people's feelings for rules that diverge in behavior from the original pylint rule over edge cases.

@tjkuson I am interested in your input since you worked on this before

---

_Comment by @Peiffap on 2024-06-21 13:03_

@charliermarsh As discussed in #11808, `R0901` shouldn't be worked on until multifile analysis is possible.

---

_Comment by @Peiffap on 2024-06-21 13:04_

@alexcdennis You might want to take a look at #11807, which tries to implement `R1716`.

---

_Referenced in [inducer/pyopencl#770](../../inducer/pyopencl/pulls/770.md) on 2024-06-26 16:35_

---

_Comment by @DetachHead on 2024-06-28 06:35_

i think `unidiomatic-typecheck` should be unchecked, since `E721` no longer bans comparisons using the `is` operator as of #11220, but [`unidiomatic-typecheck` does](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/unidiomatic-typecheck.html)

---

_Referenced in [narwhals-dev/narwhals#357](../../narwhals-dev/narwhals/pulls/357.md) on 2024-06-30 03:21_

---

_Comment by @barsikus007 on 2024-07-07 07:58_

I wrote bash script to exclude ready rules via [tool.pylint].disable pyproject.toml section
```bash
curl https://api.github.com/repos/astral-sh/ruff/issues/970 | jq -r .body | grep -F '[x]' | cut -d'/' -f 2 | cut -c 3-7 | awk '{printf "\"%s\",", $1}'
```

---

_Referenced in [frequenz-floss/frequenz-repo-config-python#275](../../frequenz-floss/frequenz-repo-config-python/issues/275.md) on 2024-07-08 07:29_

---

_Referenced in [commaai/openpilot#32961](../../commaai/openpilot/pulls/32961.md) on 2024-07-10 22:19_

---

_Comment by @martgra on 2024-07-17 07:32_

> I wrote bash script to exclude ready rules via [tool.pylint].disable pyproject.toml section
> 
> ```shell
> curl https://api.github.com/repos/astral-sh/ruff/issues/970 | jq -r .body | grep -F [x] | cut -d'/' -f 2 | cut -c 3-7 | awk '{printf "\"%s\",", $1}'
> ```

shebang it for clarity. dont work with zsh 

---

_Comment by @tusharsadhwani on 2024-07-17 08:33_

or just replace `[x]` with `'[x]'` making it compatible with zsh.

---

_Comment by @astrowonk on 2024-07-23 16:30_

Several rules in the list, like `W1202`, map to just `G`  in the parenthetical - I'm fuzzy on which rule is that exactly?

EDITED with the right mappings:

[logging-format-interpolation /  W1202](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/logging-format-interpolation.html) -> `G002`

[logging-fstring-interpolation / W1203](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/logging-fstring-interpolation.html) - > `G004`

Not so sure about [logging-not-lazy / W1201](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/logging-not-lazy.html).

---

_Referenced in [akx/pylint-to-ruff#9](../../akx/pylint-to-ruff/pulls/9.md) on 2024-07-23 17:29_

---

_Comment by @vaneseltine on 2024-07-31 20:54_

Just a heads up, three renamed Pylint convention messages at the Convention level ([renamed for 3.0](https://pylint.readthedocs.io/en/stable/user_guide/messages/messages_overview.html) earlier this year) are appearing by their old names in this list:

Unchecked:
-  [compare-to-zero / C2001](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/compare-to-zero.html) is now [use-implicit-booleaness-not-comparison-to-zero / C1805](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/use-implicit-booleaness-not-comparison-to-zero.html)

Checked:
- [compare-to-empty-string / C1901](https://pylint.readthedocs.io/en/v3.0.4/user_guide/messages/convention/compare-to-empty-string.html) is now [use-implicit-booleaness-not-comparison-to-string / C1804](https://pylint.readthedocs.io/en/v3.0.4/user_guide/messages/convention/use-implicit-booleaness-not-comparison-to-string.html)
- [unneeded-not / C0113](https://pylint.readthedocs.io/en/v3.0.4/user_guide/messages/convention/unneeded-not.html) is now  [unnecessary-negation / C0117](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/unnecessary-negation.html)


---

_Referenced in [astral-sh/ruff#12637](../../astral-sh/ruff/issues/12637.md) on 2024-08-02 17:28_

---

_Comment by @diceroll123 on 2024-08-04 21:16_

`consider-using-from-import / R0402` can be checked off, exists already as `manual_from_import`

---

_Referenced in [ACEsuit/mace#534](../../ACEsuit/mace/issues/534.md) on 2024-08-05 15:24_

---

_Referenced in [astral-sh/ruff#12705](../../astral-sh/ruff/issues/12705.md) on 2024-08-06 03:45_

---

_Comment by @fruch on 2024-08-07 06:16_

> @MartinBernstorff - Definitely. The multi-file stuff will come, just not as quickly.

@charliermarsh any advancements in that direction ? 
(just got bitten by not having one of those)

---

_Comment by @Hnasar on 2024-08-07 20:54_

> [lucaspar](https://github.com/lucaspar) wrote:
> I ported the script of @cidrblock to run [interactively](https://trinket.io/embed/python3/76efaf1927) without setting up a local env.

There was an error from a ~strikethrough~ rule in the original post — here's a patched version (uses bs4's `find_all`) — https://trinket.io/python3/2341d58536

As of that script's output, Ruff has implemented this much of the Pylint rules:

> ✅ 215 / 380 (56.58%) implemented



---

_Referenced in [python-gitlab/python-gitlab#2949](../../python-gitlab/python-gitlab/pulls/2949.md) on 2024-08-08 11:09_

---

_Comment by @rhoban13 on 2024-08-08 14:14_

`broad-exception-caught / W0718` is checked above yet I'm not seeing it in available [rules](https://docs.astral.sh/ruff/rules/#warning-w_1).  Is that implemented?

There is a similar [blind-except](https://docs.astral.sh/ruff/rules/blind-except/) from flake8, but that one seems to only apply to `except BaseException`, not `except Exception`?



---

_Comment by @lucaspar on 2024-08-08 14:46_

@rhoban13 `BLE001` also works for `except Exception`:

![image](https://github.com/user-attachments/assets/aeb921e9-8b04-4968-aa78-192464704627)


---

_Comment by @diceroll123 on 2024-08-09 05:55_

`C0305` is covered by PyCodeStyle `W391`, implemented ~5 months ago

---

_Comment by @diceroll123 on 2024-08-09 06:16_

`W0311` is covered by PyCodeStyle `E111`

---

_Label `good first issue` removed by @MichaReiser on 2024-08-09 06:24_

---

_Label `help wanted` added by @MichaReiser on 2024-08-09 06:24_

---

_Referenced in [astral-sh/ruff#12788](../../astral-sh/ruff/pulls/12788.md) on 2024-08-09 14:46_

---

_Comment by @echoix on 2024-08-13 02:51_

Is it possible to add [bad-chained-comparison / W3601](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/bad-chained-comparison.html) to the tracked list?
Thanks!

---

_Comment by @R-Peleg on 2024-08-13 07:22_

For me the no-member / E1101 and no-name-in-module / E0611 could be the most useful, since I do a lot of typo and failed merged that fails in long tests on that things. Is there a plan to support them? Is it reasonable to try and implement them as my first contribution to this repo, in terms of expected complexity?

---

_Comment by @dhruvmanila on 2024-08-13 07:57_

I think it's difficult to implement no-member / E1101 and no-name-in-module / E0611 without type-inference and multifile analysis. These two features are currently a work in progress and will require some time. The reason being that `no-member` requires Ruff to have knowledge of the variable type and `no-name-in-module` requires Ruff to follow through the import and collect all the symbols from that module which requires analyzing multiple files at once (currently, Ruff only analyzes one file at a time).

---

_Referenced in [Azure/azure-cli-dev-tools#462](../../Azure/azure-cli-dev-tools/issues/462.md) on 2024-08-15 06:42_

---

_Comment by @serjflint on 2024-08-19 20:33_

> I wrote bash script to exclude ready rules via [tool.pylint].disable pyproject.toml section
> 
> ```shell
> curl https://api.github.com/repos/astral-sh/ruff/issues/970 | jq -r .body | grep -F '[x]' | cut -d'/' -f 2 | cut -c 3-7 | awk '{printf "\"%s\",", $1}'
> ```

I used the bash script above to make a Python script for missing rules

https://gist.github.com/serjflint/d31e64a70438ab9e13630ad05c987a11

And the current result

https://gist.github.com/serjflint/0c9d8b7c44074f2450f92b1c108117f9

For me the most frequent issue is `superfluous-parens` / `C0325`.

---

_Referenced in [astral-sh/ruff#13078](../../astral-sh/ruff/issues/13078.md) on 2024-08-23 13:07_

---

_Referenced in [astral-sh/ruff#4935](../../astral-sh/ruff/issues/4935.md) on 2024-08-28 05:14_

---

_Comment by @diceroll123 on 2024-08-29 05:29_

`superfluous-parens` / `C0325` is covered **(EDIT: partially covered)** by `UP034` https://docs.astral.sh/ruff/rules/UP034

---

_Comment by @lucaspar on 2024-08-29 05:43_

@diceroll123 ruff doesn't flag the [pylint examples](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/superfluous-parens.html) though, I don't think UP034 has the same coverage.

---

_Comment by @diceroll123 on 2024-08-29 05:45_

@lucaspar Fair point!

The right move here would probably be to extend UP034's capabilities.

---

_Comment by @diceroll123 on 2024-08-29 05:49_

`unnecessary-ellipsis` / `W2301` is covered by `PIE790`

---

_Comment by @SonGokussj4 on 2024-09-09 11:23_

Hi, just a question. I'm migrating Pylint+Flake8 checks to ruff but we have `E0401` which results in pylint to
```
E0401: Unable to import 'ada_resource.minio' (import-error)
```
Do I understand this right that we can't do a full migration because of this and still have to use pylint?
The `import-error / E0401` is not checked in the first post.


---

_Comment by @MichaReiser on 2024-09-09 11:29_

> Do I understand this right that we can't do a full migration because of this and still have to use pylint?

That's correct. Ruff doesn't support multifile analysis yet.

---

_Comment by @astrowonk on 2024-09-09 11:59_

> Several rules in the list, like `W1202`, map to just `G` in the parenthetical - I'm fuzzy on which rule is that exactly?
> 
> EDITED with the right mappings:
> 
> [logging-format-interpolation / W1202](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/logging-format-interpolation.html) -> `G002`
> 
> [logging-fstring-interpolation / W1203](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/logging-fstring-interpolation.html) - > `G004`
> 
> Not so sure about [logging-not-lazy / W1201](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/logging-not-lazy.html).

Bumping this -  is it possible to update the list so that these rules W1202 and W1203 point to the correctly implemented rule, not just generically "G?"

 `pylint-to-ruff` parses this list to convert pylintrc files to ruff rules

---

_Comment by @pcorpet on 2024-09-14 12:38_

Some rules are missing the code of the ruff rule. Here are the fixed lines:

- [x] `invalid-bool-returned` / `E0304` (`PLE0304`)
- [x] `invalid-bytes-returned` / `E0308` (`PLE0308`)
- [x] `invalid-hash-returned` / `E0309` (`PLE0309`)
- [x] `invalid-index-returned` / `E0305` (`PLE0305`)
- [x] `invalid-length-returned` / `E0303` (`PLE0303`)
- [x] `bad-format-string` / `W1302` (`F521`)
- [x] `broad-exception-caught` / `W0718` (`BLE001`)
- [x] `consider-using-augmented-assign` / `R6104` (`PLR6104`)
- [x] `consider-using-from-import` / `R0402` (`PLR0402`)
- [x] `consider-using-in` / `R1714` (`PLR1714`)

I could not find any for the following, so I believe we should uncheck them for now.
- [ ] `bad-format-string-key` / `W1300`
- [ ] `duplicate-string-formatting-argument` / `W1308`


---

_Comment by @pcorpet on 2024-09-17 20:09_

Also `PGH001` has been remapped to `S307`
`PLR1701` has been remapped to `SIM101`
`PLW0117` has been remapped to `PLW0177`
`TRY200` has been remapped to `B904`
`PLE5120` is unknown, it's actually `PLE1520`
`PLR1706` was removed
`E999` is deprecated


---

_Comment by @pcorpet on 2024-09-19 07:50_

Here is the script I'm using to help us migrate our Pylint config to Ruff.
https://gist.github.com/pcorpet/e776a8e794264b818c9cc6d06c11ef15

---

_Comment by @edreamleo on 2024-09-25 15:07_

@charliermarsh Thank you for this tracking issue! I still use pylint because Ruff does not report `no-member`, as in:
```python
class Cmdr:

    def __init__(self):
        self.a = None

class Client:

    def __init__(self):
        self.c = Cmdr()
        print(self.c.y)  # No member c.y.

    def test_c_dot_x(self):
        print(self.c.x)  # No member c.x.
```
II would be happy if Ruff ran slower provided Ruff could handle type inference. YMMV.

---

_Comment by @zanieb on 2024-09-25 15:16_

@edreamleo we're working on type inference right now! It's a big project though :) You can follow progress with the https://github.com/astral-sh/ruff/labels/red-knot label.

---

_Comment by @edreamleo on 2024-09-25 17:57_

@zanieb Thanks for your comment! I've bookmarked red-knot.

---

_Referenced in [SPF-OST/pytrnsys_gui#546](../../SPF-OST/pytrnsys_gui/issues/546.md) on 2024-09-30 06:59_

---

_Referenced in [Chia-Network/chia-blockchain#18649](../../Chia-Network/chia-blockchain/pulls/18649.md) on 2024-09-30 14:11_

---

_Referenced in [open-telemetry/opentelemetry-python#4223](../../open-telemetry/opentelemetry-python/pulls/4223.md) on 2024-10-13 01:18_

---

_Referenced in [astral-sh/ruff#13742](../../astral-sh/ruff/issues/13742.md) on 2024-10-14 04:27_

---

_Referenced in [Chia-Network/chia-blockchain#18759](../../Chia-Network/chia-blockchain/pulls/18759.md) on 2024-10-24 17:35_

---

_Referenced in [astral-sh/ruff#14131](../../astral-sh/ruff/issues/14131.md) on 2024-11-06 13:51_

---

_Referenced in [astral-sh/ruff#14241](../../astral-sh/ruff/pulls/14241.md) on 2024-11-10 08:05_

---

_Referenced in [Azure/azure-cli#30361](../../Azure/azure-cli/pulls/30361.md) on 2024-11-15 04:21_

---

_Referenced in [Azure/azure-cli#30349](../../Azure/azure-cli/pulls/30349.md) on 2024-11-15 04:36_

---

_Referenced in [lektor/lektor#1208](../../lektor/lektor/pulls/1208.md) on 2024-11-18 21:08_

---

_Comment by @bebound on 2024-11-20 08:27_

Could you please add `possibly-used-before-assignment / E0606` to the list?

Ref: https://pylint.pycqa.org/en/stable/user_guide/messages/error/possibly-used-before-assignment.html

---

_Comment by @MichaReiser on 2024-11-20 08:40_

@bebound I don't think we can implement this rule without precise control flow analysis. We already have a similar rule in red knot, our new static analysis backend that we plan on using in ruff long-term.

---

_Comment by @bebound on 2024-11-21 04:21_

@MichaReiser Thanks for the reply.

Find a minor issue in the mapping list: 
``` diff
- use-dict-literal / R1735 (C406)
+ use-dict-literal / R1735 (C406, C408)
- use-list-literal / R1734 (C405)
+ use-list-literal / R1734 (C408)
```

C408 checks `dict()`, `list()` and `tuple()`.
C405 is `unnecessary-literal-set`, which converts code to set literal and is not the target of `use-list-literal`. Not sure if C408 is a superset of R1734.

Ref: 
C405: https://docs.astral.sh/ruff/rules/unnecessary-literal-set/
C406: https://docs.astral.sh/ruff/rules/unnecessary-literal-dict/
C408: https://docs.astral.sh/ruff/rules/unnecessary-collection-call/
C411: https://docs.astral.sh/ruff/rules/unnecessary-list-call/

R1735:https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/use-dict-literal.html
R1734: https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/use-list-literal.html

---

_Referenced in [Azure/azure-cli#30308](../../Azure/azure-cli/pulls/30308.md) on 2024-11-21 04:27_

---

_Referenced in [SanGreel/telegram-data-collection#24](../../SanGreel/telegram-data-collection/pulls/24.md) on 2024-11-23 23:19_

---

_Referenced in [astral-sh/ruff#14651](../../astral-sh/ruff/issues/14651.md) on 2024-11-28 06:32_

---

_Label `help wanted` removed by @MichaReiser on 2024-12-02 17:20_

---

_Referenced in [apache/superset#31262](../../apache/superset/pulls/31262.md) on 2024-12-02 21:51_

---

_Comment by @alexisdrakopoulos on 2024-12-03 12:53_

I know that mypy deals with E1101 but we are unable to use mypy at the moment, are there any plans for implementing it in Ruff?

---

_Comment by @NeilGirdhar on 2024-12-03 19:26_

You can mark off W2301.  It is covered by PIE790 [*] Unnecessary `...` literal.

@alexisdrakopoulos My guess is you'll have to wait for static typing to be completed.  Maybe track: "Static type checking à la mypy "  (don't want to link it here)

---

_Referenced in [data-apis/array-api-extra#58](../../data-apis/array-api-extra/pulls/58.md) on 2024-12-10 14:15_

---

_Referenced in [astral-sh/ruff#14979](../../astral-sh/ruff/issues/14979.md) on 2024-12-15 02:42_

---

_Referenced in [fraunhofer-isi/forecast-sites#4](../../fraunhofer-isi/forecast-sites/issues/4.md) on 2024-12-23 14:21_

---

_Comment by @echoix on 2024-12-27 17:08_

I noticed the absence of unnecessary-negation / C0117 in the list 
https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/unnecessary-negation.html

---

_Comment by @InSyncWithFoo on 2024-12-27 17:41_

@echoix That one seems to be the same as [`SIM208`](https://docs.astral.sh/ruff/rules/double-negation/). The `not a > b` pattern is not currently reported by any rules, however.

---

_Referenced in [ag2ai/ag2#334](../../ag2ai/ag2/pulls/334.md) on 2025-01-02 11:43_

---

_Referenced in [pylint-dev/pylint#214](../../pylint-dev/pylint/issues/214.md) on 2025-01-16 11:02_

---

_Referenced in [ImperialCollegeLondon/virtual_ecosystem#666](../../ImperialCollegeLondon/virtual_ecosystem/issues/666.md) on 2025-01-17 09:44_

---

_Referenced in [astral-sh/ruff#15903](../../astral-sh/ruff/pulls/15903.md) on 2025-02-03 06:01_

---

_Referenced in [astral-sh/ruff#15914](../../astral-sh/ruff/issues/15914.md) on 2025-02-03 15:49_

---

_Comment by @anis-campos on 2025-02-07 13:46_

Hi.

I'm confused about the line `implicit-str-concat / W1404 (ISC001)`, does it mean that `ISC001` is supposed to be the same as `implicit-str-concat` ?

It seems to me that there are actually opposite, and I was actually looking forward to something just like the example given in the pylint documentation:

![Image](https://github.com/user-attachments/assets/fff7db54-43c3-47b0-8f63-96c892ea824f)

This causes very finiky bugs on a missing comma in a list. 

That being said, providing two oposite rules can also be confusing, so I'm wondering if `ISC001` should not be parametrized to forbid implicit concatenation.

---

_Comment by @MichaReiser on 2025-02-07 13:55_

You can use `ISC002` and set [`lint.flake8-implicit-str-concat.allow-multiline`](https://docs.astral.sh/ruff/settings/#lint_flake8-implicit-str-concat_allow-multiline) to `true` if you want to disallow any implicit concatenated string. See [Playground](https://play.ruff.rs/f41679f1-97b4-43a2-9df7-2135517314f0)

---

_Comment by @dpinol on 2025-02-07 14:30_

> script

I get "No tags found after scraping" now

---

_Comment by @phitoduck on 2025-02-12 00:44_

Capturing: I don’t see C0103 invalid-name in the list

---

_Comment by @InSyncWithFoo on 2025-02-12 02:32_

@phitoduck It <em>is</em> listed and marked as a duplicate of [`N815`](https://docs.astral.sh/ruff/rules/mixed-case-variable-in-class-scope/). See also [other `N` rules](https://docs.astral.sh/ruff/rules/#pep8-naming-n).

---

_Referenced in [astral-sh/ruff#16189](../../astral-sh/ruff/issues/16189.md) on 2025-02-16 16:03_

---

_Referenced in [j178/prek#197](../../j178/prek/issues/197.md) on 2025-02-21 19:38_

---

_Referenced in [astral-sh/ruff#16479](../../astral-sh/ruff/pulls/16479.md) on 2025-03-03 22:38_

---

_Comment by @NeilGirdhar on 2025-03-09 22:51_

Can we please mark off:   "E0601", "R1737", "W0311", "W2301":

> E0601 is covered by F821

> Pylint's `R1737` is covered by PyUpgrade's `UP028`

> `W0311` is covered by PyCodeStyle `E111`

> `unnecessary-ellipsis` / `W2301` is covered by `PIE790`

This would help when using [the script](https://github.com/astral-sh/ruff/issues/970#issuecomment-2232627137).

---

Edit:

Thanks @MichaReiser , but R1737 is still showing unticked?

---

_Comment by @ryanc-bs on 2025-03-12 16:34_

hi, are there any plans on implementing the `redefined-outer-name` rule from pylint soon? Would find it useful as this can be a nasty source of bugs, for example in:

```
items = []
for i in range(10):
    items = get_new_items(i)  # shadowing outer variable name
    items.extend(items)

# items now only contains the results from the last iteration
```

---

_Comment by @InSyncWithFoo on 2025-03-12 16:37_

@ryanc-bs There's #15903, but it only implements parts of the upstream rule (specifically, it won't emit any diagnostics for your example).

---

_Referenced in [rucio/rucio#7499](../../rucio/rucio/issues/7499.md) on 2025-03-14 18:16_

---

_Referenced in [deepset-ai/haystack#9078](../../deepset-ai/haystack/pulls/9078.md) on 2025-03-20 12:46_

---

_Comment by @a1tus on 2025-03-24 14:02_

> [@ryanc-bs](https://github.com/ryanc-bs) There's [#15903](https://github.com/astral-sh/ruff/pull/15903), but it only implements parts of the upstream rule (specifically, it won't emit any diagnostics for your example).

+1 for this rule, in our project it would be very useful for detecting accidental imports/global vars shadowing

---

_Comment by @ssbarnea on 2025-03-25 11:08_

If I am not mistaken `consider-using-re-compile` appears to be missing and it should not be hard to implement. I should mention that pylint implementation is able to detect only `re.search` or `re.match` but not `re.sub` and the reality is that all 3 can benefit from it.

---

_Comment by @tusharsadhwani on 2025-03-25 12:57_

@ssbarnea that sounds easy and useful, I'll take it up

---

_Referenced in [openedx/openedx-filters#275](../../openedx/openedx-filters/pulls/275.md) on 2025-03-25 23:21_

---

_Referenced in [aristanetworks/anta#680](../../aristanetworks/anta/pulls/680.md) on 2025-04-09 14:40_

---

_Referenced in [mystor/git-revise#141](../../mystor/git-revise/pulls/141.md) on 2025-04-15 20:53_

---

_Comment by @vjurczenia on 2025-04-16 22:20_

Hi, is anyone available to discuss implementing `use-maxsplit-arg / C0207`? 

I decided to dive right into it [here](https://github.com/vjurczenia/ruff/commit/ddf47dd31eaf3dd79fe791407cef059085be40b5) (if only to learn a bit more about Ruff), but got a little stuck when trying to imitate Pylint's behavior [here](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/refactoring/recommendation_checker.py#L124) which, to the best of my understanding, is what prevents the rule from triggering on just any function called `split`.

I'm thinking this may not be possible, but would appreciate confirmation from someone well-versed in the SemanticModel.

---

_Comment by @ntBre on 2025-04-17 17:22_

> Hi, is anyone available to discuss implementing use-maxsplit-arg / C0207?
> 
> I decided to dive right into it [here](https://github.com/vjurczenia/ruff/commit/ddf47dd31eaf3dd79fe791407cef059085be40b5) (if only to learn a bit more about Ruff), but got a little stuck when trying to imitate Pylint's behavior [here](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/refactoring/recommendation_checker.py#L124) which, to the best of my understanding, is what prevents the rule from triggering on just any function called split.
> 
> I'm thinking this may not be possible, but would appreciate confirmation from someone well-versed in the SemanticModel.

[`has_builtin_binding`](https://github.com/astral-sh/ruff/blob/d2ebfd6ed7855140806a7cad3e23d58fe5056c31/crates/ruff_python_semantic/src/model.rs#L273) comes to mind first for this, but I'm not sure it works on methods like `str.split`. An alternative could be to try resolving the type of the string with [`ResolvedPythonType`](https://github.com/astral-sh/ruff/blob/d2ebfd6ed7855140806a7cad3e23d58fe5056c31/crates/ruff_python_semantic/src/analyze/type_inference.rs#L9) or something like [`resolve_string_literal`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs#L329-L347). I don't think you can override `str.split`, so if we know the method receiver is a `str` and the name is `split`, it should be safe to enforce the rule.

You can also look for other rules that operate on builtin method calls to see how they handle it, but I didn't find any obvious candidates in a quick search.

---

_Comment by @dylwil3 on 2025-04-17 18:04_

I think the alternative suggestions is right and you just need to know whether you're looking at a string. Maybe [`is_string`](https://github.com/astral-sh/ruff/blob/d2ebfd6ed7855140806a7cad3e23d58fe5056c31/crates/ruff_python_semantic/src/analyze/typing.rs#L979) would do? It seems to be used [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs#L85) for [`bad-str-strip-call`](https://docs.astral.sh/ruff/rules/bad-str-strip-call/) which is a similar sort of rule.

---

_Referenced in [astral-sh/ruff#17454](../../astral-sh/ruff/pulls/17454.md) on 2025-04-17 22:09_

---

_Comment by @vjurczenia on 2025-04-17 22:20_

Thanks @ntBre and @dylwil3, that did the trick! Your advice helped refine the test a bit too: I've added a test case for a class that inherits from `str` and overrides `split`. This shouldn't get flagged, and it doesn't. 

As seen above, I've opened a PR with this change based on the instructions in the [contribution guide](https://docs.astral.sh/ruff/contributing/#example-adding-a-new-lint-rule). Please correct me if I'm wrong, but I assume now we simply wait until a maintainer decides to review. 🤞 

---

_Referenced in [astral-sh/ruff#8579](../../astral-sh/ruff/issues/8579.md) on 2025-04-24 17:51_

---

_Referenced in [astral-sh/ruff#17824](../../astral-sh/ruff/pulls/17824.md) on 2025-05-03 20:23_

---

_Comment by @serjflint on 2025-05-09 19:03_

Hello! Now that ty is a thing (congratulations) is there any issue to track multi-file analysis in ruff? Some unimplemented rules here require it.

---

_Comment by @ntBre on 2025-05-09 19:20_

> Hello! Now that ty is a thing (congratulations) is there any issue to track multi-file analysis in ruff? Some unimplemented rules here require it.

Thank you! There is an issue for multi-file analysis (#7447), but I think we're still a while away from getting to use ty's capabilities here in ruff.

---

_Comment by @kaddkaka on 2025-05-15 06:18_

How should the documentation look when pylint rules are covered by rule from other ruleset? For example at https://docs.astral.sh/ruff/rules/#refactor-plr it looks like W0107 is not covered by ruff, but it's actually implemented as `PIE790`. Could the docs have a link from W0107 to PIE790?

---

_Referenced in [numpy/numpy#28972](../../numpy/numpy/pulls/28972.md) on 2025-05-15 11:15_

---

_Comment by @NeilGirdhar on 2025-05-26 05:17_

Could someone please mark:
* use-yield-from / R1737
* star-needs-assignment-target / E0114

Also, it seems like a lot of the remaining rules would be implemented by a type checker like Ty.  Should a new issue be created for those rules that are supposed to be covered by Ty?  Then these rules could be crossed off here?

---

_Comment by @MichaReiser on 2025-05-26 06:47_

> Also, it seems like a lot of the remaining rules would be implemented by a type checker like Ty. Should a new issue be created for those rules that are supposed to be covered by Ty? Then these rules could be crossed off here?

Our focus for ty's stable release is a type checker. We'll probably support more opinionated and lint-like rules in the future. That's why I don't think an "Implement pylint" issue would make sense at the moment (or even an "implement remaining pylint rules"). Opening issues for specific rules that are exclusively about correctness and fit the bill of a type checker can make sense.

---

_Comment by @NeilGirdhar on 2025-05-26 07:10_

> That's why I don't think an "Implement pylint" issue would make sense at the moment (or even an "implement remaining pylint rules"). Opening issues for specific rules that are exclusively about correctness and fit the bill of a type checker can make sense.

I understand, but I wasn't just talking about "opinionated and lint-like rules".  I meant the rules that will be automatically implemented as a consequence of having a type checker.  For example,
* inconsistent-mro / E0240
* invalid-metaclass / E1139
* invalid-repr-returned / E0306
* invalid-sequence-index / E1126
* invalid-slice-index / E1127
* invalid-slice-step / E1144
* inherit-non-class / E0239
* no-member / E1101
* no-method-argument / E0211
* no-name-in-module / E0611

These rules are not "opinionated".  They are type errors that are caught by type checkers.

I suggest:
* crossing these rules out from this issue as "implemented by a type checker", and
* ensuring that the test cases for each of these rules is a test case in Ty's test suite.

I wonder how many rules would remain after all such rules were crossed out.

---

_Comment by @MichaReiser on 2025-05-26 07:13_

That makes sense. I think we can do that once ty reaches a point where we can recommend it for productive use.

---

_Comment by @NeilGirdhar on 2025-05-29 14:15_

Could someone please add and check "`missing-maxsplit-arg` (`PLC0207`)"?

---

_Comment by @tusharsadhwani on 2025-05-31 05:30_

@ssbarnea I'm implementing the `consider-using-re-compile` rule, and have my own opinions on when `re.compile` should be used vs. just using `re.search` etc., but I'd like to know:

- Is there an existing flake8 / pylint rule for this? I can't seem to find one.
- What are your opinions on when should `re.compile` be suggested -- on _all_ uses of `re.match`? Maybe only inside loops?

---

_Comment by @jamesbraza on 2025-06-03 22:12_

I think `pylint`'s `too-many-positional-arguments`/`R0917` needs to be added to the OP as being ported to `PLR0917`

---

_Referenced in [pyccel/pyccel#2327](../../pyccel/pyccel/pulls/2327.md) on 2025-06-07 18:21_

---

_Comment by @johnklehm on 2025-06-18 15:48_

Is it possible to get the implemented/not implemented list as free text somewhere. I want to toggle off pylint stuff that ruff is handling but it seems difficult to get the current data out of  the description at the top.

---

_Comment by @ntBre on 2025-06-18 16:26_

Here you go! This also looked pretty promising in the shell if you want a command to keep retrieving an updated version.

``` shell
gh issue view 970 --json body
```

<details><summary>PR Body</summary>

```
This is the parent issue for tracking parity with Pylint. Below, we've enumerated all Pylint rules.

Rules that are checked off have been implemented in Ruff already (either as a Pylint rule, e.g., `PLE0237`, or by way of overlap with another linter like Pyflakes, as with `missing-format-string-key`).

Rules that are crossed out have been removed some consideration. (In most cases, crossed-out rules represent Pylint specific rules, e.g., rules related to Pylint configuration.)

At time of writing, many of the remaining rules require type inference and/or multi-file analysis, and aren't ready to be implemented in Ruff. (See: https://github.com/astral-sh/ruff/issues/970#issuecomment-1565594417 for an enumeration.) If you're looking to start work on a specific rule, I'd suggest commenting in the issue, to get some input on whether Ruff is capable of supporting it at present.

For guidance on getting started, see the [Contributing](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#example-adding-a-new-lint-rule) documentation.

> *Note*: Don't implement rules that are part of Pylint extension until #1774 is completed. Don't implement rules that mainly target Python 2.

# Error

- [ ] `abstract-class-instantiated` / `E0110`
- [ ] `access-member-before-definition` / `E0203`
- [x] `assigning-non-slot` / `E0237` (`PLE0237`)
- [ ] `assignment-from-no-return` / `E1111`
- [ ] `assignment-from-none` / `E1128`
- [x] `await-outside-async` / `E1142` (`PLE1142`)
- [x] ~`bad-configuration-section` / `E0014`~
- [ ] `bad-except-order` / `E0701`
- [ ] `bad-exception-cause` / `E0705`
- [x] `bad-format-character` / `E1300` (`PLE1300`)
- [x] ~`bad-plugin-value` / `E0013`~
- [ ] `bad-reversed-sequence` / `E0111`
- [x] `bad-str-strip-call` / `E1310` (`PLE1310`)
- [x] `bad-string-format-type` / `E1307` (`PLE1307`)
- [ ] ~`bad-super-call` / `E1003`~ (mainly targets Python 2)
- [x] `bidirectional-unicode` / `E2502` (`PLE2502`)
- [x] ~`broken-collections-callable` / `E6005`~
- [x] ~`broken-noreturn` / `E6004`~
- [ ] `catching-non-exception` / `E0712`
- [ ] `class-variable-slots-conflict` / `E0242`
- [x] `continue-in-finally` / `E0116` (`PLE0116`)
- [ ] `dict-iter-missing-items` / `E1141`
- [x] ~`duplicate-argument-name` / `E0108`~
- [x] `duplicate-bases` / `E0241` (`PLE0241`)
- [x] `format-needs-mapping` / `E1303` (`F502`)
- [x] `function-redefined` / `E0102` (`F811`)
- [ ] `import-error` / `E0401`
- [ ] `inconsistent-mro` / `E0240`
- [ ] `inherit-non-class` / `E0239`
- [x] `init-is-generator` / `E0100` (`PLE0100`)
- [x] `invalid-all-format` / `E0605` (`PLE0605`)
- [x] `invalid-all-object` / `E0604` (`PLE0604`)
- [x] `invalid-bool-returned` / `E0304` (`PLE0304`)
- [x] `invalid-bytes-returned` / `E0308` (`PLE0308`)
- [x] `invalid-character-backspace` / `E2510` (`PLE2510`)
- [ ] `invalid-character-carriage-return` / `E2511`
- [x] `invalid-character-esc` / `E2513` (`PLE2513`)
- [x] `invalid-character-nul` / `E2514` (`PLE2514`)
- [x] `invalid-character-sub` / `E2512` (`PLE2512`)
- [x] `invalid-character-zero-width-space` / `E2515` (`PLE2515`)
- [ ] `invalid-class-object` / `E0243`
- [ ] `invalid-enum-extension` / `E0244`
- [ ] `invalid-envvar-value` / `E1507`
- [ ] `invalid-format-returned` / `E0311`
- [ ] `invalid-getnewargs-ex-returned` / `E0313`
- [ ] `invalid-getnewargs-returned` / `E0312`
- [x] `invalid-hash-returned` / `E0309` (`PLE0309`)
- [x] `invalid-index-returned` / `E0305` (`PLE0305`)
- [ ] `invalid-length-hint-returned` / `E0310`
- [x] `invalid-length-returned` / `E0303` (`PLE0303`)
- [ ] `invalid-metaclass` / `E1139`
- [ ] `invalid-repr-returned` / `E0306`
- [ ] `invalid-sequence-index` / `E1126`
- [ ] `invalid-slice-index` / `E1127`
- [ ] `invalid-slice-step` / `E1144`
- [ ] `invalid-slots` / `E0238`
- [ ] `invalid-slots-object` / `E0236`
- [ ] `invalid-star-assignment-target` / `E0113`
- [ ] `invalid-str-returned` / `E0307`
- [ ] `invalid-unary-operand-type` / `E1130`
- [ ] `invalid-unicode-codec` / `E2501`
- [ ] `logging-format-truncated` / `E1201`
- [x] `logging-too-few-args` / `E1206` (`PLE1206`)
- [x] `logging-too-many-args` / `E1205` (`PLE1205`)
- [ ] `logging-unsupported-format` / `E1200`
- [ ] `method-hidden` / `E0202`
- [x] `misplaced-bare-raise` / `E0704` (`PLE0704`)
- [ ] `misplaced-format-function` / `E0119`
- [x] `missing-format-string-key` / `E1304`  (`F524`)
- [ ] `missing-kwoa` / `E1125`
- [x] `mixed-format-string` / `E1302` (`F506`)
- [ ] `modified-iterating-dict` / `E4702`
- [x] `modified-iterating-set` / `E4703` (`PLE4703`)
- [ ] `no-member` / `E1101`
- [ ] `no-method-argument` / `E0211`
- [ ] `no-name-in-module` / `E0611`
- [x] `no-self-argument` / `E0213` (`N805`)
- [ ] `no-value-for-parameter` / `E1120`
- [ ] `non-iterator-returned` / `E0301`
- [x] `nonexistent-operator` / `E0107` (`B002`)
- [x] `nonlocal-and-global` / `E0115` (`PLE0115`)
- [x] `nonlocal-without-binding` / `E0117` (`PLE0117`)
- [ ] `not-a-mapping` / `E1134`
- [ ] `not-an-iterable` / `E1133`
- [ ] `not-async-context-manager` / `E1701`
- [ ] `not-callable` / `E1102`
- [ ] `not-context-manager` / `E1129`
- [x] `not-in-loop` / `E0103` (`F701`, `F702`)
- [x] `notimplemented-raised` / `E0711` (`F901`)
- [x] `potential-index-error` / `E0643` (`PLE0643`)
- [ ] `raising-bad-type` / `E0702`
- [ ] `raising-non-exception` / `E0710`
- [ ] `redundant-keyword-arg` / `E1124`
- [x] `relative-beyond-top-level` / `E0402` (`TID252`)
- [x] `repeated-keyword` / `E1132` (`PLE1132`)
- [x] ~`return-arg-in-generator` / `E0106`~
- [x] `return-in-init` / `E0101` (`PLE0101`)
- [x] `return-outside-function` / `E0104` (`F706`)
- [x] `singledispatch-method` / `E1519` (`PLE1519`)
- [x] `singledispatchmethod-function` / `E1520` (`PLE1520`)
- [x] `star-needs-assignment-target` / `E0114` (syntax error in preview)
- [x] `syntax-error` / `E0001` (always enabled)
- [x] `too-few-format-args` / `E1306` (`F524`)
- [x] `too-many-format-args` / `E1305` (`F522`)
- [ ] `too-many-function-args` / `E1121`
- [x] `too-many-star-expressions` / `E0112` (`F622`)
- [x] `truncated-format-string` / `E1301` (`F501`)
- [x] `undefined-all-variable` / `E0603` (`F822`)
- [x] `undefined-variable` / `E0602` (`F821`)
- [ ] `unexpected-keyword-arg` / `E1123`
- [x] `unexpected-special-method-signature` / `E0302` (`PLE0302`)
- [ ] `unhashable-member` / `E1143`
- [ ] `unpacking-non-sequence` / `E0633`
- [ ] `unsubscriptable-object` / `E1136`
- [ ] `unsupported-assignment-operation` / `E1137`
- [ ] `unsupported-binary-operation` / `E1131`
- [ ] `unsupported-delete-operation` / `E1138`
- [ ] `unsupported-membership-test` / `E1135`
- [x] `used-before-assignment` / `E0601` (`F821`)
- [x] `used-prior-global-declaration` / `E0118` (`PLE0118`)
- [x] `yield-inside-async-function` / `E1700` (`PLE1700`)
- [x] `yield-outside-function` / `E0105` (`F704`)

# Warning

- [ ] `abstract-method` / `W0223`
- [x] `anomalous-backslash-in-string` / `W1401` (`W605`)
- [ ] `anomalous-unicode-escape-in-string` / `W1402`
- [ ] `arguments-differ` / `W0221`
- [ ] `arguments-out-of-order` / `W1114`
- [ ] `arguments-renamed` / `W0237`
- [x] `assert-on-string-literal` / `W0129` (`PLW0129`)
- [x] `assert-on-tuple` / `W0199` (`F631`)
- [ ] `attribute-defined-outside-init` / `W0201`
- [ ] `bad-builtin` / `W0141`
- [ ] `bad-chained-comparison` / `W3601`
- [x] `bad-dunder-name` / `W3201` (`PLW3201`)
- [x] `bad-format-string` / `W1302` (`F521`)
- [ ] `bad-format-string-key` / `W1300`
- [x] `bad-indentation` / `W0311` (`E111`)
- [x] `bad-open-mode` / `W1501` (`PLW1501`)
- [x] `bad-staticmethod-argument` / `W0211` (`PLW0211`)
- [ ] `bad-thread-instantiation` / `W1506`
- [x] `bare-except` / `W0702` (`E722`)
- [x] `binary-op-exception` / `W0711` (`PLW0711`)
- [x] ~`boolean-datetime` / `W1502`~
- [x] `broad-exception-caught` / `W0718` (`BLE001`)
- [x] `broad-exception-raised` / `W0719` (`TRY002`)
- [x] `cell-var-from-loop` / `W0640` (`B023`)
- [ ] `comparison-with-callable` / `W0143`
- [ ] `confusing-with-statement` / `W0124`
- [x] `consider-ternary-expression` / `W0160` (`SIM108`)
- [x] `dangerous-default-value` / `W0102` (`B006`)
- [ ] `deprecated-argument` / `W4903`
- [ ] `deprecated-class` / `W4904`
- [ ] `deprecated-decorator` / `W4905`
- [ ] `deprecated-method` / `W4902`
- [ ] `deprecated-module` / `W4901`
- [ ] `deprecated-typing-alias` / `W6001`
- [ ] `differing-param-doc` / `W9017`
- [ ] `differing-type-doc` / `W9018`
- [x] `duplicate-except` / `W0705` (`B014`)
- [x] `duplicate-key` / `W0109` (`F601`)
- [ ] `duplicate-string-formatting-argument` / `W1308`
- [x] `duplicate-value` / `W0130` (`B033`)
- [x] `eq-without-hash` / `W1641` (`PLW1641`)
- [x] `eval-used` / `W0123` (`S307`)
- [x] `exec-used` / `W0122` (`S102`)
- [x] `expression-not-assigned` / `W0106` (`B018`)
- [x] `f-string-without-interpolation` / `W1309` (`F541`)
- [x] `fixme` / `W0511` (`FIX001`, `FIX002`, `FIX003`, `FIX004`)
- [x] `forgotten-debug-statement` / `W1515` (`T100 `)
- [x] `format-combined-specification` / `W1305` (`F525`)
- [x] `format-string-without-interpolation` / `W1310` (`F541`)
- [x] `global-at-module-level` / `W0604` (`PLW0604`)
- [x] `global-statement` / `W0603` (`PLW0603`)
- [x] `global-variable-not-assigned` / `W0602` (`PLW0602`)
- [ ] `global-variable-undefined` / `W0601`
- [x] `implicit-str-concat` / `W1404` (`ISC001`)
- [x] `import-self` / `W0406` (`PLW0406`)
- [x] `inconsistent-quotes` / `W1405` (`Q000`)
- [x] `invalid-envvar-default` / `W1508` (`PLW1508`)
- [ ] `invalid-format-index` / `W1307`
- [ ] `invalid-overridden-method` / `W0236`
- [ ] `isinstance-second-argument-not-valid-type` / `W1116`
- [x] `keyword-arg-before-vararg` / `W1113` (`B026`)
- [x] `logging-format-interpolation` / `W1202` (`G001`)
- [x] `logging-fstring-interpolation` / `W1203` (`G004`)
- [x] `logging-not-lazy` / `W1201` (`G002`)
- [x] `lost-exception` / `W0150` (`B012`)
- [x] `method-cache-max-size-none` / `W1518` (`B019`)
- [x] `misplaced-future` / `W0410` (`F404`)
- [ ] `missing-any-param-doc` / `W9021`
- [x] `missing-format-argument-key` / `W1303` (`F524`)
- [ ] `missing-format-attribute` / `W1306`
- [ ] `missing-param-doc` / `W9015`
- [ ] `missing-parentheses-for-call-in-test` / `W0126`
- [ ] `missing-raises-doc` / `W9006`
- [ ] ~`missing-return-doc` / `W9011`~ (Pylint extension)
- [ ] `missing-return-type-doc` / `W9012`
- [ ] `missing-timeout` / `W3101`
- [ ] `missing-type-doc` / `W9016`
- [ ] `missing-yield-doc` / `W9013`
- [ ] `missing-yield-type-doc` / `W9014`
- [ ] `modified-iterating-list` / `W4701`
- [ ] `multiple-constructor-doc` / `W9005`
- [x] `named-expr-without-context` / `W0131` (`PLW0131`)
- [x] `nan-comparison` / `W0177` (`PLW0177`)
- [x] `nested-min-max` / `W3301` (`PLW3301`)
- [x] `non-ascii-file-name` / `W2402` (`N999`)
- [ ] `non-parent-init-called` / `W0233`
- [ ] `non-str-assignment-to-dunder-name` / `W1115`
- [ ] `overlapping-except` / `W0714`
- [ ] `overridden-final-method` / `W0239`
- [x] `pointless-exception-statement` / `W0133` (`PLW0133`)
- [x] `pointless-statement` / `W0104` (`B018`)
- [ ] `pointless-string-statement` / `W0105`
- [ ] `possibly-unused-variable` / `W0641`
- [ ] `preferred-module` / `W0407`
- [x] `protected-access` / `W0212` (`SLF001`)
- [x] `raise-missing-from` / `W0707` (`B904`)
- [ ] `raising-format-tuple` / `W0715`
- [ ] `redeclared-assigned-name` / `W0128`
- [x] `redefined-builtin` / `W0622` (`A001`)
- [x] `redefined-loop-name` / `W2901` (`PLW2901`)
- [ ] `redefined-outer-name` / `W0621`
- [ ] `redefined-slots-in-subclass` / `W0244`
- [ ] `redundant-returns-doc` / `W9008`
- [x] `redundant-u-string-prefix` / `W1406` (`UP025`)
- [ ] `redundant-unittest-assert` / `W1503`
- [ ] `redundant-yields-doc` / `W9010`
- [x] `reimported` / `W0404` (`F811`)
- [x] `self-assigning-variable` / `W0127` (`PLW0127`)
- [ ] `self-cls-assignment` / `W0642`
- [ ] `shallow-copy-environ` / `W1507`
- [ ] `signature-differs` / `W0222`
- [ ] `subclassed-final-class` / `W0240`
- [x] `subprocess-popen-preexec-fn` / `W1509` (`PLW1509`)
- [x] `subprocess-run-check` / `W1510` (`PLW1510`)
- [ ] `super-init-not-called` / `W0231`
- [x] `super-without-brackets` / `W0245` (`PLW0245`)
- [ ] `too-many-try-statements` / `W0717`
- [x] `try-except-raise` / `W0706` (`TRY302`)
- [ ] `unbalanced-dict-unpacking` / `W0644`
- [ ] `unbalanced-tuple-unpacking` / `W0632`
- [ ] `undefined-loop-variable` / `W0631`
- [x] ~`unknown-option-value` / `W0012`~
- [x] `unnecessary-ellipsis` / `W2301` (`PIE790)
- [x] `unnecessary-lambda` / `W0108` (`PLW0108`)
- [x] `unnecessary-pass` / `W0107` (`PIE790`)
- [x] `unnecessary-semicolon` / `W0301` (`E703`)
- [ ] `unreachable` / `W0101`
- [x] `unspecified-encoding` / `W1514` (`PLW1514`)
- [x] `unused-argument` / `W0613` (`ARG001`)
- [x] `unused-format-string-argument` / `W1304` (`F507`)
- [x] `unused-format-string-key` / `W1301` (`F504`)
- [x] `unused-import` / `W0611` (`F401`)
- [ ] `unused-private-member` / `W0238`
- [x] `unused-variable` / `W0612` (`F841`)
- [ ] `unused-wildcard-import` / `W0614`
- [x] `useless-else-on-loop` / `W0120` (`PLW0120`)
- [ ] `useless-param-doc` / `W9019`
- [ ] `useless-parent-delegation` / `W0246`
- [ ] `useless-type-doc` / `W9020`
- [x] `useless-with-lock` / `W2101` (`PLW2101`)
- [ ] `using-constant-test` / `W0125`
- [x] ~`using-f-string-in-unsupported-version` / `W2601`~
- [ ] `using-final-decorator-in-unsupported-version` / `W2602`
- [ ] `while-used` / `W0149`
- [x] `wildcard-import` / `W0401` (`F403`)
- [ ] `wrong-exception-operation` / `W0716`

# Convention

- [x] `bad-classmethod-argument` / `C0202` (`N804)
- [x] `bad-docstring-quotes` / `C0198` (`Q002`)
- [ ] `bad-file-encoding` / `C2503`
- [ ] `bad-mcs-classmethod-argument` / `C0204`
- [ ] `bad-mcs-method-argument` / `C0203`
- [x] `compare-to-empty-string` / `C1901` (`PLC1901`)
- [ ] `compare-to-zero` / `C2001`
- [x] `consider-iterating-dictionary` / `C0201` (`SIM118`)
- [x] `consider-using-any-or-all` / `C0501` (`SIM110`, `SIM111`)
- [x] `consider-using-dict-items` / `C0206` (`PLC0206`)
- [ ] `consider-using-enumerate` / `C0200`
- [ ] `consider-using-f-string` / `C0209`
- [ ] `dict-init-mutate` / `C3401`
- [ ] `disallowed-name` / `C0104`
- [x] `docstring-first-line-empty` / `C0199` (`D210`)
- [x] `empty-docstring` / `C0112` (`D419`)
- [x] `import-outside-toplevel` / `C0415` (`PLC0415`)
- [x] `import-private-name` / `C2701` (`PLC2701`)
- [ ] `invalid-characters-in-docstring` / `C0403`
- [x] `invalid-name` / `C0103` (`N815`)
- [x] `line-too-long` / `C0301` (`E501`)
- [x] `misplaced-comparison-constant` / `C2201` (`SIM300`)
- [x] `missing-class-docstring` / `C0115` (`D101`)
- [x] `missing-final-newline` / `C0304` (`W292`)
- [x] `missing-function-docstring` / `C0116` (`D103`)
- [x] `missing-module-docstring` / `C0114` (`D100`)
- [ ] `mixed-line-endings` / `C0327`
- [x] `multiple-imports` / `C0410` (`E401`)
- [x] `multiple-statements` / `C0321` (`E701`, `E702`)
- [x] `non-ascii-module-import` / `C2403` (`PLC2403`)
- [x] `non-ascii-name` / `C2401` (`PLC2401`)
- [x] `single-string-used-for-slots` / `C0205` (`PLC0205`)
- [x] `singleton-comparison` / `C0121` (`E711 `, `E712`)
- [ ] `superfluous-parens` / `C0325`
- [ ] ~`too-many-lines` / `C0302`~ (not compatible with the formatter)
- [x] `trailing-newlines` / `C0305` (`W391`)
- [x] `trailing-whitespace` / `C0303` (`W291`)
- [x] `typevar-double-variance` / `C0131` (`PLC0131`)
- [x] `typevar-name-incorrect-variance` / `C0105` (`PLC0105`)
- [x] `typevar-name-mismatch` / `C0132` (`PLC0132`)
- [ ] `unexpected-line-ending-format` / `C0328`
- [x] `ungrouped-imports` / `C0412` (`I001`)
- [x] `unidiomatic-typecheck` / `C0123` (`E721`)
- [x] `unnecessary-direct-lambda-call` / `C3002` (`PLC3002`)
- [x] `unnecessary-dunder-call` / `C2801` (`PLC2801`)
- [x] `unnecessary-lambda-assignment` / `C3001` (`E731`)
- [x] `unneeded-not` / `C0113` (`SIM208`)
- [ ] `use-implicit-booleaness-not-comparison` / `C1803`
- [x] `use-implicit-booleaness-not-len` / `C1802` (`PLC1802`)
- [x] `use-maxsplit-arg` / `C0207` (`PLC0207`)
- [x] `use-sequence-for-iteration` / `C0208` (`PLC0208`)
- [x] `useless-import-alias` / `C0414` (`PLC0414`)
- [x] `wrong-import-order` / `C0411` (`I001`)
- [x] `wrong-import-position` / `C0413` (`E402 `)
- [ ] `wrong-spelling-in-comment` / `C0401`
- [ ] `wrong-spelling-in-docstring` / `C0402`

# Refactor

- [ ] `chained-comparison` / `R1716`
- [x] `comparison-of-constants` / `R0133` (`PLR0133`)
- [x] `comparison-with-itself` / `R0124` (`PLR0124`)
- [ ] `condition-evals-to-constant` / `R1727`
- [ ] ~`confusing-consecutive-elif` / `R5601`~
- [x] `consider-alternative-union-syntax` / `R6003` (`UP007`)
- [x] `consider-merging-isinstance` / `R1701` (`SIM101`)
- [ ] `consider-swap-variables` / `R1712`
- [x] `consider-using-alias` / `R6002` (`UP006`)
- [ ] `consider-using-assignment-expr` / `R6103`
- [x] `consider-using-augmented-assign` / `R6104` (`PLR6104`)
- [x] `consider-using-dict-comprehension` / `R1717` (`C402`)
- [x] `consider-using-from-import` / `R0402` (`PLR0402`)
- [x] `consider-using-generator` / `R1728` (`C417`)
- [x] `consider-using-get` / `R1715` (`SIM401`)
- [x] `consider-using-in` / `R1714` (`PLR1714`)
- [ ] `consider-using-join` / `R1713`
- [x] `consider-using-min-builtin` / `R1730` (`PLR1730`)
- [x] `consider-using-max-builtin` / `R1731` (`PLR1730`)
- [ ] `consider-using-namedtuple-or-dataclass` / `R6101`
- [x] `consider-using-set-comprehension` / `R1718` (`C401`)
- [x] `consider-using-sys-exit` / `R1722` (`PLR1722`)
- [x] ~`consider-using-ternary` / `R1706` (`PLR1706 `)~
- [ ] `consider-using-tuple` / `R6102`
- [x] `consider-using-with` / `R1732` (`SIM115`)
- [ ] `cyclic-import` / `R0401`
- [ ] `duplicate-code` / `R0801`
- [x] `else-if-used` / `R5501` (`PLR5501`)
- [x] `empty-comment` / `R2044` (`PLR2044`)
- [x] `inconsistent-return-statements` / `R1710` (`RET501`, `RET502`)
- [x] `literal-comparison` / `R0123` (`F632`)
- [x] `magic-value-comparison` / `R2004` (`PLR2004`)
- [x] `no-classmethod-decorator` / `R0202` (`PLR0202`)
- [x] `no-else-break` / `R1723` (`RET508`)
- [x] `no-else-continue` / `R1724` (`RET507`)
- [x] `no-else-raise` / `R1720` (`RET506`)
- [x] `no-else-return` / `R1705` (`RET505`)
- [x] `no-self-use` / `R6301` (`PLR6301`)
- [x] `no-staticmethod-decorator` / `R0203` (`PLR0203`)
- [x] `property-with-parameters` / `R0206` (`PLR0206`)
- [x] `redefined-argument-from-local` / `R1704` (`PLR1704`)
- [ ] `redefined-variable-type` / `R0204`
- [ ] `simplifiable-condition` / `R1726`
- [x] `simplifiable-if-expression` / `R1719` (`SIM210`, `SIM211`)
- [x] `simplifiable-if-statement` / `R1703` (`SIM108`)
- [ ] `simplify-boolean-expression` / `R1709`
- [ ] `stop-iteration-return` / `R1708`
- [x] `super-with-arguments` / `R1725` (`UP008`)
- [x] `too-complex` / `R1260` (`C901`)
- [ ] `too-few-public-methods` / `R0903`
- [ ] `too-many-ancestors` / `R0901` (requires multifile analysis)
- [x] `too-many-arguments` / `R0913` (`PLR0913`)
- [x] `too-many-boolean-expressions` / `R0916` (`PLR0916`)
- [x] `too-many-positional-arguments` /  `R0917` (`PLR0917`)
- [x] `too-many-branches` / `R0912` (`PLR0912`)
- [ ] ~`too-many-instance-attributes` / `R0902`~ (requires #1774)
- [x] `too-many-locals` / `R0914` (`PLR0914`)
- [x] `too-many-nested-blocks` / `R1702` (`PLR1702`)
- [x] `too-many-public-methods` / `R0904` (`PLR0904`)
- [x] `too-many-return-statements` / `R0911` (`PLR0911`)
- [x] `too-many-statements` / `R0915` (`PLR0915`)
- [x] `trailing-comma-tuple` / `R1707` (`COM818`)
- [x] `unnecessary-comprehension` / `R1721` (`C416`)
- [x] `use-a-generator` / `R1729` (`C419`)
- [x] `unnecessary-dict-index-lookup` / `R1733` (`PLR1733`)
- [x] `use-list-literal` / `R1734` (`C405`)
- [x] `use-dict-literal` / `R1735` (`C406`)
- [x] `use-list-literal` / `R1734` (`C405`)
- [x] `use-yield-from` / `R1737` (`UP028`)
- [x] `use-set-for-membership` / `R6201` (`PLR6201`)
- [x] `useless-object-inheritance` / `R0205` (`UP004`)
- [x] ~`useless-option-value` / `R0022`~
- [x] `useless-return` / `R1711` (`PLR1711`)
```

</details>

---

_Comment by @NeilGirdhar on 2025-06-18 17:11_

> Is it possible to get the implemented/not implemented list as free text somewhere. I want to toggle off pylint stuff that ruff is handling but it seems difficult to get the current data out of the description at the top.

There's also a script somewhere in the 1000 comments on this issue that collects it for you into a nice pylint ignore, I think.

Also, if you're using a type checker, most of the remaining pylint rules are handled by the type checker.  I don't know how many pylint linter rules are actually left.  Probably only a few?

Finally, maybe it's time to close this issue and reopen just to start fresh?

---

_Comment by @johnklehm on 2025-06-18 18:35_

That's fair re most are already implemented.  It was my understanding though that ruff still didn't do the [multi-file analysis](https://github.com/astral-sh/ruff/issues/7447) yet (pending the type checker work stabilizing). My pulse check on this was pylint's `attribute-defined-outside-init`. I think it's tracked here: https://github.com/astral-sh/ruff/issues/3040 but also in this issue.   Re type checker handling it, at least in this specific case not so I think: https://github.com/python/mypy/issues/10552

Thanks a ton for the tip on how to pull the issue as json, helps a ton!

---

_Referenced in [astral-sh/ruff#18796](../../astral-sh/ruff/pulls/18796.md) on 2025-06-19 17:22_

---

_Referenced in [virt-manager/virt-manager#933](../../virt-manager/virt-manager/pulls/933.md) on 2025-06-20 12:01_

---

_Referenced in [ankitects/anki#4110](../../ankitects/anki/issues/4110.md) on 2025-06-21 02:18_

---

_Comment by @cidrblock on 2025-07-14 19:20_

Over the weekend I (with ai help) created a pre-commit hook that updates the pyproject.toml file to "enable" the remaining pylint rules (since the 50% mark has been crossed) It works with precommit ci with an internal cache, updated weekly based on parsing this issue.

I don't have the bandwidth to maintain it and was hoping I could donate it to someone to run with.  I think it's solid code and theres a fair number of tests.  It's a little opinionated with regard to toml formatting.

If anyone would like to maintain it, fork it, steal it, reference it, please LMK.  I will probably take it down in a week or so if no one is interested.  Please mention me here or put an issue in the repo please.

The repo is here: https://github.com/cidrblock/pylint-ruff-sync
and the first PR using it is here: https://github.com/ansible/ansible-creator/pull/443/files

ty team ruff again, you all are doing great things.

-cidrblock



---

_Referenced in [C2SM/icon4py#821](../../C2SM/icon4py/pulls/821.md) on 2025-07-31 07:58_

---

_Comment by @friday on 2025-08-03 23:09_

`F811` is marked as an equivalent for "reimported" (W0404), but it doesn't catch these, which pylint W0404 does:

```py
import os
import os as os2
```

```py
import module

def path_exists() -> None:
    import module  # deferred import for performance or cyclic import prevention

    ...
```

---

_Comment by @ntBre on 2025-08-04 22:34_

There's some discussion of related cases here: https://github.com/astral-sh/ruff/issues/10638, not the different name, though. Maybe that would justify a different rule.

---

_Referenced in [axolotl-ai-cloud/axolotl#3092](../../axolotl-ai-cloud/axolotl/pulls/3092.md) on 2025-08-22 04:40_

---

_Comment by @jamesbraza on 2025-08-25 17:43_

I think `flake8-bandit`'s [`request-without-timeout` (`S113`)](https://docs.astral.sh/ruff/rules/request-without-timeout/#request-without-timeout-s113) has overlap with `pylint`'s [`missing-timeout` (`W3101`)](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/missing-timeout.html), fyi

---

_Referenced in [MetOffice/fab#489](../../MetOffice/fab/issues/489.md) on 2025-08-29 14:32_

---

_Referenced in [pyslackers/website#561](../../pyslackers/website/pulls/561.md) on 2025-09-02 16:40_

---

_Referenced in [KittyCAD/zoo-mcp#2](../../KittyCAD/zoo-mcp/pulls/2.md) on 2025-09-05 17:25_

---

_Comment by @hseg on 2025-09-15 13:56_

Just noticed another rule that's missing: `possibly-used-before-assignment` (E0606):
```python
if p:
  if q:
    x = True
else:
  x = False
print(x)
```


---

_Referenced in [pydata/xarray#10743](../../pydata/xarray/pulls/10743.md) on 2025-09-16 11:41_

---

_Comment by @mlkauppila on 2025-09-24 11:44_

I have a question about the `protected-access / W0212 (SLF001)` rule:

Why is this violating the rule:
```
import my_module
my_module._function() # Pylint/flake8-self warning here
```

While this is not:
```
from my_module import _function
_function() # OK
```

Obviously they are functionally identical. If this is intended, is there another rule I could use to flag the second code snippet as rule violation?

---

_Comment by @MichaReiser on 2025-09-24 12:14_

@mlkauppila please open a new issue to discuss questions specific to a rule. 

There are many people subscribed to this issue, and we want to avoid pinging them unnecessarily. 

---

_Referenced in [astral-sh/ruff#20531](../../astral-sh/ruff/issues/20531.md) on 2025-09-25 19:29_

---

_Referenced in [astral-sh/ruff#20597](../../astral-sh/ruff/pulls/20597.md) on 2025-09-26 21:25_

---

_Comment by @bigcat88 on 2025-09-30 17:33_

> no-value-for-parameter / `E1120` is what I expected to be implemented, any timelines on this or how I can enable it manually(within ruff)?
> 
> Nevermind, It's not covered by Pyflakes rules so probably impossible to implement

it is a very good rule. having `pylint` in repo only for that rule is something that makes me cry

---

_Comment by @ntBre on 2025-09-30 18:01_

I think `E1120` in particular is probably a better candidate for a rule enforced by a type checker or at least a type-aware linter. [ty](https://play.ty.dev/dcccccb9-c45b-4cb7-ba5e-92980770b4a2), for example, already detects the example from the [pylint docs](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/no-value-for-parameter.html).

---

_Comment by @lucaspar on 2025-09-30 18:06_

@bigcat88 maybe pylint is faster with just one rule

---

_Comment by @Pierre-Sassoulas on 2025-09-30 18:46_

> @bigcat88 maybe pylint is faster with just one rule

The performance cost in pylint is primarily paid when rebuilding the ast and then secondly when additional inference is required for the first time, so it's possible that a single message is faster because it doesn't need as much inference as all the checks but rebuilding the ast is done even for a single check so the speed up is not going to be linear (if you have one check, you may as well have most checks except "freestyle" checks like cyclic-import, mccabe, or duplicate-code that have their own algorithm).

---

_Comment by @lucaspar on 2025-09-30 18:54_

it was tongue-in-cheek, but thanks for the clarification 😅 

---

_Referenced in [vil02/puzzle_generator#366](../../vil02/puzzle_generator/pulls/366.md) on 2025-10-05 20:49_

---

_Referenced in [xdslproject/xdsl#5379](../../xdslproject/xdsl/pulls/5379.md) on 2025-10-17 12:12_

---

_Comment by @ustulation on 2025-10-23 10:33_

`unnecessary-lambda / W0108 (PLW0108)` is marked as done (assuming a checked box in the OP means that) but it isn't:

```
"""Test."""

from dataclasses import dataclass, field


def func() -> str:
    """Test."""
    return "ss"


@dataclass
class Abar:
    """Test."""

    ab: str = field(default_factory=lambda: func())
```

then:

```
$ ruff --version
ruff 0.14.1

$ ruff check --select ALL main.py
warning: `incorrect-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `incorrect-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
All checks passed!
```

but:

```
$ pylint main.py
************* Module main
main.py:15:36: W0108: Lambda may not be necessary (unnecessary-lambda)

------------------------------------------------------------------
Your code has been rated at 8.75/10 (previous run: 8.89/10, -0.14)
```

---

_Comment by @jenshnielsen on 2025-10-23 11:58_

@ustulation if you consult https://docs.astral.sh/ruff/rules/unnecessary-lambda/ you will see that the rule is in preview so you would need to add the ``--preview`` flag until it is stabilized to be able to use it.

---

_Comment by @BLKSerene on 2025-10-31 13:41_

`stop-iteration-return / R1708` was implemented in 0.14.3 and could be checked off.

---

_Comment by @vintitres on 2025-11-06 04:05_

I noticed that
- PLE0307
- PLE1141
- PLE1507
- PLR1708
- PLR1736
- PLW0244
- PLW0642
- PLW1507

are available in ruff 0.14.3, but not annotated off here.

---

_Comment by @jamesbraza on 2025-12-01 20:28_

Fwiw I don't see `possibly-used-before-assignment` in the OP list

---

_Referenced in [astral-sh/ruff#21853](../../astral-sh/ruff/pulls/21853.md) on 2025-12-08 22:52_

---

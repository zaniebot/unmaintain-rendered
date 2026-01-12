```yaml
number: 12885
title: 0.5.7 tests fail on 32bit systems
type: issue
state: closed
author: WhyNotHugo
labels: []
assignees: []
created_at: 2024-08-14T09:18:56Z
updated_at: 2024-08-16T18:16:58Z
url: https://github.com/astral-sh/ruff/issues/12885
synced_at: 2026-01-12T15:54:52Z
```

# 0.5.7 tests fail on 32bit systems

---

_@WhyNotHugo_

```
failures:
---- rules::refurb::tests::rules::rule_implicitcwd_path_new_furb177_py_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB177_FURB177.py.snap
Snapshot: FURB177_FURB177.py
Source: crates/ruff_linter/src/rules/refurb/mod.rs:55
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
   10    10 │ 2 2 | from pathlib import Path
   11    11 │ 3 3 | 
   12    12 │ 4 4 | # Errors
   13    13 │ 5   |-_ = Path().resolve()
   14       │-  5 |+_ = pathlib.Path.cwd()
         14 │+  5 |+_ = Path.cwd()
   15    15 │ 6 6 | _ = pathlib.Path().resolve()
   16    16 │ 7 7 | 
   17    17 │ 8 8 | _ = Path("").resolve()
   18    18 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   31    31 │ 3 3 | 
   32    32 │ 4 4 | # Errors
   33    33 │ 5 5 | _ = Path().resolve()
   34    34 │ 6   |-_ = pathlib.Path().resolve()
   35       │-  6 |+_ = pathlib.Path.cwd()
         35 │+  6 |+_ = Path.cwd()
   36    36 │ 7 7 | 
   37    37 │ 8 8 | _ = Path("").resolve()
   38    38 │ 9 9 | _ = pathlib.Path("").resolve()
   39    39 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   51    51 │ 5 5 | _ = Path().resolve()
   52    52 │ 6 6 | _ = pathlib.Path().resolve()
   53    53 │ 7 7 | 
   54    54 │ 8   |-_ = Path("").resolve()
   55       │-  8 |+_ = pathlib.Path.cwd()
         55 │+  8 |+_ = Path.cwd()
   56    56 │ 9 9 | _ = pathlib.Path("").resolve()
   57    57 │ 10 10 | 
   58    58 │ 11 11 | _ = Path(".").resolve()
   59    59 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   71    71 │ 6  6  | _ = pathlib.Path().resolve()
   72    72 │ 7  7  | 
   73    73 │ 8  8  | _ = Path("").resolve()
   74    74 │ 9     |-_ = pathlib.Path("").resolve()
   75       │-   9  |+_ = pathlib.Path.cwd()
         75 │+   9  |+_ = Path.cwd()
   76    76 │ 10 10 | 
   77    77 │ 11 11 | _ = Path(".").resolve()
   78    78 │ 12 12 | _ = pathlib.Path(".").resolve()
   79    79 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   91    91 │ 8  8  | _ = Path("").resolve()
   92    92 │ 9  9  | _ = pathlib.Path("").resolve()
   93    93 │ 10 10 | 
   94    94 │ 11    |-_ = Path(".").resolve()
   95       │-   11 |+_ = pathlib.Path.cwd()
         95 │+   11 |+_ = Path.cwd()
   96    96 │ 12 12 | _ = pathlib.Path(".").resolve()
   97    97 │ 13 13 | 
   98    98 │ 14 14 | _ = Path("", **kwargs).resolve()
   99    99 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
  111   111 │ 9  9  | _ = pathlib.Path("").resolve()
  112   112 │ 10 10 | 
  113   113 │ 11 11 | _ = Path(".").resolve()
  114   114 │ 12    |-_ = pathlib.Path(".").resolve()
  115       │-   12 |+_ = pathlib.Path.cwd()
        115 │+   12 |+_ = Path.cwd()
  116   116 │ 13 13 | 
  117   117 │ 14 14 | _ = Path("", **kwargs).resolve()
  118   118 │ 15 15 | _ = pathlib.Path("", **kwargs).resolve()
  119   119 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
  131   131 │ 11 11 | _ = Path(".").resolve()
  132   132 │ 12 12 | _ = pathlib.Path(".").resolve()
  133   133 │ 13 13 | 
  134   134 │ 14    |-_ = Path("", **kwargs).resolve()
  135       │-   14 |+_ = pathlib.Path.cwd()
        135 │+   14 |+_ = Path.cwd()
  136   136 │ 15 15 | _ = pathlib.Path("", **kwargs).resolve()
  137   137 │ 16 16 | 
  138   138 │ 17 17 | _ = Path(".", **kwargs).resolve()
  139   139 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
  151   151 │ 12 12 | _ = pathlib.Path(".").resolve()
  152   152 │ 13 13 | 
  153   153 │ 14 14 | _ = Path("", **kwargs).resolve()
  154   154 │ 15    |-_ = pathlib.Path("", **kwargs).resolve()
  155       │-   15 |+_ = pathlib.Path.cwd()
        155 │+   15 |+_ = Path.cwd()
  156   156 │ 16 16 | 
  157   157 │ 17 17 | _ = Path(".", **kwargs).resolve()
  158   158 │ 18 18 | _ = pathlib.Path(".", **kwargs).resolve()
  159   159 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
  171   171 │ 14 14 | _ = Path("", **kwargs).resolve()
  172   172 │ 15 15 | _ = pathlib.Path("", **kwargs).resolve()
  173   173 │ 16 16 | 
  174   174 │ 17    |-_ = Path(".", **kwargs).resolve()
  175       │-   17 |+_ = pathlib.Path.cwd()
        175 │+   17 |+_ = Path.cwd()
  176   176 │ 18 18 | _ = pathlib.Path(".", **kwargs).resolve()
  177   177 │ 19 19 | 
  178   178 │ 20 20 | # OK
  179   179 │ 
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
  191   191 │ 15 15 | _ = pathlib.Path("", **kwargs).resolve()
  192   192 │ 16 16 | 
  193   193 │ 17 17 | _ = Path(".", **kwargs).resolve()
  194   194 │ 18    |-_ = pathlib.Path(".", **kwargs).resolve()
  195       │-   18 |+_ = pathlib.Path.cwd()
        195 │+   18 |+_ = Path.cwd()
  196   196 │ 19 19 | 
  197   197 │ 20 20 | # OK
  198   198 │ 21 21 | _ = Path.cwd()
────────────┴───────────────────────────────────────────────────────────────────
```

Full log for x86: https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/jobs/1482783

Full log for armv7: https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/jobs/1482787

Full log on armhf: https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/jobs/1482788

---

_Comment by @WhyNotHugo on 2024-08-14 09:20_

I'm not entirely sure if these are just test failures, or if ruff is actually producing the wrong output on these platforms.

I'm not sure if it's safe to ignore these tests, or if they're actually reflecting an issue that we wouldn't want to reach end users.

---

_Comment by @MichaReiser on 2024-08-14 09:20_

I suspect it's another case where the rule uses a `HashMap` and iterates over the elements which isn't guaranteed to give a stable result.

---

_Comment by @WhyNotHugo on 2024-08-14 09:25_

Thanks for the quick response. I'll hold the update instead of skipping the test.

---

_Comment by @MichaReiser on 2024-08-14 09:25_

Although not sure which hash map it is in this case... I don't see one in the rule itself

---

_Comment by @MichaReiser on 2024-08-14 09:38_

Okay, my suspicion is that it is due to `bindings` being a `FxHashMap` in 

https://github.com/astral-sh/ruff/blob/aa0db338d935aa40fafaa05feff7343c42491d8e/crates/ruff_python_semantic/src/model.rs#L909-L910

That means there's no guarantee which import we handle first. 

---

_Comment by @MichaReiser on 2024-08-14 09:42_

Could you give https://github.com/astral-sh/ruff/pull/12888/ a try. I don't have a 32bit machine at hand.

---

_Closed by @MichaReiser on 2024-08-16 18:16_

---

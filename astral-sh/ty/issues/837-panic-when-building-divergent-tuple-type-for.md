```yaml
number: 837
title: Panic when building divergent tuple type for implicit instance attributes
type: issue
state: closed
author: qarmin
labels:
  - fatal
assignees: []
created_at: 2025-07-17T10:23:43Z
updated_at: 2025-10-22T12:40:45Z
url: https://github.com/astral-sh/ty/issues/837
synced_at: 2026-01-10T02:06:24Z
```

# Panic when building divergent tuple type for implicit instance attributes

---

_Issue opened by @qarmin on 2025-07-17 10:23_

### Summary

```
class PipelneStage(object:
 def(  self
 run,
   if isinstance(run,PipelneStage) self._run_everywhere=  ,run._run_everywhere if r run
```
freeze ty, checked with normal default check command `ty check b.py`

[b.py.zip](https://github.com/user-attachments/files/21293229/b.py.zip)

### Version

ty unknown (39bcba418 2025-07-15)

---

_Label `hang` added by @AlexWaygood on 2025-07-17 10:31_

---

_Comment by @denivyruck on 2025-07-17 22:36_

Hmmm it ran fine for me (no freeze):

```
cargo run -p ty -- check /Users/denivyruck/workspace/projects/ty/ruff/crates/ty_python_semantic/resources/mdtest/class/huehue.py
   Compiling ty_python_semantic v0.0.0 (/Users/denivyruck/workspace/projects/ty/ruff/crates/ty_python_semantic)
   Compiling ty_ide v0.0.0 (/Users/denivyruck/workspace/projects/ty/ruff/crates/ty_ide)
   Compiling ty_project v0.0.0 (/Users/denivyruck/workspace/projects/ty/ruff/crates/ty_project)
   Compiling ty_server v0.0.0 (/Users/denivyruck/workspace/projects/ty/ruff/crates/ty_server)
   Compiling ty v0.0.0 (/Users/denivyruck/workspace/projects/ty/ruff/crates/ty)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 4.99s
     Running `target/debug/ty check /Users/denivyruck/workspace/projects/ty/ruff/crates/ty_python_semantic/resources/mdtest/class/huehue.py`
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-syntax]
 --> crates/ty_python_semantic/resources/mdtest/class/huehue.py:1:26
  |
1 | class PipelneStage(object:
  |                          ^ Expected ',', found ':'
2 |     def(  self
3 |         run,
  |

error[invalid-syntax]
 --> crates/ty_python_semantic/resources/mdtest/class/huehue.py:1:27
  |
1 | class PipelneStage(object:
  |                           ^ Expected ')', found newline
2 |     def(  self
3 |         run,
  |

error[invalid-syntax]
```

---

_Comment by @sharkdp on 2025-07-18 07:12_

> Hmmm it ran fine for me (no freeze):

I can reproduce the error.



@qarmin Thank you for reporting this. Step by step (ensuring that it still hangs at every stage), I transformed your example into a valid-syntax program that shows the same effect:
```py
class C:
    def f(self, other: "C", flag: bool):
        self.x = (1, other.x if flag else other)
```

Now we can see that this builds a divergent tuple type for `C.x`. So I am fairly confident that this is the same underlying issue as #765.

---

_Closed by @sharkdp on 2025-07-18 07:12_

---

_Comment by @AlexWaygood on 2025-07-30 16:29_

The example in #765 no longer hangs on `main` (so the issue has been closed), but @sharkdp's example in https://github.com/astral-sh/ty/issues/837#issuecomment-3087582514 still does, so I'm reopening this issue

---

_Reopened by @AlexWaygood on 2025-07-30 16:29_

---

_Renamed from "Freeze, when trying to check invalid python file" to "Hang when building divergent tuple type for implicit instance attributes" by @AlexWaygood on 2025-07-30 16:29_

---

_Comment by @qarmin on 2025-08-02 08:28_

I'm not really able to determine the exact reason for the timeout, so I'm uploading this batch of files that are taking way too long to process, instead of creating new (potentially duplicate) issues - [More Freezes.zip](https://github.com/user-attachments/files/21571259/More.Freezes.zip)

---

_Comment by @AlexWaygood on 2025-08-10 15:35_

> ```py
> class C:
>     def f(self, other: "C", flag: bool):
>         self.x = (1, other.x if flag else other)
> ```

The good news is that following https://github.com/astral-sh/ruff/commit/5a116e48c3dff2a52526258951785ff627d8fea2, we no longer hang forever when trying to check this snippet. The bad news is... we now panic:

```
error[panic]: Panicked at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/940f9c0/src/function/execute.rs:204:25 when checking `/Users/alexw/dev/ruff/bar.py`: `infer_expression_types(Id(1801)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.12.7+126 (8230b7982 2025-08-09)
info: Args: ["target/debug/ty", "check", "bar.py", "--python-version=3.13"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_scope_types(Id(1002))
             at crates/ty_python_semantic/src/types/infer.rs:134
   1: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:527
```

In a way this feels like progress of a sort: a quick panic feels preferable to an indefinite execution time. It's obviously not an ideal end state, however 😆

It still panics on the PR branches for https://github.com/astral-sh/ruff/pull/19669 and https://github.com/astral-sh/ruff/pull/19811.

---

_Renamed from "Hang when building divergent tuple type for implicit instance attributes" to "Panic when building divergent tuple type for implicit instance attributes" by @carljm on 2025-08-11 16:32_

---

_Label `fatal` added by @carljm on 2025-08-11 16:32_

---

_Label `hang` removed by @carljm on 2025-08-11 16:32_

---

_Added to milestone `GA` by @AlexWaygood on 2025-08-15 15:17_

---

_Comment by @AlexWaygood on 2025-10-01 16:01_

It looks like this was fixed in https://github.com/astral-sh/ruff/pull/20333 (and it looks like a regression test was added in that PR that is almost identical to @sharkdp's repro in https://github.com/astral-sh/ty/issues/837#issuecomment-3087582514). @carljm, is there any value in leaving this issue open at this point? Do we have plans to improve our type inference here over and above using `Type::Divergent`, or should we just close this?

---

_Comment by @carljm on 2025-10-01 16:05_

I don't think (even in principle) there _is_ a better way to handle this than `Type::Divergent`, so no, I don't think anything else is planned here; we can close this.

---

_Closed by @carljm on 2025-10-01 16:05_

---

_Reopened by @sharkdp on 2025-10-21 12:40_

---

_Comment by @sharkdp on 2025-10-21 13:07_

When we work on this again, I think we should also consider a case like the following, where the tuple type is not directly created using a syntactic `(..)` construct:
```py
from typing import TypeVar, Literal

T = TypeVar("T")

def make_tuple(a: T) -> tuple[T, Literal[1]]:
    return (a, 1)

class D:
    def f(self, other: "D"):
        self.x = make_tuple(other.x)

reveal_type(D().x)
```

---

_Closed by @sharkdp on 2025-10-22 12:29_

---

_Comment by @MichaReiser on 2025-10-22 12:31_

@sharkdp should we keep this open or do you want to open a new issue to solve this "properly"?

---

_Comment by @sharkdp on 2025-10-22 12:40_

I will open a new issue. It's not only related to this case, I think.

---

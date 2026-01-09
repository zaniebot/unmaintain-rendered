---
number: 13710
title: "[red-knot] red-knot panics by looking at certain Python files within ruff's repo"
type: issue
state: closed
author: rtpg
labels:
  - bug
  - ty
assignees: []
created_at: 2024-10-11T01:07:08Z
updated_at: 2024-11-14T10:08:30Z
url: https://github.com/astral-sh/ruff/issues/13710
synced_at: 2026-01-07T13:12:15-06:00
---

# [red-knot] red-knot panics by looking at certain Python files within ruff's repo

---

_Issue opened by @rtpg on 2024-10-11 01:07_

The following is a log of the 24 files caught by a `crates/**/*.py` glob that will panic if `red_knot` looks at them. These are grouped by the panic output, which should give an idea of the scale of each error.

I put the script I used to get this below, the short version is that the script copies single files into a temporary directory and then points `red_knot` at that.

```
Failures
-------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/semantic_index/builder.rs:882:17:
assertion failed: self.current_assignment.is_none()
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 2 files)
[PosixPath('crates/ruff_python_parser/resources/invalid/statements/invalid_augmented_assignment_target.py'), PosixPath('crates/ruff_python_parser/resources/invalid/statements/invalid_assignment_targets.py')]
------------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/infer.rs:2993:17:
name in a type expression is always 'load' but got: 'Invalid'
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 6 files)
[PosixPath('crates/ruff_python_parser/resources/invalid/statements/match/as_pattern_2.py'), PosixPath('crates/ruff_python_parser/resources/inline/err/if_stmt_misspelled_elif.py'), PosixPath('crates/ruff_python_parser/resources/inline/err/try_stmt_misspelled_except.py'), PosixPath('crates/ruff_python_formatter/resources/test/fixtures/ruff/range_formatting/clause_header.py'), PosixPath('crates/ruff_python_formatter/resources/test/fixtures/ruff/range_formatting/range_narrowing.py'), PosixPath('crates/ruff_python_formatter/resources/test/fixtures/ruff/range_formatting/decorators.py')]
------------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/infer.rs:1714:9:
assertion `left == right` failed
  left: Some(Unknown)
 right: None
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 2 files)
[PosixPath('crates/ruff_python_parser/resources/invalid/expressions/if/missing_test_expr_0.py'), PosixPath('crates/ruff_python_parser/resources/invalid/expressions/if/missing_test_expr_1.py')]
------------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/semantic_index/builder.rs:215:9:
assertion `left == right` failed
  left: Some(Definition { [salsa id]: Id(1401), file: File { [salsa id]: Id(c00), path: System("/var/folders/qp/0fmxxq1x0vx9kfgn_qpjkj7r0000gn/T/tmpn08eo1bp/file.py"), permissions: Some(33188), revision: FileRevision(31887187922474138777644432830), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } }, file_scope: FileScopeId(0), symbol: ScopedSymbolId(0), kind: NamedExpression(AstNodeRef(ExprNamed { range: 92..108, target: List(ExprList { range: 92..98, elts: [Name(ExprName { range: 93..94, id: Name("x"), ctx: Store }), Name(ExprName { range: 96..97, id: Name("y"), ctx: Store })], ctx: Store }), value: List(ExprList { range: 102..108, elts: [NumberLiteral(ExprNumberLiteral { range: 103..104, value: Int(1) }), NumberLiteral(ExprNumberLiteral { range: 106..107, value: Int(2) })], ctx: Load }) })), count: Count { ghost: PhantomData<fn(red_knot_python_semantic::semantic_index::definition::Definition)> } })
 right: None
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 1 files)
[PosixPath('crates/ruff_python_parser/resources/invalid/expressions/named/invalid_target.py')]
------------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/semantic_index/builder.rs:215:9:
assertion `left == right` failed
  left: Some(Definition { [salsa id]: Id(1402), file: File { [salsa id]: Id(c00), path: System("/var/folders/qp/0fmxxq1x0vx9kfgn_qpjkj7r0000gn/T/tmpn08eo1bp/file.py"), permissions: Some(33188), revision: FileRevision(31887187996261115071827318172), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } }, file_scope: FileScopeId(0), symbol: ScopedSymbolId(3), kind: AnnotatedAssignment(AstNodeRef(StmtAnnAssign { range: 84..100, target: Tuple(ExprTuple { range: 84..88, elts: [Name(ExprName { range: 84..85, id: Name("x"), ctx: Store }), Name(ExprName { range: 87..88, id: Name("y"), ctx: Store })], ctx: Store, parenthesized: false }), annotation: Name(ExprName { range: 90..93, id: Name("int"), ctx: Load }), value: Some(Tuple(ExprTuple { range: 96..100, elts: [NumberLiteral(ExprNumberLiteral { range: 96..97, value: Int(1) }), NumberLiteral(ExprNumberLiteral { range: 99..100, value: Int(2) })], ctx: Load, parenthesized: false })), simple: false })), count: Count { ghost: PhantomData<fn(red_knot_python_semantic::semantic_index::definition::Definition)> } })
 right: None
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 1 files)
[PosixPath('crates/ruff_python_parser/resources/inline/err/ann_assign_stmt_invalid_target.py')]
------------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/infer.rs:2265:22:
Expected the symbol table to create a symbol for every Name node
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 3 files)
[PosixPath('crates/ruff_python_parser/resources/inline/err/class_def_unclosed_type_param_list.py'), PosixPath('crates/ruff_python_formatter/resources/test/fixtures/ruff/fmt_skip/type_params.py'), PosixPath('crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/class_definition.py')]
------------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/infer.rs:194:25:
no entry found for key
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 7 files)
[PosixPath('crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/fstring.py'), PosixPath('crates/ruff_python_formatter/resources/test/fixtures/black/cases/pep_701.py'), PosixPath('crates/ruff_python_formatter/resources/test/fixtures/black/cases/preview_long_strings__regression.py'), PosixPath('crates/ruff_linter/resources/test/fixtures/pyupgrade/UP039.py'), PosixPath('crates/ruff_linter/resources/test/fixtures/pyflakes/F811_19.py'), PosixPath('crates/ruff_linter/resources/test/fixtures/pyflakes/F821_0.py'), PosixPath('crates/ruff_linter/resources/test/fixtures/pyflakes/F821_26.py')]
------------
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/semantic_index/ast_ids.rs:41:22:
no entry found for key
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting

^^^^
(Hit by the following 2 files)
[PosixPath('crates/ruff_linter/resources/test/fixtures/flake8_pie/PIE790.py'), PosixPath('crates/ruff_linter/resources/test/fixtures/pyflakes/F821_17.py')]
------------
```

<details>
<summary>My checking script for getting the above</summary>

```py
# /// script
# dependencies = [
#   "tqdm"
# ]
# ///

import argparse
from collections import defaultdict
import shutil
import subprocess
import pathlib

import tqdm

from tempfile import TemporaryDirectory

parser = argparse.ArgumentParser(
    description="Run red_knot against a file or set of files (individually) and check if they cause panics"
)

parser.add_argument(
    "filename",
    nargs="?",
    help="The name of a single file to process.",
    type=str,
)
parser.add_argument(
    "--glob",
    help="A glob pattern to match multiple files (e.g., '*.txt').",
    type=str,
)

args = parser.parse_args()

if not args.filename and not args.glob:
    parser.error("You need to provide at least filename or glob")


def run_checker(target_file: pathlib.Path, tmp_dir, capture_output):
    # copy the file over into our sample directory
    copied_file = shutil.copy(target_file, tmp_dir / "file.py")
    try:
        result = subprocess.run(
            ["target/debug/red_knot", "--current-directory", str(tmp_dir)],
            capture_output=capture_output,
        )
        # return whether the run aborted
        aborted = result.returncode == -6
        return (aborted, result.stderr)
    finally:
        copied_file.unlink()


def run_single_check(path, tmp_dir):
    failed, _= run_checker(path, tmp_dir, capture_output=False)
    print(f"Aborted? {failed}")


def run_bulk_check(glob, tmp_dir):
    files_to_check = list(pathlib.Path(".").glob(glob))
    file_iter = tqdm.tqdm(files_to_check)
    failures_by_msg = defaultdict(list)
    failure_count = 0
    file_iter.set_description(f"({failure_count} aborted so far)")
    for f in file_iter:
        aborted, stderr = run_checker(f, tmp_dir, capture_output=True)
        if aborted:
            failures_by_msg[stderr].append(f)
            failure_count += 1
            file_iter.set_description(f"({failure_count} aborted so far)")
    print("Failures\n-------")
    for msg, affected_files in failures_by_msg.items():
        print(msg.decode("utf-8"))
        print("^^^^")
        print(f"(Hit by the following {len(affected_files)} files)")
        print(affected_files)
        print("------------")
    print("Done.")


with TemporaryDirectory() as tmp_dir_path:
    tmp_dir = pathlib.Path(tmp_dir_path)
    if args.filename:
        run_single_check(pathlib.Path(args.filename), tmp_dir)
    elif args.glob:
        run_bulk_check(args.glob, tmp_dir)
```

</details>



---

_Label `red-knot` added by @dhruvmanila on 2024-10-11 04:16_

---

_Comment by @dhruvmanila on 2024-10-11 04:17_

Thanks.

> The following is a log of the 24 files caught by a `crates/**/*.py` glob that will panic if `red_knot` looks at them.

A side note that this glob will include the files that contains syntax errors (like the ones used for the parser tests) and they will likely panic because red knot doesn't have error resilience.

---

_Comment by @rtpg on 2024-10-11 06:39_

I've spent way too much free time on this so I'm going to drop some of the cases I found going into this.

```
x = [ 0 ]
x[0 if y := 2 else 1 ] = 1
```

^ the above crashes because `SemanticIndexBuilder` assumes you could only have one assignment target being handled at a time. This is an easy fix IMO (`assignment_target: Option<...> -> assignment_targets: Vec<...>`), but unfortunately after fixing that I started hitting other issues. [This PR](https://github.com/astral-sh/ruff/pull/13711) has a patch for this one



---

_Comment by @rtpg on 2024-10-11 06:42_

```
class A:
   pass

class B[T](A):
   pass
```
^ the above fails with a failed lookup on `A`. Why? Because when there's a type parameter the semantic index creates a new scope to hold onto the type parameters. While this scope gets used by the class body, it doesn't get used for the arguments to the class.

End result is the code has trouble finding things, because the inference code doesn't seem to pull up that type scope again (except for the body).

I am legitimately confused as to how the semantic index builder is supposed to work with scoping, I can't quite wrap my head around it enough to debug some of the issues I found.

EDIT: I do think there's one issue here where arguments end up being type inference targets, but we don't call `add_standalone_expression` to them. That might be all that's needed for this specific case

---

_Comment by @rtpg on 2024-10-11 06:44_

```
class B[T](A):
   pass
```
^ bit related to the previous one, but with an unbound `A`. This is a bit more clear cut: the semantic index builder creates an `A` symbol inside of the "type parameter'd" class scope. But later on when doing type inference, the "file-level" scope is used to try and lookup `A`, and can't find it. 

Hence `Expected the symbol table to create a symbol for every Name node`


---

_Label `bug` added by @carljm on 2024-10-11 14:46_

---

_Comment by @carljm on 2024-10-11 19:45_

Thanks for looking at these! PEP 695 generic function and class definitions definitely aren't working correctly yet, so that's a known gap. I took a look at your other PR and it looks great, the failing test wasn't due to scoping issues, but due to the example being invalid syntax, which we aren't resilient to yet.

If you have specific questions about how scopes are supposed to work (in the semantic index builder or in inference), feel free to ask (maybe in Discord?) and I'm happy to shed some light if I can.

---

_Comment by @carljm on 2024-10-16 16:04_

The generic classes case is now fixed (thanks @rtpg!). I guess we can leave this issue open to track specifically "can run without panic over all Python files in ruff repo". I've created https://github.com/astral-sh/ruff/issues/13778 to track the issue of error-resilience, which I suspect is the root cause of most of the remaining panics here.

---

_Comment by @sharkdp on 2024-11-14 10:08_

@rtpg Thank you for this ticket (and the script!). With your contribution, as well as #14091, #14306, #14312, #14317, #14325, and #14326 merged, we can now run your script (`uv run find_crashes.py --glob '**/*.py'`) without uncovering any problems. So the *"can run without panic over all Python files in ruff repo"* part is definitely resolved.

In fact, we can now run red knot on the full ruff repository, including `*.ipynb` notebooks and `*.pyi` stub files, and there is only one remaining problem which is documented here: #14333. So I'm going to close this ticket for now.

---

_Closed by @sharkdp on 2024-11-14 10:08_

---

_Referenced in [astral-sh/ruff#13701](../../astral-sh/ruff/pulls/13701.md) on 2024-11-14 10:14_

---

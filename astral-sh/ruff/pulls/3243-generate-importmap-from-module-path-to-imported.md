```yaml
number: 3243
title: "Generate `ImportMap` from module path to imported dependencies"
type: pull_request
state: merged
author: chanman3388
labels: []
assignees: []
merged: true
base: main
head: report_imports
created_at: 2023-02-27T02:52:04Z
updated_at: 2023-04-04T03:52:48Z
url: https://github.com/astral-sh/ruff/pull/3243
synced_at: 2026-01-12T04:28:19Z
```

# Generate `ImportMap` from module path to imported dependencies

---

_Pull request opened by @chanman3388 on 2023-02-27 02:52_

Related to #2914 amongst others.
The tests will fail at the moment but I'd like to get some feedback on the architecture first.
So I've gone the route of making a separate struct `ast::types::Import` to hold the information about an import. This includes expanding `ImportFrom` statements using `checkers::imports::check_import`.
In addition to producing the diagnostics, this now returns a tuple of the diagnostics and a hashmap with an `Option<PathBuf>` as the key (will this ever be `None`?), and the value as a `Vec` of these new `Import` structs.
Eventually one can retrieve a `Vec` of these hashmaps inside of `run.rs`.
I haven't yet taken into account wildcard imports, or if a module has an `__all__` which exports the modules under different names.

---

_Converted to draft by @chanman3388 on 2023-03-03 21:08_

---

_Comment by @chanman3388 on 2023-03-12 18:24_

So now I have a hashmap of the paths to a vec of the imports (printed in a `println!` call) they require like so:
```
{
    "/home/chris/projects/pylint_exp/R0401/src/a.py": [
        Import {
            name: ".b",
            location: Location {
                row: 1,
                column: 14,
            },
            end_location: Location {
                row: 1,
                column: 15,
            },
        },
    ],
    "/home/chris/projects/pylint_exp/R0401/src/f/a.py": [
        Import {
            name: "src.g.a",
            location: Location {
                row: 1,
                column: 18,
            },
            end_location: Location {
                row: 1,
                column: 24,
            },
        },
    ],
    "/home/chris/projects/pylint_exp/R0401/src/f/__init__.py": [],
    "/home/chris/projects/pylint_exp/R0401/src/g/__init__.py": [],
    "/home/chris/projects/pylint_exp/R0401/src/j/a.py": [
        Import {
            name: "src.a",
            location: Location {
                row: 1,
                column: 16,
            },
            end_location: Location {
                row: 1,
                column: 17,
            },
        },
        Import {
            name: "src.b",
            location: Location {
                row: 1,
                column: 19,
            },
            end_location: Location {
                row: 1,
                column: 20,
            },
        },
    ],
    "/home/chris/projects/pylint_exp/R0401/src/c/__init__.py": [],
    "/home/chris/projects/pylint_exp/R0401/src/g/a.py": [
        Import {
            name: "src.e.a",
            location: Location {
                row: 1,
                column: 0,
            },
            end_location: Location {
                row: 1,
                column: 14,
            },
        },
    ],
    "/home/chris/projects/pylint_exp/R0401/src/d/a.py": [
        Import {
            name: "src.c.a",
            location: Location {
                row: 1,
                column: 0,
            },
            end_location: Location {
                row: 1,
                column: 14,
            },
        },
    ],
    "/home/chris/projects/pylint_exp/R0401/src/j/__init__.py": [],
    "/home/chris/projects/pylint_exp/R0401/src/b.py": [
        Import {
            name: ".a",
            location: Location {
                row: 1,
                column: 14,
            },
            end_location: Location {
                row: 1,
                column: 15,
            },
        },
    ],
    "/home/chris/projects/pylint_exp/R0401/src/e/__init__.py": [],
    "/home/chris/projects/pylint_exp/R0401/src/d/__init__.py": [],
    "/home/chris/projects/pylint_exp/R0401/src/c/a.py": [
        Import {
            name: "src.d.a",
            location: Location {
                row: 1,
                column: 18,
            },
            end_location: Location {
                row: 1,
                column: 19,
            },
        },
    ],
    "/home/chris/projects/pylint_exp/R0401/src/__init__.py": [],
    "/home/chris/projects/pylint_exp/R0401/src/e/a.py": [
        Import {
            name: "src.f.a",
            location: Location {
                row: 1,
                column: 0,
            },
            end_location: Location {
                row: 1,
                column: 14,
            },
        },
    ],
}
```

For a file structure like this:
```
src
├── __init__.py
├── a.py  (from . import b)
├── b.py  (from . import a)
├── c
│   ├── __init__.py
│   └── a.py  (from src.d import a)
├── d
│   ├── __init__.py
│   └── a.py  (import src.c.a)
├── e
│   ├── __init__.py
│   └── a.py  (import src.f.a)
├── f
│   ├── __init__.py
│   └── a.py  (from src.g import a as b)
├── g
│   ├── __init__.py
│   └── a.py  (import src.e.a)
└── j
    ├── __init__.py
    └── a.py  (from src import a, b)
```

So, I still need to fix relative imports, but I'd like feedback on whether the implementation makes any sense.
I don't believe I've done anything like reporting what modules are exported yet.

---

_Comment by @github-actions[bot] on 2023-03-12 18:32_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Marked ready for review by @chanman3388 on 2023-03-15 00:43_

---

_Comment by @chanman3388 on 2023-03-15 00:43_

TODO: Still need to figure out a way to expand relative imports

---

_Comment by @charliermarsh on 2023-03-15 02:11_

Great! I'll try to give this a thorough review tomorrow.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/imports.rs`:29 on 2023-03-15 08:05_

`FxHashMap<PathBuf, Vec<Import>>` is now used in many places. Can we introduce a new-type that gives it a name and abstracts a way the internal data structure

```rust
#[derive(Debug, Clone, Default)]
struct Imports {
	inner: FxHashMap<PathBuf, Vec<Import>>
}

impl Imports {
	pub fn insert(...) {}
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/imports.rs`:67 on 2023-03-15 08:06_

Not specific to this PR: @charliermarsh it seems that it is guaranteed that location is always set. Can we introduce a helper function / extension method that calls the `unwrap` rather than sprinkling the unwraps all over our code base?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/imports.rs`:81 on 2023-03-15 08:07_

Nit: Does this work
```suggestion
                                module.unwrap_or_default()
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/imports.rs`:88 on 2023-03-15 08:07_

Is it necessary to call `collect` here? `extend` should be fine with an `Iter` argument
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/imports.rs`:90 on 2023-03-15 08:08_

Nit: Can we add a small comment explaining what guarantees that there should only be import statements?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/imports.rs`:95 on 2023-03-15 08:08_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/linter.rs`:70 on 2023-03-15 08:09_

Can you explain why the lifetime annotation is necessary? We aren't using it in any return type.

---

_Review comment by @MichaReiser on `crates/ruff/src/linter.rs`:55 on 2023-03-15 08:10_

These type seem to be somewhat common too. It could make sense to wrap them in a newtype-wrapper to avoid exposing internals (and it gives you the possibility to implement additional methods on them)

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/run.rs`:135 on 2023-03-15 08:12_

Is this a todo? 

---

_@MichaReiser reviewed on 2023-03-15 08:12_

---

_@chanman3388 reviewed on 2023-03-15 19:01_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:29 on 2023-03-15 19:01_

Sure, I'll look into doing this

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:81 on 2023-03-15 19:01_

I'll give it a go

---

_@chanman3388 reviewed on 2023-03-15 19:01_

---

_Review comment by @chanman3388 on `crates/ruff/src/linter.rs`:70 on 2023-03-15 19:03_

I think this is probably a hangover from when I was trying to make the imports work on a `&Path` rather than a `PathBuf`, so yes I will need to get rid of these

---

_@chanman3388 reviewed on 2023-03-15 19:03_

---

_@chanman3388 reviewed on 2023-03-15 19:03_

---

_Review comment by @chanman3388 on `crates/ruff_cli/src/commands/run.rs`:135 on 2023-03-15 19:03_

Yes it is, I'll edit the comment appropriately

---

_@chanman3388 reviewed on 2023-03-15 19:23_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:81 on 2023-03-15 19:23_

Hm, I think that as the `Default` trait isn't implement for `std::string::String` we can't _just_ do this, but I can do something similar.

---

_@chanman3388 reviewed on 2023-03-15 19:25_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:88 on 2023-03-15 19:25_

I'll have a look

---

_Review comment by @chanman3388 on `crates/ruff/src/linter.rs`:55 on 2023-03-15 20:20_

I _think_ `DiagnosticsAndImports` is only used in `linter.rs` whereas `MessagesAndImports` is used in both `ruff_cli/src/cache.rs` and `linter.rs`. Would you still like me to wrap these up?

---

_@chanman3388 reviewed on 2023-03-15 20:20_

---

_@MichaReiser reviewed on 2023-03-15 20:47_

---

_Review comment by @MichaReiser on `crates/ruff/src/linter.rs`:55 on 2023-03-15 20:47_

Up to you. I prefer to wrap all public (it sounds as if you can make one of them private)

---

_Review comment by @chanman3388 on `crates/ruff_cli/src/commands/run.rs`:145 on 2023-03-15 22:18_

Will get rid of this once everything is good.

---

_@chanman3388 reviewed on 2023-03-15 22:18_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2366 on 2023-03-16 08:07_

Is this a todo? 

---

_@MichaReiser approved on 2023-03-17 08:03_

LGTM. Thank you for working on this. I'll leave this to @charliermarsh for merging because I'm still learning the code base.

---

_@chanman3388 reviewed on 2023-03-18 09:57_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/ast/mod.rs`:2366 on 2023-03-18 09:57_

Good spot, I think this was actually for another piece of functionality I was looking to implement. Maybe for `assignment-from-none`? At any rate, it's not meant to be here, I'll get rid of it.

---

_Comment by @github-actions[bot] on 2023-03-18 10:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.17ms     2.8 MB/sec    1.02     14.6±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.7 MB/sec    1.02      3.6±0.02ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    454.5±0.85µs     6.5 MB/sec    1.01    460.5±1.50µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.03      6.2±0.13ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.03      7.4±0.07ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1617.9±1.75µs    10.3 MB/sec    1.03   1659.4±2.54µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.8±1.36µs    16.4 MB/sec    1.02    183.0±2.24µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.6 MB/sec    1.02      3.4±0.02ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.8±0.57ms  2000.3 KB/sec    1.00     20.8±0.67ms  2007.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.4±0.21ms     3.1 MB/sec    1.00      5.3±0.19ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   627.6±23.27µs     4.7 MB/sec    1.00   621.9±30.12µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.34ms     2.9 MB/sec    1.01      8.8±0.31ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.7±0.38ms     3.8 MB/sec    1.00     10.6±0.29ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.4±0.10ms     7.1 MB/sec    1.00      2.3±0.11ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   249.4±15.44µs    11.8 MB/sec    1.00   250.3±13.55µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.20ms     5.2 MB/sec    1.00      4.8±0.22ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @chanman3388 on 2023-03-18 11:35_

With regards to relative imports, if I have the time I'll try have a bash at this tomorrow, I think I can use the `level` information from the import to derive what the full path would be for relative import lines.

---

_Comment by @chanman3388 on 2023-03-19 14:38_

So **hopefully** I've managed the relative import thing. Would like to test further, but for now if I have the following file structure:
```
grand
├── __init__.py
├── a.py (from .parent.child import a; from .parent import a; from grand.parent.child import b)
└── parent
    ├── __init__.py
    ├── a.py (from .child import a,b; from .. import a)
    └── child
        ├── __init__.py
        ├── a.py (from ..import a; from ... import a)
        └── b.py (from . import a)
```

I get the following imports out:
```
Imports {
    inner: {
        "/home/chris/projects/pylint_exp/R0401/grand/__init__.py": [],
        "/home/chris/projects/pylint_exp/R0401/grand/parent/__init__.py": [],
        "/home/chris/projects/pylint_exp/R0401/grand/parent/child/__init__.py": [],
        "/home/chris/projects/pylint_exp/R0401/grand/parent/a.py": [
            Import {
                name: "grand.child.a",
                location: Location {
                    row: 2,
                    column: 19,
                },
                end_location: Location {
                    row: 2,
                    column: 20,
                },
            },
            Import {
                name: "grand.child.b",
                location: Location {
                    row: 2,
                    column: 22,
                },
                end_location: Location {
                    row: 2,
                    column: 23,
                },
            },
            Import {
                name: "grand.a",
                location: Location {
                    row: 3,
                    column: 15,
                },
                end_location: Location {
                    row: 3,
                    column: 16,
                },
            },
        ],
        "/home/chris/projects/pylint_exp/R0401/grand/parent/child/a.py": [
            Import {
                name: "grand.a",
                location: Location {
                    row: 2,
                    column: 15,
                },
                end_location: Location {
                    row: 2,
                    column: 16,
                },
            },
            Import {
                name: "grand.a",
                location: Location {
                    row: 3,
                    column: 16,
                },
                end_location: Location {
                    row: 3,
                    column: 17,
                },
            },
        ],
        "/home/chris/projects/pylint_exp/R0401/grand/a.py": [
            Import {
                name: "grand.parent.child.a",
                location: Location {
                    row: 2,
                    column: 26,
                },
                end_location: Location {
                    row: 2,
                    column: 27,
                },
            },
            Import {
                name: "grand.parent.a",
                location: Location {
                    row: 3,
                    column: 20,
                },
                end_location: Location {
                    row: 3,
                    column: 21,
                },
            },
            Import {
                name: "grand.parent.child.b",
                location: Location {
                    row: 4,
                    column: 25,
                },
                end_location: Location {
                    row: 4,
                    column: 26,
                },
            },
        ],
        "/home/chris/projects/pylint_exp/R0401/grand/parent/child/b.py": [
            Import {
                name: "grand.a",
                location: Location {
                    row: 2,
                    column: 14,
                },
                end_location: Location {
                    row: 2,
                    column: 15,
                },
            },
        ],
    },
}
```

---

_Comment by @chanman3388 on 2023-03-19 17:55_

@MichaReiser Sorry I've made some new changes now to deal with resolution of relative imports, would you mind giving it the once over again?

---

_@charliermarsh reviewed on 2023-03-19 19:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/imports.rs`:85 on 2023-03-19 19:31_

`import os, sys` is valid, so this _can_ have multiple entries.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/imports.rs`:87 on 2023-03-19 19:31_

I think we should introduce lifetimes to avoid the need to clone names. The AST will outlive all of this stuff, so it should be possible to just take references.

---

_@charliermarsh reviewed on 2023-03-19 19:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/imports.rs`:81 on 2023-03-19 19:36_

Can we reuse `crates/ruff_python_ast/src/helpers.rs#to_module_path` for this?

---

_@charliermarsh reviewed on 2023-03-19 19:36_

---

_@charliermarsh reviewed on 2023-03-19 19:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/imports.rs`:67 on 2023-03-19 19:37_

Yeah for context, the Python AST treats end locations as optional. In RustPython, though, we always provide them (and `Expr::new` _requires_ them). So the _interface_ mirrors CPython, but they _are_ guaranteed in practice.

---

_@charliermarsh reviewed on 2023-03-19 19:38_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/types.rs`:136 on 2023-03-19 19:38_

Can we look at, or reuse, or share logic with `crates/ruff/src/rules/flake8_tidy_imports/relative_imports.rs#fix_banned_relative_import`? I believe it's solving the same problem.

---

_@charliermarsh reviewed on 2023-03-19 19:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/types.rs`:85 on 2023-03-19 19:47_

Have we looked ahead to what the needs will be for lint rules that will rely on this, like cyclic imports? It looks like this is a string representation of the module (e.g., `import foo` => `"foo"`). Will we need to translate it to a _file path_ at all? Do we have the information we need about the containing package?


---

_@chanman3388 reviewed on 2023-03-19 21:39_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:87 on 2023-03-19 21:39_

I guess that would depend on how we want this to look. If we can make whatever it is we get from relative/from imports to be the same type then it should be doable.

---

_@chanman3388 reviewed on 2023-03-19 21:43_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:87 on 2023-03-19 21:43_

Forgot to mention, the AST does outlive _this_ stuff, but the tokens only exist until the end of the various functions inside of `linter.rs`, if we want the locations and names to live past this they must be owned, at least to the best of my current understanding.

---

_@chanman3388 reviewed on 2023-03-19 21:44_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:81 on 2023-03-19 21:44_

I'll look into it.

---

_@chanman3388 reviewed on 2023-03-19 22:00_

---

_Review comment by @chanman3388 on `crates/ruff_python_ast/src/types.rs`:85 on 2023-03-19 22:00_

Very happy for others to chime in here, I'm most familiar with pylint as that's what I use on my day job, I ran through the list of items from #970, and honestly I think this is only useful for `cyclic-import R0401`.
One could also use the logic used to report the imports for `import-self (W0406)` I reckon, but that doesn't need knowledge of other imports.

---

_@chanman3388 reviewed on 2023-03-19 22:01_

---

_Review comment by @chanman3388 on `crates/ruff/src/checkers/imports.rs`:81 on 2023-03-19 22:01_

My only qualm with using it is that it returns me a `Vec<String>` whereas here we're using `Vec<&str>`

---

_Comment by @charliermarsh on 2023-03-21 02:34_

High-level question: should we first try to draft the cyclic imports PR that leverages this extracted metadata, prior to merging? Implementing that diagnostic may point out things we need to include here.

---

_Comment by @chanman3388 on 2023-03-21 11:29_

> High-level question: should we first try to draft the cyclic imports PR that leverages this extracted metadata, prior to merging? Implementing that diagnostic may point out things we need to include here.

Seems like a sensible idea.

---

_Comment by @chanman3388 on 2023-03-24 01:49_

So on the grand/parent/child example I have above, pylint throws the following out:
```
************* Module grand.parent.child.__init__
grand/parent/child/__init__.py:1:0: R0401: Cyclic import (grand.parent.a -> grand.parent.child.a) (cyclic-import)
grand/parent/child/__init__.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.child.a -> grand.parent.a) (cyclic-import)
grand/parent/child/__init__.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.child.a) (cyclic-import)
grand/parent/child/__init__.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.a) (cyclic-import)
grand/parent/child/__init__.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.child.b -> grand.parent.child.a -> grand.parent.a) (cyclic-import)
```
I'm still tweaking the cycle detection, it's not quite there yet.

---

_Comment by @chanman3388 on 2023-03-25 15:01_

Interestingly this detects an extra cycle (correctly) compared to pylint, not sure why this would be the case, I've only briefly looked at pylint's implementation. I haven't yet done the same logic as all the other rules yet, that is to say, it's permanently on.

Here is the example output.
```
grand/a.py:2:1: I001 [*] Import block is un-sorted or un-formatted
grand/a.py:2:27: PLR0401 Cyclic import (grand.a -> grand.parent.child.a -> grand.parent.a)
grand/a.py:2:27: PLR0401 Cyclic import (grand.a -> grand.parent.child.a)
grand/a.py:2:27: PLR0401 Cyclic import (grand.a -> grand.parent.a)
grand/a.py:2:27: PLR0401 Cyclic import (grand.a -> grand.parent.child.b -> grand.parent.child.a -> grand.parent.a)
grand/a.py:2:27: PLR0401 Cyclic import (grand.a -> grand.parent.child.b -> grand.parent.child.a)
grand/parent/a.py:2:1: I001 [*] Import block is un-sorted or un-formatted
grand/parent/a.py:2:20: PLR0401 Cyclic import (grand.parent.a -> grand.a -> grand.parent.child.a)
grand/parent/a.py:2:20: PLR0401 Cyclic import (grand.parent.a -> grand.a)
grand/parent/a.py:2:20: PLR0401 Cyclic import (grand.parent.a -> grand.a -> grand.parent.child.b -> grand.parent.child.a)
grand/parent/child/a.py:2:1: I001 [*] Import block is un-sorted or un-formatted
grand/parent/child/a.py:2:16: PLR0401 Cyclic import (grand.parent.child.a -> grand.parent.a -> grand.a)
grand/parent/child/a.py:2:16: PLR0401 Cyclic import (grand.parent.child.a -> grand.parent.a -> grand.a -> grand.parent.child.b)
grand/parent/child/a.py:2:16: PLR0401 Cyclic import (grand.parent.child.a -> grand.a)
grand/parent/child/a.py:2:16: PLR0401 Cyclic import (grand.parent.child.a -> grand.a -> grand.parent.child.b)
grand/parent/child/b.py:2:15: PLR0401 Cyclic import (grand.parent.child.b -> grand.parent.child.a -> grand.parent.a -> grand.a)
grand/parent/child/b.py:2:15: PLR0401 Cyclic import (grand.parent.child.b -> grand.parent.child.a -> grand.a)
```

Note: here I am missing a cycle that _is_ detected by pylint, that was due to how I fully resolved `ImportFrom` statements which should now be fixed.

---

_Comment by @chanman3388 on 2023-03-25 20:01_

One thing I want to ask, for the moment I've made it such that in each module the cyclic import is present, there's a `Diagnostic`/`Message` for each one. I could make it more efficient by not doing this, but I kind of feel like it should be reported in each one. Thoughts @charliermarsh?

---

_Comment by @charliermarsh on 2023-03-25 22:40_

@chanman3388 - Yeah I think it's okay to flag once per file per module.

As a heads up, I might ask that we carve the actual cycle detection and rule out into a separate PR, so that they're independently reviewable and mergable. (The cycle-detection PR would mark this branch as its upstream.)

---

_Converted to draft by @chanman3388 on 2023-03-28 23:35_

---

_Marked ready for review by @chanman3388 on 2023-03-29 21:41_

---

_Comment by @chanman3388 on 2023-03-29 21:41_

So this time, I believe it's ready. I didn't need the module mappings in the end.
Here is the implementation of the cyclic imports rule.
https://github.com/chanman3388/ruff/pull/1
I still need to add a fixture for it.

---

_Comment by @chanman3388 on 2023-03-30 02:28_

@charliermarsh Something I wasn't sure upon, what did you mean by "The cycle-detection PR would mark this branch as its upstream" exactly? I've made a PR which merged in the cyclic import detection rule implementation in my own fork, but that naturally won't show here.
I've linked it in my previous message so others can look at it.

---

_Comment by @charliermarsh on 2023-03-30 13:31_

@chanman3388 - Ah sorry, I was suggesting that you create a second PR in Ruff for the cyclic import rule, and mark `chanman3388:report_imports` as the upstream branch in GitHub. But, I don't think you can make a PR in Ruff against an upstream branch in a separate fork, so what you've done (creating a separate PR in your fork and linking to it for now) seems like the right thing to do.

---

_Comment by @chanman3388 on 2023-03-30 19:58_

Another question I had was related to fixtures. I think fixtures only work on file paths rather than directories? I think we'd need more plumbing if we wanted to test this feature via the current method?

---

_Comment by @charliermarsh on 2023-03-30 20:35_

I think you're right.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:65 on 2023-03-30 21:58_

Why did this need to be boxed?

---

_@charliermarsh reviewed on 2023-03-30 21:58_

---

_@chanman3388 reviewed on 2023-03-30 22:40_

---

_Review comment by @chanman3388 on `crates/ruff_cli/src/diagnostics.rs`:65 on 2023-03-30 22:40_

Clippy said the type was too large and suggested boxing it. It was a warning.

---

_Comment by @chanman3388 on 2023-04-01 01:29_

@charliermarsh what else would you like see on this PR? Should we implement the feature for fixtures to also be directories separately? There some more small changes I'd like to cherry pick from the rule implementation PR. I think for now I'll have to just unit test it.

---

_Comment by @charliermarsh on 2023-04-03 17:21_

@chanman3388 - I think this is reasonable as far as the scope of the PR goes. I just need to find a bit of time to do a final pass on it and some benchmarking before I'd be able to merge.

Heads up that I may also iterate a bit on the exact model and execution flow we use here over time as we expand the need to resolve imports. (For example, Pyright does a pass collecting and resolving all imports prior to traversing the AST for semantic analysis. Right now, we're going in the opposite order.)


---

_Comment by @chanman3388 on 2023-04-03 22:04_

> @chanman3388 - I think this is reasonable as far as the scope of the PR goes. I just need to find a bit of time to do a final pass on it and some benchmarking before I'd be able to merge.
> 
> Heads up that I may also iterate a bit on the exact model and execution flow we use here over time as we expand the need to resolve imports. (For example, Pyright does a pass collecting and resolving all imports prior to traversing the AST for semantic analysis. Right now, we're going in the opposite order.)
> 

No problem, I had some minor improvement where I used an `Rc<String>` instead of the module name. I also saw an interesting crate which we might find useful? https://github.com/xfbs/imstr

---

_Renamed from "Report imports" to "Generate `ImportMap` from module path to imported dependencies" by @charliermarsh on 2023-04-04 03:26_

---

_Merged by @charliermarsh on 2023-04-04 03:31_

---

_Closed by @charliermarsh on 2023-04-04 03:31_

---

_Comment by @charliermarsh on 2023-04-04 03:36_

Thanks @chanman3388 for all your work here! I made a few small touch-ups, mostly just renaming a few of the structs and moving them into new modules. Let me know if you have any questions or run into any issues as you rebase the cycle detection work.

---

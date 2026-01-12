```yaml
number: 4459
title: "Implement Pylint `bad-super-call` as `PLE1003`"
type: pull_request
state: closed
author: nunokaeru
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2023-05-16T21:38:12Z
updated_at: 2023-05-20T19:46:39Z
url: https://github.com/astral-sh/ruff/pull/4459
synced_at: 2026-01-12T15:55:15Z
```

# Implement Pylint `bad-super-call` as `PLE1003`

---

_@nunokaeru_

Made an attempt at implementing `E1003`.

Awaiting for changes that will let us see the path of inheritance 

https://github.com/charliermarsh/ruff/issues/970

---

_@nunokaeru reviewed on 2023-05-16 21:38_

---

_Review comment by @nunokaeru on `crates/ruff/resources/test/fixtures/pylint/bad_super_call.py`:32 on 2023-05-16 21:38_

will fix

---

_Review comment by @nunokaeru on `crates/ruff/src/checkers/ast/mod.rs`:2668 on 2023-05-16 21:39_

Temporary place for it.

Might keep it here, but if I do I will comment `//pylint` here

---

_@nunokaeru reviewed on 2023-05-16 21:39_

---

_Review comment by @nunokaeru on `crates/ruff/src/rules/pylint/rules/bad_super_call.rs`:71 on 2023-05-16 21:40_

This is where I have found the difficulty. I do not know how to get the `base_class` name of the current class. Otherwise it should be quite simple

---

_@nunokaeru reviewed on 2023-05-16 21:40_

---

_Review comment by @nunokaeru on `crates/ruff/src/rules/pylint/snapshots/ruff__rules__pylint__tests__PLE1003_bad_super_call.py.snap.new`:14 on 2023-05-16 21:40_

Will be regenerated when the code works

---

_@nunokaeru reviewed on 2023-05-16 21:40_

---

_@nunokaeru reviewed on 2023-05-16 21:41_

---

_Review comment by @nunokaeru on `crates/ruff/src/rules/pylint/rules/bad_super_call.rs`:83 on 2023-05-16 21:41_

A fixer will be implemented after I know how to get the base class

---

_Review comment by @nunokaeru on `crates/ruff/src/rules/pylint/rules/bad_super_call.rs`:13 on 2023-05-16 21:41_

Documentation will also be made before finished the PR

---

_@nunokaeru reviewed on 2023-05-16 21:41_

---

_Converted to draft by @nunokaeru on 2023-05-16 21:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/bad_super_call.rs`:71 on 2023-05-16 22:18_

Where you have this:

```rust
// Find the enclosing class definition (if any).
let Some(Stmt::ClassDef(ast::StmtClassDef {
    name: parent_name, ..
})) = parents
    .find(|stmt| matches!(stmt, Stmt::ClassDef (_))) else {
    return;
};
```

You should be able to add the `bases`:

```rust
// Find the enclosing class definition (if any).
let Some(Stmt::ClassDef(ast::StmtClassDef {
    name: parent_name, ..
    bases: bases,
})) = parents
    .find(|stmt| matches!(stmt, Stmt::ClassDef (_))) else {
    return;
};
```


---

_@charliermarsh reviewed on 2023-05-16 22:18_

---

_@charliermarsh reviewed on 2023-05-16 22:19_

Hmm, I don't think we have the infrastructure we need to support this rule reliably. The issue is that Pylint correctly allows you to do this:

```py
class Animal:
    pass


class Feline(Animal):
    pass


class Cat(Feline):
    def __init__(self):
        super(Animal, self).__init__()
```

Which requires that you're able to traverse up the inheritance chain, even across files.

---

_Comment by @nunokaeru on 2023-05-17 00:21_

> Which requires that you're able to traverse up the inheritance chain, even across files.

Unfortunate. I got the rule working with the first idea (not multiple inheritance), however I am lacking the knowledge to make the unit-test pass even though now the behaviour is correct. I do not know what I am missing to declare which line should fail and which one should pass.


Anyway, I will leave this PR on-hold for now, since to implement Pylint you will be looking into traversing across files. Can look into it whenever I have the GO signal.

---

_Renamed from "add PLE1003 'bad-super-call' from pylint 'E1003'" to "add PLE1003 'bad-super-call' from pylint `E1003`" by @nunokaeru on 2023-05-18 10:31_

---

_Renamed from "add PLE1003 'bad-super-call' from pylint `E1003`" to "Implement Pylint E1003 `bad-super-call` as`PLE1003`" by @nunokaeru on 2023-05-18 10:32_

---

_Renamed from "Implement Pylint E1003 `bad-super-call` as`PLE1003`" to "Implement Pylint E1003 `bad-super-call` as `PLE1003`" by @nunokaeru on 2023-05-18 10:32_

---

_Renamed from "Implement Pylint E1003 `bad-super-call` as `PLE1003`" to "Implement Pylint `bad-super-call` as `PLE1003`" by @nunokaeru on 2023-05-18 10:32_

---

_@nunokaeru reviewed on 2023-05-18 10:52_

---

_Review comment by @nunokaeru on `crates/ruff/src/rules/pylint/rules/bad_super_call.rs`:83 on 2023-05-18 10:52_

Regarding a fix, there are a few ways we can go about it.

Example:
```python

class Animal:
    pass


class Tree:
    pass


class Cat(Animal):
    def __init__(self):
        super(Tree, self).__init__()
```

The super call could be to the correct class the user wants and the error is in the inheritance class.

So `Animal` could be the wrong class and the fix should instead make `Tree` the parent class.

So for this rule, either `Animal` or `Tree` could be the problem, depending on what the user wants to do. Imo that means this violation cannot be auto-fixable, unless we look at context (but I'd rather the user be the one looking at context).

I prefer violations that are auto-fixable, so looking for opinions if I should just implement something that changes `Tree` to `Animal` in the super call, or not do anything for the fixing.

---

_@charliermarsh reviewed on 2023-05-18 15:03_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/bad_super_call.rs`:83 on 2023-05-18 15:03_

I think it's too difficult to know what the "correct" behavior is here. It's also possible that neither `Tree` nor `Animal` are the intended argument to `super` -- the user could be trying to invoke `super` on some other parent class in the chain. My advice would be to avoid implementing a fix.

---

_Comment by @github-actions[bot] on 2023-05-18 20:45_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.04ms     2.4 MB/sec    1.09     18.5±0.06ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.01ms     4.0 MB/sec    1.05      4.4±0.01ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    512.5±2.55µs     5.8 MB/sec    1.03    526.2±1.12µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.02ms     3.6 MB/sec    1.07      7.6±0.02ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.01ms     5.0 MB/sec    1.18      9.6±0.02ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1758.6±6.04µs     9.5 MB/sec    1.14   1996.4±6.10µs     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.7±1.16µs    15.2 MB/sec    1.10    214.2±1.73µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.14      4.2±0.01ms     6.1 MB/sec
parser/large/dataset.py                    1.00      6.5±0.01ms     6.3 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1271.8±1.25µs    13.1 MB/sec    1.00   1268.6±2.23µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    131.4±1.90µs    22.5 MB/sec    1.00    131.0±3.39µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.00ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.7±0.61ms     2.2 MB/sec    1.01     18.8±0.58ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.0±0.21ms     3.4 MB/sec    1.00      4.8±0.20ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.03   601.5±59.97µs     4.9 MB/sec    1.00   584.6±27.23µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.33ms     3.2 MB/sec    1.01      8.2±0.30ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      9.4±0.37ms     4.3 MB/sec    1.00      9.4±0.37ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.0±0.08ms     8.3 MB/sec    1.00  1974.2±65.32µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   227.7±13.06µs    13.0 MB/sec    1.02   231.7±11.66µs    12.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.22ms     6.0 MB/sec    1.00      4.3±0.17ms     6.0 MB/sec
parser/large/dataset.py                    1.00      7.6±0.22ms     5.3 MB/sec    1.02      7.8±0.25ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1470.3±67.97µs    11.3 MB/sec    1.01  1485.8±60.16µs    11.2 MB/sec
parser/numpy/globals.py                    1.00    149.2±9.86µs    19.8 MB/sec    1.01    150.9±8.70µs    19.6 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.13ms     7.9 MB/sec    1.02      3.3±0.13ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @nunokaeru on 2023-05-18 20:49_

Something seems to have changed. CI is running now on my draft when it didnt a few days ago

---

_Comment by @charliermarsh on 2023-05-20 15:26_

(I'm going to close for now as I like to keep the PR list actionable to the degree that I can. We can always re-open later, it will continue to exist in the project history here.)

---

_Closed by @charliermarsh on 2023-05-20 15:26_

---

_Comment by @nunokaeru on 2023-05-20 19:46_

Alright with me

---

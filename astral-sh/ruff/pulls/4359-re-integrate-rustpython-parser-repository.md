```yaml
number: 4359
title: Re-integrate RustPython parser repository
type: pull_request
state: merged
author: youknowone
labels:
  - internal
assignees: []
merged: true
base: main
head: rustpython
created_at: 2023-05-10T19:48:26Z
updated_at: 2023-05-11T09:11:43Z
url: https://github.com/astral-sh/ruff/pull/4359
synced_at: 2026-01-12T15:55:15Z
```

# Re-integrate RustPython parser repository

---

_@youknowone_

_No description provided._

---

_Review comment by @youknowone on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:16 on 2023-05-10 19:54_

You may worry about this patterns by looking more verbose than before. Enum definition is changed to be using struct of enums. This is transitional state and it can be written like this later:
```suggestion
        ExprKind::Constant(constant) => Some(constant.value)
```

By allowing adding methods to `ast::ExprConstant` structs unlike before, it will be even better in the future.

---

_@youknowone reviewed on 2023-05-10 19:54_

---

_Review comment by @youknowone on `crates/ruff/src/rules/flake8_2020/rules.rs`:188 on 2023-05-10 19:55_

The name is changed not to be confusing with types using `SourceLocation`. This is also fitting better to CPython definition. (with_attributes)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-10 19:59_

---

_@youknowone reviewed on 2023-05-10 20:06_

---

_Comment by @youknowone on 2023-05-10 20:16_

The failing tests seems not related and looks like github actions problem.

---

_Comment by @github-actions[bot] on 2023-05-10 20:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.8±0.06ms     2.9 MB/sec    1.00     13.7±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    417.6±3.02µs     7.1 MB/sec    1.00    415.2±2.72µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.04ms     4.4 MB/sec    1.00      5.7±0.02ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1468.7±5.42µs    11.3 MB/sec    1.00   1447.7±5.39µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.2±0.45µs    18.1 MB/sec    1.00    161.8±0.64µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.02ms     7.6 MB/sec    1.10      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1043.4±0.96µs    16.0 MB/sec    1.08   1123.4±0.93µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    106.4±0.36µs    27.7 MB/sec    1.06    113.1±0.24µs    26.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.08      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.9±0.43ms     2.4 MB/sec    1.00     16.4±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.00      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   494.5±11.36µs     6.0 MB/sec    1.00    492.1±8.58µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.18ms     3.6 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.12ms     4.9 MB/sec    1.00      8.2±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1741.8±28.59µs     9.6 MB/sec    1.00  1737.9±19.68µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.5±4.64µs    15.0 MB/sec    1.01    198.9±4.72µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.07ms     6.8 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.7±0.12ms     6.1 MB/sec    1.00      6.6±0.07ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1259.1±20.16µs    13.2 MB/sec    1.00  1253.8±16.40µs    13.3 MB/sec
parser/numpy/globals.py                    1.01    127.8±2.50µs    23.1 MB/sec    1.00    126.7±1.66µs    23.3 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.07ms     9.0 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-10 21:06_

Oh very interesting, we now have separate structs for each variant. I'd love to get @MichaReiser's review.

---

_Comment by @youknowone on 2023-05-11 05:14_

Does udeps error related to this change? Not sure i have to fix it or not

---

_Comment by @youknowone on 2023-05-11 06:14_

The struct BestFitting is actually same named in 2 modules and used as public.
Renaming one seems better. Not sure why it is only triggered on this patch.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_2020/rules.rs`:188 on 2023-05-11 06:16_

I personally find `Attributed` somewhat more difficult to understand than `Located` because it's very generic. Attributed with what. But I don't feel strongly about it. 

A term that is used by many other compilers for a wrapper that adds source locations is [`Spanned`](https://www.google.com/search?client=firefox-b-d&q=rust+spanned) or (since we don't have a span type), could also name it `Ranged`. But again, I'm okay with keeping it. Just my two cents ;)

Note: I'm exploring how we can have location information on every node. We'll probably need this to correctly associate comments with nodes.  

---

_@MichaReiser reviewed on 2023-05-11 06:16_

---

_@MichaReiser reviewed on 2023-05-11 06:17_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:16 on 2023-05-11 06:17_

I like it! The possibility for adding methods to `ExprConstant`, and that we can now pass specific `Stmt` instead of passing `Stmt` and than unwrapping a specific kind makes the API much more ergonomic 

The methods that I found very useful (besides methods that are specific to the node kind) to have on the union types when working at Rome were

* `as_<node>`: Returns an `Option<&<Node>>` if the variant is `<Node>` or `None` otherwise
* `into_<node>`: The same as `as_node` but consumes `self` and returns an owned version
* `is_<node>`: Tests if it holds the variant `<node>`

---

_Review comment by @MichaReiser on `Cargo.lock`:147 on 2023-05-11 06:19_

Oh nice, so many removed dependencies. This removes our total dependencies from 354 to 335!

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_bugbear/rules/assignment_to_os_environ.rs`:26 on 2023-05-11 06:32_

```suggestion
    if attr != "environ" {
```

This change isn't necessary because `Identifier` implements `PartialEq<str>`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_list_call.rs`:48 on 2023-05-11 06:34_

Idea: It would be nice to have `argument.is_<node>()` methods on unions so that we could simplify this to 

```rust
if !argument.is_list_comp() {
	...
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_django/rules/model_without_dunder_str.rs`:100 on 2023-05-11 06:36_

```suggestion
        if name  != "Meta" {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pep8_naming/rules/invalid_first_argument_name_for_class_method.rs`:79 on 2023-05-11 06:48_

Would it make sense to also implement `PartialEq<str> for String` and `PartialEq<String> for &str` and (the same for `std::String`)?

---

_@MichaReiser approved on 2023-05-11 06:52_

This is awesome. I was just thinking yesterday that I should look into how we can change our fork to fork the `Parser` repository instead and if we should introduce named variants for enums... and a few hours later, there's this PR! 

There are so many good things in here. I plan to merge this today, and we can discuss and solve some of the smaller nits later. 

---

_Comment by @MichaReiser on 2023-05-11 06:53_

Should `String` and `Identifier` implement `Deref<str>`. It could simplify some of the code (e.g `option.as_deref()` would give you a `Option<&str>`).

---

_@youknowone reviewed on 2023-05-11 06:54_

---

_Review comment by @youknowone on `crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_list_call.rs`:48 on 2023-05-11 06:54_

Yes, definitely. Actually that's also done in my local repository a few hours ago xD 

---

_Comment by @youknowone on 2023-05-11 07:26_

I removed unnecessary `.as_str()` and made `Deref<str> for Identifier`. I wish I reverted them correctly.
@MichaReiser what should I do for udeps error?

---

_Comment by @MichaReiser on 2023-05-11 07:30_

Build-time: 
* Reduces the Total units for release builds from 345 to 309
* Reduces build time from 113 to 107s

Binary: 
* Increases by ~100Kb

Enum Sizes:
The size of `StmtKind` and `ExprKind` hasn't changed (They're still huge with 128 and 64 bytes, maybe we can reduce some of them by using `ThinVec`?)


---

_Comment by @MichaReiser on 2023-05-11 07:34_

> I removed unnecessary `.as_str()` and made `Deref<str> for Identifier`. I wish I reverted them correctly. @MichaReiser what should I do for udeps error?

Running `udeps` locally gives me:

```bash
cargo +nightly udeps
...
unused dependencies:
`ruff_python_ast v0.0.0 (/home/micha/astral/ruff/crates/ruff_python_ast)`
└─── dependencies
     └─── "ruff_rustpython"
`ruff_python_formatter v0.0.0 (/home/micha/astral/ruff/crates/ruff_python_formatter)`
└─── dependencies
     └─── "rustpython-common"
`ruff_rustpython v0.0.0 (/home/micha/astral/ruff/crates/ruff_rustpython)`
└─── dependencies
     ├─── "once_cell"
     └─── "rustpython-common"
Note: These dependencies might be used by other targets.
      To find dependencies that are not used by any target, enable `--all-targets`.
Note: They might be false-positive.
      For example, `cargo-udeps` cannot detect usage of crates that are only used in doc-tests.
      To ignore some dependencies, write `package.metadata.cargo-udeps.ignore` in Cargo.toml.
```

I'll look into it and push the changes so that we can then merge this right away

---

_Comment by @youknowone on 2023-05-11 07:35_

Ah, i had to see that parts, not the warning parts. That's because common is replaced by literal in a few places. I will do it.

---

_@youknowone reviewed on 2023-05-11 07:42_

---

_Review comment by @youknowone on `crates/ruff/src/rules/flake8_2020/rules.rs`:188 on 2023-05-11 07:42_

`Ranged` sounds good. Do you want it on this PR?

---

_@MichaReiser reviewed on 2023-05-11 07:44_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_2020/rules.rs`:188 on 2023-05-11 07:44_

We can do it in another PR to avoid unnecessary rebasing

---

_Merged by @MichaReiser on 2023-05-11 07:47_

---

_Closed by @MichaReiser on 2023-05-11 07:47_

---

_Branch deleted on 2023-05-11 07:48_

---

_Comment by @youknowone on 2023-05-11 07:49_

Thank you for quickly merging it! It literally was making conflict for every a few hours 🤣 

---

_Comment by @youknowone on 2023-05-11 07:53_

About the enum size, I think moving boxing point will be good. 

e.g. `StmtFunctionDef` wraps `args` with box. But StmtKind actually can box every variant. Otherwise any variant that contains Box field inside.

```
#[derive(Clone, Debug, PartialEq)]
pub struct StmtFunctionDef<U = ()> {
    pub name: Identifier,
    pub args: Box<Arguments<U>>,
    pub body: Vec<Stmt<U>>,
    pub decorator_list: Vec<Expr<U>>,
    pub returns: Option<Box<Expr<U>>>,
    pub type_comment: Option<String>,
}
```

Does it make sense?

---

_Label `internal` added by @MichaReiser on 2023-05-11 08:06_

---

_Comment by @MichaReiser on 2023-05-11 08:08_

Boxing every variant is what https://github.com/Boshen/oxc/blob/main/crates/oxc_ast/src/ast/js.rs does

The downside is that pattern matching can become somewhat annoying without using nightly features (at least, that's what I remember to have heard). 

We may get away by only boxing the largest variants? 

---

_Comment by @youknowone on 2023-05-11 08:22_

Do you have suggestions? The last option will be manually maintaining tables.
If we can do decide it from asdl data, it will be better.

currently `boxed` is related in https://github.com/RustPython/Parser/blob/main/ast/asdl_rs.py

---

_Comment by @MichaReiser on 2023-05-11 09:06_

> Do you have suggestions? The last option will be manually maintaining tables. If we can do decide it from asdl data, it will be better.
> 
> currently `boxed` is related in [RustPython/Parser@`main`/ast/asdl_rs.py](https://github.com/RustPython/Parser/blob/main/ast/asdl_rs.py?rgh-link-date=2023-05-11T08%3A22%3A54Z)

We can try boxing all of them to get a feel for the API's convenience. 

I'm considering to inline `range` into every node and add a `Ranged` trait instead that each node and all unions implement:

```rust
trait Ranged {
	fn range(&self) -> TextRange;

	fn start(&self) -> TextSize { self.range().start() }

	fn end(&self) -> TextSize { self.range().end() }
}
```

That removes the need for `Attributed` except for where someone wants to store a `custom` value with the node. For that use case I think it's best if a new struct is introduced per-use case (or we can keep `Attributed` around just for that use-case)

---

_Comment by @youknowone on 2023-05-11 09:11_

custom field also can be inlined to every node with `trait Custom`.
It will be easier to keep `Fold` working.

---

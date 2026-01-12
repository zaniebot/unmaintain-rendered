```yaml
number: 6838
title: "Use `PatternMatchAs` for mapping pattern's `rest` node"
type: pull_request
state: closed
author: charliermarsh
labels:
  - formatter
  - parser
assignees: []
draft: true
base: main
head: charlie/pattern
created_at: 2023-08-24T04:47:29Z
updated_at: 2023-09-14T18:11:18Z
url: https://github.com/astral-sh/ruff/pull/6838
synced_at: 2026-01-12T02:39:09Z
```

# Use `PatternMatchAs` for mapping pattern's `rest` node

---

_Pull request opened by @charliermarsh on 2023-08-24 04:47_

## Summary

In the mapping pattern match, you can have a rest argument to capture the remaining fields, like:

```python
match foo:
    case {"a": 1, "b": 2, **rest}:
        pass
```

The `rest` argument has to be an identifier, and has to come last, and can't be `_`. Right now, we represent it as an identifier; this PR instead changes it to a `Pattern`, using `PatternMatchAs` with an empty `pattern` (which is equivalent to an identifier, and appears elsewhere in the pattern matching AST -- this is the pattern you get when you do `case x:` to match a wildcard and assign it to `x`).

The nice thing about this change is that the `rest` in `**rest` is now a node, not just an identifier, which I believe will significantly simplify some of the comment handling in https://github.com/astral-sh/ruff/pull/6836 (and will also enable that comment handling to look a _lot_ more like the dictionary comment handling).

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-08-24 05:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.6±0.04ms    11.4 MB/sec    1.00      3.6±0.04ms    11.4 MB/sec
formatter/numpy/ctypeslib.py               1.00    714.2±6.55µs    23.3 MB/sec    1.00   716.6±12.62µs    23.2 MB/sec
formatter/numpy/globals.py                 1.00     74.5±0.30µs    39.6 MB/sec    1.01     75.1±1.13µs    39.3 MB/sec
formatter/pydantic/types.py                1.00  1433.6±11.89µs    17.8 MB/sec    1.01  1446.9±22.46µs    17.6 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.02ms     3.9 MB/sec    1.01     10.4±0.02ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.8 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    319.7±0.87µs     9.2 MB/sec    1.00    317.2±0.78µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.01ms     4.7 MB/sec    1.00      5.4±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.4 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1176.1±6.71µs    14.2 MB/sec    1.01   1186.1±4.80µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    123.7±0.42µs    23.8 MB/sec    1.00    123.2±0.49µs    24.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.01      2.5±0.02ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.1±0.13ms     8.0 MB/sec    1.02      5.2±0.15ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1021.7±39.10µs    16.3 MB/sec    1.03  1050.4±47.48µs    15.9 MB/sec
formatter/numpy/globals.py                 1.00    105.2±9.08µs    28.1 MB/sec    1.05    110.2±9.28µs    26.8 MB/sec
formatter/pydantic/types.py                1.00      2.1±0.11ms    12.3 MB/sec    1.03      2.1±0.08ms    12.0 MB/sec
linter/all-rules/large/dataset.py          1.02     16.2±0.45ms     2.5 MB/sec    1.00     16.0±0.35ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.16ms     3.8 MB/sec    1.00      4.4±0.17ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   557.5±29.41µs     5.3 MB/sec    1.00   549.2±22.30µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.4±0.30ms     3.0 MB/sec    1.00      8.3±0.30ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01      9.0±0.33ms     4.5 MB/sec    1.00      8.9±0.21ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1882.5±57.22µs     8.8 MB/sec    1.02  1914.1±68.33µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    221.4±8.06µs    13.3 MB/sec    1.00    221.8±8.64µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.0±0.12ms     6.3 MB/sec    1.00      3.9±0.12ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @MichaReiser on 2023-08-24 06:57_

---

_Label `formatter` removed by @MichaReiser on 2023-08-24 06:59_

---

_Label `formatter` added by @MichaReiser on 2023-08-24 06:59_

---

_Label `parser` added by @MichaReiser on 2023-08-24 06:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1889 on 2023-08-24 07:14_

Why `Box<Pattern>` and not limiting it to the specific node?

---

_@MichaReiser reviewed on 2023-08-24 07:18_

This is interesting for two reasons:

* I don't understand why there's a dedicated node for `*name` but not `**name`
* It would be similar to `arguments` and `parameters`

The main downside that I see is that the AST now allows representing more invalid syntaxes (assuming someone mutates the AST). 

Other than that, there's a limitation that `**_` isn't valid syntax. The PEP only mentionts in the quick intro... 

> Mapping patterns: {"bandwidth": b, "latency": l} captures the "bandwidth" and "latency" values from a dict. Unlike sequence patterns, extra keys are ignored. A wildcard **rest is also supported. (But **_ would be redundant, so it is not allowed.) [[source](https://peps.python.org/pep-0636/#appendix-a-quick-intro)]

```
...     case {"a": 1, "b": 2, **_}: ...
  File "<stdin>", line 2
    case {"a": 1, "b": 2, **_}: ...
                            ^
SyntaxError: invalid syntax
```

Our parser silently parsed this before (and continues to do so now).  So this isn't a specific to this PR

Nevertheless, I'm a bit hesitant from making this change because I wonder why Python decided on their AST structure. Did you do some research to find why Python structured their AST the way they did?

---

_@konstin approved on 2023-08-24 10:44_

that's a good idea

---

_Comment by @charliermarsh on 2023-08-24 13:25_

> Nevertheless, I'm a bit hesitant from making this change because I wonder why Python decided on their AST structure. Did you do some research to find why Python structured their AST the way they did?

No, I can look into it. I assume it's because you _can't_ have arbitrary patterns on the right-hand side of a `**`, it _has_ to be an identifier, so it makes sense to represent it as an identifier. This is different from kwargs, where `**(1 + 2)` and so on are all valid.

---

_Comment by @charliermarsh on 2023-08-24 18:36_

I can't find much on it, after searching through the Discuss and the CPython repo itself, but it's hard to Google for it since there were a bunch of PEPs that went through a lot of iteration (some rejected, some superseded, etc.).

However, I did notice that in the PEP, they use `capture_pattern` for double star: https://peps.python.org/pep-0634/#mapping-patterns.

```
mapping_pattern: '{' [items_pattern] '}'
items_pattern: ','.key_value_pattern+ ','?
key_value_pattern:
    | (literal_pattern | value_pattern) ':' pattern
    | double_star_pattern
double_star_pattern: '**' capture_pattern
```

But we resolve `capture_pattern` to a `PatternMatchAs` with no `as` attribute:

```rust
CapturePattern: ast::Pattern = {
    <location:@L> <name:Identifier> <end_location:@R> => ast::PatternMatchAs {
        pattern: None,
        name: if name.as_str() == "_" { None } else { Some(name) },
        range: (location..end_location).into()
    }.into(),
}
```

This is the pattern we match when we see `case x` in:

```python
match foo:
    case x:
        ...
```

(Python also parses this as a `MatchAs` with no `as` attribute.)

And subsequently, we _don't_ use `CapturePattern` for `double_star_pattern` -- instead, we match a name identifier directly:

```rust
MappingPattern: ast::Pattern = {
    <location:@L> "{" "**" <rest:Identifier> ","? "}" <end_location:@R> => {
        ast::PatternMatchMapping {
            keys: vec![],
            patterns: vec![],
            rest: Some(rest),
            range: (location..end_location).into()
        }.into()
    },
}
```

Anyway, I'm not really sure either way on this one. I think this will make our lives easier, but it is a deviation from the CPython AST.


---

_Comment by @charliermarsh on 2023-08-24 18:48_

> Our parser silently parsed this before (and continues to do so now). So this isn't a specific to this PR

Separately: we can fix that by validating the name -- we already do that for `AsPattern`:

```rust
AsPattern: ast::Pattern = {
    <location:@L> <pattern:OrPattern> "as" <name:Identifier> <end_location:@R> =>? {
        if name.as_str() == "_" {
            Err(LexicalError {
                error: LexicalErrorType::OtherError("cannot use '_' as a target".to_string()),
                location,
            })?
        } else {
            Ok(ast::Pattern::MatchAs(
                ast::PatternMatchAs {
                   pattern: Some(Box::new(pattern)),
                   name: Some(name),
                   range: (location..end_location).into()
                },
            ))
        }
    },
}
```

---

_Comment by @charliermarsh on 2023-08-25 18:59_

Thinking on this more, though it is a formatter-centric change, I still find it odd that `rest` is not an AST node... I think I would expect it to be.

---

_Comment by @MichaReiser on 2023-08-25 21:36_

> Thinking on this more, though it is a formatter-centric change, I still find it odd that `rest` is not an AST node... I think I would expect it to be.

Maybe Identifier should be a node? It already has a range

---

_Comment by @charliermarsh on 2023-08-25 21:43_

That's an interesting idea, I wouldn't mind it.

---

_Comment by @MichaReiser on 2023-08-26 10:20_

> That's an interesting idea, I wouldn't mind it.

For me, changing Identifier to a node is easier to assess than whether we should change the pattern structure. I'm just not familiar enough with its representation and I would need to spend some time to get up to speed. 

Having a dedicated node for Identifiers would move toward my long-term goal of having a `ReferenceIdentifier` (when reading a value), and a `BindingIdentifier` (when declaring or re-declaring a binding). Having separate nodes should can be neat when querying only uses or writes. 

In the meantime, having `Identifier` as a node could be neat for other use cases too: E.g. finding all used names in a program. 

I'll leave it up to you to decide whether you want to change pattern or identifier because you have more context than I.

---

_Comment by @charliermarsh on 2023-08-26 13:43_

@MichaReiser - That makes sense, I will convert to draft until I have time to try it.

---

_Converted to draft by @charliermarsh on 2023-08-26 13:43_

---

_Comment by @charliermarsh on 2023-09-14 18:11_

Not a priority right now, will revisit eventually.

---

_Closed by @charliermarsh on 2023-09-14 18:11_

---

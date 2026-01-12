```yaml
number: 12419
title: "Explore using `bumpalo` for the AST"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: bump-allo
created_at: 2024-07-20T14:47:27Z
updated_at: 2024-08-07T14:48:38Z
url: https://github.com/astral-sh/ruff/pull/12419
synced_at: 2026-01-12T15:55:41Z
```

# Explore using `bumpalo` for the AST

---

_@MichaReiser_

This doesn't make full use of the bump allocator yet. We should also

* Use `bumpalo:Vec` instead of `Vec` except in places where the vec can become large (mainly suite?)
* Use the allocator in the lexer to avoid calls to `alloc_str` in the parser (which copies over the strings)
* Change `Int` to store a `&'ast str`
* Verify that no node (other than nodes storing a `Suite`) store any heap allocated data. Replace `Box<'ast, T>` with `&'ast mut T`. 


FIXME: 

I think the current implementation leaks memory. We need to wrap all `Stmt` in `Box`


## Performance results

### Linux

Baseline is this PR

```
     Running benches/parser.rs (target/release/deps/parser-ec26802c53781234)
parser/numpy/globals.py time:   [10.445 µs 10.449 µs 10.453 µs]
                        thrpt:  [282.28 MiB/s 282.39 MiB/s 282.49 MiB/s]
                 change:
                        time:   [+9.7857% +10.037% +10.220%] (p = 0.00 < 0.05)
                        thrpt:  [-9.2721% -9.1216% -8.9135%]
                        Performance has regressed.
parser/unicode/pypinyin.py
                        time:   [42.188 µs 42.200 µs 42.214 µs]
                        thrpt:  [99.538 MiB/s 99.570 MiB/s 99.600 MiB/s]
                 change:
                        time:   [+21.042% +21.139% +21.228%] (p = 0.00 < 0.05)
                        thrpt:  [-17.510% -17.450% -17.384%]
                        Performance has regressed.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) low mild
parser/pydantic/types.py
                        time:   [330.02 µs 330.14 µs 330.27 µs]
                        thrpt:  [77.219 MiB/s 77.248 MiB/s 77.277 MiB/s]
                 change:
                        time:   [+18.761% +18.851% +18.938%] (p = 0.00 < 0.05)
                        thrpt:  [-15.923% -15.861% -15.797%]
                        Performance has regressed.
Found 4 outliers among 100 measurements (4.00%)
  2 (2.00%) high mild
  2 (2.00%) high severe
parser/numpy/ctypeslib.py
                        time:   [141.37 µs 141.55 µs 141.76 µs]
                        thrpt:  [117.46 MiB/s 117.63 MiB/s 117.78 MiB/s]
                 change:
                        time:   [+20.951% +21.285% +21.647%] (p = 0.00 < 0.05)
                        thrpt:  [-17.795% -17.550% -17.322%]
                        Performance has regressed.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
parser/large/dataset.py time:   [806.13 µs 807.07 µs 808.15 µs]
                        thrpt:  [50.341 MiB/s 50.408 MiB/s 50.467 MiB/s]
                 change:
                        time:   [+23.943% +24.686% +25.328%] (p = 0.00 < 0.05)
                        thrpt:  [-20.209% -19.799% -19.318%]
                        Performance has regressed.
Found 16 outliers among 100 measurements (16.00%)
  3 (3.00%) high mild
  13 (13.00%) high severe

```

### Windows

```
parser/numpy/globals.py time:   [10.741 µs 10.780 µs 10.826 µs]
                        thrpt:  [272.56 MiB/s 273.73 MiB/s 274.72 MiB/s]
                 change:
                        time:   [+17.135% +18.111% +19.307%] (p = 0.00 < 0.05)
                        thrpt:  [-16.183% -15.334% -14.628%]
                        Performance has regressed.
Found 4 outliers among 100 measurements (4.00%)
  1 (1.00%) high mild
  3 (3.00%) high severe
parser/unicode/pypinyin.py
                        time:   [40.251 µs 40.344 µs 40.458 µs]
                        thrpt:  [103.86 MiB/s 104.15 MiB/s 104.39 MiB/s]
                 change:
                        time:   [+16.971% +17.762% +18.521%] (p = 0.00 < 0.05)
                        thrpt:  [-15.627% -15.083% -14.508%]
                        Performance has regressed.
Found 5 outliers among 100 measurements (5.00%)
  2 (2.00%) high mild
  3 (3.00%) high severe
parser/pydantic/types.py
                        time:   [309.84 µs 311.47 µs 313.18 µs]
                        thrpt:  [81.432 MiB/s 81.879 MiB/s 82.311 MiB/s]
                 change:
                        time:   [+17.420% +19.085% +20.424%] (p = 0.00 < 0.05)
                        thrpt:  [-16.960% -16.026% -14.835%]
                        Performance has regressed.
Found 4 outliers among 100 measurements (4.00%)
  2 (2.00%) high mild
  2 (2.00%) high severe
parser/numpy/ctypeslib.py
                        time:   [133.15 µs 133.65 µs 134.17 µs]
                        thrpt:  [124.11 MiB/s 124.59 MiB/s 125.06 MiB/s]
                 change:
                        time:   [-1.7608% -0.6216% +0.6461%] (p = 0.31 > 0.05)
                        thrpt:  [-0.6420% +0.6255% +1.7924%]
                        No change in performance detected.
Found 4 outliers among 100 measurements (4.00%)
  3 (3.00%) high mild
  1 (1.00%) high severe
parser/large/dataset.py time:   [785.39 µs 786.57 µs 787.76 µs]
                        thrpt:  [51.644 MiB/s 51.722 MiB/s 51.799 MiB/s]
                 change:
                        time:   [-0.6162% +0.1879% +1.0326%] (p = 0.67 > 0.05)
                        thrpt:  [-1.0221% -0.1875% +0.6200%]
                        No change in performance detected.
Found 7 outliers among 100 measurements (7.00%)
  1 (1.00%) low mild
  3 (3.00%) high mild
  3 (3.00%) high severe
```

---

_Comment by @charliermarsh on 2024-07-20 15:44_

Very interested in this!

---

_Comment by @MichaReiser on 2024-07-20 16:25_

Very painful is this

---

_Comment by @charliermarsh on 2024-07-20 16:52_

Do or do not, there is no try...

---

_Comment by @codspeed-hq[bot] on 2024-07-21 15:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/bump-allo)

### Merging #12419 will **improve performances by 19.16%**

<sub>Comparing <code>bump-allo</code> (56edbef) with <code>main</code> (2e2b1b4)</sub>



### Summary

`⚡ 10` improvements
`✅ 3` untouched benchmarks

`⁉️ 20` dropped benchmarks


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/bump-allo)._

### Benchmarks breakdown

|     | Benchmark | `main` | `bump-allo` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `formatter[large/dataset.py]` | 9.3 ms | N/A | N/A |
| ⁉️ | `formatter[numpy/ctypeslib.py]` | 1.9 ms | N/A | N/A |
| ⁉️ | `formatter[numpy/globals.py]` | 240.3 µs | N/A | N/A |
| ⁉️ | `formatter[pydantic/types.py]` | 3.5 ms | N/A | N/A |
| ⁉️ | `formatter[unicode/pypinyin.py]` | 656.2 µs | N/A | N/A |
| ⚡ | `lexer[large/dataset.py]` | 1,071.2 µs | 997 µs | +7.45% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 216.3 µs | 204.4 µs | +5.8% |
| ⚡ | `lexer[pydantic/types.py]` | 481.5 µs | 453 µs | +6.29% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 73.9 µs | 69.3 µs | +6.65% |
| ⁉️ | `linter/all-rules[large/dataset.py]` | 16.3 ms | N/A | N/A |
| ⁉️ | `linter/all-rules[numpy/ctypeslib.py]` | 4.1 ms | N/A | N/A |
| ⁉️ | `linter/all-rules[numpy/globals.py]` | 719.7 µs | N/A | N/A |
| ⁉️ | `linter/all-rules[pydantic/types.py]` | 8 ms | N/A | N/A |
| ⁉️ | `linter/all-rules[unicode/pypinyin.py]` | 2.2 ms | N/A | N/A |
| ⁉️ | `linter/default-rules[large/dataset.py]` | 3.7 ms | N/A | N/A |
| ⁉️ | `linter/default-rules[numpy/ctypeslib.py]` | 916.8 µs | N/A | N/A |
| ⁉️ | `linter/default-rules[numpy/globals.py]` | 183.9 µs | N/A | N/A |
| ⁉️ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | N/A | N/A |
| ⁉️ | `linter/default-rules[unicode/pypinyin.py]` | 356 µs | N/A | N/A |
| ⁉️ | `linter/all-with-preview-rules[large/dataset.py]` | 19.2 ms | N/A | N/A |
| ... | ... | ... | ... | ... |

<br/>

> :information_source: _Only the first 20 benchmarks are displayed. [Go to the app to view all benchmarks](https://codspeed.io/astral-sh/ruff/branches/bump-allo)._


---

_Comment by @MichaReiser on 2024-07-22 05:58_

The last commit where we use `bumpalo::Vec` seems like to have regressed performance by a bit. 

---

_Comment by @MichaReiser on 2024-07-23 08:19_

If anyone is interested in fixing some lifetime issues, feel free to PR it (or directly push to this branch). I might drop the last commit again because the result isn't promising enough.

---

_Comment by @MichaReiser on 2024-08-01 10:45_

Overall this is very painful for *marginal* wins. I wonder if an alternative AST structure that uses indices would be a better approach, similar to what Zig does https://ziglang.org/download/0.8.0/release-notes.html#Reworked-Memory-Layout

Let's say we have

```rust
#[derive(Debug, Clone)]
struct SyntaxNode {
	kind: NodeKind,
	parent: NodeIndex,
	children: Range<NodeIndex>,
	extra_data: Range<ExtraDataIndex>
}
```

and 

```rust
struct SyntaxTree {
	nodes: IndexVec<NodeIndex, SyntaxNode>,
	extra_data: IndexVec<ExtraDataIndex, ExtraData??>
}

struct Parsed {
	root: SyntaxNode,
	tree: Arc<SyntaxTree>
}
```

We could then have AST facade nodes similar to Biome/RustAnalzyer

```rust
#[derive(Clone)]
struct IfStatement {
	syntax: SyntaxNode,
	tree: Arc<SyntaxTree>

	fn test(&self) -> Expression {
		Expression::cast_unwrap(self.children(&tree)[0])
	}

	fn body(&self) -> Suite {
		Suite::cast_unwrap(self.children(&tree)[1])
	}


	fn children(&self) -> &[SyntaxNode] {
		self.syntax.children(&self.tree)
	}
```

It would require some changes to our AST, most notably that a node can no longer contain a sequence of children. Instead, it would need a single list node (e.g. suite) that wraps the list of children.

---

_Comment by @MichaReiser on 2024-08-07 14:48_

I still think this is interesting and if anyone wants to work on this, feel free to pick it up. But I don't have the required time to finish this anytime soon.

---

_Closed by @MichaReiser on 2024-08-07 14:48_

---

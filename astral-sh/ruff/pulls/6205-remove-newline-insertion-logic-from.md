```yaml
number: 6205
title: "Remove newline-insertion logic from `JoinNodesBuilder`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/move-joiner
created_at: 2023-07-31T17:52:30Z
updated_at: 2023-07-31T21:04:35Z
url: https://github.com/astral-sh/ruff/pull/6205
synced_at: 2026-01-12T02:58:30Z
```

# Remove newline-insertion logic from `JoinNodesBuilder`

---

_Pull request opened by @charliermarsh on 2023-07-31 17:52_

## Summary

This PR moves the "insert empty lines" behavior out of `JoinNodesBuilder` and into the `Suite` formatter. I find it a little confusing that the logic is split between those two formatters right now, and since this is _only_ used in that one place, IMO it is a bit simpler to just inline it and use a single approach to tracking state (right now, both are stateful).

The only other place this was used was for decorators. As a side effect, we now remove blank lines in both of these cases, which is a known but intentional deviation from Black (which preserves the empty line before the comment in the first case):

```python
@foo

# Hello
@bar
def baz():
    pass

@foo

@bar
def baz():
    pass
```


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-31 17:52_

---

_Review requested from @konstin by @charliermarsh on 2023-07-31 17:52_

---

_@charliermarsh reviewed on 2023-07-31 17:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/builders.rs`:79 on 2023-07-31 17:53_

This now has nothing specific to AST nodes (if that's what "nodes" means here), instead it's just a joiner that requires you to specify a custom separator for every node (which the generic joiner doesn't support).

---

_Comment by @github-actions[bot] on 2023-07-31 18:43_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     11.2±0.52ms     3.6 MB/sec     1.14     12.7±1.17ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.20ms     7.4 MB/sec     1.12      2.5±0.26ms     6.6 MB/sec
formatter/numpy/globals.py                 1.00   245.2±19.67µs    12.0 MB/sec     1.03   251.4±23.10µs    11.7 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.29ms     5.4 MB/sec     1.04      4.9±0.37ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.76ms     2.5 MB/sec     1.03     16.5±1.00ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.22ms     4.2 MB/sec     1.04      4.1±0.30ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   518.3±37.51µs     5.7 MB/sec     1.03   531.7±35.18µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.3±0.56ms     3.5 MB/sec     1.00      7.1±0.69ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.05      8.7±0.41ms     4.7 MB/sec     1.00      8.3±0.39ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1794.7±108.30µs     9.3 MB/sec    1.00  1760.2±119.66µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   198.9±12.94µs    14.8 MB/sec     1.08   215.1±25.98µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.23ms     6.8 MB/sec     1.08      4.0±0.31ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.22ms     3.9 MB/sec    1.00     10.4±0.15ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.01  1983.7±40.27µs     8.4 MB/sec    1.00  1969.0±32.06µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    213.0±4.35µs    13.9 MB/sec    1.00    213.7±6.65µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.07ms     5.9 MB/sec    1.01      4.4±0.08ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.30ms     2.9 MB/sec    1.01     14.2±0.27ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.6 MB/sec    1.00      3.6±0.06ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    441.0±9.05µs     6.7 MB/sec    1.00    443.0±8.51µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.14ms     4.1 MB/sec    1.00      6.2±0.12ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.12ms     5.3 MB/sec    1.01      7.8±0.13ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1566.7±20.73µs    10.6 MB/sec    1.00  1555.2±59.06µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.6±2.98µs    17.0 MB/sec    1.01    175.2±4.22µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.05ms     7.7 MB/sec    1.01      3.3±0.05ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-07-31 18:50_

How does join_nodes now differ from join?

Edit: I just saw your comment. I think using a for loop with individual write calls is easier. This joiner doesn't provide any real functionality anymore and is also not node specific 

---

_Comment by @charliermarsh on 2023-07-31 19:33_

Cool, that's fine with me. So I'll just remove it entirely?

---

_Comment by @charliermarsh on 2023-07-31 19:46_

Done.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:01_

Okay, I really need to write the restore context on drop helper... this new version never restored the context if any of the write operations above fail. This is not a real concern now but we should male sure to always properly restore the context. 

The easiest is to wrap the logic above in a format_with and call fmt directly on it

What I have in mind but is probably worth its own PR is to create a new struct that wraps a &mut Formatter<T>, implements Write, and restores the Context in its Drop implementation 

---

_@MichaReiser reviewed on 2023-07-31 20:01_

---

_@charliermarsh reviewed on 2023-07-31 20:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:05_

Ok got it, that makes sense. I will change it here.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:09_

Actually I'm not sure I know how to do this. I wrapped the above in a `format_with`, but now I have:

```rust
formatted.fmt(f)?;

f.context_mut().set_node_level(saved_level);

Ok(())
```

Isn't that still problematic? This seems problematic too since the `format_with` would now use the wrong node level:

```rust
f.context_mut().set_node_level(saved_level);

formatted.fmt(f)
```

---

_@charliermarsh reviewed on 2023-07-31 20:09_

---

_@MichaReiser reviewed on 2023-07-31 20:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:20_

You have to do store the result as the old implementation did, restore the context, and then return the result.

```
let result = formatted.fmt(f); 
f.context_mut().set_node_level(saved_level); 
result
```

But I'll put up a PR for a helper. we have this exact pattern in many places and it's very easy to get it wrong

---

_@charliermarsh reviewed on 2023-07-31 20:22_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:22_

Ah right, ok, thank you.

---

_@charliermarsh reviewed on 2023-07-31 20:23_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:23_

I can't do it because `iter` is captured in the closure :sob:

---

_@MichaReiser reviewed on 2023-07-31 20:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:27_

Can move the `iter` into the closure too?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:128 on 2023-07-31 20:29_

Yes solved, sorry.

---

_@charliermarsh reviewed on 2023-07-31 20:29_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-31 20:35_

---

_@MichaReiser approved on 2023-07-31 20:51_

---

_Merged by @charliermarsh on 2023-07-31 20:58_

---

_Closed by @charliermarsh on 2023-07-31 20:58_

---

_Branch deleted on 2023-07-31 20:58_

---

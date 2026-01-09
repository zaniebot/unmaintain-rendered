---
number: 15167
title: "Request: Option to only apply `superfluous-else-*` on multi-statement blocks (`RET505`, `RET506`, `RET507`, `RET508`)"
type: issue
state: open
author: Avasam
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-12-28T19:52:47Z
updated_at: 2025-07-26T06:36:43Z
url: https://github.com/astral-sh/ruff/issues/15167
synced_at: 2026-01-07T13:12:16-06:00
---

# Request: Option to only apply `superfluous-else-*` on multi-statement blocks (`RET505`, `RET506`, `RET507`, `RET508`)

---

_Issue opened by @Avasam on 2024-12-28 19:52_

I'm really into if-guards, and `superfluous-else-*` rules help enable that pattern.
```py
# RET505 will catch this
def foo(bar):
    if do_early_validation(bar):
        return  # or throw
    else:
        print(bar)
        # some longer block of code
        # ...
        # ...
        # ...
        # ...
        return bar

# and turn it into that
def foo(bar):
    if do_early_validation(bar):
        return  # or throw
        # my mind will mostly ignore this part when quickly reading the method to understand what it does

    print(bar)
    # some longer block of code
    # ...
    # ...
    # ...
    # ...
    return bar
```

However, I find that for single statements (a simple "if do this else do that") it doesn't work as well and the `else` keyword could actually help with the conditional flow readability.

Here's two concrete example from typeshed:

```py
def tests_path(distribution_name: str) -> Path:
    if distribution_name == "stdlib":
        return STDLIB_PATH / TESTS_DIR
    else:
        return STUBS_PATH / distribution_name / TESTS_DIR

def allowlists(distribution_name: str) -> list[str]:
    prefix = "" if distribution_name == "stdlib" else "stubtest_allowlist_"
    version_id = f"py{sys.version_info.major}{sys.version_info.minor}"

    platform_allowlist = f"{prefix}{sys.platform}.txt"
    version_allowlist = f"{prefix}{version_id}.txt"
    combined_allowlist = f"{prefix}{sys.platform}-{version_id}.txt"
    local_version_allowlist = version_allowlist + ".local"

    if distribution_name == "stdlib":
        return ["common.txt", platform_allowlist, version_allowlist, combined_allowlist, local_version_allowlist]
    else:
        return ["stubtest_allowlist.txt", platform_allowlist]
```
vs
```py
def tests_path(distribution_name: str) -> Path:
    if distribution_name == "stdlib":
        return STDLIB_PATH / TESTS_DIR
    return STUBS_PATH / distribution_name / TESTS_DIR

def allowlists(distribution_name: str) -> list[str]:
    prefix = "" if distribution_name == "stdlib" else "stubtest_allowlist_"
    version_id = f"py{sys.version_info.major}{sys.version_info.minor}"

    platform_allowlist = f"{prefix}{sys.platform}.txt"
    version_allowlist = f"{prefix}{version_id}.txt"
    combined_allowlist = f"{prefix}{sys.platform}-{version_id}.txt"
    local_version_allowlist = version_allowlist + ".local"

    if distribution_name == "stdlib":
        return ["common.txt", platform_allowlist, version_allowlist, combined_allowlist, local_version_allowlist]
    return ["stubtest_allowlist.txt", platform_allowlist]
```

So I'm suggesting that these rule could be only applied to cases where the `else` block is more than a single statement.

---

Moreover, users of ternary conditions (like [if-else-block-instead-of-if-exp (SIM108)](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-if-exp/#if-else-block-instead-of-if-exp-sim108)) may prefer turning the single-statement cases into something like this anyway:
```py
def tests_path(distribution_name: str) -> Path:
    return STDLIB_PATH / TESTS_DIR if distribution_name == "stdlib" else STUBS_PATH / distribution_name / TESTS_DIR

def allowlists(distribution_name: str) -> list[str]:
    return (
        ["common.txt", platform_allowlist, version_allowlist, combined_allowlist, local_version_allowlist]
        if distribution_name == "stdlib"
        else ["stubtest_allowlist.txt", platform_allowlist]
    )
```

Granted SIM108 doesn't currently cover that, it'd probably be a different rule (https://github.com/astral-sh/ruff/issues/13898#issuecomment-2436959490) but this is getting out of topic.

---

More keywords: superfluous-else-return , superfluous-else-raise, superfluous-else-continue, superfluous-else-break
Ruff: 0.8.4

---

_Referenced in [python/typeshed#13323](../../python/typeshed/pulls/13323.md) on 2024-12-28 20:01_

---

_Comment by @MichaReiser on 2024-12-30 10:53_

> So I'm suggesting that these rule could be only applied to cases where the else block is more than a single statement.

I think this would lead to too many false-negatives, making the user less useful for users who strictly prefer `if` without an else block. I could see an argument to skip over `if` that is at the end of the function, and both branches end in a return statement (without any other early returns?)

My worry is that this will make the rule harder to explain, and I do think that flagging your examples is in the rule's intent. However, I do agree that explicit `if..else` branches can sometimes be useful, and the way this is done in Rust is that clippy flags:

```rust
if cond {
	return val;
} else {
	return val2;
}
```

but the following is fine

```rust
if cond {
	val
} else {
	val2
}
```



---

_Label `rule` added by @MichaReiser on 2024-12-30 10:53_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-30 10:53_

---

_Renamed from "Request: Option to only apply `superfluous-else-*` on single-statements (`RET505`, `RET506`, `RET507`, `RET508`)" to "Request: Option to only apply `superfluous-else-*` on multi-statement blocks (`RET505`, `RET506`, `RET507`, `RET508`)" by @Avasam on 2025-06-21 03:50_

---

_Comment by @Anselmoo on 2025-07-26 06:36_

### Additional comment about`RET506` feel too aggressive for simple two-way branches

## Environment
* **Ruff**: 0.12.5  
* **Python**: 3.11  
* Rule: `RET506  superfluous-else-return` (same family as `RET505/RET507/RET508`)

---

#### Reproducer

```python
def transform(direction: str, params: Params) -> Result:
    if direction == "case_a":
        return transformer.case_a(params)
    elif direction == "case_b":
        return transformer.case_b(params)
    else:
        raise ValueError(f"Unknown direction: {direction}")
```

Ruff reports:

```Unnecessary `elif` after `return` statementRuff(RET505)```

```
[{
	"resource": "demo.py",
	"owner": "Ruff",
	"code": {
		"value": "RET505",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/superfluous-else-return",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 4,
	"message": "Unnecessary `elif` after `return` statement",
	"source": "Ruff",
	"startLineNumber": 4,
	"startColumn": 5,
	"endLineNumber": 4,
	"endColumn": 9,
	"origin": "extHost2"
}]
```

---

### Points to consider:

1. **Logical grouping** –  
   The three branches form one closed decision tree (`case_a` / `case_b` / “everything else`).  
   Converting the middle branch to a top-level `if` breaks that visual grouping and can make the final
   `raise` look disconnected from the decision.

2. **Maintaining the “catch-all” semantics** –  
   The trailing `else` is deliberately the error path.  
   An `elif … else …` chain communicates that more directly than two separate `if …` blocks followed
   by an unconditional `raise`.

3. **Readability in longer chains** –  
   In real code there may be several mutually-exclusive directions (`case_c`, …).  
   Using `if/elif/…/else` makes it obvious that exactly one branch will run, while a series of
   top-level `if` statements requires the reader to notice the early returns.


> Concerning Point **3**, is there a `RUFF` rule to transform `if-elif` chains into the [`match-case`-syntax](https://docs.python.org/3/whatsnew/3.10.html#pep-634-structural-pattern-matching)?


---

### Proposed change / discussion points

* Provide an option (e.g. `superfluous-else-min-body-lines = 2`) so `RET506` only fires when the
  `elif` block spans more than one statement, similar to the discussion in #15167.
* Alternatively, add a project-level toggle to disable `RET506` while keeping the other
  `superfluous-else-*` rules enabled.
* Document common stylistic exceptions where keeping the `elif` is idiomatic.

Thanks for considering!


---

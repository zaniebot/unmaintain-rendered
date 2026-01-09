---
number: 14007
title: "Feature request: Add rule to detect potential unintended string iteration"
type: issue
state: open
author: l1n
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-10-30T19:16:40Z
updated_at: 2025-05-28T08:31:39Z
url: https://github.com/astral-sh/ruff/issues/14007
synced_at: 2026-01-07T13:12:16-06:00
---

# Feature request: Add rule to detect potential unintended string iteration

---

_Issue opened by @l1n on 2024-10-30 19:16_

## Problem Statement
Python strings being sequences can lead to unintended character-by-character iteration, which is a common source of bugs (https://mail.python.org/pipermail/python-3000/2006-April/000759.html is 2006!). Currently, there's no built-in rule (that I know of) to detect potential cases where a string might be accidentally iterated over instead of being treated as a single value or being split first.

## Use Cases

```python
# Common bug cases this would catch:

# Case 1: Direct string iteration
name = "Alice"
for char in name:  # Should warn - likely wanted to iterate over a list of names
    process_name(char)  # Gets 'A', 'l', 'i', etc. instead of "Alice"

# Case 2: String in collections
names = get_name()  # Returns str
for name in names:  # Should warn - iterating over characters
    print(name)

# Case 3: Type annotated strings
def process_items(items: str):  # Type hint indicates string
    for item in items:  # Should warn - likely wanted List[str]
        handle_item(item)

# Safe cases (no warning):
names = ["Alice", "Bob"]
for name in names:
    print(name)

text = "hello"
chars = list(text)  # Explicit conversion to character list
for char in chars:
    print(char)
```

## Proposed Rule Details

- Message: "Possible unintended string iteration. Consider using split(), list(), or ensuring iteration over string is intended."

The rule would detect:
1. For loops iterating over string literals
2. For loops iterating over variables annotated as `str`
3. Iteration over strings in common higher-order functions (map, filter, etc.)
4. Would respect explicit noqa comments and configurations

---

_Comment by @MichaReiser on 2024-10-31 07:22_

@AlexWaygood I would value your opinion on this. To me, using `for` to iterate over the characters of a string seems very legit. 

---

_Label `rule` added by @MichaReiser on 2024-10-31 07:22_

---

_Label `needs-decision` added by @MichaReiser on 2024-10-31 07:22_

---

_Comment by @AlexWaygood on 2024-10-31 10:14_

Iteration over the characters of a string is a useful Python feature that's never going to go away. It's also one of the most common footguns in Python programming; there have been many requests to Python _type checkers_ (let alone linters) to try to tackle this problem, and pytype actually implements a version of this. For examples, see:
- https://discuss.python.org/t/removing-sequence-str-as-base-class-of-str/56907
- https://github.com/python/typing/issues/256
- https://github.com/microsoft/pyright/pull/8259
- https://github.com/hauntsaninja/useful_types/blob/305c66f860117ea41e0dd1f951f2fc3b4786c57e/useful_types/__init__.py#L283-L295

There's definitely a strong user need for rules that can warn about this, even if they will necessarily come with false positives for the (rare) occasion when you really do need to iterate over the characters of a string. So I think we should definitely explore implementing this check... But it will probably have to wait until we migrate to using red-knot as the backend for Ruff (it needs strong type inference).

---

_Label `needs-decision` removed by @AlexWaygood on 2024-10-31 10:14_

---

_Label `type-inference` added by @AlexWaygood on 2024-10-31 10:15_

---

_Label `accepted` added by @AlexWaygood on 2024-10-31 10:15_

---

_Label `accepted` removed by @AlexWaygood on 2024-10-31 10:15_

---

_Comment by @l1n on 2024-11-03 06:28_

 I agree that there are legitimate cases for iterating over characters of a string and I find the syntax elegant, but I'd prefer to make the intent explicit (or turn off the rule for projects that use it often), so thank you for the update! Is #11653 the right issue to follow for the red-knot move?

---

_Comment by @AlexWaygood on 2024-11-03 09:15_

> Is #11653 the right issue to follow for the red-knot move?

Unfortunately that's just one subtask we need to get to for an initial release of red-knot (and it will be only after the initial red-knot release that we'll start migrating Ruff rules to use red-knot as the backend). So it will probably be a while, I'm afraid :-)

---

_Comment by @opk12 on 2025-05-28 08:31_

5. Passing `str` to an API that wants `Iterable[str]`.

```
def f(sentences: Iterable[str]):
    if 'Alice' in s.split():
        print("Found")

f("Hello Alice")
```

---

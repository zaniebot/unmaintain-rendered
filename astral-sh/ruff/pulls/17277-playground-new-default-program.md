```yaml
number: 17277
title: "[playground] New default program"
type: pull_request
state: merged
author: sharkdp
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: david/new-default-program
created_at: 2025-04-07T14:24:23Z
updated_at: 2025-04-07T20:18:30Z
url: https://github.com/astral-sh/ruff/pull/17277
synced_at: 2026-01-10T19:40:37Z
```

# [playground] New default program

---

_Pull request opened by @sharkdp on 2025-04-07 14:24_

## Summary

This PR proposes to change the default example program in the playground. I realize that this is somewhat underwhelming, but I found it rather difficult to come up with something that circumvented missing support for overloads/generics/self-type, while still looking like (easy!) code that someone might actually write, and demonstrating some Red Knot features. One thing that I wanted to capture was the experience of adding type constraints to an untyped program. And I wanted something that could be executed in the Playground once all errors are fixed.

Happy for any suggestions on what we could do instead. I had a lot of different ideas, but always ran into one or another limitation. So I guess we can also iterate on this as we add more features to Red Knot.

Try it here: https://playknot.ruff.rs/8e3a96af-f35d-4488-840a-2abee6c0512d
```py
from typing import Literal

type Style = Literal["italic", "bold", "underline"]

# Add parameter annotations `line: str, word: str, style: Style` and a return
# type annotation `-> str` to see if you can find the mistakes in this program.

def with_style(line, word, style):
    if style == "italic":
        return line.replace(word, f"*{word}*")
    elif style == "bold":
        return line.replace(word, f"__{word}__")

    position = line.find(word)
    output = line + "\n"
    output += " " * position
    output += "-" * len(word)


print(with_style("Red Knot is a fast type checker for Python.", "fast", "underlined"))
```

closes https://github.com/astral-sh/ruff/issues/17267

## Test Plan

<!-- How was it tested? -->


---

_Label `playground` added by @sharkdp on 2025-04-07 14:24_

---

_Label `red-knot` added by @sharkdp on 2025-04-07 14:24_

---

_Comment by @InSyncWithFoo on 2025-04-07 15:03_

I think there should be a little bit more than that.

Currently, Red Knot has multiple language features. It also understands code flows. The example should demonstrate those:

```python
class User: ...

def create_user(preferred_name: str | None = None) -> User:
	if preferred_name is None:       # Hover over `preferred_name` on this line to see its type.
		print(preferred_name)        # Do that again for this line. The type is now `None`.

		preferred_name = 'Red Knot'
		print(preferred_name)        # Do that a third time. The type is now `Literal['Red Knot']`.

	return User(preferred_name)      # At this point, the type is `str`.

# Red Knot knows where types are defined as well.
# Try it out by right clicking `create_user`, then choose "Go to Type Definition".
user = create_user()
```

---

_@MichaReiser approved on 2025-04-07 16:04_

I do like the example. I could see us have a readme / documentation page where we list some interesting playground that could demonstrate other important features (as @InSyncWithFoo is suggesting)

---

_Comment by @sharkdp on 2025-04-07 19:51_

@InSyncWithFoo Thank you for the example, it demonstrates the LSP features well, but that's not all there is. I don't want to make the code too long, i.e. I don't want to have a collection of unrelated functions. But I like what @MichaReiser suggests, so let's keep this in mind.

As I said, I'm also happy to change this again, but for now, I like it more than ..
```py
import os
```
.. so I'm merging this for now :smile:.

---

_Merged by @sharkdp on 2025-04-07 19:52_

---

_Closed by @sharkdp on 2025-04-07 19:52_

---

_Branch deleted on 2025-04-07 19:52_

---

_Comment by @carljm on 2025-04-07 19:58_

Using hover on this example shows some issues that we probably want to fix if this will be our default playground sample:
1. We infer `@Todo` for `str + Literal["\n"]`
2. We show `Never` as the hover type of `output` in `output += ...`
3. We infer `@Todo` for `Literal["*"] * int`


---

_Comment by @sharkdp on 2025-04-07 20:11_

> Using hover on this example shows some issues that we probably want to fix if this will be our default playground sample:

Yes, I should have mentioned this. I did realize that and accepted it because it "only" requires support for overloads. There are no generics in `str.__add__` and `str.__mul__`.

> 2\. We show `Never` as the hover type of `output` in `output += ...`

This part I missed. Should we open a ticket for it?

---

_Comment by @MichaReiser on 2025-04-07 20:17_

> > Using hover on this example shows some issues that we probably want to fix if this will be our default playground sample:
> 
> 
> 
> Yes, I should have mentioned this. I did realize that and accepted it because it "only" requires support for overloads. There are no generics in `str.__add__` and `str.__mul__`.
> 
> 
> 
> > 2\. We show `Never` as the hover type of `output` in `output += ...`
> 
> 
> 
> This part I missed. Should we open a ticket for it?


There's a ticket for 2. That I opened last week

Edit: https://github.com/astral-sh/ruff/issues/17122

---

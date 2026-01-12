```yaml
number: 2102
title: local vars reported as types in lsp autocompletion
type: issue
state: open
author: egorbn
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-19T06:50:09Z
updated_at: 2026-01-09T15:52:29Z
url: https://github.com/astral-sh/ty/issues/2102
synced_at: 2026-01-12T15:54:26Z
```

# local vars reported as types in lsp autocompletion

---

_@egorbn_

### Question

Not sure if this is expected behavior, but locally defined variables are reported as `Struct` or `Value`, when really they should be reported as `Variable` in the lsp autocompletion.

Here's some example code:
```python
def bar(baz: int) -> int:
    return baz


if __name__ == "__main__":
    msg = "Hello ty"
    print(msg)

    number = bar(3)
    print(number)
```
https://play.ty.dev/8825913f-afcf-46be-875e-0ce1553f977e

# `Value` kind
When completing `msg` in the print statement, the "kind" of the autocomplete is shows as `Value` in `neovim`.

In the online playground the kind icon is also not variable kind, so I'm guessing it's also linked to something like `Value`.

This kind is shown when variables are declared with literal values.

## `neovim`

<img width="1118" height="65" alt="Image" src="https://github.com/user-attachments/assets/851296e5-f875-47d6-9c20-46999bacd588" />

## online playground

<img width="545" height="65" alt="Image" src="https://github.com/user-attachments/assets/0ee73a4f-c416-4b30-ab95-e600adb79c71" />

# `Struct` kind

When a variable is assigned the return value of a function, the kind is reported as `Struct`.

## `neovim`

<img width="965" height="87" alt="Image" src="https://github.com/user-attachments/assets/05c7514e-1dc5-4f6a-abbe-acb0a23fe2df" />

## online playground

<img width="547" height="70" alt="Image" src="https://github.com/user-attachments/assets/cb0a02d1-9697-4831-b748-f6bfdcce26e1" />

# Question
Is this expected behavior?

# Suggestion
I think it would be better to report both as `Variable` kind, in-line with most tools (at least the ones I'm familiar with).

When I see an autocomplete for a local variable, I want to know that it's a local variable.

### Version

ty 0.0.4 (c1e6188b1 2025-12-18)

---

_Label `question` added by @egorbn on 2025-12-19 06:50_

---

_Label `question` removed by @AlexWaygood on 2025-12-19 08:32_

---

_Label `server` added by @AlexWaygood on 2025-12-19 08:32_

---

_Label `completions` added by @AlexWaygood on 2025-12-19 08:32_

---

_Assigned to @BurntSushi by @AlexWaygood on 2025-12-19 08:32_

---

_Comment by @BurntSushi on 2025-12-19 14:39_

This is a little tricky to me. I'm not quite sure what the right answer is here. I definitely do agree that using a kind of "struct" for an `int` is very odd though, and we should improve there.

At least currently, the "kind" of a completion for a symbol in scope is determined by its type:

https://github.com/astral-sh/ruff/blob/e177cc2a5a5d5e25c06ea6b0bbac9209f37fe756/crates/ty_ide/src/completion.rs#L325-L383

An `int` value has this type:

```
NominalInstance(
    NominalInstanceType(
        NonTuple(
            NonGeneric(
                ClassLiteral(
                    Id(4c01),
                ),
            ),
        ),
    ),
)
```

Arguably changing this to a `Value` kind would be an improvement over `Struct`. But it's still not quite what you're looking for here.

This makes me wonder whether using the type (or only the type) is the right thing? Consider this example:

```python
class zqzqzq: pass
zqzqzq_var = zqzqzq
zqzq<CURSOR>
```

Right now we return:

```
zqzqzq :: Class
zqzqzq_var :: Class
```

I think a kind of `Class` for `zqzqzq` is correct. But what should the kind for `zqzqzq_var` be? It still seems like `Class` is appropriate here _and_ more specific than, say, `Variable`.

Maybe I'm overthinking this and we should use simpler rules for determining the kind and just go by how the name was bound initially. So `zqzqzq` would continue being a `Class` but `zqzqzq_var` would be a `Variable`.

---

_Comment by @MichaReiser on 2025-12-19 15:08_

I would expect `Variable` for `_var`, which is also what I'd expect us to show if I hover it (it tells me it's a variable of type `Class`). This is also what pylance does.

Is `zqzqzq_var = zqzqzq` the var in this example a legacy type alias? in which case variable does make sense too, I think

---

_Comment by @AlexWaygood on 2025-12-19 15:10_

This does sound like we should be determining this on the `DefinitionKind` of the variable (from the use-def map) rather than the type of the variable

---

_Comment by @BurntSushi on 2025-12-19 15:43_

@AlexWaygood Do you think we can use [`Type::definition`](https://github.com/astral-sh/ruff/blob/e177cc2a5a5d5e25c06ea6b0bbac9209f37fe756/crates/ty_python_semantic/src/types.rs#L8400) to get a [`TypeDefinition`](https://github.com/astral-sh/ruff/blob/e177cc2a5a5d5e25c06ea6b0bbac9209f37fe756/crates/ty_python_semantic/src/types/definition.rs#L8-L17)? And then from there it's straight-forward to get a `Definition` and thus a `DefinitionKind`.

This would avoid needing to use the use-def map directly (which seems trickier?).

---

_Comment by @BurntSushi on 2025-12-19 15:46_

No, sadly, that doesn't work. That's for the _type_ definition. Ignore me.

---

_Comment by @MichaReiser on 2025-12-19 16:17_

> This would avoid needing to use the use-def map directly (which seems trickier?).

You might also be able to use some of the go-to-definition machinery for this

---

_Comment by @egorbn on 2025-12-19 16:29_

Thanks folks.

---

_Comment by @BurntSushi on 2025-12-19 16:32_

> > This would avoid needing to use the use-def map directly (which seems trickier?).
> 
> You might also be able to use some of the go-to-definition machinery for this

Yeah I looked at that. But it occurred to me that, at least in some cases, we're already accessing the use-def map to get completions in the first place. And we can usually get the definition when we do it.

---

_Unassigned @BurntSushi by @BurntSushi on 2026-01-09 15:52_

---

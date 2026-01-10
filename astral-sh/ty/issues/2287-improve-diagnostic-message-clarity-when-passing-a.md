```yaml
number: 2287
title: Improve diagnostic message clarity when passing a class type instead of an instance
type: issue
state: open
author: kkpattern
labels:
  - diagnostics
assignees: []
created_at: 2025-12-31T06:40:08Z
updated_at: 2026-01-05T16:51:39Z
url: https://github.com/astral-sh/ty/issues/2287
synced_at: 2026-01-10T01:56:41Z
```

# Improve diagnostic message clarity when passing a class type instead of an instance

---

_Issue opened by @kkpattern on 2025-12-31 06:40_


When passing a class object (e.g., `Foo`) to a function that expects an instance of that class (e.g., `a: Foo`), the current error message can be visually confusing.

The distinction between `Expected Foo` and `found <class 'Foo'>` is subtle and can be difficult to spot at a glance, especially for beginners or in complex error logs.

```python
class Foo:
    pass

def test(a: Foo):
    pass

def main() -> None:
    a = Foo
    test(a)  # Passing the class itself, not an instance
```

### Current Output

```text
error[invalid-argument-type]: Argument to function `test` is incorrect
  --> test.py:11:10
   |
 9 | def main() -> None:
10 |     a = Foo
11 |     test(a)
   |          ^ Expected `Foo`, found `<class 'Foo'>`
   |
```

The string `<class 'Foo'>` looks very similar to the expected type `Foo`. It doesn't clearly communicate that the issue is a **Type vs. Instance** mismatch.

We can adopt the `type[Foo]` notation used by other type checkers (like `pyright` and `mypy`). This makes it more clear that a Type object was passed instead of an instance.

**Pyright:**
```text
test.py:11:10 - error: Argument of type "type[Foo]" cannot be assigned to parameter "a" of type "Foo" in function "test"
```

**Mypy:**
```text
test.py:11: error: Argument 1 to "test" has incompatible type "type[Foo]"; expected "Foo"  [arg-type]
```

---

_Label `diagnostics` added by @MichaReiser on 2025-12-31 08:18_

---

_Comment by @AlexWaygood on 2026-01-01 15:07_

Thanks for the report, and sorry for the confusing diagnostic!

Unfortunately `<class 'Foo'>` represents a different type in our model to `type[Foo]`. The reason why we don't use `type[Foo]` in our diagnostic here is because `type[Foo]` isn't the type we inferred for the object you passed in!

`type[Foo]` represents "the class object `Foo`, _and any subclass of `Foo`_". Like most types in Python, we cannot say how many possible runtime objects there might be that would satisfy this type: it has potentially infinite size. `<class 'Foo'>`, on the other hand, is a much more precise type. It represents "only the class object `Foo` (and not any subclasses of `Foo`)". We know exactly how big this type is: it has exactly one runtime inhabitant.

`<class 'Foo'>` is a subtype of `type[Foo]`, therefore, but they're not the same type. So we shouldn't use `type[Foo]` in our display, because that would inaccurately blur the distinction between the two types.

Here are some other strategies I can think of to make this situation less confusing:
- Use another display for "class-literal" types? But I'm not immediately sure what display we could give class-literal types that would be less confusing
- Always add a `note:` subdiagnostic explaining what class-literal types are, whenever class-literal types appear in a diagnostic. But this would be tricky to implement, I think -- I'm not sure there's an easy, generalised way for us to be able to detect whenever a diagnostic mentions a class-literal type in its prose.
- Add an FAQ to https://docs.astral.sh/ty/reference/typing-faq/ that describes what class-literal types are and how they differ from `type[]` types
- Provide documentation when hovering over a class-literal type that describes what the type means, and how it's different to `type[]` types

The last two of these strategies are relatively straightforward to implement, I think; we should probably just do them. But they might not be very effective at reducing user confusion. The first two would be more effective to reduce confusion but there are some open questions about how we'd implement them.

---

_Comment by @kkpattern on 2026-01-01 16:06_

Thanks for the detailed explanation!

Confusion regarding `<class 'Foo'>` usually arises when `Foo` is expected, but the class object is passed instead. Perhaps we could handle this by adding a specific note for this case (e.g., 'Did you mean to pass an instance?') rather than changing the type display entirely. It wouldn't cause much confusion if we incorrectly pass a class literal like `Foo` to distinct types like `int`.

Also, currently this is quite misleading when combined with instance variables:

```python
class Foo:
    pass

def dosomething_with_foo(foo: Foo):
    pass

class Bar:
    def __init__(self, foo: Foo):
        self._foo = Foo

    def dosomething(self):
        dosomething_with_foo(self._foo)
```

```
error[invalid-argument-type]: Argument to function `dosomething_with_foo` is incorrect
  --> test.py:14:30
   |
13 |     def dosomething(self):
14 |         dosomething_with_foo(self._foo)
   |                              ^^^^^^^^^ Expected `Foo`, found `Unknown | <class 'Foo'>`
   |
info: Element `<class 'Foo'>` of this union is not assignable to `Foo`
info: Function defined here
 --> test.py:5:5
  |
5 | def dosomething_with_foo(foo: Foo):
  |     ^^^^^^^^^^^^^^^^^^^^ -------- Parameter declared here
6 |     pass
  |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

In this scenario, developers often suspect the Unknown type is the culprit, failing to realize it's actually a class literal issue.

---

_Comment by @AlexWaygood on 2026-01-01 16:19_

> In this scenario, developers often suspect the Unknown type is the culprit, failing to realize it's actually a class literal issue.

hmm, though we already have a `info:` subdiagnostic there that specifically says which union element it is that caused the issue (`<class 'Foo'>`, not `Unknown`). But yes, I can see that the wording there is confusing -- we could certainly add more subdiagnostics to certain specific error codes if we think that class-literal types are going to be a common source of confusion for those error codes.

---

_Comment by @kkpattern on 2026-01-01 16:38_

> hmm, though we already have a info: subdiagnostic there that specifically says which union element it is that caused the issue (<class 'Foo'>, not Unknown). 

This is nice! Sorry I didn't notice it because my neovim setup didn't show the info in editor. I think it's because the `concise` output format is used. Can we ask ty server to output in full format?

<img width="1243" height="183" alt="Image" src="https://github.com/user-attachments/assets/3a0d8ad6-0a53-4cdd-9be3-b84bcd6cbf95" />

---

_Comment by @MichaReiser on 2026-01-03 17:28_

> This is nice! Sorry I didn't notice it because my neovim setup didn't show the info in editor. I think it's because the concise output format is used. Can we ask ty server to output in full format?

We do for clients that support [`relatedInformation`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#publishDiagnosticsClientCapabilities) and it seems neovim [supports it](https://neovim.io/doc/user/lsp.html#lsp-diagnostic) (I'm not a neovim user myself). 

However, we don't show additional `info` level messages in the LSP today because the LSP specification, to my knowledge, doesn't provide a way to annotating a diagnostic with additional information (that goes beyond the one line summary message)

 

---

_Comment by @dhruvmanila on 2026-01-05 10:09_

> We do for clients that support [`relatedInformation`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#publishDiagnosticsClientCapabilities) and it seems neovim [supports it](https://neovim.io/doc/user/lsp.html#lsp-diagnostic) (I'm not a neovim user myself).

This is relatively new feature (https://github.com/neovim/neovim/pull/34474) and I think it's only available on the nightly Neovim.

---

_Comment by @carljm on 2026-01-05 15:41_

There is also #1471 to reconsider our display for class-literal types in the LSP.

I think besides that, the other thing that potentially makes sense to me to do here is to add an extra info diagnostic specifically when we fail assignability of a class type to an instance type of the same class?

I also think the LSP-specific discussion here is another argument in favor of making our concise and LSP diagnostics less lossy? (I assume we can actually put as many "lines" as we want in the LSP "one-line summary" .)

---

_Comment by @MichaReiser on 2026-01-05 16:51_

> (I assume we can actually put as many "lines" as we want in the LSP "one-line summary" .)


That's something we had to try. I don't know how different clients render very long diagnostics on hover (do they truncate? to they wrap the lines? How do they render very long diagnostics in the summary panel)

---

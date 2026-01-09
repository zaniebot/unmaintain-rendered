---
number: 3893
title: Static type checking à la mypy
type: issue
state: open
author: saada
labels:
  - type-inference
assignees: []
created_at: 2023-04-05T23:12:09Z
updated_at: 2025-05-07T19:14:03Z
url: https://github.com/astral-sh/ruff/issues/3893
synced_at: 2026-01-07T13:12:14-06:00
---

# Static type checking à la mypy

---

_Issue opened by @saada on 2023-04-05 23:12_

Would be amazing if Ruff had static type checking. Mypy is a well known solution in this area as well as pytype, pyre, and others.

As a follow up, we could have some way of auto-fixing type annotations.

Thank you for this incredible project 😍

---

_Label `type-inference` added by @charliermarsh on 2023-04-13 17:27_

---

_Comment by @charliermarsh on 2023-04-13 17:29_

I will admit that I'm interested in this and it fits into the vision of what we're trying to do, but it would be irresponsible of me to promise or commit to anything yet :)

---

_Referenced in [astral-sh/ruff#6543](../../astral-sh/ruff/issues/6543.md) on 2023-08-14 23:48_

---

_Comment by @NeilGirdhar on 2023-08-15 00:12_

I'm not sure if you are aware, but [Pylyzer](https://github.com/mtshiba/pylyzer) is a Python type checker that's written in Rust.  It might be worth seeing if there's a way to have all the desired features with less effort.

---

_Comment by @zanieb on 2023-08-15 05:25_

Oh that's interesting! It uses Erg and isn't quite feature complete, I'm not sure if we could use it but it's great to see another Rust Python tool.

---

_Comment by @NeilGirdhar on 2023-08-15 07:31_

I don't know much about Rust, but would it be possible for them to expose an API that Ruff could call into?  If so, the Ruff and Pylyzer teams could work out an API that Pylyzer exposes and Rust could call into?  Even if you don't end up using Pylyzer, knowing the API that Ruff needs would be useful if Ruff were to implement its own type analysis.  (Just thinking out loud.)

---

_Referenced in [astral-sh/ruff#970](../../astral-sh/ruff/issues/970.md) on 2023-10-26 19:39_

---

_Referenced in [GenericMappingTools/pygmt#2808](../../GenericMappingTools/pygmt/pulls/2808.md) on 2023-12-01 23:03_

---

_Referenced in [astral-sh/ruff#9816](../../astral-sh/ruff/issues/9816.md) on 2024-02-04 20:59_

---

_Referenced in [astral-sh/ruff#9820](../../astral-sh/ruff/issues/9820.md) on 2024-02-05 07:05_

---

_Referenced in [astral-sh/ruff-lsp#378](../../astral-sh/ruff-lsp/issues/378.md) on 2024-02-10 12:24_

---

_Comment by @thibaut-st on 2024-02-15 08:05_

Is there any dev started on that feature? It would be so awesome to have all the essential python code quality tools in one fast as hell dependency! 

---

_Referenced in [astral-sh/ruff#10234](../../astral-sh/ruff/issues/10234.md) on 2024-03-05 09:06_

---

_Comment by @nolanking90 on 2024-04-02 23:38_

I'm interested in moving forward on this. I would like to begin working on features that could be exposed to ruff-lsp to add more of the traditional LSP capabilities, and it sounds like getting some type checking/inference working is going to be necessary before something like 'textDocument/hover' can be handled (by ruff-lsp) in a useful way. 

Some input from the maintainers about implementation would be helpful, if this is still something the core team is interested in. 



---

_Comment by @zanieb on 2024-04-03 01:43_

Thanks for expressing your interest! Unfortunately the scope of this feature is far too large for external contribution, it will require major architectural changes to Ruff and extensive collaboration with our team. We're moving in this direction though. We can mark any related issues that would be good for external contribution with a "help wanted" label, as I'm sure there will be lots of smaller tasks that arise.

We're also recently kicked off a rewrite of `ruff-lsp` in Rust, see https://github.com/astral-sh/ruff/pull/10158. Once it's established, I'm sure there will be issues in that project that are good for contribution.

---

_Referenced in [torchgeo/torchgeo#1986](../../torchgeo/torchgeo/issues/1986.md) on 2024-04-07 10:59_

---

_Comment by @johnthagen on 2024-05-19 11:54_

For those interested, looks like initial work for this is here

- https://github.com/astral-sh/ruff/tree/main/crates/red_knot

---

_Comment by @KotlinIsland on 2024-07-23 03:32_

> à la mypy

please don't make it like mypy, closer to pyright would be much more tolerable. although considering how based the maintainers are here, i'm confident they would know what they are doing

---

_Comment by @kszlim on 2024-07-30 19:23_

Curious if there's a tracking issue for the progression of this feature? Would be great to gain some visibility into this (with the caveat that I certainly don't have any expectations around the arrival date).

---

_Referenced in [astral-sh/ruff#12999](../../astral-sh/ruff/issues/12999.md) on 2024-08-20 01:18_

---

_Referenced in [astral-sh/ruff#13065](../../astral-sh/ruff/issues/13065.md) on 2024-08-23 02:04_

---

_Referenced in [ami-iit/jaxsim#190](../../ami-iit/jaxsim/issues/190.md) on 2024-08-26 12:44_

---

_Referenced in [vega/altair#3536](../../vega/altair/pulls/3536.md) on 2024-09-16 14:39_

---

_Comment by @leontrolski on 2024-11-01 12:00_

An API that I'd love to see from a future typechecker (I feel this is sorely missing with `mypy` and friends):

<details>
Given a module `my.module` like:

```python
class Foo:
    def call_db(self) -> int:
        return 42


class Bar:
    foos: list[Foo]


def no_call_db(f: Any) -> Any:
    return f


@no_call_db
def f() -> None:
    bar = Bar()
    for foo in bar.foos:
        print(foo.call_db())
```

I'd like to be able to use the results of typing-checking to perform further static analysis (or maybe even insane runtime stuff/documentation generation). 

In this case, I'd like to check there are no calls to `my.module.Foo.call_db` in any function decorated with `@my.module.no_call_db`.

The API would look something like this:

```python
from ruff.typechecker import ast_extended as ast, types, typecheck, parse_ast_with_types

top_level_types = typecheck(Path("."), use_cache=True)
assert top_level_types == {
    "my.module.Foo": types.Class(methods={"call_db": types.Method(...)}),
    "my.module.no_call_db": types.Function(args=[types.Any], ret=types.Any),
    ...,
}
```

More useful would be coupling the above with the AST:

```python
node = parse_ast_with_types(Path("my/module.py"), use_cache=True)
assert node == ast.FunctionDef(
    name="f",
    decorator_list=[
        ast.Name(
            id="no_call_db",
            type=types.TypeRef("my.module.no_call_db"),
        )
    ],
    body=[
        ast.Assign(
            targets=[ast.Name(id="bar")],
            value=ast.Call(
                func=ast.Name(
                    id="Bar",
                    type=types.Type(types.TypeRef("my.module.Bar")),
                ),
                args=[],
                type=types.TypeRef("my.module.Bar"),
            ),
            type=types.TypeRef("my.module.Bar"),
        ),
        ast.For(
            target=ast.Name(
                id="foo",
                type=types.TypeRef("my.module.Foo"),
            ),
            iter=ast.Attribute(
                value=ast.Name(
                    id="bar",
                    type=types.TypeRef("my.module.Bar"),
                ),
                attr="foos",
                type=types.List(types.TypeRef("my.module.Foo")),
            ),
            body=[
                ast.Expr(
                    value=ast.Call(
                        func=ast.Name(id="print"),
                        args=[
                            ast.Call(
                                func=ast.Attribute(
                                    value=ast.Name(id="foo"),
                                    attr="call_db",
                                    type=types.MethodRef("my.module.Foo", "call_db"),
                                ),
                                args=[],
                                type=types.None_,
                            )
                        ],
                        type=types.TypeRef("builtins.print"),
                    )
                )
            ],
        ),
    ],
)
```

A checker for "no db calls where decorator disallows" is then just:

```python
def is_decorated_no_call_db(node: ast.AST) -> TypeIs[ast.FunctionDef]:
    return (
        isinstance(node, ast.FunctionDef)
        and any(d.type == types.TypeRef("my.module.no_call_db") for d in node.decorator_list)
    )

def is_call_to_call_db(node: ast.AST) -> TypeIs[ast.Call]:
    return (
        isinstance(child, ast.Call)
        and child.func.type == types.MethodRef("my.module.Foo", "call_db")
    )

for node in walk(module):
    if is_decorated(node) and if any(is_call_to_call_db(child) for child in walk(node)):
        yield Error(...)
```

Reliably achieving this kind of thing right now is really difficult as I can't easily eg. hook into `mypy`'s internals.
</details>

---

_Referenced in [triton-lang/triton#5502](../../triton-lang/triton/pulls/5502.md) on 2024-12-26 13:56_

---

_Comment by @johnthagen on 2025-01-29 18:23_

Looks like this was officially announced

- https://x.com/charliermarsh/status/1884651482009477368


> We’re building a new static type checker for Python, from scratch, in Rust.

---

_Comment by @holmanb on 2025-01-30 19:15_

> Looks like this was officially announced
> 
>     * https://x.com/charliermarsh/status/1884651482009477368
> 
> 
> > We’re building a new static type checker for Python, from scratch, in Rust.

As a potential user, I have some initial questions:

Will it use typeshed's stubs or start from scratch?

Are there any publicly facing design docs?

Mypy and pyright, the two most popular open source Python type checkers, have very different semantics. Will this new project match the behavior of an existing type checker or will they add another alternative semantic implementation to the Python type checker ecosystem?

---

_Comment by @charliermarsh on 2025-01-30 19:42_

I definitely appreciate the questions but we're really focused on building the thing right now -- it's not yet ready for real-world usage. We'll share more when we can!

---

_Comment by @josephrubin on 2025-01-30 21:40_

Looks very cool. Would really appreciate hermetic approach to cache files, like Guido's approach
https://github.com/python/mypy/pull/4759
https://github.com/python/mypy/issues/3912

---

_Comment by @mon-jai on 2025-01-31 06:06_

@charliermarsh Would you publish a placeholder package to hold the place in the PYPI repo?

Package name collision is very common in the JavaScript ecosystem. Not sure if it is the same for Python.

---

_Comment by @zanieb on 2025-01-31 20:35_

We probably won't use the `red-knot` name for the final product, though that's still an open question.

---

_Comment by @kszlim on 2025-01-31 20:42_

I'd personally prefer that it remains under ruff, one of the nice things about ruff is that it's monolithic and you don't get tools clobbering each other's outputs. Ensuring that there's only ever one universe of tools that is used is a feature in this case. At most I could see it being gated as an optional feature, but please keep it all under ruff!

---

_Referenced in [astral-sh/ruff#15828](../../astral-sh/ruff/issues/15828.md) on 2025-01-31 23:46_

---

_Comment by @josiahcoad on 2025-02-08 05:01_

I'm here because running mypy on my medium codebase froze my computer and I'm waiting for it to unfreeze. Needless to say, very excited for a fast, scalable type checker! It will make my pre-commit much less painful.

---

_Referenced in [astral-sh/ty#198](../../astral-sh/ty/issues/198.md) on 2025-02-14 16:07_

---

_Referenced in [GOTO-OBS/goto-tile#111](../../GOTO-OBS/goto-tile/pulls/111.md) on 2025-03-06 16:21_

---

_Comment by @pgulb on 2025-04-09 09:41_

This is what Python world needs


---

_Comment by @johnthagen on 2025-05-07 19:04_

Looks like this has been published as `ty`

- https://github.com/astral-sh/ty
- https://pypi.org/project/ty/

---

_Comment by @hauntsaninja on 2025-05-07 19:13_

Just note:

> This project is still in development and is not ready for production use.

Also check out https://www.youtube.com/watch?v=XVwpL_cAvrw !

---

_Referenced in [astral-sh/ty#291](../../astral-sh/ty/issues/291.md) on 2025-05-09 11:43_

---

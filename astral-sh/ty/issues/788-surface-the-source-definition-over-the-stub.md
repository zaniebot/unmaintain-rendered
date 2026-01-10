```yaml
number: 788
title: "Surface the \"source definition\" over the stub definition"
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-07-09T08:52:19Z
updated_at: 2025-08-11T14:54:24Z
url: https://github.com/astral-sh/ty/issues/788
synced_at: 2026-01-10T02:06:24Z
```

# Surface the "source definition" over the stub definition

---

_Issue opened by @MichaReiser on 2025-07-09 08:52_

Untyped libraries use [stub files (`.pyi`)](https://typing.python.org/en/latest/spec/distributing.html#stub-files) to give downstream users a typed API even though the library itself might be untyped (or not fully typed). The stub files are all we need for type checking and ty's module resolver returns the stub files if both a regular python file and a stub file are present. That means, a type's `Definition` always points to the `Definition` in the stub file. This is fine for type checking but having access to the source definition has advantages for the LSP use case:

* A user wants to jump to the source definition when using *go-to definition* (except for go-to declaration?)
* Stub files often lack documentation because the source definition is documented. 


This requires:

* Adding logic to resolve the location of the standard-library source files from a system or virtual environment's `sys.prefix` value
* Extending the module resolver (see `resolve_module`) so that it can be parametrized whether it should return the stub or source file. We have a few options here:
  * Add a `source` field to `Module` and always populate it when the resolved module is a stub (this goes a bit against or lazy approach as the source file is only needed in the editor context for now)
  * parametrize `resolve_module` 
  * A new method to resolve the source module
* Adding a new method to `Definition` to resolve the `source_definition` if it is defined in a stub file. This will require some heuristic for how we map a stub definition to a source definition. 
* Use the source definition when extracting docstrings, in go-to definition, ... 
 

---

_Label `server` added by @MichaReiser on 2025-07-09 08:52_

---

_Assigned to @Gankra by @Gankra on 2025-07-17 23:25_

---

_Comment by @Gankra on 2025-07-21 14:22_

Currently planned approach for a v1 implementation:

---

given a ResolvedDefinition:

find its module + file
if the file isn't a stub: return the input
find the "real" module + file (rerun resolve_module but with .pyi files forbidden)
if the ResolvedDefinition is for an entire file (module): return the "real" file (module)
(up to here is all easy and i have implemented locally)

if the ResolvedDefinition is for a Definition, do a heuristic match...

idea:

a definition has a "path" in a file based on nested definitions (~scopes):

```py
class myclass:  # ./myclass
  def some_func():  # ./myclass/some_func
  def other_func(args: bool):  
               # ^~~~ ./myclass/other_func/args/

def other_func():  # ./other_func 
```

So in the .pyi we figure out the "path" and then we try to find the equivalent "path" in the .py. Presumably end-of-module definitions are sufficient since the relevant definition is exported from the module?

---

It sounds like the main hitch in this design is just when the stub and implementation actually disagree on the module an item comes from, but that this is otherwise a good 80/20 starting point.

---

_Closed by @Gankra on 2025-07-22 12:42_

---

_Reopened by @MichaReiser on 2025-07-22 12:43_

---

_Comment by @MichaReiser on 2025-07-22 12:44_

@Gankra did you intend on closing this. I assume there's some more to do for other types

---

_Comment by @Gankra on 2025-07-24 13:55_

Oh whoops, no I did not intend to.

---

_Comment by @JaRoSchm on 2025-08-07 10:27_

One place where this seems to be not implemented yet (using ty 0.0.1-alpha.17) is the hover functionality of many editors. I tested this for `np.linspace` in nvim (using `K`, see https://neovim.io/doc/user/lsp.html#vim.lsp.buf.hover()). This shows the definitions from the stub file (and no docstring) while for the completion of function arguments the source definition (and the docstring) is shown. I was not able to reproduce this in the playground, where stub files are seemingly not supported (https://play.ty.dev/542eca97-4072-48d9-848a-ef88d9ecda74).

---

_Comment by @AxillV on 2025-08-10 08:55_

> One place where this seems to be not implemented yet (using ty 0.0.1-alpha.17) is the hover functionality of many editors. I tested this for `np.linspace` in nvim (using `K`, see https://neovim.io/doc/user/lsp.html#vim.lsp.buf.hover()). This shows the definitions from the stub file (and no docstring) while for the completion of function arguments the source definition (and the docstring) is shown. I was not able to reproduce this in the playground, where stub files are seemingly not supported (https://play.ty.dev/542eca97-4072-48d9-848a-ef88d9ecda74).

Same applies for VSCode, can easily be tested with `print`

update: seems like the devs know that, regarding stdlib, from [astral-sh/ruff#19471](https://github.com/astral-sh/ruff/pull/19471)

update 2: further looking into the code, it seems like the hover behaviour is incomplete, from
[hover.rs](https://github.com/astral-sh/ruff/blob/8230b79829db0148afeefa634f3dde00b3a52410/crates/ty_ide/src/hover.rs#L26-L27)
> // TODO: Add documentation of the symbol (not the type's definition).
> // TODO: Render the symbol's signature instead of just its type.

This belongs in a separate discussion (which can be found [here](https://github.com/astral-sh/ty/issues/102))
   

---

_Comment by @Gankra on 2025-08-11 14:54_

I think with all the PRs I've landed this is done now -- we do get docstrings from source-defs, and yeah hover just doesn't render them (I am however literally implementing that now).

---

_Closed by @Gankra on 2025-08-11 14:54_

---

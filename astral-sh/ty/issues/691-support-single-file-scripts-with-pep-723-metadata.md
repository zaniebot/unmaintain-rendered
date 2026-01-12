```yaml
number: 691
title: Support single-file scripts with PEP 723 metadata
type: issue
state: open
author: thoughtpolice
labels: []
assignees: []
created_at: 2025-06-22T21:47:05Z
updated_at: 2026-01-06T00:52:07Z
url: https://github.com/astral-sh/ty/issues/691
synced_at: 2026-01-12T15:54:23Z
```

# Support single-file scripts with PEP 723 metadata

---

_@thoughtpolice_

I looked a bit through the bug tracker but didn't see this: I have a few projects that I develop with `uv run --script`, along with PEP 723 inline metadata for my dependencies. I find this is really nice for simple scripts in my monorepo:

```python
#!/usr/bin/env -S uv run --script
# /// script
# requires-python = "==3.12.*"
# dependencies = [
#     "mlx>=0.26.1",
#     "mlx-lm>=0.25.2",
#     "urllib3==1.26.6",
#     "click>=8.0.0",
#     "rich>=13.0.0",
#     "prompt-toolkit>=3.0.0",
# ]
# ///
...
```

However, if I execute `uvx ty check foo.py` on these files, then the following occurs:

```
aseipp@navi bizarro % uvx ty check bizarro.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Cannot resolve imported module `click`
  --> bizarro.py:31:8
   |
30 | # Third-party imports
31 | import click
   |        ^^^^^
... many more errors follow ...
```

I think this can obviously be avoided by using `pyproject.toml`, but it would be wonderful if ty could also work for this case too. I have a bunch of one-shot scripts like this that I occasionally need to import a library with, so it'd be nice to not require more project scaffolding. Q: Perhaps it should also require a `--script` argument to `ty check` to make it more uniform with `uv run`?

> P.S. Many thanks for uv/ruff/ty!

---

_Comment by @sharkdp on 2025-06-23 06:03_

Thank you for your feature request!

The way we intend to solve this is through uv. In fact, there's an open feature request for a [`--with-requirements script.py`](https://github.com/astral-sh/uv/issues/6542) option (and an [open PR](https://github.com/astral-sh/uv/pull/12763)), which would serve this exact use case. So once this is implemented, you could run something like
```
uvx --with-requirements script.py ty check script.py
```

There's probably a follow-up question here whether or not there should be a more convenient command for this that would avoid having to repeat the path to the script twice.

---

_Label `cli` added by @sharkdp on 2025-06-23 06:04_

---

_Comment by @sharkdp on 2025-06-23 06:14_

A one-liner hack that I use is the following:
```bash
uvx ty check script.py --python "$(uv python find --script script.py)"
```

I think it requires that you run the script once via `uv run script.py` in order to create a cached venv.

---

_Comment by @MichaReiser on 2025-06-23 06:20_

> The way we intend to solve this is through uv.

We also plan on exploring a tighter uv integration in the LSP, so that the LSP could automatically setup the venv when opening a script file. 

---

_Label `cli` removed by @MichaReiser on 2025-06-23 06:20_

---

_Label `wish` added by @MichaReiser on 2025-06-23 06:20_

---

_Comment by @bortels on 2025-06-29 02:01_

I am not sure if this qualifies as part of this wish, or as a separate issue.

When you try to run ty against a single file script that does not end in .py (ie. the scripts in my ~/bin directory) - it says:

WARN No python files found under the given path(s)
All checks passed!

If I add a .py extension, it's happy. I can fix this with a symlink of "scriptname.py" to "scriptname", but that makes me sad.

I also note it does this with a shebang line of "#!/bin/env python3" - so it's not the shebang, it's the extension.

---

_Comment by @PetterS on 2025-09-13 16:53_

> A one-liner hack that I use is the following:
> 
> uvx ty check script.py --python "$(uv python find --script script.py)/../.."
> I think it requires that you run the script once via `uv run script.py` in order to create a cached venv.

That seems to work pretty well! But why the `../..`? It works for me without (i.e. ty seems to accept being passed a python binary)

---

_Comment by @sharkdp on 2025-09-15 07:47_

> But why the `../..`? It works for me without (i.e. ty seems to accept being passed a python binary)

You're right. This may have been fixed in https://github.com/astral-sh/ruff/pull/18827, which was merged around the same time that I wrote my comment. Will update the comment to include your suggestion. Hopefully, we can soon use `uvx --with-requirements script.py ty check script.py` instead of this workaround. See https://github.com/astral-sh/ty/issues/989#issuecomment-3265190699.

---

_Comment by @Janno on 2025-11-05 12:27_

> The way we intend to solve this is through uv. In fact, there's an open feature request for a [`--with-requirements script.py`](https://github.com/astral-sh/uv/issues/6542) option (and an [open PR](https://github.com/astral-sh/uv/pull/12763)), which would serve this exact use case. So once this is implemented, you could run something like [..]

AFAICT  the `uv` feature is now implemented but your code example does not work for me. `uvx` happily installs the dependencies but `ty` seems to be oblivious about that. Is there extra work needed in `ty` to make this work?

---

_Comment by @MichaReiser on 2025-11-05 12:34_

@Janno this is related to https://github.com/astral-sh/ty/issues/989

---

_Label `wish` removed by @carljm on 2025-11-13 00:30_

---

_Added to milestone `Stable` by @carljm on 2025-11-13 00:38_

---

_Comment by @sharkdp on 2025-11-13 09:00_

> The way we intend to solve this is through uv. In fact, there's an open feature request for a [`--with-requirements script.py`](https://github.com/astral-sh/uv/issues/6542) option (and an [open PR](https://github.com/astral-sh/uv/pull/12763)), which would serve this exact use case. So once this is implemented, you could run something like
> 
> ```
> uvx --with-requirements script.py ty check script.py
> ```

With recent changes in ty and uv, this is working now.

One thing I've noticed is that we do not seem to make use of the `requires-python` constraint from the inline metadata in the script. For example, when running the following command from inside the ruff repository (which has a `pyproject.toml` with a `requires-python` of 3.7), I get:

`test.py`

```py
# /// script
# requires-python = ">=3.14"
# dependencies = [
#     "httpx",
# ]
# ///
from typing_extensions import reveal_type
import httpx
import sys

r = httpx.get("https://www.example.org/")
reveal_type(r)

reveal_type(sys.version_info[:2])
```

```bash
▶ uvx --with-requirements test.py ty check test.py
info[revealed-type]: Revealed type
  --> test.py:12:13
   |
11 | r = httpx.get("https://www.example.org/")
12 | reveal_type(r)
   |             ^ `Response`
13 |
14 | reveal_type(sys.version_info[:2])
   |

info[revealed-type]: Revealed type
  --> test.py:14:13
   |
12 | reveal_type(r)
13 |
14 | reveal_type(sys.version_info[:2])
   |             ^^^^^^^^^^^^^^^^^^^^ `tuple[Literal[3], Literal[7]]`
   |

Found 2 diagnostics
```

---

_Comment by @MichaReiser on 2025-11-13 09:11_

> One thing I've noticed is that we do not seem to make use of the requires-python constraint from the inline metadata in the script. For example, when running the following command from inside the ruff repository (which has a pyproject.toml with a requires-python of 3.7), I get:

That makes sense to me. ty only looks at `ty.toml` and `pyproject.toml` files but not any inline script metadata. I'm not yet sure if we want a `ty check --script test.py` or if we should always try to read the metadata or if ty should try to read the metadata if a single file was specified. 

---

_Assigned to @Gankra by @Gankra on 2025-11-13 16:33_

---

_Comment by @Gankra on 2025-11-13 16:34_

I think we should always read the metadata, we need to do so in the LSP context.

---

_Comment by @MichaReiser on 2025-11-13 16:37_

> I think we should always read the metadata, we need to do so in the LSP context.

I don't think we can? Like, if you're checking an entire project, respecting the script metadata would require a separate db instance.

---

_Comment by @carljm on 2025-11-13 16:47_

I think the ideal experience in an LSP is that when you open a PEP 723 script, it gets checked independently from everything else, using its own inline metadata. This should be no different from opening files from multiple projects at once in the editor?

Not sure what the CLI experience should be if you check a project that has PEP 723 scripts inside it -- we might need to just notify you that they have to be checked separately? Or maybe we really do need to at some point build "multiple db instances in a single CLI run" feature, even if its just via subprocesses or something...

---

_Comment by @Gankra on 2025-11-13 16:57_

> I don't think we can? Like, if you're checking an entire project, respecting the script metadata would require a separate db instance.

Yes, I am roughly picturing a workflow like:

* detect this is a PEP 723 script by finding the metadata at all (easy)
* detect that they have uv installed
* ask uv to ensure the script has a venv and to give us a path to it
* two paths:
  * parse the metadata ourselves for the info we need (would let us analyze scripts partially, or fully for non-dep-having ones)
  * have uv report the metadata to us (we need it to setup the script's private venv anyway)

---

_Comment by @Gankra on 2025-11-13 16:58_

This is to say, the script is already very much so "in a separate workspace", and should be treated as such.

---

_Comment by @MichaReiser on 2025-11-13 17:15_

Agree, that we should pick up the script metadata in the LSP

> Not sure what the CLI experience should be if you check a project that has PEP 723 scripts inside it -- we might need to just notify you that they have to be checked separately? Or maybe we really do need to at some point build "multiple db instances in a single CLI run" feature, even if its just via subprocesses or something...


What we discussed originally is that they're checked with the same configuration and that users would exclude it if they shouldn't be part of the ty project. 

Either way, I think a first step is to make scripts work when you pass a single file or in the LSP. 

---

_Comment by @Jay-Madden on 2025-12-01 21:21_

Very much looking forward to this for neovim integration

For now i have just disabled the lspconfig version of ty and instead am using this to support single file scripts. 

```lua
    vim.api.nvim_create_autocmd("FileType", {
      pattern = "python",
      callback = function(_)
        local first_line = vim.api.nvim_buf_get_lines(0, 0, 1, false)[1] or ""
        local has_inline_metadata = first_line:match("^# /// script")

        local cmd, name, root_dir
        if has_inline_metadata then
          local filepath = vim.fn.expand("%:p")
          local filename = vim.fn.fnamemodify(filepath, ":t")

          -- Set a unique name for the server instance based on the filename
          -- so we get a new client for new scripts
          name = "ty-" .. filename

          local relpath = vim.fn.fnamemodify(filepath, ":.")

          cmd = { "uvx", "--with-requirements", relpath, "ty", "server" }
          root_dir = vim.fn.fnamemodify(filepath, ":h")
        else
          name = "ty"
          cmd = { "ty", "server" }
          root_dir = vim.fs.root(0, { 'ty.toml', 'pyproject.toml', 'setup.py', 'setup.cfg', 'requirements.txt', '.git' })
        end

        vim.lsp.start({
          name = name,
          cmd = cmd,
          root_dir = root_dir,
        })
      end,
    })

```

Seems to be working so far in my testing, but its far from battle tested so use at your own risk 😁 

Edit: I have actually ended up using this a lot more than I expected. So I spun it out as a very small plugin for convenience if anyone wants. https://github.com/Jay-Madden/tylsp-pep723.nvim 

---

_Comment by @a-alak on 2025-12-29 13:41_

> Very much looking forward to this for neovim integration
> 
> For now i have just disabled the lspconfig version of ty and instead am using this to support single file scripts.
> 
>     vim.api.nvim_create_autocmd("FileType", {
>       pattern = "python",
>       callback = function(_)
>         local first_line = vim.api.nvim_buf_get_lines(0, 0, 1, false)[1] or ""
>         local has_inline_metadata = first_line:match("^# /// script")
> 
>         local cmd, name, root_dir
>         if has_inline_metadata then
>           local filepath = vim.fn.expand("%:p")
>           local filename = vim.fn.fnamemodify(filepath, ":t")
> 
>           -- Set a unique name for the server instance based on the filename
>           -- so we get a new client for new scripts
>           name = "ty-" .. filename
> 
>           local relpath = vim.fn.fnamemodify(filepath, ":.")
> 
>           cmd = { "uvx", "--with-requirements", relpath, "ty", "server" }
>           root_dir = vim.fn.fnamemodify(filepath, ":h")
>         else
>           name = "ty"
>           cmd = { "uvx", "ty", "server" }
>           root_dir = vim.fs.root(0, { 'ty.toml', 'pyproject.toml', 'setup.py', 'setup.cfg', 'requirements.txt', '.git' })
>         end
> 
>         vim.lsp.start({
>           name = name,
>           cmd = cmd,
>           root_dir = root_dir,
>         })
>       end,
>     })
> Seems to be working so far in my testing, but its far from battle tested so use at your own risk 😁

Do you still have vim.lsp.enable('ty') somewhere else with this or do you completely take over managing the lsp server with this?

---

_Comment by @max397574 on 2025-12-29 16:38_

no need for the lsp.enable with this

---

_Comment by @DinoChiesa on 2025-12-30 06:07_

re: @Gankra [comment](https://github.com/astral-sh/ty/issues/691#issuecomment-3528778986)

The "workflow" that you describe seems mostly reasonable, but most of that is not done within ty LSP, right?  It's done within the client. 

| task | responsible actor |
| --- | --- | 
| detect this is a PEP 723 script by finding the metadata at all (easy) | editor / client | 
| detect that they have uv installed | editor / per-user configuration (maybe they use uv, maybe something else) |
| ask uv to ensure the script has a venv and to give us a path to it | editor / client | 
| parse the metadata ourselves for the info we need (would let us analyze scripts partially, or fully for non-dep-having ones) | only uv or similar should parse PEP723 metadata |
| have uv report the metadata to _us_ (we need it to setup the script's private venv anyway) | who is _us_ and _we_?   The LSP client should inform the LSP Server about where to find installed dependencies, aka the venv. |


Parsing PEP723 metadata should not be the job of any LSP; or at the very least I think _doing anything_ with the parsed metadata is not part of the responsibilities of an LSP.  From what I understand, the metadata is intended to be use only by tools such as uv that set up a magic venv somewhere (definitely not in .venv) _just for that script_, and install the named dependencies into the venv.  Unless ty LSP is going to do that, it should not act on the PEP723 metadata. It should not care about that metadata.

ty LSP is a _consumer_ or _user_ of that prepared per-script venv, which is an indirect product of the metadata.  But ty LSP is not a system that needs to parse PEP723 metadata, beyond checking for well-formedness maybe.

_detecting that uv or some other tool is installed_ is also not the job of an LSP.  _having uv (or other) report things to the LSP server_ is also not a thing the LSP Server should actively DO.   That is client-configuration stuff.

It seems to me....
ty language server needs to allow an LSP client like vim, zed, emacs a way to provide workspace configuration information like: "for this workspace, use **this** python interpreter, and  **this specific** venv directory".   True, determining that information is easy with uv; for example the python is returned by `uv python find --script myfile.py`.  And there are similar ways to find the venv. But that determination should be done by the LSP client (editor) according to configuration done by the user; it should not be done by the LSP Server.  There are other non-uv tools that may set up per-script venv's (eg poetry), and there would be different commands for those other tools obviously. But the client-to-server workspace configuration should be the same.

<img width="814" height="394" alt="Image" src="https://github.com/user-attachments/assets/3c4c4316-d074-4a5d-a8b8-83fceb2e8639" />

In theory an LSP client would just run the necessary command, and pass the required information either in `initializationOptions` or `workspace/configuration` via LSP protocol to ty LSP.  Then ty LSP would use that venv  and that python for resolving symbols and etc. 

But at this moment [ty LSP documentation says](https://docs.astral.sh/ty/features/language-server/#feature-reference) that ty LSP supports neither of those LSP operations/features. 

Seems like a gap. PEP723 and per-script dependency lists means it is no longer viable to assume a single venv (always named .venv !) per directory.  So every tool that depends on a venv, and this includes ty LSP, needs to have a way for a caller to specify the location of the specific venv for any particular python file.   And the LSP protocol has a nice facility to do that.

-----
Another problem is that the interfaces aren't standardized, AFAIK.  emacs can tell basedpyright where to find the python (and implicitly, the venv), but ... does that same LSP message work with pylsp? with ty?  Do those servers use the same LSP messages and same message content?  vim can ask poetry where the venv is, but... it uses a different command with poetry than it if were using pip directly, or uv.  So we end up having an NxM configuration problem which will inevitably make it confusing for users.

---

_Renamed from "Request: support single-file scripts with PEP 723 metadata" to "Support single-file scripts with PEP 723 metadata" by @MichaReiser on 2025-12-31 16:47_

---

_Comment by @MichaReiser on 2025-12-31 16:49_

> detect this is a PEP 723 script by finding the metadata at all (easy)

Are there any clients that do this today? 

---

_Comment by @DinoChiesa on 2026-01-02 18:36_

@MichaReiser - Not sure if you count emacs as a worthwhile client, but ... yes.  In emacs, everything is a matter of extensions via lisp.  So ...
```
(defun epep723/has-pep723-p (&optional _file)
  "Return non-nil if current buffer contains PEP 723 script metadata."
  (let ((case-fold-search nil))
    (save-match-data
      (save-restriction
        (widen)
        (save-excursion
          (goto-char (point-min))
          ;; Search first 2048 chars for the script tag
          (re-search-forward "^# /// script" 2048 t))))))
```

The thinking is that the module docstring can precede the `# /// script` markup, so it needs to search potentially past that.

This test is used before starting up an LSP server, to let the LSP serverknow, either via startup arguments, or via the LSP messages themselves, like `initializationOptions` (probably not), or `workspace/configuration` (better):
- where is the python interpreter and venv
- this is a single-file project; don't load or analyze other files 

This test has been useful with basedpyright - it accepts `python.pythonPath` in the `workspace/configuration` options.  I have not figured out how to launch ty with the appropriate information.


---

_Comment by @MichaReiser on 2026-01-02 20:08_

emacs is a notable client but I don't see why we should wait for all other major clients to implement script support, just so that we can provide great experience in ty (besides that I think there are other advantages of implementing script support natively)

---

_Comment by @Jay-Madden on 2026-01-04 15:37_

I think fundamentally the problem statement of "how is the venv created" is seperate from ty's scope of responsibility. 

The various clients plugins can trivially handle creating the venv itself in either an ephemeral or persistent capacity. 

All ty needs is a way to configure the path it looks at. Let the editor clients handle the rest.

---

_Comment by @Gankra on 2026-01-04 18:45_

> But ty LSP is not a system that needs to parse PEP723 metadata, beyond checking for well-formedness maybe.

I disagree with this part. `uv` and `ty` have fundamentally different goals when they read the metadata. The contents of a script's metadata (and the equivalent metadata in a pyproject.toml) are input constraints that uv wants to solve to a set of definite answers that it checks into a lockfile and renders into a venv.

However ty already has analysis to the effect of "ok, so, you claim you work with python 3.12, but, you're using APIs that were introduced in 3.13 (and don't have a `version >= 3.13` check surrounding that code)". This is a quite substantial issue because it's quite common for a project to have conservative python requirements for compatibility, but the PEP scripts in the project to have bleeding edge requirements because they don't need compatibility and `uv` will silently handle installing and using the more modern python.

It's much more pie-in-the-sky for ty to do similar analysis for every random library (so I don't really care about it as much), but in principle we can do the same thing with random pypi packages as we do for the python stdlib, so we also have a reason to want to get the raw dependency constraints for those too.

Less pie-in-the-sky, it's already an established precedent in the typing ecosystem for typecheckers to include special knowledge and handling of important libraries in the python ecosystem, and so it wouldn't be unreasonable for us to go "hey it looks like you're trying to use pydantic, but it's not in your script's dependencies, do you want to add it?" ([actually this may not even require special-casing](https://peps.python.org/pep-0794/)). 


---

_Comment by @DinoChiesa on 2026-01-04 20:21_

Regarding this: 

> "ok, so, you claim you work with python 3.12, but, you're using APIs that were introduced in 3.13 (and don't have a version >= 3.13 check surrounding that code)". 

Is it not sufficient for ty to rely on the constructed venv to test "does this script work with this version of python?"   In the pep723 markup, a script can specify the version of python to use (`requires-python = ">=3.12"`).   If uv or poetry or some other pep723-aware venv-manager creates a venv complete with a version of python, .... using the version specified via `requires-python` in the markup..... then.... ty should just use that venv to check for compatibility, right? 

I don't know specifically what a "claim to work with python 3.12" would look like, other than a specification of the python version in the pep723 markup (or if we are expanding beyond single-file projects, in the pyproject.toml).  If that is the extent of it, why wouldn't ty just use the venv that has been created _from that specification_? 

re:
>  "hey it looks like you're trying to use pydantic, but it's not in your script's dependencies, do you want to add it?"

Yes that makes sense to me. I hadn't thought of that.  There are two places where this is relevant: (1) the _import_  statement in the script, and (2) the specification of the dependency itself.  If ty wants to be able to offer code actions (via the `textDocument/codeAction` message in LSP), for item (2), it would need to be able to suggest changes in either PEP723 markup or the pyproject.toml file to add a dependency.   So it would need to be able to parse the pep723 markup I guess. 


---

_Comment by @quencs on 2026-01-04 20:32_

This feels a bit out of scope for ty to reason about explicitly.

If a script specifies requires-python via PEP 723 (or via pyproject.toml), and a PEP 723–aware tool creates a venv using that specification, then Python version compatibility should already be encoded in the environment itself. From that point on, ty can simply type-check against the interpreter it is given.

In that model, using a 3.13-only API in a 3.12 environment is just a normal type error, without ty needing to interpret or enforce “version claims” on its own.

Dependency diagnostics feel different: missing imports vs declared dependencies do seem like something ty could reasonably help with, which would require understanding (and possibly editing) PEP 723 metadata. But version selection itself seems better left to the environment manager.

---

_Comment by @DinoChiesa on 2026-01-04 20:38_

I agree with both of those points ^^ .

---

_Comment by @quencs on 2026-01-04 20:40_

I don’t think this is something ty should be responsible for at all.

If a script declares requires-python (via PEP 723 or pyproject.toml), then selecting and provisioning the correct Python version is the job of the environment manager. Once a venv is constructed, ty should simply type-check against the interpreter it is given.

Introducing additional logic in ty to interpret “claims to work with Python X” duplicates responsibility that already belongs to the toolchain, and risks divergence between what the environment enforces and what the type checker assumes.

In contrast, dependency completeness (e.g. importing a library that is not declared) is plausibly in scope, since it’s not enforced by the interpreter itself and benefits from static analysis. But Python version selection and compatibility should remain strictly out of scope for ty.

In short, ty should trust the interpreter it is invoked with. Anything beyond that is a layering violation.


---

_Comment by @Jay-Madden on 2026-01-04 21:59_

@quencs the problem is currently ty does not support being given a specific venv to use.

Once that is supported the various clients can support 723 via plugins. 

---

_Comment by @a-alak on 2026-01-04 22:17_

> the problem is currently ty does not support being given a specific venv to use.

What about the environment.python configuration to the right interpreter (e.g. output of uv python find). Will that not work?

---

_Comment by @carljm on 2026-01-04 22:32_

FWIW, ty (intentionally) does not ever "just check according to the Python version of the given environment." If your `pyproject.toml` declares that your project is supposed to work with Python >=3.12, then ty will (by default, if not otherwise configured) check against 3.12, even if the env is e.g. 3.14. We currently don't even consider the active env as signal for the Python version to check against. (One reason for this is that the only way to reliably detect the Python version of an env is to actually execute the Python binary in the env and check its self-reported version, and this is something we would strongly prefer not to do, for speed reasons.)

I don't think there's a clear right answer here. For e.g. some CI scenarios (which is not an LSP use case), it might make more sense to just use the Python version from the env, since CI typically runs across a matrix of Python versions, and each check is done in its own env. But for local development of a library that is intended to support 3.12+, there won't likely be a matrix of envs -- I may well be developing on 3.14 locally, but still want ty to alert me if I use something that doesn't exist on 3.12.

> ty does not support being given a specific venv to use.

It does, via `environment.python` config. But the point here may be that it isn't currently possible to configure this directly via LSP, without involving a ty configuration file? I'm not totally clear on the status here, https://github.com/astral-sh/ty/issues/2010#issuecomment-3665555239 suggests there is a way to do this? More discussion in https://github.com/astral-sh/ty/issues/2032

---

_Comment by @AlexWaygood on 2026-01-04 22:42_

> We currently don't even consider the active env as signal for the Python version to check against.

@carljm that's not true -- if we can determine the Python version from a virtual environment's pyvenv.cfg file or the layout of a Python environment on disk, then we do consider the version of your active environment as a fallback in some situations. (But we only do this if you don't have the Python version set via the command line, you don't have `tool.ty.environment.python-version` set in a configuration file, and you don't have `project.requires-python` set in a configuration file.) You can see this in our code [here](https://github.com/astral-sh/ruff/blob/92a2f2c99226706feb4bd1b35d4fea205ae2113c/crates/ty_project/src/metadata/options.rs#L223-L231), and it's documented [here](https://docs.astral.sh/ty/reference/configuration/#python-version) where it's listed as the second fallback if no explicit configuration is given in a pyproject.toml or ty.toml file.

---

_Comment by @quencs on 2026-01-05 00:03_

Thanks for the clarification — that’s helpful. You’re right that ty does consider the active environment’s Python version as a fallback when it can be inferred from pyvenv.cfg or the on-disk layout, and when no explicit configuration is provided.

I think the remaining point of tension is that this behavior is both conditional and implicit. From a user perspective, it’s not always obvious which signal ends up being used in a given run, especially once CI, lockfile generation, or tool-driven environments are involved. In those cases, the effective Python version used for checking can diverge from the one users intuitively expect.

That’s why this came up in the first place: not because the fallback doesn’t exist, but because relying on implicit inference makes it harder to reason about why a particular check or error is happening, and harder to ensure that validation is aligned with the environment users think they’re targeting.


---

_Comment by @Gankra on 2026-01-05 12:07_

> I think the remaining point of tension is that this behavior is both conditional and implicit. From a user perspective, it’s not always obvious which signal ends up being used in a given run

We're actually quite explicit about it in diagnostics (unfortunately these extra-rich details only appear on the CLI right now, but improving that is orthogonal to this discussion).

<img width="679" height="292" alt="Image" src="https://github.com/user-attachments/assets/48e349bb-aa2d-46e9-89c4-69c748b7010c" />

---

_Comment by @DinoChiesa on 2026-01-06 00:41_

>  if we can determine the Python version from a virtual environment's pyvenv.cfg file or the layout of a Python environment on disk, then we do consider the version of your active environment as a fallback in some situations.

This seems like an example of the layering violation described by [quencs](https://github.com/astral-sh/ty/issues/691#issuecomment-3708408548)

_There should be_ a venv, and it _should be_ consistent with the pyenv.cfg, but it is not the job of ty (I think) to check that consistency.  Instead ty should just use the python it is given.  The ask here I think, is for ty (LSP or command line)  to accept a pythonpath, as an invocation option or LSP message, not as a configuration file entry, which points to the python provisioned in the venv.  Maybe that means this issue is a duplicate of #2032.

re: [carljm's comment yesterday](https://github.com/astral-sh/ty/issues/691#issuecomment-3708486141)
> I may well be developing on 3.14 locally, but still want ty to alert me if I use something that doesn't exist on 3.12.

This seems like a broken development process that ty would be wise to discourage. If a dev wants to build something for deployment on 3.12, the Dev should use a venv with 3.12, and ty or any checker should just line up on that venv.  It does not make sense for ty to try to figure out the set of what works and what doesn't across the matrix of possibilities.  The python and features available should be determined by the venv. Simple, clear, easy. What is the downside with this?  It's not as if it's difficult to install 3.12 and 3.14 in different venvs.



---

_Comment by @carljm on 2026-01-06 00:51_

> _There should be_ a venv, and it _should be_ consistent with the pyenv.cfg, but it is not the job of ty (I think) to check that consistency.

ty is not checking consistency, it is just trusting the `pyvenv.cfg` as a source of truth, given a lack of other good options.

You are assuming that there's some easy and reliable way to detect the actual Python version, given a Python environment, but there really isn't one. The most reliable option is to execute the Python binary and ask it to tell you its version, but this is a) very slow relative to ty's check speed, and b) also not really reliable, as it opts into a bunch of assumptions about ty's ability to execute a random binary.

@AlexWaygood was simply listing the two unreliable methods we use: check `pyvenv.cfg`, and check the `lib/pythonX.X` directory existence. Unfortunately even the combination of these two methods does not cover nearly all possible Python environments. The first method applies only to virtual environments created by the stdlib `venv` module, and the second method applies only to POSIX layouts (Windows Python environments, for example, don't include the Python version in their lib directory name.)

> If a dev wants to build something for deployment on 3.12, the Dev should use a venv with 3.12, and ty or any checker should just line up on that venv.

That's a reasonable point of view. I think it would probably be best to open a separate issue to discuss it.

One major challenge in adopting this point of view is the difficulty of reliably obtaining a Python version from a Python environment, as discussed above.

---

---
number: 13608
title: "FR: Easier way to suppress errors/formatting for custom code block before imports"
type: issue
state: open
author: dbarnett
labels:
  - rule
  - suppression
  - needs-design
assignees: []
created_at: 2024-10-02T19:26:37Z
updated_at: 2024-10-07T14:30:58Z
url: https://github.com/astral-sh/ruff/issues/13608
synced_at: 2026-01-07T13:12:15-06:00
---

# FR: Easier way to suppress errors/formatting for custom code block before imports

---

_Issue opened by @dbarnett on 2024-10-02 19:26_

Could there be a way to allowlist a code block at the top of a python file above imports, to avoid linter errors module-import-not-at-top-of-file (E402) and unsorted-imports (I001) as well as formatter changes?

After inserting a code block [at the top of a python file, before imports](https://github.com/insanum/gcalcli/blob/e6d452129b2ab2c5ef639e7403d21ddcac5243cb/gcalcli/cli.py#L26-L31), I had to wrestle with the formatter and linter errors for E402 plus several others. It gets especially ugly because I'm not suppressing errors just for the code block I'm adding but for every import block after it. I noticed in the [docs for E402](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/#why-is-this-bad) that...

> This rule makes an exception for both sys.path modifications (allowing for sys.path.insert, sys.path.append, etc.) and os.environ modifications between imports

and wondered if I might somehow hook into the same mechanism for a code block like `import foo; foo.register_hooks()`.

---

Additional info:
* List of keywords you searched for before creating this issue. -- "ignore E402"
* The command you invoked. -- `ruff format --isolated gcalcli/cli.py; ruff check --isolated  --select I --fix gcalcli/cli.py`
* The current Ruff version. -- ruff 0.6.2

---

_Comment by @dbarnett on 2024-10-02 19:32_

And to lift some context from the linked code snippet:
```python
# Import trusted certificate store to enable SSL, e.g., behind firewalls.
# Must be called as early as possible to avoid bugs.
# fmt: off
import truststore; truststore.inject_into_ssl()  # noqa: I001,E702
# fmt: on
# ruff: noqa: E402

# … other imports …
```

I had to:

1. disable `unsorted-imports (I001)` for the weird import
2. disable `module-import-not-at-top-of-file (E402)` for the entire rest of the file (because that code block made it so none of the other imports were at the "top" anymore)
3. disable formatter + `multiple-statements-on-one-line-semicolon (E702)` to keep the truststore code block organized together

For 3, I didn't strictly need it to be concatenated onto one line, but I didn't want it spread across 3 with a blank line between and the formatter / import sorter refused to leave other variants how I wanted them formatted.

I'm okay with some ugly error suppression comments but hoping to reduce it down to slightly less than that mess.

---

_Comment by @AlexWaygood on 2024-10-03 10:25_

On the face of it, making this configurable seems like a reasonable request to me, since there's obviously an arbitrary number of things you could do in Python that might need to be done before one or more imports take place. And I can definitely see that the mess of pragma comments in your snippet there is pretty suboptimal 😆

Here's where we currently apply this special-casing. The E402 rule is only applied to an import if the AST visitor detects we're at a point in the module after the "import boundary", and the "import boundary" is crossed after we reach any statement that's not an import, and also isn't one of the various special cases that you've already noted:

https://github.com/astral-sh/ruff/blob/7e3894f5b3573d77b0000bbddf0293fbbb5dc986/crates/ruff_linter/src/checkers/ast/mod.rs#L496-L505

The actual functions that implement the special casing are all currently quite particular to each specific thing we're special-casing, however. We'd need to think up a design for a more generalised mechanism if we wanted to make it pluggable via a configuration option. Here's where these functions currently live: https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_semantic/src/analyze/imports.rs.

Any ideas for what specifically the config option would look like? :-)

---

_Label `rule` added by @AlexWaygood on 2024-10-03 10:25_

---

_Label `configuration` added by @AlexWaygood on 2024-10-03 10:25_

---

_Label `needs-design` added by @AlexWaygood on 2024-10-03 10:25_

---

_Comment by @dbarnett on 2024-10-03 15:32_

I'd imagined something to declare the following code block is intentionally before imports, something like 

```python
import truststore  # noqa: ignore-pre-import
truststore.inject_into_ssl()

# … other imports …
```

Note that for it to remain one single "block", the formatters need to know not to try to insert a blank line. That _COULD_ be manually done with another `# fmt: skip` directive, but I had trouble getting that to work correctly, and I think it's reasonable to assume if a code block is intentionally plopped into the top of the file, the formatter won't be a good judge of where to put extra newlines.

An alternative approach would be to try to have an allowlist of pre-import statements in the project config, but that's clunkier, less self-contained, and more limited than just tagging each block as intended pre-import in place.

---

_Comment by @AlexWaygood on 2024-10-03 15:37_

Ahh, I see. Hmm, in that case I'm not sure. I think we generally like to keep the different kinds of suppression comments we support to a bare minimum; I think we'd need quite a strong rationale to add a new kind of suppression comment. This has the advantage of keeping things simple for users to understand, and also keeps our implementation fast and simple.

---

_Comment by @dbarnett on 2024-10-03 16:27_

Was there a less complex alternative you had in mind? 

Overrides aside, it would be nice if by default (a) this only emitted one linter nag for the couple statements above the normal imports and (b) the formatters were less invasive about adding blank lines.

---

_Comment by @AlexWaygood on 2024-10-03 18:13_

> Was there a less complex alternative you had in mind?

When you said "allowlist", I assumed you meant some kind of configuration setting that you might put in `pyproject.toml` or `ruff.toml` -- which I'd be very open too, if we can think of a good design for how the config setting would work!

---

_Comment by @dbarnett on 2024-10-03 18:44_

Could be a list of statements (in my case `import truststore` and `truststore.inject_into_ssl()`).

But either way, would it make sense to try to reduce the noise by default first before looking at new config mechanisms? If ruff were smarter about the "import boundary" then it'd at least be complaining about only the weird lines of code and not all the innocent bystander lines of imports that follow it.

---

_Comment by @MichaReiser on 2024-10-07 08:00_

I agree that this is rather annoying but I'm not sure it's worth special casing this one-off use case. 

We do have plans to rework our suppression comments holistically. What I have in mind is that you could suppress this with a single comment:

```python
# Import trusted certificate store to enable SSL, e.g., behind firewalls.
# Must be called as early as possible to avoid bugs.
# ruff-ignore[format, lint/I001, lint/E702] -- Import trusted certificate store to enable SSL, e.g., behind firewalls. Must be called as early as possible to avoid bugs.
import truststore; truststore.inject_into_ssl()

# … other imports …
```

---

_Label `configuration` removed by @MichaReiser on 2024-10-07 08:00_

---

_Label `suppression` added by @MichaReiser on 2024-10-07 08:00_

---

_Comment by @dbarnett on 2024-10-07 13:44_

What about the E402 module-import-not-at-top-of-file for the entire rest of the file? That's the especially annoying one. Are you suggesting that issue would get resolved in the mix, or just not considering that part significant?

---

_Comment by @MichaReiser on 2024-10-07 13:59_

Right, that would require a second suppression comment. But I think that's fine, considering that this is a one-off exception/

---

_Comment by @dbarnett on 2024-10-07 14:26_

But my point is that ruff is deciding the cutoff point of what is "before imports" and "at top of file" and creating a lot of fallout error spam as a result. Suppressing a few _other_ errors on one line doesn't help much. I'd love to see it either use better heuristics (so it's more likely to only complain about the lines that are "out of order" in some meaningful sense) or at least let me provide a hint for where the main "imports" actually begin in this file.

---

_Comment by @MichaReiser on 2024-10-07 14:30_

I do understand the use case. I'm just not convinced that it's worth the complexity to special case. 

You could also consider creating a new module `inject_truststore`

```python
import truststore

truststore.inject_into_ssl()
```

Then use it like this

```python
import inject_truststore # isort: skip

import os

import first_party
```

---

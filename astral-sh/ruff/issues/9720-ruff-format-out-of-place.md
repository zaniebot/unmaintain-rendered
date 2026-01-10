---
number: 9720
title: ruff format out of place?
type: issue
state: closed
author: suo
labels:
  - formatter
  - wish
assignees: []
created_at: 2024-01-30T20:48:31Z
updated_at: 2024-02-23T21:34:06Z
url: https://github.com/astral-sh/ruff/issues/9720
synced_at: 2026-01-10T01:22:49Z
---

# ruff format out of place?

---

_Issue opened by @suo on 2024-01-30 20:48_

The default behavior of `ruff format` is to do an in-place rewrite of the provided files. If you pass source code in via stdin, ruff will output the formatted source to stdout.

It would be great to have an option where we can pass multiple files into `ruff format`, and get a structured response back with formatted sources for each of the files, somewhat analogous to `ruff --output-format=json <files>`.

This would be helpful for a project like ours (PyTorch) which calls out to ruff programmatically from various tools.

Happy to contribute this if folks are okay with it.

---

_Comment by @zanieb on 2024-01-30 20:52_

Thanks for the issue and offer to contribute :)

We're thinking about rewriting our LSP in Rust so it can be in `ruff` directly. I wonder if that would help with your use case? I think it'd provide an API like what you're looking for.

I'm not sure if different output formats make sense in the `ruff format` CLI. Curious what @MichaReiser thinks.

---

_Label `formatter` added by @zanieb on 2024-01-30 20:52_

---

_Label `wish` added by @MichaReiser on 2024-01-31 07:35_

---

_Comment by @MichaReiser on 2024-01-31 07:42_

It's funny. I've just been talking with @charliermarsh about this because it would be nice if range formatting could return only the portion of the code that needs replacing together with the range that must be replaced rather than the entire file. 

We concluded that it would be nice to have but will no longer be necessary once we've rewritten our LSP in Rust because it can then call the internal API directly. I also think that promoting the LSP as a programmatic Ruff API (until we have a Python API) has benefits:

* It keeps the CLI API smaller and removes the need to maintain two programmatic APIs
* The LSP is well-tested because it is used in IDEs
* The LSP is a server, Ruff can cache state between calls, potentially making it faster than spawning the CLI for each file
* There's an official specification (we don't need to come up with good APIs)

The main downside is that using it is more involved. For example, to format a file:

* Start the LSP process
* Send the initialize request
* Send the open file request
* Send the format request
* Send the close file request (optional, but you may want Ruff to release some memory every now and then)
* Send the shutdown request
* Wait for the process to end





---

_Comment by @MichaReiser on 2024-02-23 21:34_

I'll close this in favor of https://github.com/astral-sh/ruff/issues/659, which asks for a Ruff API (not just formatting). 

---

_Closed by @MichaReiser on 2024-02-23 21:34_

---

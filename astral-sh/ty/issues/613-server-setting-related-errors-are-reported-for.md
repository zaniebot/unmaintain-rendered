```yaml
number: 613
title: Server setting related errors are reported for the Python file
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-06-09T07:09:04Z
updated_at: 2025-07-23T08:49:39Z
url: https://github.com/astral-sh/ty/issues/613
synced_at: 2026-01-10T02:06:24Z
```

# Server setting related errors are reported for the Python file

---

_Issue opened by @MichaReiser on 2025-06-09 07:09_

I've a `main.py` with an unresolved reference error and an error in my pyproject.toml

The server groups the pyproject.error under the `main.py`


`pyproject.toml`:

```toml
[project]
name = "micha-test"

[tool.ty.src]
exclude = ["***"]

```

`main.py`:

```py
def foo(a: str | None, b):
	if a is not None:
	   print(a)

unresolved

```

<img width="942" alt="Image" src="https://github.com/user-attachments/assets/59b2e9c5-eeea-4fdb-a772-1ac2b66924cb" />

---

_Label `bug` added by @MichaReiser on 2025-06-09 07:09_

---

_Label `server` added by @MichaReiser on 2025-06-09 07:09_

---

_Comment by @dhruvmanila on 2025-06-09 08:38_

Is this on `main`? Due to https://github.com/astral-sh/ty/issues/76, I'm unable to get this as an error.

---

_Comment by @MichaReiser on 2025-06-09 14:36_

Yes and no. It's on my include/exclude branch but I think it's a pre existing problem. You need to start with a valid configuration or the server crashes. Then make changes which the server picks up

---

_Comment by @MichaReiser on 2025-06-09 15:56_

The reason for this behavior is the code here

https://github.com/astral-sh/ruff/blob/b44062b9ae08dcd8941ac76dbce2ab2c61840691/crates/ty_project/src/lib.rs#L251-L256

The benefit it has is that setting related diagnostics show up even if only calling `check_file` and never `check`. The downside is that the file is incorrect. 

We should probably remove the setting diagnostics from there and find another way to expose them.

---

_Renamed from "server: setting related errors are reported for the python file." to "Server setting related errors are reported for the Python file" by @dhruvmanila on 2025-06-25 09:49_

---

_Comment by @Gankra on 2025-07-08 13:10_

My initial feeling is we want to send a [ShowMessage](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#window_showMessageRequest) request if we encounter a settings diagnostic (prompting the user to open the file and then get in-file diagnostics). I'll look at what other LSPs do here.

---

_Comment by @Gankra on 2025-07-08 13:19_

rust-analyzer reserves this little button which goes yellow when this kind of issue occurs. you have to just notice it and hover your mouse over it (or click it to go to rust-analyzer's global logs). Not the UX we want.

<img width="1358" height="871" alt="Image" src="https://github.com/user-attachments/assets/a7270c2c-25e8-48e8-a5a5-463201d13014" />

---

_Comment by @Gankra on 2025-07-08 13:26_

By contrast pylance/pyright seems to simply take every possible configuration error in stride? Like if you properly define settings it will respect them, but if you mess them up in anyway it will act like they're not there... I also don't really love this, but it's certainly implementable.

---

_Comment by @MichaReiser on 2025-07-08 14:02_

> Like if you properly define settings it will respect them, but if you mess them up in anyway it will act like they're not there... I also don't really love this, but it's certainly implementable.

This touches on https://github.com/astral-sh/ty/issues/76. I'm not sure what we currently do if the settings are invalid during startup (I think we simply panic) but ty preserves the old configuration if a user makes changes and the configuration then becomes invalid. For this issue, we can focus on how we want to surface those configuration errors, specifically how we want to do this if the check mode is `OpenFiles`.

The reason configuration files need special handling is because our LSP doesn't promote that our extension can handle `pyproject.toml` or `ty.toml` files. I see two solutions:

1. Add `pyproject.toml` and `ty.toml` files to the support files. Surface the setting diagnostics if `check_file` is called with the corresponding configuration file. I feel like this is a more invasive change and it's not quiet clear to me where this breaks our current architecture. Which is why I'd probably go with 2.
2. Use the LSPs publish diagnostic to:
  * Publish the initiale diagnostics after the project was opened
  * Re-Publish the diagnostics after a change to a `ty.toml` or `project.toml`
  * Clearing the diagnostics after closing a project (not something that can easily be tested today)

---

_Assigned to @Gankra by @MichaReiser on 2025-07-08 14:02_

---

_Comment by @Gankra on 2025-07-08 14:28_

Looking into option 2.

---

_Comment by @erictraut on 2025-07-08 16:03_

> By contrast pylance/pyright seems to simply take every possible configuration error in stride?

When pyright detects a config error, it emits an error message to a log. If it's the CLI version of the pyright, this error text is output directly to stdout (or stderr if the user has requested JSON output in stdout). If pyright is run as a language server, this log information is output using the [LogTrace](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#logTrace) mechanism in the language server protocol. In VS Code, this appears in the "Output" tab. That's admittedly not very discoverable for language server users.

---

_Comment by @MichaReiser on 2025-07-23 08:49_

This has been fixed in the latest release.

---

_Closed by @MichaReiser on 2025-07-23 08:49_

---

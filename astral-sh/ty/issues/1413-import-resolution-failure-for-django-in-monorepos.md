```yaml
number: 1413
title: Import resolution failure for django in monorepos in Helix Editor
type: issue
state: closed
author: rgasper
labels:
  - question
assignees: []
created_at: 2025-10-23T02:28:25Z
updated_at: 2025-10-24T17:25:34Z
url: https://github.com/astral-sh/ty/issues/1413
synced_at: 2026-01-10T02:06:25Z
```

# Import resolution failure for django in monorepos in Helix Editor

---

_Issue opened by @rgasper on 2025-10-23 02:28_

### Summary

When using ty in a django project inside of a larger monorepo, import resolution fails in the Helix editor. Same behavior works from VSCode with ty extension. `ty check` works. `python-lsp-server` in Helix works.


bug reproducible from: https://github.com/rgasper/ty-django-issue

```console
tree .
.
├── README.md
├── main.py
├── pyproject.toml
├── uv.lock
└── webserver
    └── mysite
        ├── README.md
        ├── app2
        │   ├── __init__.py
        │   ├── admin.py
        │   ├── apps.py
        │   ├── migrations
        │   │   └── __init__.py
        │   ├── models.py
        │   ├── tests.py
        │   └── views.py
        ├── db.sqlite3
        ├── main.py
        ├── manage.py
        ├── mysite
        │   ├── __init__.py
        │   ├── asgi.py
        │   ├── settings.py
        │   ├── urls.py
        │   └── wsgi.py
        ├── pyproject.toml
        └── uv.lock
```

```console
$ cd webserver/mysite
$ uv run hx --version      
helix 25.07.1 (a05c151b)
$ uv run ty --version           
ty 0.0.1-alpha.23 (5c51b8480 2025-10-16)
$ uv run ty check mysite/urls.py
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                                                                                                                                       All checks passed!
$ uv run hx --health python
Configured language servers:
  ✓ ty: /Users/raymondgasper/ty-django-issue/webserver/mysite/.venv/bin/ty
Configured debug adapter: None
Configured formatter: None
Tree-sitter parser: ✓
Highlight queries: ✓
Textobject queries: ✓
Indent queries: ✓
```

However, in the editor:
```console
$ uv run hx mysite/urls.py
# or this, then open mysite/urls.py in the editor. Doesnt matter:
$ uv run hx .
```
<img width="633" height="344" alt="Image" src="https://github.com/user-attachments/assets/5f7adcd4-f4c0-485c-a168-9202f0629760" />

Helix languages.toml - reduced for this example, but also happens with more complex settings:
```toml
[[language]]
name = "python"
scope = "source.python"
injection-regex = "python"
file-types = ["py"]
shebangs = ["python"]
roots = ["pyproject.toml"]
comment-token = "#"

language-servers = ["ty"] # (unresolved-import)
# language-servers = ["pylsp"] # no problems!

[language-server.ty]
command = "ty"
args = ["server"]

[language-server.pylsp]
command = "pylsp"

```

To see if it mattered, I tried setting up the `venvs` for `mysite` with `uv init`, `uv init --app`, `uv init --app --no-workspace`, and `poetry init` and while these can change the output of `uv run which ty`, it doesn't change any of the actual outcomes - `uv run ty check` (or `poetry run ty check`) successfully resolves the imports, whereas `ty server` run in Helix does not.

Using the `python-lsp-server` in Helix is able to resolve the imports reliably.

messages from `ty server` in the helix logs when running as `hx -vvv`:
```
2025-10-22T22:27:13.780 helix_lsp::transport [ERROR] ty err: <- StreamClosed
2025-10-22T22:27:20.099 helix_lsp::transport [ERROR] ty err <- "2025-10-22 22:27:20.099468000  INFO Version: 0.0.1-alpha.23 (5c51b8480 2025-10-16)\n"
2025-10-22T22:27:20.102 helix_lsp::transport [ERROR] ty err <- "2025-10-22 22:27:20.102529000  INFO Defaulting to python-platform `darwin`\n"
2025-10-22T22:27:20.102 helix_lsp::transport [ERROR] ty err <- "2025-10-22 22:27:20.102862000  INFO Python version: Python 3.13, platform: darwin\n"
2025-10-22T22:27:20.103 helix_lsp::transport [ERROR] ty err <- "2025-10-22 22:27:20.103070000  WARN Your LSP client doesn't support file watching outside of project: You may see stale results when dependencies change\n"
2025-10-22T22:27:20.109 helix_lsp::transport [ERROR] ty err <- "2025-10-22 22:27:20.109137000  INFO Indexed 14 file(s) in 0.003s\n"
2025-10-22T22:27:20.111 helix_lsp::transport [ERROR] ty err <- "2025-10-22 22:27:20.111935000  INFO Error looking up `types.ModuleType` in typeshed: expected to find a class definition on Python 3.13, but found a symbol of type `Never` instead. Falling back to `Unknown` for the symbol instead.\n"
```




### Version

ty 0.0.1-alpha.23 (5c51b8480 2025-10-16)

---

_Comment by @rgasper on 2025-10-23 02:39_

I installed vscode w/ the `ty` extension pretty easily. I added the recommended to my user settings:

```json
{
  "python.languageServer": "None"
}
```

and it seems like import resolution is working when running in the VSCode extension opened to that same directory (`webserver/mysite`). So is the ty extension in vscode somehow running it in a different manner? From looking thru the extension code, it does appear to launch `ty server`, just like Helix is, but I'm not sure what other settings/configuration might be happening around that.

VSCode Python logs:
```
2025-10-22 22:35:03.651 [info] Editor support is inactive since language server is set to None.
2025-10-22 22:35:03.652 [info] Active interpreter [/Users/raymondgasper/ty-django-issue/webserver/mysite]:  /Users/raymondgasper/ty-django-issue/webserver/mysite/.venv/bin/python
2025-10-22 22:35:03.652 [info] Discover tests for workspace name: mysite - uri: /Users/raymondgasper/ty-django-issue/webserver/mysite
```
VSCode Ty Language Server logs:
```
2025-10-22 22:35:03.681526000  INFO Version: 0.0.1-alpha.23 (5c51b8480 2025-10-16)
2025-10-22 22:35:03.683702000  INFO Defaulting to python-platform `darwin`
2025-10-22 22:35:03.684046000  INFO Python version: Python 3.13, platform: darwin
2025-10-22 22:35:03.690254000  INFO Indexed 13 file(s) in 0.003s
```

---

_Comment by @rgasper on 2025-10-23 02:59_

I also opened that ^ issue in the helix repo, since I'm not sure where the bug is coming from.

---

_Comment by @MichaReiser on 2025-10-23 06:03_

Thanks for the detailed issue. 

Is my understanding correct that you use two separate venvs: one for the root and one for mysite? 

> ```
> $ uv run hx mysite/urls.py
> # or this, then open mysite/urls.py in the editor. Doesnt matter:
> $ uv run hx .
> ```

I don't use helix myself so I'm guessing here but I suspect that helix will set the current working directory to `webserver` when running `uv run hx mysite/urls.py`. That most likely means that ty incorrectly picks up the `pyproject.toml` and venv at the root of your project (it starts from the current working directory) and not the one from `mysite`. Can you try starting ty from within the mysite directory?

If that's not it, I suggest changing the [`logLevel`](https://docs.astral.sh/ty/reference/editor-settings/#loglevel) setting to at least debug. The logs should then tell you which search paths ty found and registered. That will hopefully give us an idea of what's happening.

---

_Label `question` added by @MichaReiser on 2025-10-23 06:03_

---

_Comment by @rgasper on 2025-10-23 17:08_

I think helix is properly using `ty` from the correct venv on account of this health check:

```
$ uv run hx --health python
Configured language servers:
  ✓ ty: /Users/raymondgasper/ty-django-issue/webserver/mysite/.venv/bin/ty
```

How could I set the loglevel for `ty server` run from the cli? Is there a global config file I can use that's not specific to a certain LSP client implementation in an IDE? That's all I see in the docs. I looked around in the documentation for this before as well, and don't see how to set the loglevel in this situation.

---

_Comment by @MichaReiser on 2025-10-23 17:16_

I'm not familiar with helix but you'd have to set it in the helix configuration (where you configure the LSP). 

>   ✓ ty: /Users/raymondgasper/ty-django-issue/webserver/mysite/.venv/bin/ty

This only says which ty binary helix is using. It doesn't tell you anything about which VENV ty discovers.

---

_Comment by @rgasper on 2025-10-23 19:59_

hmmmm I see

---

_Comment by @MichaReiser on 2025-10-24 06:30_

Looking at [Ruff's helix documentation](https://docs.astral.sh/ruff/editors/setup/#helix), you might be able to do something like this 


```toml
[language-server.ty.config.settings]
logLevel = "debug"
logFile = "/path/to/log/file
```

---

_Comment by @rgasper on 2025-10-24 14:42_

Okay! Setting the helix ty config to: 

```toml
[language-server.ty.config]
logLevel = "debug"
logFile = "/Users/raymondgasper/.cache/ty/helix-ty.log"
```

resulted in the following logfile:
```
2025-10-24 10:40:45.039618000 DEBUG Initialization options: InitializationOptions { log_level: Some(Debug), log_file: Some("/Users/raymondgasper/.cache/ty/helix-ty.log"), options: ClientOptions { global: GlobalOptions { diagnostic_mode: None, experimental: None }, workspace: WorkspaceOptions { disable_language_services: None, inlay_hints: None, python_extension: None }, unknown: {} } }
2025-10-24 10:40:45.039994000  INFO Version: 0.0.1-alpha.23 (5c51b8480 2025-10-16)
2025-10-24 10:40:45.040904000 DEBUG Requesting workspace configuration for workspaces
2025-10-24 10:40:45.041393000 DEBUG Deferring `workspace/didChangeConfiguration` notification until all workspaces are initialized
2025-10-24 10:40:45.041502000 DEBUG Deferring `textDocument/didOpen` notification until all workspaces are initialized
2025-10-24 10:40:45.041781000 DEBUG client_response{id=0 method="workspace/configuration"}: Received workspace configurations, initializing workspaces
2025-10-24 10:40:45.041851000 DEBUG client_response{id=0 method="workspace/configuration"}: No workspace options provided for file:///Users/raymondgasper/ty-django-issue, using default options
2025-10-24 10:40:45.041899000 DEBUG Initializing workspace `file:///Users/raymondgasper/ty-django-issue`
2025-10-24 10:40:45.041944000 DEBUG Searching for a project in '/Users/raymondgasper/ty-django-issue'
2025-10-24 10:40:45.042288000 DEBUG Resolving requires-python constraint: `>=3.13`
2025-10-24 10:40:45.042452000 DEBUG Resolved requires-python constraint to: 3.13
2025-10-24 10:40:45.042529000 DEBUG Project without `tool.ty` section: '/Users/raymondgasper/ty-django-issue'
2025-10-24 10:40:45.042555000 DEBUG Searching for a user-level configuration at `/Users/raymondgasper/.config/ty/ty.toml`
2025-10-24 10:40:45.043442000  INFO Defaulting to python-platform `darwin`
2025-10-24 10:40:45.043499000 DEBUG Resolving `VIRTUAL_ENV` environment variable: /Users/raymondgasper/ty-django-issue/webserver/mysite/.venv
2025-10-24 10:40:45.043619000 DEBUG Attempting to parse virtual environment metadata at '/Users/raymondgasper/ty-django-issue/webserver/mysite/.venv/pyvenv.cfg'
2025-10-24 10:40:45.043770000 DEBUG Searching for site-packages directory in sys.prefix /Users/raymondgasper/ty-django-issue/webserver/mysite/.venv
2025-10-24 10:40:45.043949000 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/raymondgasper/ty-django-issue/webserver/mysite/.venv/lib/python3.13/site-packages"})
2025-10-24 10:40:45.044232000 DEBUG Searching for real stdlib directory in sys.prefix /opt/homebrew/Cellar/python@3.13/3.13.7/Frameworks/Python.framework/Versions/3.13
2025-10-24 10:40:45.044261000 DEBUG Resolved real stdlib path for this virtual environment is: /opt/homebrew/Cellar/python@3.13/3.13.7/Frameworks/Python.framework/Versions/3.13/lib/python3.13
2025-10-24 10:40:45.044286000 DEBUG Including `.` in `environment.root`
2025-10-24 10:40:45.044308000 DEBUG Adding first-party search path `/Users/raymondgasper/ty-django-issue`
2025-10-24 10:40:45.044323000 DEBUG Using vendored stdlib
```

If i'm interpreting that correctly, `ty` is using the correct virtual environment - the django server's venv:
`2025-10-24 10:40:45.043499000 DEBUG Resolving `VIRTUAL_ENV` environment variable: /Users/raymondgasper/ty-django-issue/webserver/mysite/.venv`

but the search-path is starting at the top of the repository, instead of at `webserver/mysite`

---

_Comment by @rgasper on 2025-10-24 14:57_

OK! So I was able to fix the behavior. Pushed updates to the ty-django-issue repo

What fixed it was that I had to add:
```toml
[tool.ty.environment]
extra-paths = ["./webserver/mysite"]
```
to the top-level `pyproject.toml`. Any `ty` config I put in `webserver/mysite/pyproject.toml` seems to be entirely ignored. This seems quite unexpected to me, since that's the `pyproject.toml` which corresponds to the `.venv` which `ty` is operating from.

that results in logs - redundant stuff / noise redacted:
```
2025-10-24 10:53:20.967317000 DEBUG Adding extra search-path `/Users/raymondgasper/ty-django-issue/webserver/mysite`
2025-10-24 10:53:20.974123000 DEBUG notification{method="textDocument/didOpen"}:check_file{file=file(Id(c01))}: Checking file '/Users/raymondgasper/ty-django-issue/webserver/mysite/mysite/urls.py'
2025-10-24 10:53:20.979913000 DEBUG notification{method="textDocument/didOpen"}:check_file{file=file(Id(c01))}: Resolving dynamic module resolution paths
```
 
This seems not ideal to me - if I have multiple webservers in my monorepo, I now have both to add each of them to the search paths at the top-level `pyproject.toml` and have to be concerned about potential name collisions between the webservers for `ty` to work properly; e.g. if multiple webservers have an `app2.views`, `ty` would only work correctly if I'm looking at the code for the webserver w/ the first entry in `extra-paths`.

---

_Comment by @MichaReiser on 2025-10-24 15:45_

From this log:

```
2025-10-24 10:40:45.041899000 DEBUG Initializing workspace `file:///Users/raymondgasper/ty-django-issue`
```

It still looks like `helix` sends the outer directory as the workspace rather than the inner project. That's why ty uses the configuration from the outer project too

```
2025-10-24 10:40:45.044308000 DEBUG Adding first-party search path `/Users/raymondgasper/ty-django-issue`
```

Also suggests that the first party search path is incorrect. 

I'm not sure why helix would start the server in `/Users/raymondgasper/ty-django-issue` instead of `/Users/raymondgasper/ty-django-issue/webserver/mysite/` if you start helix from *within* `/Users/raymondgasper/ty-django-issue/webserver/mysite/`



---

_Comment by @rgasper on 2025-10-24 17:06_

Figured this out by changing my `workspace-lsp-roots` setting in the helix languages config. Thanks very much for your help! 

---

_Closed by @rgasper on 2025-10-24 17:06_

---

_Comment by @rgasper on 2025-10-24 17:25_

lol actually that didn't solve it... ugh. Looks like the root cause is over on helix's side , hto

---

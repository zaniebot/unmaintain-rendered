```yaml
number: 1708
title: "Log a warning message if we can detect that ty Python version doesn't match active environment Python version"
type: issue
state: open
author: alex
labels:
  - diagnostics
  - imports
assignees: []
created_at: 2025-12-01T14:39:37Z
updated_at: 2025-12-03T22:12:38Z
url: https://github.com/astral-sh/ty/issues/1708
synced_at: 2026-01-12T15:54:25Z
```

# Log a warning message if we can detect that ty Python version doesn't match active environment Python version

---

_@alex_

### Summary

In pyca/cryptography we have code like:

```py
if sys.version_info >= (3, 11):
    import tomllib
else:
    import tomli as tomllib
```

This produces errors like:

```
error[unresolved-import]: Cannot resolve imported module `tomli`
  --> noxfile.py:21:12
   |
19 |     import tomllib
20 | else:
21 |     import tomli as tomllib
   |            ^^^^^^^^^^^^^^^^
22 |
23 | nox.options.reuse_existing_virtualenvs = True
   |
info: Searched in the following paths during module resolution:
info:   1. /Users/alex_gaynor/projects/cryptography/src (first-party code)
info:   2. /Users/alex_gaynor/projects/cryptography (first-party code)
info:   3. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   4. /Users/alex_gaynor/projects/cryptography/.nox/flake/lib/python3.14/site-packages (site-packages)
info:   5. /Users/alex_gaynor/projects/cryptography/vectors (editable install)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```

As far as I can tell what's happening is: `ty` is in a Python 3.14 venv, so `tomli` isn't installed. However, `ty` infers that it should run for Python 3.8 from my project's `requires-python`.

Therefore it type-checks the `import tomli` path and is said that the package isn't installed.

I'm not _quite_ sure what the right behavior is, but the current behavior is perplexing and its not clear how to fix :-)

### Version

ty 0.0.1-alpha.29 (0c3cae494 2025-11-28)

---

_Comment by @AlexWaygood on 2025-12-01 14:52_

Often the solution to this kind of thing is to declare `tomli` as a dev-dependency, so that for type-checking it's always installed, even if it's not always installed at runtime. This is similar to the way you might specify a stubs package as a dev-dependency for type-checking purposes, even though you don't use those stubs at runtime. Mypy has this exact problem when type-checking itself, for example: https://github.com/python/mypy/blob/ad0f41eea63ef227739b66f08afbb67544597d79/test-requirements.in#L14



---

_Comment by @alex on 2025-12-01 14:54_

I think in practice we don't see this issue on mypy because it uses the python interpreter's version, rather than the inferring it from pyproject.toml?

---

_Comment by @AlexWaygood on 2025-12-01 14:58_

that's what mypy does by default, yes. You can specify `--python-version=3.14` manually on the command line if you want to override ty's default behaviour -- but our default assumption is different to mypy's. Our default is to assume that you would like your project to type-check on the lowest version of Python your project supports, even if you're using a newer version of Python for local development.

(Note: when I mentioned mypy above, I was referring to the dependencies mypy installs in CI when it type-checks _itself_! It also has to declare `tomli` as a dev-dependency, because of similar `sys.version_info` branches in its own source code.)

---

_Comment by @alex on 2025-12-01 17:04_

One note is that this changes one of the workflows we currently use, which is to run our type checking job on both our minimum python version and maximum version. For ty it seems like if we want to keep that workflow, we should have noxfile specify `--python-version={sys.version}`.

---

_Label `question` added by @sharkdp on 2025-12-01 18:04_

---

_Comment by @AlexWaygood on 2025-12-03 12:27_

I think everything's working as expected here, and our behaviour is pretty well documented in https://docs.astral.sh/ty/reference/configuration/#python-version IMO, so I'm going to close this. I think it's also clear to see from our existing diagnostic that we're using the `site-packages` directory from your Python 3.14 environment (it's listed as the fourth search path where we looked for the module). I acknowledge that this behaviour might seem surprising if you're used to mypy's behaviour where it defaults to the Python version of the interpreter currently being used, but I'd argue that our behaviour makes more sense _a priori_ -- if my Python project supports Python 3.9, I'd always want to check that my types and syntax are valid on 3.9, even if I'm using 3.14 for local development.

Attempting to figure out the Python version of the currently active environment also isn't always easy. We can do it for a virtual environment pretty well by parsing the pyvenv.cfg file, and on Unix we can look at the directory structure (we do both of these things as a fallback if `--python-version` wasn't specified on the CLI and there was no configuration in the pyproject.toml file!), but neither of those techniques work for Windows -- we might have to invoke the Python interpreter itself as a subprocess, which would be slow.

---

_Closed by @AlexWaygood on 2025-12-03 12:27_

---

_Comment by @AlexWaygood on 2025-12-03 13:20_

(Very happy to reopen if anybody has any specific suggestions for how to further improve our diagnostic here, but I'm struggling to see how we could in this specific case. Our diagnostic already includes a lot of information here.)

---

_Comment by @carljm on 2025-12-03 19:26_

I think the trickiest thing here is just the fact that dependencies may vary by Python version, so there's an inherent potential mismatch whenever we type-check with a Python version that doesn't match the environment we're using. I could see a justification for logging a pre-emptive warning message (maybe only in verbose mode) if we can detect that's the case (which, as you say, we can only easily do on Unix or if we have a `pyvenv.cfg`).

But beyond that, ultimately we provide the necessary tools here (via an explicit-set Python version) to do whatever you need, it's just a question of picking the right defaults, and there's no one "right answer" there.

---

_Reopened by @AlexWaygood on 2025-12-03 19:27_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-03 19:27_

---

_Label `question` removed by @AlexWaygood on 2025-12-03 19:27_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-03 19:27_

---

_Label `imports` added by @AlexWaygood on 2025-12-03 19:27_

---

_Renamed from "Perplexing errors with version checks + inferred version" to "Log a warning message if we can detect that ty Python version doesn't match active environment Python version" by @carljm on 2025-12-03 19:29_

---

_Comment by @zanieb on 2025-12-03 19:35_

Dependencies that are conditional on the Python version is the kind of thing that a uv integration can improve diagnostics for!

---

_Comment by @AlexWaygood on 2025-12-03 19:50_

I would not want to log a warning every time ty starts up if the user is using a newer version of Python for local development than the oldest version of Python their project supports. That's pretty common and reasonable; the solution in this kind of situation is to list the problematic import as a dev-dependency so that the type checker can resolve it on all Python versions. A warning even if there are no diagnostics feels very noisy to me.

I do think, however, we can note the version mismatch as a subdiagnostic when import resolution fails. And as @zanieb says, this subdiagnostic will become much more helpful if/when we're easily able to query uv about whether the dependency would be installed on other Python versions.

---

_Comment by @AlexWaygood on 2025-12-03 19:58_

Gah, and flipping back on this again having tried it out -- no, a subdiagnostic just won't be useful at all right now without being able to query uv, I don't think. So I guess a debug-only warning for now might be the best we can do right now?

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-12-03 20:01_

---

_Comment by @MichaReiser on 2025-12-03 21:08_

I don't think we should add any non suppressable warnings for something that's a very valid use case

---

_Comment by @carljm on 2025-12-03 21:49_

I was thinking that's ok for a debug-only (verbose mode) message -- the idea is to provide information to the user that might be relevant when they are trouble shooting a problem. It's not going to be noisy in normal usage. But I don't feel strongly about it. I do think we need uv integration in order to fine-tune the message to only appear when there are actual version-dependent dependencies.

---

_Comment by @MichaReiser on 2025-12-03 22:10_

I'm okay with a debug or even info level message. But it shouldn't be warn or higher

---

_Comment by @AlexWaygood on 2025-12-03 22:12_

(I would not be okay with info-level personally, but I would be okay with debug-level)

---

---
number: 9255
title: "`uv sync` doesn't respect `--index-url` flag in `requirements.txt`"
type: issue
state: closed
author: fynnsu
labels: []
assignees: []
created_at: 2024-11-19T22:38:03Z
updated_at: 2024-11-19T22:47:59Z
url: https://github.com/astral-sh/uv/issues/9255
synced_at: 2026-01-10T01:24:38Z
---

# `uv sync` doesn't respect `--index-url` flag in `requirements.txt`

---

_Issue opened by @fynnsu on 2024-11-19 22:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I have a requirements.txt file with a custom index-url for one of the packages, ex.
```
--index-url https://my_custom_index.org
some_package
```
and in my `pyproject.toml`
```
[project]
name = "my_project"
dynamic = ["dependencies"]
...
[tool.setuptools.dynamic]
dependencies = { file = ["requirements.txt"] }
...
```

Running
```bash
uv sync
```
results in:
```
❯ uv sync
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.10`.
  × Failed to build `my_package @ file:///home/fynnsu/projects/my_package`
  ╰─▶ Build backend failed to determine requirements with `build_editable()` (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/_vendor/packaging/requirements.py", line 36,
      in __init__
          parsed = _parse_requirement(requirement_string)
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/_vendor/packaging/_parser.py", line 62, in
      parse_requirement
          return _parse_requirement(Tokenizer(source, rules=DEFAULT_RULES))
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/_vendor/packaging/_parser.py", line 71, in
      _parse_requirement
          name_token = tokenizer.expect(
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/_vendor/packaging/_tokenizer.py", line 142,
      in expect
          raise self.raise_syntax_error(f"Expected {expected}")
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/_vendor/packaging/_tokenizer.py", line 167,
      in raise_syntax_error
          raise ParserSyntaxError(
      packaging._tokenizer.ParserSyntaxError: Expected package name at the start of dependency specifier
          --index-url https://my_custom_index.org
          ^

      The above exception was the direct cause of the following exception:

      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/build_meta.py", line 483, in
      get_requires_for_build_editable
          return self.get_requires_for_build_wheel(config_settings)
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/build_meta.py", line 334, in
      get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/build_meta.py", line 304, in _get_build_requires
          self.run_setup()
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 85, in <module>
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/__init__.py", line 117, in setup
          return distutils.core.setup(**attrs)
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/_distutils/core.py", line 157, in setup
          dist.parse_config_files()
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/_virtualenv.py", line 20, in parse_config_files
          result = old_parse_config_files(self, *args, **kwargs)
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/dist.py", line 648, in parse_config_files
          pyprojecttoml.apply_configuration(self, filename, ignore_option_errors)
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/config/pyprojecttoml.py", line 73, in
      apply_configuration
          return _apply(dist, config, filepath)
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/config/_apply_pyprojecttoml.py", line 59, in
      apply
          dist._finalize_requires()
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/dist.py", line 380, in _finalize_requires
          self._normalize_requires()
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/dist.py", line 395, in _normalize_requires
          self.install_requires = list(map(str, _reqs.parse(install_requires)))
        File "/home/fynnsu/.cache/uv/builds-v0/.tmpSKP82X/lib/python3.10/site-packages/setuptools/_vendor/packaging/requirements.py", line 38,
      in __init__
          raise InvalidRequirement(str(e)) from e
      packaging.requirements.InvalidRequirement: Expected package name at the start of dependency specifier
          --index-url https://my_custom_index.org
          ^
```

Even if I also add the index to `pyproject.toml` it still fails:
```
[[tool.uv.index]]
url = "https://my_custom_index.org"
default = false
```

Perhaps this is intended behaviour and I need to fully switch to the uv specification, however, that makes it difficult to gradually switch the project, while maintaining support for `requirements.txt`. Also the fact that `uv pip install` handles `--index-url` in `requirements.txt` correctly makes me think this might be a bug. 


---

_Comment by @charliermarsh on 2024-11-19 22:39_

We do respect this. That looks like a setuptools error? Which isn't in uv itself.

---

_Comment by @fynnsu on 2024-11-19 22:47_

Oh I see, my bad. 

---

_Closed by @fynnsu on 2024-11-19 22:47_

---

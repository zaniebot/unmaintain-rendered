---
number: 11872
title: "Following destructions at https://block.github.io/goose/docs/tutorials/custom-extensions I get a build fail at https://block.github.io/goose/docs/tutorials/custom-extensions#step-4-test-your-mcp-server"
type: issue
state: closed
author: Bazmundi
labels:
  - needs-mre
assignees: []
created_at: 2025-03-01T01:40:52Z
updated_at: 2025-03-01T02:56:23Z
url: https://github.com/astral-sh/uv/issues/11872
synced_at: 2026-01-10T01:25:12Z
---

# Following destructions at https://block.github.io/goose/docs/tutorials/custom-extensions I get a build fail at https://block.github.io/goose/docs/tutorials/custom-extensions#step-4-test-your-mcp-server

---

_Issue opened by @Bazmundi on 2025-03-01 01:40_

```
(goose) asterion@MonstaPC:~/AI/goose/mcp-wiki$ uv sync
Resolved 33 packages in 0.48ms
  × Failed to build `mcp-wiki @ file:///home/asterion/AI/goose/mcp-wiki`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
          wheel_filename = backend.build_editable("/home/asterion/.cache/uv/builds-v0/.tmpYX0cfN", {}, None)
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/build.py", line 83, in build_editable
          return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                                  ~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/builders/plugin/interface.py", line 155, in build
          artifact = version_api[version](directory, **build_data)
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/builders/wheel.py", line 496, in build_editable
          return self.build_editable_detection(directory, **build_data)
                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/builders/wheel.py", line 507, in build_editable_detection
          for included_file in self.recurse_selected_project_files():
                               ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/builders/plugin/interface.py", line 180, in recurse_selected_project_files
          if self.config.only_include:
             ^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/asterion/miniconda3/envs/goose/lib/python3.13/functools.py", line 1042, in __get__
          val = self.func(instance)
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/builders/config.py", line 713, in only_include
          only_include = only_include_config.get('only-include', self.default_only_include()) or self.packages
                                                                 ~~~~~~~~~~~~~~~~~~~~~~~~~^^
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/builders/wheel.py", line 262, in default_only_include
          return self.default_file_selection_options.only_include
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/asterion/miniconda3/envs/goose/lib/python3.13/functools.py", line 1042, in __get__
          val = self.func(instance)
        File "/home/asterion/.cache/uv/builds-v0/.tmpL5GZLi/lib/python3.13/site-packages/hatchling/builders/wheel.py", line 250, in default_file_selection_options
          raise ValueError(message)
      ValueError: Unable to determine which files to ship inside the wheel using the following heuristics: https://hatch.pypa.io/latest/plugins/builder/wheel/#default-file-selection

      The most likely cause of this is that there is no directory that matches the name of your project (mcp_wiki).

      At least one file selection option must be defined in the `tool.hatch.build.targets.wheel` table, see: https://hatch.pypa.io/latest/config/build/

      As an example, if you intend to ship a directory named `foo` that resides within a `src` directory located at the root of your project, you can define the following:

      [tool.hatch.build.targets.wheel]
      packages = ["src/foo"]

      hint: This usually indicates a problem with the package or the build environment.

```

---

_Label `needs-mre` added by @charliermarsh on 2025-03-01 01:48_

---

_Comment by @charliermarsh on 2025-03-01 01:50_

Unfortunately we'd need more information to help you here. Please see: https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/. This is a problem with your package, rather than a problem with uv, and it's something you need to solve with the build backend (hatchling). Are your files in `src/mcp_wiki`?

---

_Closed by @Bazmundi on 2025-03-01 02:54_

---

_Comment by @Bazmundi on 2025-03-01 02:55_

User thumbs, I missnamed a directory.


---

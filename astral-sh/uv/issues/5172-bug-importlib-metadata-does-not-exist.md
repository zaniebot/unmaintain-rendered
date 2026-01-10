---
number: 5172
title: "[BUG] importlib.metadata does not exist"
type: issue
state: closed
author: 17Reset
labels: []
assignees: []
created_at: 2024-07-18T01:46:46Z
updated_at: 2024-07-18T02:34:25Z
url: https://github.com/astral-sh/uv/issues/5172
synced_at: 2026-01-10T01:23:46Z
---

# [BUG] importlib.metadata does not exist

---

_Issue opened by @17Reset on 2024-07-18 01:46_

When I use pyinstaller to package the library after installing it with uv, I get the error shown below, The solution is a separate uv pip install importlib.metadata :

```
16451 INFO: Loading module hook 'hook-importlib_metadata.py' from 'G:\\X\\\Joker\\Configure\\joker_env\\Lib\\site-packages\\PyInstaller\\hooks'...
Traceback (most recent call last):
  File "D:\Python\Lib\importlib\metadata\__init__.py", line 397, in from_name
    return next(cls.discover(name=name))
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
StopIteration

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\imphook.py", line 383, in _load_hook_module
    self._hook_module = importlib_load_source(self.hook_module_name, self.hook_filename)
                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\compat.py", line 583, in importlib_load_source
    mod_loader.exec_module(mod)
  File "<frozen importlib._bootstrap_external>", line 995, in exec_module
  File "<frozen importlib._bootstrap>", line 488, in _call_with_frames_removed
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\hooks\hook-importlib_metadata.py", line 24, in <module>
    datas = copy_metadata('importlib_metadata')
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\utils\hooks\__init__.py", line 957, in copy_metadata
    dist = importlib_metadata.distribution(package_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "D:\Python\Lib\importlib\metadata\__init__.py", line 862, in distribution
    return Distribution.from_name(distribution_name)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "D:\Python\Lib\importlib\metadata\__init__.py", line 399, in from_name
    raise PackageNotFoundError(name)
importlib.metadata.PackageNotFoundError: No package metadata was found for importlib_metadata

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "G:\X\Joker\Configure\joker_env\Scripts\pyinstaller.exe\__main__.py", line 8, in <module>
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\__main__.py", line 231, in _console_script_run
    run()
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\__main__.py", line 215, in run
    run_build(pyi_config, spec_file, **vars(args))
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\__main__.py", line 70, in run_build
    PyInstaller.building.build_main.main(pyi_config, spec_file, **kwargs)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\building\build_main.py", line 1216, in main
    build(specfile, distpath, workpath, clean_build)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\building\build_main.py", line 1156, in build
    exec(code, spec_namespace)
  File "G:\X\Joker\xLib\compiler\release\xLib Compiler.spec", line 4, in <module>
    a = Analysis(
        ^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\building\build_main.py", line 556, in __init__
    self.__postinit__()
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\building\datastruct.py", line 184, in __postinit__
    self.assemble()
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\building\build_main.py", line 699, in assemble
    self.graph.add_hiddenimports(self.hiddenimports)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 744, in add_hiddenimports
    nodes = self.import_hook(modnm)
            ^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1232, in import_hook
    target_package, target_module_partname = self._find_head_package(
                                             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1411, in _find_head_package
    target_package = self._safe_import_module(
                     ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1232, in import_hook
    target_package, target_module_partname = self._find_head_package(
                                             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1411, in _find_head_package
    target_package = self._safe_import_module(
                     ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1232, in import_hook
    target_package, target_module_partname = self._find_head_package(
                                             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1411, in _find_head_package
    target_package = self._safe_import_module(
                     ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1245, in import_hook
    submodule = self._safe_import_module(head, mname, submodule)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1232, in import_hook
    target_package, target_module_partname = self._find_head_package(
                                             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1411, in _find_head_package
    target_package = self._safe_import_module(
                     ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1245, in import_hook
    submodule = self._safe_import_module(head, mname, submodule)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1245, in import_hook
    submodule = self._safe_import_module(head, mname, submodule)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1232, in import_hook
    target_package, target_module_partname = self._find_head_package(
                                             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1411, in _find_head_package
    target_package = self._safe_import_module(
                     ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2257, in _safe_import_hook
    self.import_hook(
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1271, in import_hook
    for target_submodule in self._import_importable_package_submodules(
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1520, in _import_importable_package_submodules
    submodule = self._safe_import_module(
                ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1232, in import_hook
    target_package, target_module_partname = self._find_head_package(
                                             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1411, in _find_head_package
    target_package = self._safe_import_module(
                     ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 437, in _safe_import_hook
    ret_modules = super()._safe_import_hook(
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2048, in _safe_import_hook
    target_modules = self.import_hook(
                     ^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1232, in import_hook
    target_package, target_module_partname = self._find_head_package(
                                             ^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1411, in _find_head_package
    target_package = self._safe_import_module(
                     ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 501, in _safe_import_module
    return super()._safe_import_module(module_basename, module_name, parent_package)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 1792, in _safe_import_module
    self._process_imports(n)
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\lib\modulegraph\modulegraph.py", line 2591, in _process_imports
    target_modules = self._safe_import_hook(*import_info, **kwargs)
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 368, in _safe_import_hook
    excluded_imports = self._find_all_excluded_imports(source_module.identifier)
                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\analysis.py", line 356, in _find_all_excluded_imports
    excluded_imports.update(module_hook.excludedimports)
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\imphook.py", line 316, in __getattr__
    self._load_hook_module()
  File "G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\depend\imphook.py", line 386, in _load_hook_module
    raise ImportErrorWhenRunningHook(self.hook_module_name, self.hook_filename)
PyInstaller.exceptions.ImportErrorWhenRunningHook: Failed to import module __PyInstaller_hooks_0_importlib_metadata required by hook for module G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\hooks\hook-importlib_metadata.py. Please check whether module __PyInstaller_hooks_0_importlib_metadata actually exists and whether the hook is compatible with your version of G:\X\Joker\Configure\joker_env\Lib\site-packages\PyInstaller\hooks\hook-importlib_metadata.py: You might want to read more about hooks in the manual and provide a pull-request to improve PyInstaller
```

---

_Comment by @17Reset on 2024-07-18 02:34_

This is setuptools bug

---

_Closed by @17Reset on 2024-07-18 02:34_

---

```yaml
number: 1578
title: allow centralized venv storage like pyenv-virtualenv
type: issue
state: closed
author: wakamex
labels:
  - duplicate
assignees: []
created_at: 2024-02-17T08:07:42Z
updated_at: 2024-10-04T15:02:51Z
url: https://github.com/astral-sh/uv/issues/1578
synced_at: 2026-01-10T04:45:09Z
```

# allow centralized venv storage like pyenv-virtualenv

---

_Issue opened by @wakamex on 2024-02-17 08:07_

This provides a few handy features:
- easily use the same venv in multiple locations
- don't worry about losing your venv when you `rm -rf` a folder

To identify which venv should be used in a folder, pyenv-virtualenv uses `.python-version`. I suggest `uv` could use `.uvenv` as follows:

```
uv venv ~/.venvs/myvenv
```
by invoking `uv venv` with a non-standard location, we know we'll want to keep track of that location, so I suggest storing it in `.uvenv` right away without forcing the user to do `echo "~/.venvs/myvenv" > .uvenv`.

I can already use a script to auto-activate my venv from this remote location upon entering a folder (like an [oh-my-zsh plugin](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/virtualenvwrapper/virtualenvwrapper.plugin.zsh)):
```sh
if [ -f .uvenv ]; then
  source "$(cat .uvenv)/bin/activate"
fi
```

but if this were built-in behavior defined in a central location equivalent to `PYENV_ROOT` then you could just give the name:
```sh
uv venv local myvenv
```

pyenv blurb from https://github.com/pyenv/pyenv-virtualenv?tab=readme-ov-file#activate-virtualenv:
> If eval "$(pyenv virtualenv-init -)" is configured in your shell, pyenv-virtualenv will automatically activate/deactivate virtualenvs on entering/leaving directories which contain a .python-version file that contains the name of a valid virtual environment as shown in the output of pyenv virtualenvs (e.g., venv34 or 3.4.3/envs/venv34 in example above) . .python-version files are used by pyenv to denote local Python versions and can be created and deleted with the [pyenv local](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-local) command.

---

_Comment by @hauntsaninja on 2024-02-17 09:16_

I've been using a `.venv` file that can contain a location to accomplish something similar for the last half decade. It's great. I think when PEP 704 was being discussed the `.venv` file was suggested and had some support. This would also mean `uv` only needs to have a single special case name. But maybe I should just symlink?

Maybe something like the following diff:

<details>

```diff
diff --git a/crates/uv-interpreter/src/lib.rs b/crates/uv-interpreter/src/lib.rs
index 684c7f1..7c1caab 100644
--- a/crates/uv-interpreter/src/lib.rs
+++ b/crates/uv-interpreter/src/lib.rs
@@ -26,6 +26,8 @@ pub enum Error {
     MissingPyVenvCfg(PathBuf),
     #[error("Broken virtualenv `{0}`, it contains a pyvenv.cfg but no Python binary at `{1}`")]
     BrokenVenv(PathBuf, PathBuf),
+    #[error("File `{0}` points to invalid location `{1}`")]
+    InvalidVenvFile(PathBuf, PathBuf),
     #[error("Both VIRTUAL_ENV and CONDA_PREFIX are set. Please unset one of them.")]
     Conflict,
     #[error("No versions of Python could be found. Is Python installed?")]
diff --git a/crates/uv-interpreter/src/virtual_env.rs b/crates/uv-interpreter/src/virtual_env.rs
index 46c727c..a4b63a3 100644
--- a/crates/uv-interpreter/src/virtual_env.rs
+++ b/crates/uv-interpreter/src/virtual_env.rs
@@ -1,5 +1,6 @@
 use std::env;
 use std::env::consts::EXE_SUFFIX;
+use std::io::Read;
 use std::path::{Path, PathBuf};
 
 use tracing::debug;
@@ -116,21 +117,34 @@ pub(crate) fn detect_virtual_env(target: &PythonPlatform) -> Result<Option<PathB
         }
     };
 
-    // Search for a `.venv` directory in the current or any parent directory.
+
+    // Search for `.venv` in the current or any parent directory.
     let current_dir = env::current_dir().expect("Failed to detect current directory");
     for dir in current_dir.ancestors() {
         let dot_venv = dir.join(".venv");
-        if dot_venv.is_dir() {
-            if !dot_venv.join("pyvenv.cfg").is_file() {
-                return Err(Error::MissingPyVenvCfg(dot_venv));
-            }
-            let python = target.venv_python(&dot_venv);
-            if !python.is_file() {
-                return Err(Error::BrokenVenv(dot_venv, python));
+
+        let venv_dir;
+        if dot_venv.is_file() {
+            let mut contents = String::new();
+            fs_err::File::open(&dot_venv)?.read_to_string(&mut contents)?;
+            venv_dir = PathBuf::from(contents.trim());
+            if !venv_dir.is_dir() {
+                return Err(Error::InvalidVenvFile(dot_venv, venv_dir));
             }
-            debug!("Found a virtualenv named .venv at: {}", dot_venv.display());
-            return Ok(Some(dot_venv));
+        } else if dot_venv.is_dir() {
+            venv_dir = dot_venv;
+        } else {
+            continue;
+        }
+        if !venv_dir.join("pyvenv.cfg").is_file() {
+            return Err(Error::MissingPyVenvCfg(venv_dir));
+        }
+        let python = target.venv_python(&venv_dir);
+        if !python.is_file() {
+            return Err(Error::BrokenVenv(venv_dir, python));
         }
+        debug!("Found a virtualenv at: {}", venv_dir.display());
+        return Ok(Some(venv_dir));
     }
 
     Ok(None)
```

</details>

---

_Comment by @swaldhoer on 2024-02-17 12:02_

Basically a duplicate of https://github.com/astral-sh/uv/issues/1526

---

_Comment by @zanieb on 2024-02-17 19:34_

Not a duplicate of #1526, but this is a duplicate of https://github.com/astral-sh/uv/issues/1495.

---

_Closed by @zanieb on 2024-02-17 19:34_

---

_Label `duplicate` added by @zanieb on 2024-02-17 19:34_

---

_Comment by @callegar on 2024-10-04 15:02_

May I suggest the PDM model? That is nice IMHO. Via a config variable you decide if you want venvs to be generally managed centrally or locally. When they are centrally managed, PDM sorts out things nicely in an almost automatic way.

---

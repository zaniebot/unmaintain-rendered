```yaml
number: 13692
title: "Ruff incorrectly matches .gitignore rules with sub-directories and has issues with `respect-gitignore = false`"
type: issue
state: closed
author: mihnatenko
labels:
  - bug
  - cli
assignees: []
created_at: 2024-10-09T15:08:31Z
updated_at: 2025-12-31T17:45:08Z
url: https://github.com/astral-sh/ruff/issues/13692
synced_at: 2026-01-10T11:09:55Z
```

# Ruff incorrectly matches .gitignore rules with sub-directories and has issues with `respect-gitignore = false`

---

_Issue opened by @mihnatenko on 2024-10-09 15:08_

Recently I have faced two issues with ruff. And they might not be that tightly related, but the description for the first one serves as a context for the second one, so I have combined them into a single issue. Let me know if it would be better to separate them.

Here is a minimal reproducible example I was able to prepare.
1. Initialize a git repository
```sh
cd ruff-ignore-subdir-bug
git init
```
2. Create `.gitignore` file with the following content:
```
/backend/log
```
3. Locate files and directories with the following structure:
```bash
mkdir -p backend/log
mkdir -p backend/src/subdir/log
touch backend/pyproject.toml
touch backend/log/example.py
touch backend/src/subdir/log/some_logging_lib.py
# the resulting structure should look like this:
tree
.
└── backend
    ├── log
    │   └── example.py
    ├── pyproject.toml
    └── src
        └── subdir
            └── log
                └── some_logging_lib.py

6 directories, 3 files
```
Contents of backend/pyproject.toml:
```toml
[tool.ruff.lint]
select = ["D"]
ignore = ["D203", "D212"]
```

Contents of backend/log/example.py:
```python
# just for example purposes of what ruff linter finds. This file is not that important,
# as in our project, we store logs in backend/log


def please_ignore_me(arg):
    return arg * 2
```

Contents of backend/src/subdir/log/some_logging_lib.py:
```python
def do_not_ignore_me(*args, **kwargs):
    print("got: ", args, kwargs)
```

### 1. Issue with ignore rule and subdir
Ruff mistakenly ignores backend/src/subdir/log, while it shouldn't.

Actual behavior:
```bash
ruff --version
ruff 0.6.9

ruff check --output-format=concise backend
All checks passed!

cd backend

ruff check --output-format=concise
All checks passed!

```

Expected behavior:
```bash
ruff check --output-format=concise backend
backend/src/subdir/log/some_logging_lib.py:1:1: D100 Missing docstring in public module
backend/src/subdir/log/some_logging_lib.py:1:5: D103 Missing docstring in public function
Found 2 errors.

cd backend

ruff check --output-format=concise
src/subdir/log/some_logging_lib.py:1:1: D100 Missing docstring in public module
src/subdir/log/some_logging_lib.py:1:5: D103 Missing docstring in public function
Found 2 errors.

```

Here are two log entries I was able to see when added `--verbose` flag:
```
[2024-10-09][16:25:25][ignore::walk][DEBUG] ignoring /tmp/ruff-ignore-subdir-bug/backend/log: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/ruff-ignore-subdir-bug/.gitignore"), original: "/backend/log", actual: "backend/log", is_whitelist: false, is_only_dir: false })))
[2024-10-09][16:25:25][ignore::walk][DEBUG] ignoring /tmp/ruff-ignore-subdir-bug/backend/src/subdir/log: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/ruff-ignore-subdir-bug/.gitignore"), original: "/backend/log", actual: "backend/log", is_whitelist: false, is_only_dir: false })))
```
Also, I might be not that familiar with rust, but I checked Cargo.toml file, and I was able to find that you are using ignore crate, which is a part of ripgrep repository. So this issue might be related: https://github.com/BurntSushi/ripgrep/issues/2778

### 2. Ruff doesn't apply `respect-gitignore = false` when pyproject.toml is not in the same directory where you run command

I can't tell too much details about the real project where I faced this issue, but we have our pyproject.toml located inside of backend directory and not in the root, since there are other directories and pieces of functionality, which also might contain python code, and they are not using ruff linter. So running linter from the root of repository without arguments will produce a lot of errors. That's why I run commands like `ruff check backend` or `cd backend && ruff check`, but not `ruff check` from root.

So once I have faced the issue above with .gitignore and sub-directories, I have added the following to the backend/pyproject.toml file:
```toml
[tool.ruff]
respect-gitignore = false
```
And here is what it shows.

Actual behavior:
```bash
ruff check --output-format=concise backend
All checks passed!

cd backend

ruff check --output-format=concise
log/example.py:1:1: D100 Missing docstring in public module
log/example.py:5:5: D103 Missing docstring in public function
src/subdir/log/some_logging_lib.py:1:1: D100 Missing docstring in public module
src/subdir/log/some_logging_lib.py:1:5: D103 Missing docstring in public function
Found 4 errors.
```

Expected behavior:
🤷‍♂️ 😄 
I don't know whether it's a correct expectation, but since ruff already applies lint rules to backend/ and it's sub-directories, listed in backend/pyproject.toml, it feels logical that it should apply this `respect-gitignore` setting as well.

We don't have a configured git hooks via pre-commit, or something similar, so I have added the following pre-commit hook `ruff check backend` for myself, since ruff is fast enough to do that (thank you so much for that, btw). And once I have found that there might be issues with ignoring sub-directories, I changed the command to be `ruff check --no-respect-gitignore backend`.

---

_Comment by @MichaReiser on 2024-10-13 15:24_

Hi @mihnatenko 

> Ruff mistakenly ignores backend/src/subdir/log, while it shouldn't.

Can you explain me why you think that Ruff shouldn't ignore the file? What I understand from the description is that the file gets ignored because you ignored the entire `backend/log`by listing it in the `.gitignore` file.


>  I don't know whether it's a correct expectation, but since ruff already applies lint rules to backend/ and it's sub-directories, listed in backend/pyproject.toml, it feels logical that it should apply this respect-gitignore setting as well.

The `respect-gitignore` setting is special because it changes how Ruff traverses the directories and how it finds relevant files and configuration. E.g. the `backend` folder could be gitignored, in which case it shouldn't visit the directory at all. 

That's why the `respect-gitignore` is only respected in the root configuration. The root configuration is the closest configuration of the current working directory, only considering the files in the directory itself or in any ancestor directory. 

You could consider adding a `ruff check --config backend/pyproject.toml` alias to your project (e.g. an npm script, just file, ...)

---

_Label `question` added by @MichaReiser on 2024-10-13 15:24_

---

_Comment by @mihnatenko on 2024-10-14 10:52_

Hello @MichaReiser 
> Can you explain me why you think that Ruff shouldn't ignore the file?

Since git doesn't ignore it. You can verify that by ~repeating steps in that reproducible example I prepared above~ Ok, I have prepared even smaller reproducible example, so that you can simply copy-paste it and run from a single input.
```sh
mkdir ruff-ignore-subdir-bug && cd ruff-ignore-subdir-bug
git init
echo "backend/log" > .gitignore
mkdir -p backend/log
mkdir -p backend/src/subdir/log
echo "# no docstring, but should be ignored" > backend/log/ignore_me.py
echo "# no docstring, should NOT be ignored" > backend/src/subdir/log/do_not_ignore_me.py

```

Git does ignore `backend/log` and all of it's contents, but doesn't ignore `backend/src/subdir/log`. You can verify that by running:

```sh
/tmp/ruff-ignore-subdir-bug ❯ git add .gitignore backend/
/tmp/ruff-ignore-subdir-bug ❯ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   .gitignore
	new file:   backend/src/subdir/log/do_not_ignore_me.py
```

And this in my opinion is expected behavior, since if you would need to ignore any arbitrarily nested `log` directory inside of `backend`, you would add `backend/**/log` to .gitignore, not `backend/log`.

But ruff ignores both `backend/log` and `backend/src/subdir/log`:
```sh
/tmp/ruff-ignore-subdir-bug ❯ ruff check --select=D100 backend
warning: No Python files found under the given path(s)
All checks passed!

/tmp/ruff-ignore-subdir-bug ❯ cd backend

/tmp/ruff-ignore-subdir-bug/backend ❯ ruff check --select=D100
warning: No Python files found under the given path(s)
All checks passed!
```

So does ripgrep. But ripgrep does that in a different manner. It works correctly when executed from root of the repository, but incorrectly when executed from backend:
```sh
/tmp/ruff-ignore-subdir-bug ❯ rg "be ignored"
backend/src/subdir/log/do_not_ignore_me.py
1:# no docstring, so should NOT be ignored

/tmp/ruff-ignore-subdir-bug ❯ cd backend

/tmp/ruff-ignore-subdir-bug/backend ❯ rg "be ignored"
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

I presume both ruff and ripgrep do that incorrectly, since they use the same `ignore` crate, and the same issue, described here, applies to them: https://github.com/BurntSushi/ripgrep/issues/2778

---

_Comment by @MichaReiser on 2024-10-14 12:11_

Thanks for the example.

I tried reproducing the problem you're mentioning but running ruff in the directory when enabling the pydocstyle rules does flag the missing docstring in `do_not_ignore_me.py`

```bash
uvx ruff check . -v --select D
[2024-10-14][14:10:15][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/micha/astral/test/pyproject.toml
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
[2024-10-14][14:10:15][ignore::gitignore][DEBUG] opened gitignore file: /home/micha/.gitignore
[2024-10-14][14:10:15][ignore::gitignore][DEBUG] opened gitignore file: /home/micha/.gitignore
[2024-10-14][14:10:15][ignore::gitignore][DEBUG] opened gitignore file: /home/micha/astral/test/.git/info/exclude
[2024-10-14][14:10:15][ignore::gitignore][DEBUG] opened gitignore file: /home/micha/astral/test/ruff-ignore-subdir-bug/.gitignore
[2024-10-14][14:10:15][ignore::gitignore][DEBUG] opened gitignore file: /home/micha/astral/test/ruff-ignore-subdir-bug/.git/info/exclude
[2024-10-14][14:10:15][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/micha/astral/test/ruff-ignore-subdir-bug/.git"
[2024-10-14][14:10:15][ignore::walk][DEBUG] ignoring /home/micha/astral/test/ruff-ignore-subdir-bug/backend/log: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/micha/astral/test/ruff-ignore-subdir-bug/.gitignore"), original: "backend/log", actual: "backend/log", is_whitelist: false, is_only_dir: false })))
[2024-10-14][14:10:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/micha/astral/test/ruff-ignore-subdir-bug/backend/src/subdir/log/do_not_ignore_me.py"
[2024-10-14][14:10:15][ruff::commands::check][DEBUG] Identified files to lint in: 5.132392ms
[2024-10-14][14:10:15][ruff::diagnostics][DEBUG] Checking: /home/micha/astral/test/ruff-ignore-subdir-bug/backend/src/subdir/log/do_not_ignore_me.py
[2024-10-14][14:10:15][ruff::commands::check][DEBUG] Checked 1 files in: 388.274µs
backend/src/subdir/log/do_not_ignore_me.py:1:1: D100 Missing docstring in public module
Found 1 error.

```

---

_Comment by @mihnatenko on 2024-10-15 08:17_

Please try `uvx ruff check backend -v --select D`

As I mentioned above, the reason we are running ruff only for backend is because we have another pieces of functionality, written in python (behave tests, to be more precise). We are not managing their code and they didn't switch to ruff yet.

So calling `ruff check` from root will produce a lot of errors, which are not applicable to backend. So we are either running `ruff check backend`, or `cd backend` and then `ruff check`.

---

_Label `bug` added by @MichaReiser on 2024-10-15 08:31_

---

_Label `question` removed by @MichaReiser on 2024-10-15 08:31_

---

_Label `cli` added by @MichaReiser on 2024-10-15 08:32_

---

_Comment by @brandoncazander on 2025-01-03 16:37_

Just wanted to say that I was bit by this as well, in the same manner, reproducible with a small git repo (using ruff 0.8.5):

```bash
mkdir /tmp/ruff-repro
cd /tmp/ruff-repro
git init
cat <<EOF | git apply -
diff --git a/.gitignore b/.gitignore
new file mode 100644
index 0000000..f189725
--- /dev/null
+++ b/.gitignore
@@ -0,0 +1 @@
+web/__init__.py
diff --git a/web/myapp/__init__.py b/web/myapp/__init__.py
new file mode 100644
index 0000000..e69de29
diff --git a/web/myapp/models/__init__.py b/web/myapp/models/__init__.py
new file mode 100644
index 0000000..da5fac8
--- /dev/null
+++ b/web/myapp/models/__init__.py
@@ -0,0 +1 @@
+from foo import *
diff --git a/web/myapp/models/foo.py b/web/myapp/models/foo.py
new file mode 100644
index 0000000..e69de29
EOF
```

Running `ruff check .` yields the expected F403 violation in `web/myapp/models/__init__.py`:

```bash
$ ruff --verbose check .
[2025-01-03][08:29:35][ruff::resolve][DEBUG] Using Ruff default settings
[2025-01-03][08:29:35][ignore::gitignore][DEBUG] opened gitignore file: /home/brandon.cazander@bruker.com/.config/git/.gitignore
[2025-01-03][08:29:35][ignore::gitignore][DEBUG] opened gitignore file: /tmp/foo/.gitignore
[2025-01-03][08:29:35][ignore::gitignore][DEBUG] opened gitignore file: /tmp/foo/.git/info/exclude
[2025-01-03][08:29:35][ignore::walk][DEBUG] ignoring /tmp/foo/.ruff_cache: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/brandon.cazander@bruker.com/.config/git/.gitignore"), original: ".ruff_cache", actual: "**/.ruff_cache", is_whitelist: false, is_only_dir: false })))
[2025-01-03][08:29:35][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/tmp/foo/.git"
[2025-01-03][08:29:35][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/foo/web/myapp/__init__.py"
[2025-01-03][08:29:35][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/foo/web/myapp/models/__init__.py"
[2025-01-03][08:29:35][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/foo/web/myapp/models/foo.py"
[2025-01-03][08:29:35][ruff::commands::check][DEBUG] Identified files to lint in: 7.007262ms
[2025-01-03][08:29:35][ruff::commands::check][DEBUG] Checked 3 files in: 195.033µs
web/myapp/models/__init__.py:1:1: F403 `from foo import *` used; unable to detect undefined names
  |
1 | from foo import *
  | ^^^^^^^^^^^^^^^^^ F403
  |

Found 1 error.
```

But `ruff check web` does not:

```bash
$ ruff --verbose check web
[2025-01-03][08:29:37][ruff::resolve][DEBUG] Using Ruff default settings
[2025-01-03][08:29:37][ignore::gitignore][DEBUG] opened gitignore file: /home/brandon.cazander@bruker.com/.config/git/.gitignore
[2025-01-03][08:29:37][ignore::gitignore][DEBUG] opened gitignore file: /tmp/foo/.gitignore
[2025-01-03][08:29:37][ignore::gitignore][DEBUG] opened gitignore file: /tmp/foo/.git/info/exclude
[2025-01-03][08:29:37][ignore::walk][DEBUG] ignoring /tmp/foo/web/myapp/__init__.py: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/foo/.gitignore"), original: "web/__init__.py", actual: "web/__init__.py", is_whitelist: false, is_only_dir: false })))
[2025-01-03][08:29:37][ignore::walk][DEBUG] ignoring /tmp/foo/web/myapp/models/__init__.py: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/tmp/foo/.gitignore"), original: "web/__init__.py", actual: "web/__init__.py", is_whitelist: false, is_only_dir: false })))
[2025-01-03][08:29:37][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/foo/web/myapp/models/foo.py"
[2025-01-03][08:29:37][ruff::commands::check][DEBUG] Identified files to lint in: 3.593057ms
[2025-01-03][08:29:37][ruff::commands::check][DEBUG] Checked 1 files in: 43.404µs
All checks passed!
```

---

Even more perplexing to me is that if I repeatedly run `ruff --verbose check --no-cache web`, it will sometimes work (catching F403 despite the `.gitignore` exclude behavior). When this happens, I can see in the verbose logs that it's still reading the `.gitignore` file, but never logs about matching on the specific path.

However, this only happens in my main repository and I haven't been able to reproduce this inconsistent behavior in the toy repo example above.

---

_Comment by @MichaReiser on 2025-06-18 12:55_

Running ruff at the root of the repro now seems to work. The issue really is when running it in the `backend` directory. It then incorrectly matches `/Users/micha/astral/test/ignore-bug/backend/src/subdir/log` against `Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/micha/astral/test/ignore-bug/.gitignore"), original: "/backend/log", actual: "backend/log", is_whitelist: false, is_only_dir: false })))`. Which is somewhat surprising but that seems like a bug in the ignore crate. But not sure where to start looking :) @BurntSushi any ideas?

---

_Comment by @BurntSushi on 2025-06-18 14:42_

Yeah I think the bug linked in the OP here is probably the right one: https://github.com/BurntSushi/ripgrep/issues/2778

Looks like it might be fixed by this PR: https://github.com/BurntSushi/ripgrep/pull/2933

So I think I just need to get that merged and released. :-)

---

_Comment by @ntBre on 2025-12-31 17:45_

I believe this issue was resolved by our upgrade to ignore 0.4.24 in https://github.com/astral-sh/ruff/pull/20979 and released in Ruff 0.14.3. Using the original example:

```
$ tree
.
└── backend
    ├── log
    │   └── example.py
    ├── pyproject.toml
    └── src
        └── subdir
            └── log
                └── some_logging_lib.py

6 directories, 3 files
$ uvx ruff@0.14.2 check --output-format=concise backend
All checks passed!
$ uvx ruff@0.14.3 check --output-format=concise backend
backend/src/subdir/log/some_logging_lib.py:1:1: D100 Missing docstring in public module
backend/src/subdir/log/some_logging_lib.py:1:5: D103 Missing docstring in public function
Found 2 errors.
$ cd backend
$ uvx ruff@0.14.2 check --output-format=concise
All checks passed!
$ uvx ruff@0.14.3 check --output-format=concise
src/subdir/log/some_logging_lib.py:1:1: D100 Missing docstring in public module
src/subdir/log/some_logging_lib.py:1:5: D103 Missing docstring in public function
Found 2 errors.
```

which I believe is the expected behavior!



---

_Closed by @ntBre on 2025-12-31 17:45_

---

---
number: 12250
title: Linter and formatter considers unrecognized file extension as Python files
type: issue
state: closed
author: dhruvmanila
labels:
  - wontfix
assignees: []
created_at: 2024-07-09T03:34:07Z
updated_at: 2024-07-11T04:34:26Z
url: https://github.com/astral-sh/ruff/issues/12250
synced_at: 2026-01-07T13:12:15-06:00
---

# Linter and formatter considers unrecognized file extension as Python files

---

_Issue opened by @dhruvmanila on 2024-07-09 03:34_

Linter:
```console
❯ ruff check --extend-select=Q biome.json
biome.json:1:2: Q000 [*] Double quotes found but single quotes preferred
  |
1 | {"$schema": "https://biomejs.dev/schemas/1.8.2/schema.json"}
  |  ^^^^^^^^^ Q000
  |
  = help: Replace double quotes with single quotes

biome.json:1:13: Q000 [*] Double quotes found but single quotes preferred
  |
1 | {"$schema": "https://biomejs.dev/schemas/1.8.2/schema.json"}
  |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Q000
  |
  = help: Replace double quotes with single quotes

Found 2 errors.
[*] 2 fixable with the `--fix` option.
```

Formatter:
```console
❯ ruff format biome.json 
1 file reformatted
```

The root cause of this is that `PySourceType::from_extension` falls back to `Python` source type if the extension isn't recognized. But, we have the `--extension` flag to allow users to map any file extension to a Python file so we should use an `Option<PySourceType>` return type instead.

---

_Label `bug` added by @dhruvmanila on 2024-07-09 03:34_

---

_Comment by @samamorgan on 2024-07-10 00:11_

Is there an intermediate workaround for this?

---

_Comment by @dhruvmanila on 2024-07-10 05:46_

@samamorgan Can you describe your workflow where you're facing this issue? Is it in an editor context? If so, are you using `ruff-lsp` or the new native server (`ruff server`)? Are you facing this issue via the command-line?

---

_Comment by @AlexWaygood on 2024-07-10 12:37_

Strictly speaking, I think this behaviour might be... correct? If you pass a `.json` file directly to the Python interpreter and ask it to parse it as Python, the interpreter will happily do so, so it makes sense to me that Ruff would do the same if we passed it a `.json` file directly:

```
~/dev/experiment % cat script.json
print("Hello, world!")
~/dev/experiment % python script.json
Hello, world!
```

Although it's obviously not very common for people to save their Python scripts with `.json` extensions, it's not uncommon for people to save single-file scripts without any extension at all. E.g. here's a whole folder of them in a pretty popular project: https://github.com/asottile/reorder-python-imports/tree/main/testing

---

_Comment by @charliermarsh on 2024-07-10 15:17_

I think this is correct (or at least intentional). If you pass a file _directly_ to Ruff, we will always try to analyze it. You could probably add `*.json` to `extend-exclude` and then run with `--force-exclude` if you don't want it.

---

_Comment by @samamorgan on 2024-07-10 18:56_

> @samamorgan Can you describe your workflow where you're facing this issue? Is it in an editor context? If so, are you using `ruff-lsp` or the new native server (`ruff server`)? Are you facing this issue via the command-line?

I'm trying to update my project from `ruff` 0.2.2 -> 0.5.1 and have added a pre-commit hook to run `ruff format`. I hadn't fully thought it through, and needed to add a `files` pattern for pre-commit, since it's running the hook against all committed files.

My pattern that seems to do the trick: `"\\.(py|pyi|ipynb)$"`

---

_Comment by @charliermarsh on 2024-07-10 19:19_

I think pre-commit should only be passing Python files? You can also set `types_or: [ python, pyi, jupyter ]`

---

_Comment by @dhruvmanila on 2024-07-11 04:33_

Pre-commit docs reference: https://pre-commit.com/#filtering-files-with-types

Yeah, I think this behavior makes sense. I'll close this issue. 

---

_Closed by @dhruvmanila on 2024-07-11 04:33_

---

_Label `bug` removed by @dhruvmanila on 2024-07-11 04:34_

---

_Label `wontfix` added by @dhruvmanila on 2024-07-11 04:34_

---

_Referenced in [astral-sh/ruff#12318](../../astral-sh/ruff/pulls/12318.md) on 2024-07-14 13:58_

---

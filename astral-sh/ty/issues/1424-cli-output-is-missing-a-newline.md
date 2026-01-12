```yaml
number: 1424
title: cli output is missing a newline
type: issue
state: closed
author: zilto
labels:
  - cli
assignees: []
created_at: 2025-10-23T17:18:49Z
updated_at: 2025-10-24T15:35:24Z
url: https://github.com/astral-sh/ty/issues/1424
synced_at: 2026-01-12T15:54:25Z
```

# cli output is missing a newline

---

_@zilto_

### Summary

When running `ty check` inside a `pre-commit` or `prek` hook, the progress bar is caught by the logs. This problem is worsed when running ty via pre-commit inside a GitHub actions because the progress bar refresh adds log lines. It's also missing a new line after the `9/9 files` (end of progress bar). 

The command output renders correctly in a terminal but not inside pre commits

For example
```
ty-check.................................................................Failed
- hook id: ty-check
- exit code: 1
  Uninstalled 2 packages in 19ms
Installed 4 packages in 6ms
  WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 9/9 files path/to/file:17:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Foo | ((...) -> Any)] | None`, found `tuple[FunctionType, ...]`
  Found 1 diagnostic
error: Recipe `type-check` failed on line 20 with exit code 1
``` 

### Version

ty 0.0.1-alpha.19

---

_Label `cli` added by @AlexWaygood on 2025-10-23 17:19_

---

_Comment by @MichaReiser on 2025-10-23 17:20_

We should probably not show a progress when running ty inside pre-commit

---

_Comment by @AlexWaygood on 2025-10-23 17:28_

I don't think this is an issue specific to pre-commit. I think OP here is right that something's funny with how we're rendering diagnostics currently. See for example the way the primary diagnostic appears on the same line as the progress bar in https://github.com/astral-sh/ty/issues/1425#issue-3545768233 -- you have to scroll a long way to the right before you see the first line of the diagnostic.

https://github.com/user-attachments/assets/4a0a8bca-9085-465c-8a84-fbdfda37ccca

I've noticed this myself when copying and pasting diagnostics into GitHub issues recently. Not sure what's up here, as things look fine when I execute ty directly from the terminal, but what is rendered as a newline on the terminal doesn't seem to be rendered as a newline when you copy-and-paste it elsewhere

---

_Comment by @MichaReiser on 2025-10-23 17:38_

I'm not sure if those are related. I also can't reproduce this

```
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                                                                                                                                                                                                                  warning[undefined-reveal]: `reveal_type` used without importing it
 --> main.py:5:1
  |
3 |         self.x = (other.x, 1)
4 |
5 | reveal_type(C().x)  # revealed: Unknown | tuple[Divergent, Literal[1]]
  | ^^^^^^^^^^^
  |
info: This is allowed for debugging convenience but will fail at runtime
info: rule `undefined-reveal` is enabled by default

info[revealed-type]: Revealed type
 --> main.py:5:13
  |
3 |         self.x = (other.x, 1)
4 |
5 | reveal_type(C().x)  # revealed: Unknown | tuple[Divergent, Literal[1]]
  |             ^^^^^ `Unknown | tuple[Divergent, Literal[1]]`
  |

Found 2 diagnostics
```

copies just fine

---

_Comment by @AlexWaygood on 2025-10-23 17:40_

> I also can't reproduce this

what do you mean? The example you copied and pasted _does_ reproduce this 😆

https://github.com/user-attachments/assets/5969194d-8980-454b-acb6-6d3ffdb35f5c

---

_Comment by @MichaReiser on 2025-10-23 17:44_

<img width="1255" height="899" alt="Image" src="https://github.com/user-attachments/assets/21426f30-1bf3-4410-af7e-3403a9975d04" />

Uhm, this could be a github issue?

but it's possible that we need to render a newline somewhere

---

_Comment by @AlexWaygood on 2025-10-23 17:46_

it _might_ be a different issue, but I think it may well be related to the pre-commit issue that the OP is reported, because the newline is missing in the same place that OP reported it being missing here (between the progress bar and the first line of the diagnostic). This is the relevant line from the pre-commit output that OP pasted:

> ```
> Checking ------------------------------------------------------------ 9/9 files path/to/file:17:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Foo | ((...) -> Any)] | None`, found `tuple[FunctionType, ...]`
> ```

---

_Comment by @zilto on 2025-10-23 19:32_

I have no idea about the internals of `ty` and how the CLI works. My humble guess is:
1. progress bar is drawn once
2. progress bar updates redraw the same line until completion
3. diagnostic is printed

step 2 should probably add a `newline` after the last update showing `..... n/n files`.

Also, if that helps, I'm on Ubuntu and using `zsh`

---

_Comment by @AlexWaygood on 2025-10-23 19:43_

> Uhm, this could be a github issue?

Copying and pasting diagnostics from the terminal into https://www.soscisurvey.de/tools/view-chars.php also reports that there is no newline in between the progress bar and the first line of the diagnostic, FWIW 

---

_Comment by @zilto on 2025-10-23 19:48_

Another hint that there are 2 mechanisms writing to the terminal output is that redirecting the results to `logs.txt` shows the progress bar in the terminal but not the logs

```shell
❯ ty check --output-format concise . > logs.txt
# output following directly the command on the next line
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 9/9 files     
```

```txt
# logs.txt
path/to/file.py:17:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[foo | ((...) -> Any)] | None`, found `tuple[FunctionType, ...]`
Found 1 diagnostic
```

I'm guessing the progress bar is "printed" and the diagnostic is "output". In the context of pre-commit, they're both redirected to the same place and the lack of newline between the two causes the issue

---

_Comment by @MichaReiser on 2025-10-24 08:13_

https://github.com/console-rs/indicatif/issues/695

---

_Comment by @zilto on 2025-10-24 13:19_

@MichaReiser Thanks for sharing. I commented there too. 

Currently, the progress bar has 2 issues:
- each redraw adds a line to GitHub Actions logs
- the last line of the progress bar doesn't use `newline` before returning diagnostics

In the meantime, would it be possible to add a flag to disable the progress bar? `--no-progress` is seen in `uv`

---

_Comment by @MichaReiser on 2025-10-24 13:31_

A `--no-progress` option is certainly somewhing we can do but is probably worth tracking separately

Edit, see https://github.com/astral-sh/ruff/pull/21063

---

_Closed by @MichaReiser on 2025-10-24 15:35_

---

---
number: 8239
title: "Formatter: use aliases for format command"
type: issue
state: closed
author: savioxavier
labels:
  - formatter
  - wish
assignees: []
created_at: 2023-10-26T03:33:33Z
updated_at: 2023-10-27T03:50:18Z
url: https://github.com/astral-sh/ruff/issues/8239
synced_at: 2026-01-10T01:22:48Z
---

# Formatter: use aliases for format command

---

_Issue opened by @savioxavier on 2023-10-26 03:33_


Consider adding aliases for the new formatter feature. For instance, `fmt` is a commonly used alias for "format code"

That is, `ruff fmt` will work exactly like `ruff format`. I could perhaps make a shell alias for this, but `ruff fmt` just seems cleaner.

Current output (using ruff 0.1.2):


```
❯ ruff fmt .
fmt:1:1: E902 The system cannot find the file specified. (os error 2)
main.py:3:24: F401 [*] `rich.align.Align` imported but unused
main.py:4:25: F401 [*] `rich.layout.Layout` imported but unused
main.py:5:1: E401 Multiple imports on one line
main.py:5:12: F401 [*] `sys` imported but unused
Found 5 errors.
[*] 3 fixable with the `--fix` option.

❯ ruff format .                                                                                                  
1 file reformatted
```

---

_Label `formatter` added by @MichaReiser on 2023-10-26 03:40_

---

_Label `wish` added by @MichaReiser on 2023-10-26 03:40_

---

_Comment by @MichaReiser on 2023-10-26 03:42_

Hy. Thanks for your feedback. 

I prefer if a tool has exactly one way to accomplish a goal. Having multiple names results in users finding different examples in blog posts, documentation, examples etc. without knowing if it means the same or not. This is just my opinion. I wonder how other people feel about this.

---

_Closed by @MichaReiser on 2023-10-27 02:02_

---

_Comment by @savioxavier on 2023-10-27 03:50_

No problem! I can understand your argument and it seems pretty reasonable.

If anyone is still looking to implement this feature, you can accomplish the same using a custom bash function:

```sh
function better_ruff() {
    if [[ "$1" == "fmt" ]]; then
        shift
        ruff format "$@"
    else
        ruff "$@"
    fi
}

alias ruff="better_ruff"
```

Add the above to `.bashrc`/`.zshrc`, restart your shell, run `ruff fmt .` and it should work.

Thanks for taking a look into this issue, and I always appreciate the work that you guys continue to do!

---

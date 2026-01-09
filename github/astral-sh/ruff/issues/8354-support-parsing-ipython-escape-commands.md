---
number: 8354
title: Support parsing IPython escape commands
type: issue
state: closed
author: maximlt
labels:
  - parser
assignees: []
created_at: 2023-10-30T11:24:08Z
updated_at: 2024-01-16T20:02:25Z
url: https://github.com/astral-sh/ruff/issues/8354
synced_at: 2026-01-07T13:12:15-06:00
---

# Support parsing IPython escape commands

---

_Issue opened by @maximlt on 2023-10-30 11:24_

With ruff 0.1.3, running `ruff check test.ipynb` on a notebook whose content is:

```python
%time x = 2
print(x)
```

returns:

```
test.ipynb:cell 2:1:7: F821 Undefined name `x`
Found 1 error.
```

While I would expect no error to be found. It seems the IPython `%time` magic isn't supported. I haven't tested other magics.

---

_Label `parser` added by @konstin on 2023-10-30 13:24_

---

_Comment by @konstin on 2023-10-30 13:25_

Currently, we don't yet parse the contents of ipython, we merely ignore them.

---

_Comment by @charliermarsh on 2023-10-30 13:26_

@dhruvmanila - What would it take for us to selectively support common magics, i.e., parse the bodies?

---

_Comment by @dhruvmanila on 2023-10-31 03:27_

Related https://github.com/astral-sh/ruff/issues/8094. I'll close that one in favor of this and bring any relevant information here.

I'm a bit hesitant to support parsing magics as there's no clear grammar for it. There are other reasons as well which I'll outline here.

### Magic arguments

Each magic command can have any number of CLI type of arguments which needs to be skipped as they aren't valid Python syntax. For example, the `%timeit` [documentation](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit) usage is:

```
%timeit [-n<N> -r<R> [-t|-c] -q -p<P> -o] statement
```

But then how do we differentiate between a CLI type of argument and an actual Python code. That would probably require having an understanding of the supported magic commands and basically adding the logic for parsing such commands in Ruff.

### Syntax

The linked issue (https://github.com/astral-sh/ruff/issues/8094) provides a real world example for a false positive. I've narrowed it down to the following lines:

```python
versions = {'Latest': 1, 'v7.2-112': 2, 'v6': 3}
# ^^^^^^ F841 unused variable
!sed -i '/^GPUOWL_PRP_PM1=/c\GPUOWL_PRP_PM1={versions[gpuowl_prp_pm1]}' '{file}'
#                                            ^^^^^^^^ used here
```

Here, the warning we raise is that the `versions` variable is defined but unused (`F841`). The problem here is that the variable is used in bash syntax and not Python.

That said, if we do decide to parse the escape commands, the following would be required:
1. Understanding the said magic command (in case of `%`), it's CLI type arguments and the position of Python code in the command itself
2. Integrating a shell parser (in case of `!`), updating our AST structure to include that.

Even in the case of (2), we would further need to add an ability to parse arguments for the shell commands (see the above `sed` example).

---

_Label `needs-decision` added by @dhruvmanila on 2023-10-31 03:27_

---

_Referenced in [astral-sh/ruff#8094](../../astral-sh/ruff/issues/8094.md) on 2023-10-31 03:28_

---

_Renamed from "IPython `%time` magic not supported" to "Support parsing IPython escape commands" by @dhruvmanila on 2023-10-31 03:28_

---

_Comment by @dhruvmanila on 2023-10-31 03:29_

(Converting this into a general issue to discuss parsing IPython escape commands.)

---

_Comment by @tdulcet on 2023-10-31 10:20_

> 2\. Integrating a shell parser (in case of `!`), updating our AST structure to include that.

I am not sure if Ruff would actually need a full shell parser, as least not for that example from my notebook. Ruff would only need to look for matching pairs of curly brackets and check if there is valid Python syntax inside. This would need to be done before running a shell parser anyway, as it could be invalid Bash syntax, especially if it is unquoted. Jupyter seems to be smart enough to silently ignore any curly brackets with invalid Python syntax inside, such as with Bash [brace expansion](https://www.gnu.org/software/bash/manual/html_node/Brace-Expansion.html) or an `awk` command for example.

Supporting my first example in #8094 likely would require a shell parser, but that would only affect a single Ruff rule (`F841`) compared to the bracket syntax that could affect dozens of rules since it can include arbitrary Python expressions. Note that Python does include a (partial) shell parser with the [shlex library](https://docs.python.org/3/library/shlex.html).

---

_Comment by @henryiii on 2023-11-29 16:14_

The simplest starting point for cell magics would be to mark some cell magics as transparent (possibly a customizable list to allow third party magics to be added?). That would allow this:

```ipython
y = 1
%%timeit
x = y
```

to no longer mark y as unused. It's only a first order fix - `%%timeit` doesn't add variables to the environment, it only captures it. So x isn't set to y after this statement. But that's probably a lot better than all the errors that pop up about unused imports and variables; I'm having to deactivate related checks due to this. Some other cell magics, like `%%time`, are truly transparent, they are just like normal code.

I think these are the built-in transparent cell magics (based on looking at the details given by `%magic`):

* `%%capture`
* `%%debug`
* `%%prun` 
* `%%pypy`
* `%%python`
* `%%python3`
* `%%time`
* `%%timeit` (captures environment, but doesn't set new variables)
* `%%writefile`/`%%file` (only with a `.py` extension).

Supporting inline magics seems like a second order issue, and a tricker one, since the arguments are inline with the body. And you can rewrite the above example into a cell magic easily.

---

_Comment by @henryiii on 2023-11-29 17:34_

Also, this is exactly how Black works, I think:

https://black.readthedocs.io/en/latest/usage_and_configuration/the_basics.html#python-cell-magics

It seems to have a pre-built list of magics and that can be extended.

https://github.com/psf/black/blob/a0e270d0f246387202e676b25abbf7a02ddcbc71/src/black/handle_ipynb_magics.py#L35C1-L43

---

_Comment by @charliermarsh on 2023-11-29 17:35_

I'm very interested in supporting this!

---

_Comment by @dhruvmanila on 2023-11-29 20:36_

> The simplest starting point for cell magics would be to mark some cell magics as transparent (possibly a customizable list to allow third party magics to be added?).

Correct me if I'm wrong but by transparent, I'm assuming you mean that the variables / imports defined in that cell are available in the global scope to be accessed by other code in other cells.

> That would allow this:
> 
> ```
> y = 1
> %%timeit
> x = y
> ```
> 
> to no longer mark y as unused.

I see what you mean here. I think this should be trivial to implement. We would just need to update the `is_magic_cell` function to consider the cells as valid if it's one of the listed magics. The parser then should take care of the rest.

https://github.com/astral-sh/ruff/blob/ec7456bac049d1d4f4763f8c7ad764d27dd383bd/crates/ruff_notebook/src/cell.rs#L62-L63

For example, taking the above code:
```rs
Module(
    ModModule {
        range: 0..20,
        body: [
            Assign(
                StmtAssign { .. },  // y = 1
            ),
            IpyEscapeCommand(
                StmtIpyEscapeCommand {
                    range: 6..13,
                    kind: Magic2,
                    value: "timeit",
                },
            ),
            Assign(
                StmtAssign { .. },  // x = y
            ),
        ],
    },
)
```

This means we wouldn't mark `y` as unused then.

---

_Referenced in [astral-sh/ruff#8911](../../astral-sh/ruff/pulls/8911.md) on 2023-11-29 21:26_

---

_Comment by @charliermarsh on 2024-01-03 17:08_

I believe we now support these transparent / simple cases, which is probably the best we can do.

---

_Closed by @charliermarsh on 2024-01-03 17:08_

---

_Label `needs-decision` removed by @dhruvmanila on 2024-01-16 20:02_

---

_Referenced in [astral-sh/ruff#10454](../../astral-sh/ruff/issues/10454.md) on 2024-03-19 08:41_

---

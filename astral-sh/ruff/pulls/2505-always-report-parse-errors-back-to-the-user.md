```yaml
number: 2505
title: Always report parse errors back to the user
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/syntax
created_at: 2023-02-03T00:02:28Z
updated_at: 2023-02-09T23:22:58Z
url: https://github.com/astral-sh/ruff/pull/2505
synced_at: 2026-01-12T04:52:00Z
```

# Always report parse errors back to the user

---

_Pull request opened by @charliermarsh on 2023-02-03 00:02_

# Summary

We treat parse errors as lint errors -- they have their own code (`E999`). If we fail to parse a file, we add an `E999` diagnostic, and move on. Parse errors _are_ recoverable, as many checks don't rely on the AST (like `E501`, line-length enforcement) -- so even if the parse fails, we can still report some other diagnostics.

However, here's a common and confusing workflow: say a user introduces (e.g.) a `match` statement into their code, then runs `ruff --select I /path/to/file.py`.  They'll then wonder why their imports aren't being sorted. We can't parse `match` statements, so under the hood, we're adding an `E999` to the diagnostic list, but it's being suppressed (since `E999` isn't in the `select`), so the user doesn't get any feedback.

This PR modifies the linter to propagate parse errors. They're still included as `E999`, and we still try to find other violations; however, we _also_ log a warning to the console _and_ avoid doing any caching to the file (so the warning appears on every invocation).

Here's an example.

Given these code changes:

![Screen Shot 2023-02-02 at 7 01 54 PM](https://user-images.githubusercontent.com/1309177/216478374-07e43c4e-500e-4d46-acd0-32ddc29284fd.png)

We now yield:

![Screen Shot 2023-02-02 at 7 02 19 PM](https://user-images.githubusercontent.com/1309177/216478400-a01a1f98-e09f-42ca-b84b-b5b4871d6db5.png)

So it's a little redundant, between the warning and the `E999`, but I think it's much clearer.

Closes #2473.


---

_@charliermarsh reviewed on 2023-02-03 00:05_

---

_Review comment by @charliermarsh on `src/linter.rs`:365 on 2023-02-03 00:05_

Having access to the parse error here means that we can also catch errors that _we_ introduced.

---

_Merged by @charliermarsh on 2023-02-03 00:12_

---

_Closed by @charliermarsh on 2023-02-03 00:12_

---

_Branch deleted on 2023-02-03 00:12_

---

_Comment by @gertvdijk on 2023-02-06 13:55_

Hi! I've just upgraded to 0.0.241 from 0.0.240 and seeing this as _new_ error in `--diff` mode. I just wanted to make sure that the following behaviour is as expected.

- 0 exit status code (seems OK to me in `--diff` mode, but just looks odd in output with red `error`-prefixed lines.)
- There seems to be no way to silence this error - having code E999 separate from the syntax error. A little bit of a show-stopper to me.

---

_Comment by @charliermarsh on 2023-02-06 15:00_

@gertvdijk - Hi! We could omit this warning if you run under `-q` (which already omits everything that _isn't_ a lint error). Would that fix this for you?

Curious though - what's the context in which you're running over code consistently that fails to parse?

---

_Comment by @gertvdijk on 2023-02-06 22:36_

> @gertvdijk - Hi! We could omit this warning if you run under `-q` (which already omits everything that _isn't_ a lint error). Would that fix this for you?

Yes, I think I would like that, but I'm afraid that it also hides more/other/future problems. It produced errors in regular mode, but it allowed me start using Ruff in `--diff` mode, until 0.0.241. Ideally, I would like to see a way to silence the "syntax error" error. I am sure the Python interpreter with proper test coverage is more accurate than Ruff on that anyway.

> Curious though - what's the context in which you're running over code consistently that fails to parse?

The infamous #282. 😐

<sup>I just released [a project](https://github.com/gertvdijk/purepythonmilter) last weekend and this started to show suddenly when running the [`run-all-linters` script](https://github.com/gertvdijk/purepythonmilter/blob/f06f004a6ed2162d66893732c8610e12323d5577/run-all-linters#L24-L31) also in diff-mode which used to be OK in 0.0.240.</sup>

---

_Comment by @ashb on 2023-02-08 20:41_

Interestingly this seemed to remove the ability for us to do `#noqa: E999` on match lines which we had as a working pattern before.

---

_Comment by @charliermarsh on 2023-02-08 21:04_

> Interestingly this seemed to remove the ability for us to do #noqa: E999 on match lines which we had as a working pattern before.

The intent is that the behavior is totally unchanged, except that we also show you an `error` message in the console. If it's behaving at all differently, then it's a bug and I should fix it :joy:


---

_Comment by @charliermarsh on 2023-02-08 21:06_

I could add an `--ignore-parse-errors` flag to suppress these. I don't mind doing so, I just worry that it can lead to other confusing situations for users.

---

_Comment by @charliermarsh on 2023-02-08 21:07_

The other option is that we could avoid omitting these if you add `# noqa` to the top of the file.

---

_Comment by @charliermarsh on 2023-02-08 22:25_

To summarize a few options -- and I'd love feedback:

1. Add `--ignore-parse-errors` to `pyproject.toml` and the CLI to always suppress the extra warning message. (The downside here is that you're opting in to the confusing possibility of having a parse error hidden from you and thereby suppressing all other lint errors.)
2. Avoid these messages when `# noqa` is present at the top of the file. (But then you can't get any lint enforcement on those files. Maybe that's fine though.)
3. Only show this message if `E999` is disabled. That way, you can continue to suppress it with a `# noqa: E999` if you want, but we also avoid silently failing.

---

_Comment by @gertvdijk on 2023-02-08 23:19_

Thanks for the thoughts!

I would vote for option 1, if it's not possible to silence it on a block-level (whole match/case block). If you see the chance of making it fail on the use of that flag if there are no parse errors, that would be great. It will then act as a notification that a future Ruff will understand the syntax. (Just like how mypy will complain if a typing-ignore is not needed (anymore).)

Against option 2 for the given reason - it would silence half my project.

Option 3 sounds confusing to me. I think I get both E999 and the syntax error on the match/case block. I would need a `# noqa E999` on that and then I'm back at the error message I came here to complain about. 😅 

---

_Comment by @charliermarsh on 2023-02-08 23:22_

(Sorry, for Option 3, I meant that we'd only show the error if the E999 code wasn't selected at all. So if you ran `ruff /path/to/file.py --select B`, and hit a syntax error, we'd show the warning. If you ran `ruff /path/to/file.py --select B --select E`, we wouldn't show the warning, even if gets suppressed by a `noqa`.)

---

_Comment by @ashb on 2023-02-09 12:21_

Oh yes, I was just confused by the error output when I'd already had the `# noqa: E999` on the line, but didn't look closely at the exit code.

I _think_ I'd vote for a 4th option: don't show the parse errors if the line is decorated with E999 ignore, even if that error isn't selected.

For clarity, my code is:

```
  62   │     if not settings:
  63   │         settings = get_settings()
  64   │     match settings.app_kind:  # noqa: E999
  65   │         case AppKind.hypervisor:
```

And the output I get is

```
❯ poetry run ruff src; echo $?
error: Failed to parse src/laminar/app.py: invalid syntax. Got unexpected token 'settings' at line 64 column 11
0
```

---

_Comment by @charliermarsh on 2023-02-09 23:22_

That's a nice idea. I went ahead and added it. So if you have this:

```py
x = 1

y = 2


match x:  # noqa: E999
    1
```

We'll always suppress that parse error, regardless of whether `E999` is active for the current `ruff` invocation.

---

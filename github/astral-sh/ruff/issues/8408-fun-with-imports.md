---
number: 8408
title: "Fun with imports :)"
type: issue
state: closed
author: vgoklani
labels:
  - question
assignees: []
created_at: 2023-11-01T16:13:09Z
updated_at: 2023-11-14T04:49:26Z
url: https://github.com/astral-sh/ruff/issues/8408
synced_at: 2026-01-07T13:12:15-06:00
---

# Fun with imports :)

---

_Issue opened by @vgoklani on 2023-11-01 16:13_

Hey there,

Thanks for updating Ruff to support the python-black formatter!

I'm using this on Sublime Text 4 via LSP-ruff, and these are my settings:

    {
      "log_debug": true,
      "lsp_format_on_save": true,
      "lsp_code_actions_on_save": {
        "lsp_format_on_save": true,
        "source.lsp_format_on_save": true,
        "source.fixAll": true,
        "source.organizeImports": true,
        "source.applyFormat": true
      },
      "clients": {
        "ruff-lsp": {
          "command": [
            "ruff-lsp"
          ],
          "enabled": false,
          "selector": "source.python",
          "initializationOptions": {
            "settings": {
              "args": [],
            },
          },
        },
      },
    }


There are a few quirks that I just wanted to point out:

1. imports that aren't yet used automatically get removed when I save the file.  

> As I'm writing code, I add import statements, and instinctively hit the save button, only to find that those imports are then removed. I do like how you *organize* the imports, but I don't like how they disappear! Is it possible to add an additional parameter to separate these two cases.

2. When writing f-strings, this case is not being caught by Ruff:

    my_string = f"Hello {data["name"]}"

previously `python-black` would catch the fact that we are using `"` in both the string declaration and the `data` dictionary. Ruff is not catching these cases, which leads to run-time errors.

Thanks for releasing Ruff, looking fowrard to more features :)



---

_Comment by @zanieb on 2023-11-01 16:20_

Thanks for the issue!

1. You should disable [`F401`](https://docs.astral.sh/ruff/rules/unused-import/) when using your IDE i.e. by adding `--ignore F401` to the additional lint command arguments.

2. That's valid syntax in Python 3.12 now (https://docs.python.org/3/whatsnew/3.12.html#whatsnew312-pep701) which we have full support for. If your target Python version is lower, we can probably detect that and include a violation cc @dhruvmanila 

---

_Label `question` added by @zanieb on 2023-11-01 16:21_

---

_Comment by @dhruvmanila on 2023-11-02 04:17_

> previously `python-black` would catch the fact that we are using `"` in both the string declaration and the `data` dictionary. Ruff is not catching these cases, which leads to run-time errors.

Can you expand on how is `python-black` catching this case? Is it throwing an error stating that this syntax isn't supported? I think https://github.com/astral-sh/ruff/issues/6591 would solve your use-case.

---

_Comment by @dhruvmanila on 2023-11-02 04:21_

For (1), do you wish to just disable the fix and allow the editor to highlight unused imports? If so, you can use the [`unfixable`](https://docs.astral.sh/ruff/settings/#unfixable) setting to do that.

For additional context, the organize imports action isn't responsible for removing unused imports. It's the [`F401`](https://docs.astral.sh/ruff/rules/unused-import/) rule whose auto-fix removes them and as @zanieb suggested, you could either disable the rule entirely or just disable the fix.

I hope this helps :)

---

_Comment by @charliermarsh on 2023-11-03 02:05_

Going to close for now, but happy to answer any follow-up questions.

---

_Closed by @charliermarsh on 2023-11-03 02:05_

---

_Comment by @vgoklani on 2023-11-07 12:47_

@dhruvmanila sorry I missed your reply

I used `python-black` in sublime text 4. For case #2, it would highlight the row when either `'` or `"` were used incorrectly as part of the f-string. 

---

_Comment by @dhruvmanila on 2023-11-14 04:49_

> I used `python-black` in sublime text 4. For case #2, it would highlight the row when either `'` or `"` were used incorrectly as part of the f-string.

By incorrectly, do you mean that the outer and inner quotes were the same? The linked issue in my comment should solve this although how do we go about implementing it isn't final yet.

---

```yaml
number: 9361
title: About format imports
type: issue
state: closed
author: Byxs20
labels: []
assignees: []
created_at: 2024-01-02T17:35:45Z
updated_at: 2024-01-02T18:23:21Z
url: https://github.com/astral-sh/ruff/issues/9361
synced_at: 2026-01-12T15:54:49Z
```

# About format imports

---

_@Byxs20_

![image](https://github.com/astral-sh/ruff/assets/64669718/a02559fe-7e81-4921-9e99-b385a8fc3fff)

Why is it that before it was going to format the import in three pieces and now it's suddenly a two piece area, I'm not sure what I set up to cause this.

Now FORMAT is shown below:

![image](https://github.com/astral-sh/ruff/assets/64669718/3883671a-4769-4528-ac5c-6fddf03745e1)


---

_Comment by @charliermarsh on 2024-01-02 17:37_

Most likely it can't find `core` (assuming `core` is a local module, and not a third-party module). Could you share your rough project structure, and your `pyproject.toml` or `ruff.toml` file?

---

_Comment by @Byxs20 on 2024-01-02 17:40_

pyproject.toml
```
line-length = 120

[lint]
select = ["E4", "E7", "E9", "F", "I"]
```

![image](https://github.com/astral-sh/ruff/assets/64669718/3e09d66a-7fa6-40e1-a94f-1727294633d0)


---

_Comment by @Byxs20 on 2024-01-02 17:43_

I'm using the latest version of ruff-vscode. I was using it fine before, but now I'm not sure what's going on? Obviously, everything under `core` is a local package.

---

_Comment by @charliermarsh on 2024-01-02 17:45_

And where is the `pyproject.toml` relative to those files?

---

_Comment by @Byxs20 on 2024-01-02 17:48_

I know the cause of the bug, it's actually the location where the `.ruff.toml` is stored, what's the best way to store this file in an arbitrary location and still recognize the local package of the project being used.

---

_Comment by @charliermarsh on 2024-01-02 17:54_

Where is the `.ruff.toml` being stored? (The above example works fine for me without _any_ `.ruff.toml` file so want to make sure I'm giving you the correct answer.)

---

_Comment by @Byxs20 on 2024-01-02 18:00_

I want him to automatically detect `import` irregularities for me, so I need him to be able to go ahead and auto-recognize them, which is why I went ahead and added an `"I"`, but the problem with adding an `"I"` at the moment is, do I need to add a `.ruff.toml` file underneath all of my projects? Is it possible to add a `select "I"` to the `check` function by default with the `ruff-vscode` command line parameter?

---

_Comment by @charliermarsh on 2024-01-02 18:03_

Yeah you can -- you can add something like this to your `settings.json` in VS Code:

```json
{
  "ruff.lint.args": ["--extend-select", "I"]
}
```

---

_Comment by @Byxs20 on 2024-01-02 18:08_

You solved the dilemma for me, thank you so much for your help, haha, I'm going to bed, `Ruff` works great, today was my first time using it!

---

_Closed by @Byxs20 on 2024-01-02 18:10_

---

_Comment by @charliermarsh on 2024-01-02 18:23_

Awesome, so glad to resolve! Sorry for the trouble.

---

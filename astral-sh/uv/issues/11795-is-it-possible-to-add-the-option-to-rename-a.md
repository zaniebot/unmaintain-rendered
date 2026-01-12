```yaml
number: 11795
title: Is it possible to add the option to rename a project to UV?
type: issue
state: open
author: Wintreist
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-02-26T09:51:15Z
updated_at: 2025-12-31T14:09:31Z
url: https://github.com/astral-sh/uv/issues/11795
synced_at: 2026-01-12T16:00:46Z
```

# Is it possible to add the option to rename a project to UV?

---

_@Wintreist_

### Summary

Now I'm faced with the fact that the name I chose for the project turned out to be unsuitable for me. I'm going to rename the project, and for this I need to change the pyproject.toml file, the src/project_name folder, and the imports in all internal files.
Therefore, I have a question, is it possible to add such "sugar" to UV?

### Example

uv rename new_project_name

---

_Label `enhancement` added by @Wintreist on 2025-02-26 09:51_

---

_Label `wish` added by @zanieb on 2025-02-26 15:28_

---

_Comment by @zanieb on 2025-02-26 15:28_

It's possible, but pretty complicated. We won't be able to tackle this in the short-term.

---

_Comment by @vladyslav-burylov on 2025-02-26 20:01_

@Wintreist why not to use "replace all" in VS Code (whatever)?

1. Change name in pyproject.toml
2. Rename project folder
3. Mass replace regex '^from <old_name>\.$' => 'from <new_name>.'
4. Mass replace regex '^import <old_name>' => 'import <new_name>'


---

_Comment by @Wintreist on 2025-02-26 21:21_

@vladyslav-burylov, I did it. I got it by with even less steps. I changed the name in pyproject.toml, then renamed the folder via F2, and the Python extension replaced the imports with the new one.

that's why I wrote that it's "sugar". I can do without this functionality, but if it is there, it will be convenient

---

_Comment by @xteveg on 2025-03-19 00:37_

I would also like you to develop this functionality; changing names and having to do certain things manually is tedious. I hope you can implement it in the near future.

---

_Comment by @lawsonM525 on 2025-06-12 20:50_

looking forward to this

---

_Comment by @vladyslav-burylov on 2025-06-12 21:02_

> looking forward to this

Try Claude code (paid, sorry). It runs such tasks perfectly. 

---

_Comment by @castillouparela on 2025-09-19 05:14_

I'm also having the same problem integrating the UV library into Databricks, especially with Spark and its parallelization-based system. I'll wait patiently for a solution for this type of platform.

---

_Comment by @zk1989 on 2025-09-26 15:09_

I've just had the same problem - I firstly created a project (thoughtlessly) through PyCharm GUI, then initialized through uv, then after some time decided the name has to be changed to better reflect the code purpose. I'm implementing changes manually, however I'd feel more certain about making all appropriate changes if uv did this for me. Thanks in advance :)

---

_Comment by @DBankx1 on 2025-12-31 14:09_

just had the same problem as well. working on a startup and my friend gave me a better name suggestion lol

---

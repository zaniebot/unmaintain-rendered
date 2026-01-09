---
number: 6471
title: "`uv init` should create a runnable project"
type: issue
state: closed
author: charliermarsh
labels:
  - projects
assignees: []
created_at: 2024-08-22T23:22:44Z
updated_at: 2024-08-27T18:08:11Z
url: https://github.com/astral-sh/uv/issues/6471
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv init` should create a runnable project

---

_Issue opened by @charliermarsh on 2024-08-22 23:22_

I got some feedback on Twitter that it'd be nice if `uv init` created something that you could then `uv run` and immediately see feedback, similar to how `cargo new` gives you a "Hello, world!" application. You shouldn't need to figure out how to change the project to make it runnable, etc.

---

_Label `projects` added by @charliermarsh on 2024-08-22 23:22_

---

_Comment by @charliermarsh on 2024-08-22 23:22_

(Open to feedback, an idea.)

---

_Comment by @zanieb on 2024-08-22 23:23_

Yes please! I wanted this but didn't want to require it for `uv init` to start.

---

_Comment by @hauntsaninja on 2024-08-23 02:29_

This is the perfect opportunity to unleash the puffin:
```
print(r"""
         .-"-.
        /  _uv\_     .---------------.
        \  \__))}> _/  Hello, World!  \
        ,) ." \    \__________________/
       /  (    \
     ,"   )    ;
     )   /     /
   ,/_."`  _.-`
    /_/`"\\___
         `~~~`
""")

```

---

_Comment by @charliermarsh on 2024-08-23 02:29_

This is actually not bad...

---

_Comment by @zanieb on 2024-08-23 12:43_

Wow!

---

_Comment by @suomela on 2024-08-23 12:44_

I think ideally this would be fully compatible with uv's "tools", so that e.g. this works directly without any extra hassle:

- On one computer, I run `uv init foo`, and upload the newly created repository "foo" somewhere on Github (without any manual edits).
- On another computer I run `uv tool install git+https...`, and I have got my newly created command-line tool "foo" installed there, and it directly works: I can run `foo` and it prints out "Hello world!" or something.
- And now naturally I can add further functionality, push to Github, and run `uv tool upgrade` everywhere to get the latest features.

This would give a really low-threshold approach for everyone (including very beginners) to develop their custom command-line tools and install them conveniently on their own computers.

---

_Comment by @epequeno on 2024-08-26 19:49_

I agree that being able to run the project directly after `init` would be a useful feature but in the meantime this is what I'm doing.

`rye` supported `project.scripts` which allowed you to create an alias or command to run your project: https://rye.astral.sh/guide/pyproject/#projectscripts

I might be mistaken but I didn't see anywhere in the `uv` documentation that this feature/config was supported but it seems that it is.

So, after you `init` a project you can add a section to the `pyproject.toml` to create a convenient run command:

```
~ $ mkdir foo
~ $ cd foo/
~/foo $ uv init
Initialized project `foo`
~/foo $ echo -e "\n[project.scripts]\nmain = 'foo:hello'" >> pyproject.toml
~/foo $ uv sync
~/foo $ uv run main
Hello from foo!
```

I'd love to hear if there's a better way to approach this but this works for me.

---

_Comment by @suomela on 2024-08-26 19:56_

Interesting, I didn't realize setting up `[project.scripts]` is enough; I thought I'd have to add `print()` or something in the `hello()` function for it to actually print something.

---

_Referenced in [astral-sh/uv#6689](../../astral-sh/uv/pulls/6689.md) on 2024-08-27 15:33_

---

_Closed by @zanieb on 2024-08-27 18:08_

---

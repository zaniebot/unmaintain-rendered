```yaml
number: 10813
title: How can I install project dependencies from an existing pyproject.toml?
type: issue
state: closed
author: alexted
labels:
  - question
assignees: []
created_at: 2025-01-21T13:16:27Z
updated_at: 2026-01-10T12:29:52Z
url: https://github.com/astral-sh/uv/issues/10813
synced_at: 2026-01-12T16:00:22Z
```

# How can I install project dependencies from an existing pyproject.toml?

---

_@alexted_

The official documentation doesn't clearly explain how to install project dependencies. It's all vague and indirect, with no concrete example/instructions!
How am I supposed to install project dependencies from an existing pyproject.toml? 
Where is that notorious `uv install` command that would install all the project packages?

---

_Comment by @FishAlchemist on 2025-01-21 13:35_

* ``uv pip install ``

https://docs.astral.sh/uv/pip/packages/#installing-packages-from-files
```bash
uv pip install -r pyproject.toml
```
* ``uv sync``

https://docs.astral.sh/uv/concepts/projects/sync/#creating-the-lockfile
If you run it directly as a project, then using uv sync should also work.

---

_Comment by @alexted on 2025-01-21 13:44_

Sorry, but who designed the CLI interface? What does pip have to do with it at all?
If you don’t mind a bit of criticism: the current UX of `uv` is astonishing in the worst possible way - everything is so counter-intuitive and confusing. My goodness, why?

---

_Comment by @FishAlchemist on 2025-01-21 13:53_

Actually, pip(24.3.1) can also install from pyproject.toml.

![Image](https://github.com/user-attachments/assets/3a086cf0-359e-439c-9a72-b6b4c8746beb)
I'm not sure about the UX, but it's definitely one of pip's features.

---

_Comment by @charliermarsh on 2025-01-21 13:59_

It sounds like you're looking for `uv sync`? You're being incredibly rude.

---

_Comment by @alexted on 2025-01-21 14:03_

Sorry for the rudeness, but I get really frustrated with unclear interfaces that are difficult to navigate and hard to figure out.

---

_Comment by @charliermarsh on 2025-01-21 14:06_

- `uv sync` will sync the project dependencies into the `.venv` in the project directory.
- `uv run {command}` will run a command in the project's environment, automatically locking and syncing if anything is out-of-date.
- `uv pip install` (and other `uv pip` commands) exist for compatibility with users that want a pip-style workflow. Commands like `uv sync` and `uv run` are project-centric, but much of the existing Python ecosystem is not. `uv pip` is an alternative interface to `uv sync`, `uv run`, etc. You would typically not use them together.

The project guide walks through adding dependencies, locking, syncing, and running commands: https://docs.astral.sh/uv/guides/projects/.

---

_Closed by @charliermarsh on 2025-01-21 14:06_

---

_Label `question` added by @charliermarsh on 2025-01-21 14:06_

---

_Comment by @alexted on 2025-01-21 14:09_

Thank you!

---

_Comment by @jrosell on 2026-01-10 09:13_

> It sounds like you're looking for `uv sync`? You're being incredibly rude.


To be constructive, I believe that the meaning of sync/build should be clarified to non native speaking users. Installs first time? Installs if not installed? Doesn't uprade? What's the difference?

```
### Syncing the environment

While the environment is synced automatically, it may also be explicitly synced using uv sync:

uv sync 

Syncing the environment manually is especially useful for ensuring your editor has the correct versions of dependencies.
```


---

_Comment by @alexted on 2026-01-10 11:26_

    The tool is functionally capable. The problem is not what it does, but how that functionality is exposed to users.
From a user’s perspective, the interface and documentation expose internal implementation concepts rather than user-level tasks. As a result, performing a basic operation - such as installing project dependencies - requires understanding the system’s internal model instead of invoking a clear, intention-driven command.
A user comes with a simple intent:  
`install project packages`
Instead, they are presented with commands like:  
`uv sync`, `uv add`, `uv run`
    These are internal operations, not task-oriented actions. The interface mirrors how the system works rather than what the user wants to do, forcing the user to infer which internal command matches their goal.
This also breaks established CLI expectations shaped by tools like `pip install`, `npm install`, or `cargo install`, which provide a clear entry point for common workflows. Requiring users to adapt to internal abstractions instead of meeting those expectations increases cognitive load and friction.
This is not a documentation problem. Documentation cannot compensate for a missing abstraction in the interface. If users must read architectural explanations, issues, or source code to perform a basic task, the interface is incomplete.
    Well-known tools demonstrate that this is avoidable. **Git** explicitly separates user-facing *porcelain* commands from internal *plumbing* operations. **Mercurial (hg)** goes even further by aggressively hiding internal concepts behind an intention-driven interface. Both show that implementation complexity does not have to leak into UX.

**In short:**  
The interface exposes internal mechanics instead of user workflows, forcing users to think like system implementers rather than consumers of functionality.


---

_Comment by @alexted on 2026-01-10 11:29_

### Useful Resources

Here are some sources that explore this topic in both academic and practical ways:

- **Donald Norman – Design of Everyday Things**  
  Covers user-centered interaction design. Explains how to create interfaces focused on user goals, not implementation details.  [Link to book](https://www.amazon.com/Design-Everyday-Things-Revised-Expanded/dp/0465050654)

- **Cognitive Load in UX**  
  Explains why additional "mental work" for the user always worsens the experience.  [Article: Cognitive Load Theory in UX](https://www.nngroup.com/articles/cognitive-load/)

- **Principle of Reducing Cognitive Load**  
  Practical approaches for minimizing the number of decisions and complex steps required from the user.  [Article: Reducing Cognitive Load in UI](https://uxdesign.cc/reduce-cognitive-load-in-your-ui-10-practical-tips-1463e57ef9a4)

- **Leaky Abstractions Research**  
  Academic perspective on how leaking implementation details into higher-level layers affects user experience and engineering practices.  [Joel Spolsky – The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)


---

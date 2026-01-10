---
number: 9754
title: Add support for templating and creating projects with uv
type: issue
state: open
author: hellmrf
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-12-09T22:34:34Z
updated_at: 2025-10-14T05:50:49Z
url: https://github.com/astral-sh/uv/issues/9754
synced_at: 2026-01-10T01:24:45Z
---

# Add support for templating and creating projects with uv

---

_Issue opened by @hellmrf on 2024-12-09 22:34_

## Problem Statement

Starting a new project often involves repetitive tasks like configuring project structures, setting up tools, and applying team-specific templates. While these steps are common across projects, they remain manual and error-prone within the `uv` ecosystem. Developers need:

- Predefined templates with standard structures and settings.
- Scaffolding for frameworks (e.g., Django, FastAPI, or React).
- Standardized configurations for CI/CD, linters, or documentation (e.g., `.gitignore`, README, LICENSE).

Existing tools like [`hatch`](https://hatch.pypa.io/latest/cli/reference/#hatch-new) and [`cookiecutter`](https://cookiecutter.readthedocs.io/) address these issues but lack integration with `uv`, leading to fragmented workflows for developers who rely on `uv` as their project manager. `npm`-world tools like [`create-react-app`](https://create-react-app.dev/docs/getting-started#creating-an-app), available via [`npm init`](https://docs.npmjs.com/cli/v8/commands/npm-init) or [`yarn create`](https://classic.yarnpkg.com/blog/2017/05/12/introducing-yarn/), are also great and popular options, sadly specific to Node.js ecosystem.

## Proposed Solution: `uv create`

Introduce a `uv create` subcommand to streamline project bootstrapping. This command would allow users to scaffold projects using predefined or custom templates, inspired by tools like `hatch new`, `create-react-app`, and `cookiecutter`.

### Example Usage

#### Using Built-in Templates

```bash
$ uv create django my_project
$ cd my_project
$ uv run start
```

#### Using Custom Templates from GitHub

```bash
$ uv create myuser/mytemplate my_project
$ cd my_project
$ uv run start
```

#### Alternative Syntax: `uv init --from`

For alignment with `npm init` and avoiding redundancy with `uv init`, an alternative could implementing this as a `uv init` feature:

```bash
$ mkdir my_project && cd my_project
$ uv init --from django
```

For custom templates:
```bash
$ uv init --from myuser/mytemplate
```

## Benefits of `uv create`

- **Seamless integration:** Unified workflow within `uv`.
- **Flexibility:** Support for both built-in and user-defined templates.
- **Productivity:** Automates repetitive tasks, reducing setup time and errors.

## Contribution

If the team approves, I’m happy to contribute to the implementation with guidance on project structure and integration points.


---

_Comment by @zanieb on 2024-12-09 22:48_

Thanks for the thorough overview. How does this differ from https://github.com/astral-sh/uv/issues/4759?

---

_Comment by @hellmrf on 2024-12-09 23:51_

Hi, @zanieb! Thank you for the quick reply.

First of all, I confess I did an extensive search in this repo for related issues (I even used Copilot!) but didn’t find the related issue, so I apologize for any oversight. That said, I believe there are significant differences between this proposal and #4759, so I will share my thoughts on why this merits consideration.

While #4759 proposes **integrating external tools** like Cookiecutter to `uv init`, I'm focusing on **standardizing workflows** with a **fully native solution** (thinking ultimately in `uv` becoming the Python's `cargo`/`npm`).

## Cookiecutter?

[Cookiecutter](https://github.com/cookiecutter/cookiecutter), while popular and language-agnostic, faces some challenges such as:
 
- slow development activity ([50- commits on 2024](https://github.com/cookiecutter/cookiecutter/commits/main));
- poor community support ([200+ opened issues](https://github.com/cookiecutter/cookiecutter/issues));

Some basic and some serious problems were detected by the community, filled as issues and remains unsolved (opened), for example:

- running `mypy` on templates fails ([#2116](https://github.com/cookiecutter/cookiecutter/issues/2116));
- lack of incremental templates ([#342](https://github.com/cookiecutter/cookiecutter/issues/342));
- 'cookiecutter' repeated everywhere ([#380](https://github.com/cookiecutter/cookiecutter/issues/380)).

Relying on such external tools could undermine `uv`'s reputation for speed and reliability, particularly as a Rust-based project. 

### Cookiecutter + `uv`?

Regarding the specific integration of `cookiecutter` into `uv init`, I see at least these risks that could arise:

1. **Misalignment with `uv`'s project structure**: Many Cookiecutter templates available online may not align well with `uv`'s streamlined approach, potentially resulting in duplicated or conflicting files such as `pyproject.toml`, `setup.py`, `requirements.txt`, and `environment.yml`.
2. **Inconsistent user experience**: Cookiecutter templates vary widely in quality, documentation, and compatibility, leading to fragmented workflows and confusion among developers.
3. **Performance overhead**: Cookiecutter's Python implementation introduces an additional dependency and potential bottleneck, conflicting with `uv`'s emphasis on speed and simplicity.
4. **Dependency risks**: Cookiecutter's slow development and unresolved issues could introduce maintenance headaches for `uv`, especially if future updates break compatibility.
5. **Limited flexibility for customization**: Cookiecutter templates often lack support for incremental updates, meaning developers must either rebuild projects from scratch or manually adjust files—an inefficient process for larger teams.

### `uv` + Cookiecutter: `uv` already "fully supports" `cookiecutter`

While Cookiecutter in general isn't compatible with `uv`, **`uv` itself is already fully compatible with Cookiecutter**:

```bash
$ uv tool run cookiecutter <template>
```

The command above works seamlessly and is readily available to any user who wants to leverage Cookiecutter templates within the `uv` ecosystem.

### Why `uv create`?

A native `uv create` command would resolve the limitations associated with relying on external tools like Cookiecutter, ensuring stability, performance, and a seamless experience fully aligned with `uv`'s ecosystem. Unlike external integrations, a native approach would provide consistent quality, tighter control over the user experience, and avoid the maintenance risks tied to third-party tools with slower development cycles or unresolved issues. 

By offering a fully integrated solution, the addition of `uv create` would elevate `uv` as a comprehensive, reliable project management tool, much like how `npm` or `cargo` serve their respective ecosystems. This would enable developers to scaffold projects with confidence, uniformity, and ease, while maintaining the speed and simplicity that defines `uv`.

## Learn from other people's mistakes and successes

While Cookiecutter is a well-known tool, **it hasn't become the standard for project initialization**, leaving workflows fragmented across different teams and ecosystems.

In contrast, tools like [`create-react-app`](https://create-react-app.dev/docs/getting-started#creating-an-app), [`npm init`](https://docs.npmjs.com/cli/v8/commands/npm-init), and [`yarn create`](https://classic.yarnpkg.com/en/docs/cli/create/) have established themselves as **widely adopted standards** in the Node.js/Deno ecosystems (e.g. [React Native's Get Started Docs](https://reactnative.dev/docs/environment-setup#start-a-new-react-native-project-with-expo)). 

These tools, being the standard in the industry, provide stability, uniformity, and a consistent developer experience, making it easier for new users to onboard and adopt best practices seamlessly.

## Conclusion 

This way, implementing a native `uv create` command would bring similar benefits to the `uv` ecosystem, offering an intuitive and standardized approach to project scaffolding that developers and teams can rely on.

---

_Comment by @zanieb on 2024-12-10 00:58_

Thanks for explaining! I'm going to close this as a duplicate, since we can track the functionality in the other issue — but I'll link to this context.

---

_Closed by @zanieb on 2024-12-10 00:58_

---

_Label `duplicate` added by @zanieb on 2024-12-10 00:59_

---

_Referenced in [astral-sh/uv#4759](../../astral-sh/uv/issues/4759.md) on 2024-12-10 00:59_

---

_Comment by @zanieb on 2024-12-10 01:00_

Actually sorry to flip flop, I think I'm okay tracking this separately if you're suggesting we define our _own_ template tooling — that's the case, yes?

---

_Comment by @hellmrf on 2024-12-10 01:07_

@zanieb yes, that's correct: I'm proposing the **development of a new, `uv`-specific, template tooling**, written in Rust, to standardize workflows within the `uv` ecosystem, mostly inspired by [`npm init`](https://docs.npmjs.com/cli/v8/commands/npm-init). For that, I'm arguing that, while cookiecutter users can `uv tool run cookiecutter`, the `uv create` approach should ensure tight integration with sane defaults, enhance performance, and provide a consistent user experience, aligning with `uv`'s goals of speed and reliability. 

---

_Reopened by @zanieb on 2024-12-10 03:53_

---

_Renamed from "Feature request: Add a new `uv create` command to scaffold new projects (like `hatch new` or `yarn create`)" to "Add support for templating and creating projects with uv" by @zanieb on 2024-12-10 03:54_

---

_Label `duplicate` removed by @zanieb on 2024-12-10 03:54_

---

_Label `enhancement` added by @zanieb on 2024-12-10 03:54_

---

_Label `wish` added by @zanieb on 2024-12-10 03:54_

---

_Comment by @baggiponte on 2024-12-10 11:26_

I'd love for uv to develop its own template tooling! On the other hand, there are quite a lot of templates lying around and it would be a waste to not be able to support them.

A built-in template engine would be really dope and could add features that are lacking in engines such as cookiecutter - chief of all, the ability to update the current project to a more recent version of a template.

---

_Comment by @kevinhav on 2025-01-20 18:52_

I would also like to see this supported by `uv`. In my opinion project setup is one of the more cumbersome tasks, and it seems to fit well in `uv` from a workflow perspective.

---

_Comment by @astrojuanlu on 2025-01-20 20:04_

> chief of all, the ability to update the current project to a more recent version of a template.

Have a look at https://copier.readthedocs.io !

---

_Comment by @josecannete on 2025-02-15 14:34_

I think this could be a great addition to uv!

---

_Referenced in [astral-sh/uv#8178](../../astral-sh/uv/issues/8178.md) on 2025-02-20 03:35_

---

_Comment by @lucafaggianelli on 2025-03-05 17:59_

I second this, it's a great addition to uv!

For starting small, as a PoC, why don't we just support a hit repository or folder copy without any templating support? I think this would already solve many use cases.

---

_Comment by @pasunboneleve on 2025-06-16 04:45_

An alternative would be to direct template users to a language-agnostic scaffolding engine and instead providing a standard `uv` configuration for it. That's what I did, BTW: [baker-uv-template](https://github.com/dmvianna/baker-uv-template).

---

_Comment by @aretrace on 2025-10-14 05:50_

Having a `uv` equivalent to `npm create` would be **awesome**.

---

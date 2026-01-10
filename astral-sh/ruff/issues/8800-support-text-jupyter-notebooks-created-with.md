```yaml
number: 8800
title: Support text Jupyter notebooks created with Jupytext
type: issue
state: open
author: owenlamont
labels:
  - wish
  - notebook
assignees: []
created_at: 2023-11-21T03:44:14Z
updated_at: 2025-04-29T19:32:32Z
url: https://github.com/astral-sh/ruff/issues/8800
synced_at: 2026-01-10T11:09:51Z
```

# Support text Jupyter notebooks created with Jupytext

---

_Issue opened by @owenlamont on 2023-11-21 03:44_

I have a use case for Ruff and Ruff formatter that is a bit related to some of the other Markdown / Docstring feature requests but specifically I hoped to run Ruff and Ruff formatter on Jupyter notebooks that had been exported to markdown with [Jupytext](https://jupytext.readthedocs.io/en/latest/).

The company I'm at prefer converting notebooks to Markdown as it makes the notebook diffs much easier to read on Bitbucket (which doesn't support any notebook rendering/diffing like GitHub).

At first I noticed I could add markdown as a target file format for Ruff formatter and linter which got my hopes up that this would just work:

```yaml
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.1.6
    hooks:
      - id: ruff-format
        types_or: [python, pyi, jupyter, markdown]
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
        types_or: [python, pyi, jupyter, markdown]
```

But when I ran Ruff I see it is failing to parse the markdown properly - I had hoped it would just run on the python comment code blocks in the same way it would parse Jupyter notebook cells and ignore all the other markdown content but its obviously trying to parse all the markdown, e.g.

<img width="1142" alt="image" src="https://github.com/astral-sh/ruff/assets/12672027/384f83de-5236-4184-89b2-b4ae4b232b1d">






---

_Comment by @dhruvmanila on 2023-11-21 14:45_

Hey, we currently don't have support for linting / formatting Python code in markdown blocks (https://github.com/astral-sh/ruff/issues/8237, https://github.com/astral-sh/ruff/issues/3792). I'll close this in favor of the markdown issue for bookkeeping purposes as I think that should solve this but correct me if I'm wrong here.

---

_Closed by @dhruvmanila on 2023-11-21 14:45_

---

_Comment by @dhruvmanila on 2023-11-21 14:48_

Hmm, actually it would be a bit different as for markdown we wouldn't need to have the concatenated source code from all code blocks but if it's a notebook converted to markdown then I think it should have context from other code blocks? @owenlamont Do you think this is true?

Another solution currently that I can think of is to lint / format _before_ converting it to markdown. I'm not sure how feasible this would be given my lack of knowledge about your setup.

---

_Reopened by @dhruvmanila on 2023-11-21 14:49_

---

_Comment by @owenlamont on 2023-11-21 20:41_

Hi @dhruvmanila - yeah it would have to have the concatenated source code - I can see Ruff still tracks which code was in which Jupyter cell when raising warnings so if it could treat comment blocks exactly as Jupyter cells are treated that would be ideal.

As a work-around it could be exported to ipynb, linted and formatted, then re-exported to markdown - but that would be onerous. When working with Jupytext the notebook never gets persisted (in any permanent/visible way) as an ipynb - it gets loaded from Markdown and saved back to Markdown.

The ideal solution (from my perspective) would be to parse the YAML front matter of the Markdown, identify this as a Juptext generated Markdown, then recognise the code blocks need to be concatenated and treated as notebook cells. I totally understand though if this use case is too niche to justify the effort though. I can't speak much as to how many people use this format - as a repo [jupytext ](https://github.com/mwouts/jupytext) is relatively popular (around 6k users - I recognise some relatively prominent Jupyter developers as contributors).

---

_Label `wish` added by @dhruvmanila on 2023-12-01 01:14_

---

_Comment by @tvatter on 2024-01-26 09:10_

There's a similar request for quarto notebooks (#6140), and generally for Python code included in Markdown code blocks (#3792).

---

_Label `notebook` added by @dhruvmanila on 2024-02-12 06:14_

---

_Renamed from "Feature request: Lint and format Jupyter notebooks exported to markdown with Jupytext" to "Support text Jupyter notebooks created with Jupytext" by @dhruvmanila on 2024-04-26 14:46_

---

_Comment by @davidorme on 2024-06-06 10:39_

I think I'm looking at the same issue.  We also use Myst Markdown notebooks in several projects and have been using the `jupytext` ability to pipe code through `black` to apply Python formatting:

```sh
$ jupytext --pipe black docs/source/users/pmodel/c3c4model.md 
[jupytext] Reading docs/source/users/pmodel/c3c4model.md in format md
[jupytext] Executing black -
reformatted -

All done! ✨ 🍰 ✨
1 file reformatted.
[jupytext] Writing docs/source/users/pmodel/c3c4model.md in format md:myst
```

We have that as part of a `pre-commit` hook to ensure that the code in our notebooks is properly formatted.

```yaml
  - repo: https://github.com/mwouts/jupytext
    rev: v1.16.2
    hooks:
    - id: jupytext
      args: [--pipe, black]
      files: docs/source 
      additional_dependencies:
        - black==24.4.2 # Matches hook
```

I haven't been able to work out exactly what happens, but the `jupytext.cli` module provides a `pipe_notebook` function that is used to round trip _something_ (I think it must be just the code cell contents?) through `black`.


---

_Comment by @davidorme on 2024-06-07 09:16_

OK - so `jupytext` converts the notebook to `percent` format, which is a python file with the markdown content stored as comments. `black` can then run on the code alone and the format can be converted back. 

---

_Comment by @tovrstra on 2024-07-17 06:58_

Probably related: Ruff does currently not see `py:percent` files as notebooks. For example, for `py:percent` files, #8590 is still unresolved. (I'm not sure if this comment belongs here. I'm happy to open a new issue for it.)

---

_Comment by @dhruvmanila on 2024-07-17 07:44_

Yes, that's true. Currently, Ruff only supports the [official Jupyter Notebook format](https://github.com/jupyter/nbformat/blob/main/nbformat/v4/nbformat.v4.schema.json) and none of the other formats.

---

_Comment by @bluss on 2024-07-17 13:19_

@tovrstra py percent belongs here because #11160 was redirected here.

---

_Comment by @davidorme on 2024-07-17 14:45_

Just to add details about our use case. 

`jupytext` allows you to [pair notebook formats](https://jupytext.readthedocs.io/en/latest/paired-notebooks.html) so that you can have a synchronised MyST `.md` and `.ipynb` files. It seems like one solution here is that `ruff` formats the `.ipynb` version and then `jupytext` can resynchronise the files.

However, we're not using paired notebooks and only have the MyST `.md` versions in our repo. That does mean that the files don't contain the outputs of running code, so we don't get the nicely rendered versions in GitHub, but we also avoid quite a lot of commit chaff from trivial changes to `.ipynb` files and only commit changes to the notebook source. So I guess with the existing functionality, we could convert files to `.ipynb`, format using `ruff`, convert back to MyST `.md` and delete the `.ipynb` formats.

---

_Comment by @ctcjab on 2025-04-29 17:35_

#17117 was recently closed as a duplicate of this issue. Should that instead be added as a subtask of this issue, to make sure that specific issue is resolved as part of this more general effort?

---

_Comment by @dhruvmanila on 2025-04-29 19:32_

> [#17117](https://github.com/astral-sh/ruff/issues/17117) was recently closed as a duplicate of this issue. Should that instead be added as a subtask of this issue, to make sure that specific issue is resolved as part of this more general effort?

Thank you for the suggestion, but I don't think that's necessary here as that rule is already ignored for Jupyter Notebooks and adding support for notebooks created by Jupytext should automatically consider that.

---

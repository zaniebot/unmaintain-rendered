```yaml
number: 13809
title: "[red-knot] Autoformat `mdtest` Python snippets using `blacken-docs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: redknot-formatting
created_at: 2024-10-18T10:30:51Z
updated_at: 2024-10-19T16:10:34Z
url: https://github.com/astral-sh/ruff/pull/13809
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Autoformat `mdtest` Python snippets using `blacken-docs`

---

_@AlexWaygood_

## Summary

This PR adds a `blacken-docs` hook to our `.pre-commit-config.yaml` so that we can easily autoformat all Python code snippets in our `mdtest` Markdown files _en masse_. Since we run pre-commit in CI, this will also enforce a consistent style for these Python snippets in CI. The first commit adds the hook to the pre-commit config file; the second commit is the effect of running `pre-commit run -a` locally with this configuration.

Although Ruff is able to format Python code snippets in docstrings, it doesn't seem like it's yet capable of formatting Python code snippets in _Markdown files_, so it seems like blacken-docs is the best option here for now.

I went with `--line-length=1000`, because the assertion comments in our test files can get quite long, and it seems more annoying than helpful if autoformatting results in changes such as

```diff
- x = a  # error: some verrrrrrrrrrrrrrrrrry verbosssssssssssse error messageeeeeeeeeeee
+ x = (
+     a # error: some verrrrrrrrrrrrrrrrrry verbosssssssssssse error messageeeeeeeeeeee
+ )
```

## Test Plan

`pre-commit run -a`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-18 10:30_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-18 10:30_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-18 10:30_

---

_Comment by @AlexWaygood on 2024-10-18 10:31_

Cc. @sharkdp as well, for interest :-)

---

_Comment by @MichaReiser on 2024-10-18 10:34_

Nice but.... that would force me to use pre-commit which I rather don't. But not sure how we can accomplish this today without pre-commit.


I also think that a line length of 1000 is too high. How about something more realistic like 120? What I remember is that
`revealed` and `expected` can be used as own line comments. I would argue that that's probably a better style for very long diagnostics.


---

_Comment by @MichaReiser on 2024-10-18 10:35_

A line length of 1000 is actually problematic because Black then collapses expressions onto a single line. E.g. long comprehensions or binary expressions would be printed on a single line. 

---

_Comment by @MichaReiser on 2024-10-18 10:38_

I wonder if we should integrate this into the `mdtest` framework instead where setting an environment variable formats the snippets and files that are not correctly formatted fail otherwise (that seems somewhat annoying)

---

_Comment by @AlexWaygood on 2024-10-18 10:39_

> Nice but.... that would force me to use pre-commit which I rather don't. But not sure how we can accomplish this today without pre-commit.

You can also run blacken-docs manually. Pre-commit makes the experience much nicer IMO, but there's some docs [here](https://github.com/adamchainz/blacken-docs?tab=readme-ov-file#usage) on how to manually run it.

We already do nearly all our other linting (typos-checking, Markdown checking, etc.) via pre-commit in this repo, so I'd personally prefer to stick to that here.

> I also think that a line length of 1000 is too high. How about something more realistic like 120? What I remember is that
> `revealed` and `expected` can be used as own line comments. I would argue that that's probably a better style for very long diagnostics.

Yeah, that's a fair point. I'll switch to 130, for no other reason than it's what typeshed uses, and I like the formatting of typeshed's stubs 😄

---

_Comment by @AlexWaygood on 2024-10-18 10:40_

> I wonder if we should integrate this into the `mdtest` framework instead where setting an environment variable formats the snippets and files that are not correctly formatted fail otherwise (that seems somewhat annoying)

I would _much_ prefer to be able to easily autoformat all the files with a single command. And this also feels like a linting issue than something that should cause the _tests_ to fail, to me

---

_Comment by @github-actions[bot] on 2024-10-18 10:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-10-18 10:49_

(To elaborate on how _I_ use pre-commit, in case it's helpful: I don't much like installing git hooks locally. I actually never set them up; I never run `pre-commit install` in a repo locally. I _only_ ever use pre-commit as a command runner. But for the purpose of orchestrating various linting tools and running them over a codebase, it's pretty nice IMO. Just because it _can_ be used as a way of coordinating git hooks that run on every commit, doesn't mean that that's the way you _have_ to use it.)

---

_@AlexWaygood reviewed on 2024-10-18 10:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/literal/string.md`:16 on 2024-10-18 10:58_

here we're not testing what we wanted to test anymore

---

_Comment by @calumy on 2024-10-18 11:07_

Out of curiosity, is there any particular reason for using black to format rather than the ruff formatter, which is how the ruff docs are formatted (https://github.com/astral-sh/ruff/blob/main/scripts/check_docs_formatted.py#L147C1-L176C23)?

---

_Comment by @AlexWaygood on 2024-10-18 11:09_

> Out of curiosity, is there any particular reason for using black to format rather than the ruff formatter, which is how the ruff docs are formatted ([`main`/scripts/check_docs_formatted.py#L147C1-L176C23](https://github.com/astral-sh/ruff/blob/main/scripts/check_docs_formatted.py?rgh-link-date=2024-10-18T11%3A07%3A08Z#L147C1-L176C23))?

The `check_docs_formatted.py` script will _check_ that the docs are formatted and complain if they aren't, but it won't actually format them for you. That feels like the opposite of what I want for mdtest :-)

I want something that's going to make my life easier (by making it easy to apply a consistent style across these Markdown files, using only a single CLI command). If `check_docs_formatted.py` complains at me about things being badly formatted but doesn't actually _fix_ things for me, that feels like it makes my life harder, not easier.

---

_@AlexWaygood reviewed on 2024-10-18 11:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/literal/string.md`:16 on 2024-10-18 11:13_

I fixed this with a judicious use of `# fmt: skip`. Don't tell @JelleZijlstra.

---

_Comment by @MichaReiser on 2024-10-18 11:24_

> We already do nearly all our other linting (typos-checking, Markdown checking, etc.) via pre-commit in this repo, so I'd personally prefer to stick to that here.

Yes, we use pre-commit to check consistent styling in CI. But it's not a tool required when developing locally (or only very rarely and I then often have to run it multiple times which I don't understand why). 

> You can also run blacken-docs manually. Pre-commit makes the experience much nicer IMO, but there's some docs [here](https://github.com/adamchainz/blacken-docs?tab=readme-ov-file#usage) on how to manually run it.

What's nice about Rust is that you can check out any project and you know how to run, test, and format it. Requiring users to figure out how to install a python tool and figure out the right pattern seems too much to ask (yes, uvx makes that easier). 

That's why I still think an integration into `mdtest` is the better solution because the framework can print a useful message telling you that there are incorrectly formatted tests at the end of the run (as the last check) and suggest the command how you can update it, without any extra dependencies. `mdtest` can also use ruff's formatter

---

_Comment by @AlexWaygood on 2024-10-18 11:34_

> Yes, we use pre-commit to check consistent styling in CI. But it's not a tool required when developing locally (or only very rarely

Actually, I've found that our existing pre-commit hooks for Markdown linting have had complaints about the initial revision of nearly every red-knot PR I've submitted using mdtest 😆 So the situation you describe here has arguably already changed recently, whether for better or worse

> I then often have to run it multiple times which I don't understand why

It generally stops after a single hook fails, so that you can then evaluate the changes that the hook might have made to your code before deciding whether you want to proceed. I agree that this is a bit annoying sometimes.

> What's nice about Rust is that you can check out any project and you know how to run, test, and format it. Requiring users to figure out how to install a python tool and figure out the right pattern seems too much to ask (yes, uvx makes that easier).

I would say a common convention in a Python project is that you can run all the linting tools using `pre-commit run -a` 😄

And again, this is an existing problem on `main`. We should re-evaluate our existing pre-commit hooks if you have this concern; they're already quite picky about Markdown styling, and we have many more Markdown files now than we used to.

> That's why I still think an integration into `mdtest` is the better solution because the framework can print a useful message telling you that there are incorrectly formatted tests at the end of the run (as the last check) and suggest the command how you can update it, without any extra dependencies. `mdtest` can also use ruff's formatter

It still feels "wrong" to me for the test framework to fail because of a linting error. To use your analogy of how nice it is that Cargo can generally manage everything for you in a typical Rust project, the result of `cargo test` is wholly independent from whether or not `cargo fmt` is going to reformat your code or not. They're fully separate concerns.

---

_Comment by @MichaReiser on 2024-10-18 11:50_

> Actually, I've found that our existing pre-commit hooks for Markdown linting have had complaints about the initial revision of nearly every red-knot PR I've submitted using mdtest 😆 So the situation you describe here has arguably already changed recently, whether for better or worse

Uhh true. I wasn't aware of this

---

_Comment by @AlexWaygood on 2024-10-18 12:25_

> > I then often have to run it multiple times which I don't understand why
> 
> It generally stops after a single hook fails, so that you can then evaluate the changes that the hook might have made to your code before deciding whether you want to proceed. I agree that this is a bit annoying sometimes.

I don't think I have this problem in other repos that use pre-commit. I think it's because of a specific setting in our pre-commit config. I filed https://github.com/astral-sh/ruff/pull/13811 to fix it.

---

_Comment by @calumy on 2024-10-18 12:52_

> > Out of curiosity, is there any particular reason for using black to format rather than the ruff formatter, which is how the ruff docs are formatted ([`main`/scripts/check_docs_formatted.py#L147C1-L176C23](https://github.com/astral-sh/ruff/blob/main/scripts/check_docs_formatted.py?rgh-link-date=2024-10-18T11%3A07%3A08Z#L147C1-L176C23))?
> 
> The `check_docs_formatted.py` script will _check_ that the docs are formatted and complain if they aren't, but it won't actually format them for you. That feels like the opposite of what I want for mdtest :-)
> 
> I want something that's going to make my life easier (by making it easy to apply a consistent style across these Markdown files, using only a single CLI command). If `check_docs_formatted.py` complains at me about things being badly formatted but doesn't actually _fix_ things for me, that feels like it makes my life harder, not easier.

Understood. So if there were "ruffen-docs", it would be preferred, but as this doesn't exist (as far as I'm aware), then `blacken-docs` is a good substitute? 

---

_Comment by @AlexWaygood on 2024-10-18 12:53_

> Understood. So if there were "ruffen-docs", it would be preferred, but as this doesn't exist (as far as I'm aware), then `blacken-docs` is a good substitute?

Yup!

---

_@JelleZijlstra reviewed on 2024-10-18 14:03_

---

_Review comment by @JelleZijlstra on `crates/red_knot_python_semantic/resources/mdtest/literal/string.md`:16 on 2024-10-18 14:03_

Then don't mention me :) If you're ever going to use `fmt: skip` this seems like the best reason.

---

_@AlexWaygood reviewed on 2024-10-18 14:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/literal/string.md`:16 on 2024-10-18 14:26_

> Then don't mention me :)

hehe, sorry, that was an attempt at a joke 😄

---

_Comment by @MichaReiser on 2024-10-18 14:58_

I'll submit to the pre-commit torture, considering that this PR doesn't make it worse. We can re-visit if we decide to use a different mechanism to format our markdown file.

---

_@zanieb reviewed on 2024-10-18 15:04_

---

_Review comment by @zanieb on `.pre-commit-config.yaml`:52 on 2024-10-18 15:04_

Can you put this config somewhere else so you can run the command outside pre-commit?

---

_Review comment by @carljm on `.pre-commit-config.yaml`:52 on 2024-10-18 16:27_

I don't know if this is possible (and if not, it's not a blocker), but if it is, I like the idea.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:17 on 2024-10-18 16:28_

I find the extra vertical spacing more of a negative than a positive in these tests, but it's not a big negative, and I'm OK with it for the sake of consistent formatting.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/shadowing/class.md`:6 on 2024-10-18 16:33_

I didn't realize Black preferred ellipsis over `pass`, even in non-stub files?

---

_@carljm approved on 2024-10-18 16:33_

My only real concern here is how often we'll run into cases where a particular unusual formatting actually matters for what we want to test. I'm thinking of e.g. testing multi-line diagnostic ranges.

But I think using `# fmt: skip` for those cases is a reasonable option, and helps to signal to human readers/editors also that "formatting matters here."

---

_@AlexWaygood reviewed on 2024-10-18 16:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:17 on 2024-10-18 16:35_

Yeah, same. It would be nice if we could get blacken-docs to think of these as `pyi` stub files, but not sure if that's possible. I find having an autoformatter for local use and CI more important than the specific formatting decisions it makes 😆

---

_@AlexWaygood reviewed on 2024-10-18 16:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/shadowing/class.md`:6 on 2024-10-18 16:37_

It does not! This change was made manually by me in commit 3 of this PR: "[Manual cleanups following autoformatting](https://github.com/astral-sh/ruff/pull/13809/commits/adafb79b5c46647e3092153feaa9e14e66dcd6bf)".

My motivation for changing the `pass` to `...` is that if you use `...`, Black figures that it's a stub-like class definition, so you probably want stub-like formatting.

(It lets you put the whole class definition on one line with `...`. If you use `pass`, it splits it over two lines.)

---

_@AlexWaygood reviewed on 2024-10-18 16:40_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:52 on 2024-10-18 16:40_

blacken-docs does not support persistent configuration, only a very small number of CLI flags that it just forwards on to Black. We could put this configuration in our repo-wide pyproject.toml's `tool.black` configuration -- but I don't feel like I have context on why we already _have_ a pre-existing Black configuration. I think it's possibly not necessary after @calumy switched `check_docs_formatted.py` so that it uses Ruff rather than Black. (Thanks again for that PR @calumy!)

If that Black configuration section is now unnecessary, then I think I can just rework it to make it red-knot blacken-docs specific.

---

_@JelleZijlstra reviewed on 2024-10-18 17:06_

---

_Review comment by @JelleZijlstra on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:17 on 2024-10-18 17:06_

Black does have an option for this (`--pyi`); not sure if it carries through to blacken-docs.

---

_@AlexWaygood reviewed on 2024-10-18 17:34_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:52 on 2024-10-18 17:34_

Actually, it seems like even if you have a persistent black configuration in a pyproject.toml file, it's ignored by blacken-docs (because blacken-docs runs black "as a library" rather than running it as a subprocess). So it seems like there's no way to have a persistent configuration for blacken-docs at all.

---

_@AlexWaygood reviewed on 2024-10-18 17:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:17 on 2024-10-18 17:34_

Unfortunately it seems like that's not one of the flags supported by blacken-docs, and it seems like it ignores any black configuration files (https://github.com/astral-sh/ruff/pull/13809#discussion_r1806820453)

---

_Comment by @AlexWaygood on 2024-10-19 14:56_

I'm going to land this as-is. The less-than-optimal parts of this are:
- We'd prefer to use our own formatter to format these Python snippets rather than Black. I agree this would be better (dogfooding is good!), but there currently isn't a `ruffen-docs` equivalent to `blacken-docs`. (`check_docs_formatted.py` will _check_ formatting using our formatter, but won't do autofixes, and the capacity to do autofixes to me feels crucial for ease of development.)
- We'd prefer for these snippets to be formatted concisely, as if they were stubs. Black has a `--pyi` flag but it's not possible to pass that flag to `blacken-docs`. To me, _having_ an autoformatter feels much more important than the specific formatting choices it makes. Possibly we could file a feature request with `blacken-docs` asking them to add a `--pyi` flag.
- We're not necessarily wild about doing all our linting via pre-commit for this repo: it's not such a popular tool in the Rust ecosystem as it is in the Python ecosystem, it can be slowish, and we don't want to require contributors to have to download another tool if they want to contribute. I agree that in a perfect world we'd use a faster tool that comes builtin with cargo to do our linting, but I'm not sure such a tool exists right now. We already do a bunch of our linting via pre-commit currently, and the existing hooks we have are already very picky about Markdown files, so this PR doesn't change much in this regard.

  I'd be happy to review another PR that gets rid of our `.pre-commit-config.yaml` file in favour of a `justfile` to coordinate all our linting tools, but that's outside the scope of this PR. (Even if we switched to using `just` rather than pre-commit, that would also still be a separate tool that we'd require contributors to install, so this wouldn't fully solve the problem.)
- We'd prefer for the `--line-length=130` flag to be set in some kind of persistent configuration outside of the pre-commit config file, so that it's easier for folks to run `blacken-docs` without having to install pre-commit if that's what they'd prefer. Unfortunately `blacken-docs` does not read any configuration files directly, and because of the way it runs Black, Black-as-invoked-by-blacken-docs also ignores all `[tool.black]` configuration files in any `pyproject.toml` files in the repo. This therefore does not seem possible for now.

Overall, then, lots of issues here and potential for improvement, but I still think the benefits outweigh the costs here. Lots of these things could possibly be improved by us building our own `ruffen-docs` tool!

---

_Merged by @AlexWaygood on 2024-10-19 14:57_

---

_Closed by @AlexWaygood on 2024-10-19 14:57_

---

_Branch deleted on 2024-10-19 14:57_

---

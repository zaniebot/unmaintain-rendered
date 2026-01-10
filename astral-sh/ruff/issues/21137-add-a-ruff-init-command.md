---
number: 21137
title: "Add a \"ruff init\" command?"
type: issue
state: closed
author: LoicRiegel
labels: []
assignees: []
created_at: 2025-10-30T12:30:09Z
updated_at: 2025-10-30T13:29:08Z
url: https://github.com/astral-sh/ruff/issues/21137
synced_at: 2026-01-10T01:23:02Z
---

# Add a "ruff init" command?

---

_Issue opened by @LoicRiegel on 2025-10-30 12:30_

Hi everyone,
I was wondering if you have already considered (or would consider) adding a "ruff init" command, similar to [cz init](https://commitizen-tools.github.io/commitizen/commands/init/#interactive-configuration) or [eslint](https://eslint.org/docs/latest/use/getting-started#configuration)  for instance.
Goal is to provide a CLI tool that allows user to build their config in a convenient way by answering a few prompts.

I searched in the issues and didn't find anything related to this. There is the playground website, but it's different.

Another option would be to have this in a separate tool, if you don't want to embed this directly in the ruff binary.

This looks like something fun to work on, so I'd be happy to do it. On the other hand, maybe it's not the priority.
But what are your thoughts on this? 


---

_Comment by @MichaReiser on 2025-10-30 12:50_

Thanks for proposing this. This has come up before in https://github.com/astral-sh/ruff/issues/12111. 

There are cases where an init command would certainly be useful but an alternative is to improve ruff's default. 

---

_Closed by @MichaReiser on 2025-10-30 12:50_

---

_Comment by @LoicRiegel on 2025-10-30 13:17_

Ah I did not see this issue!

But I think what I had in mind is different that what is proposed in the other issue. My proposal is to guide the creation of a config in an interactive way, by asking simples questions one by one, and kind of guiding the user through the whole process interactively. Again, I think this [demo of cz init](https://commitizen-tools.github.io/commitizen/commands/init/#interactive-configuration) is a great example.

From what I read in the other issue, no interaction was mentioned, and it focuses more on applying default or custom configuration than building them...

I was thinking we could ask questions like: "do you plan or using async", "do you want your multi-line docstring summaries to start on the first or second line?" (D212 VS D213). My main use case is actually to select which linter group or individual rule to pick. Last month I just scrolled through the entire doc, and modifying my config file manually. 

But as you said, maybe just improving ruff's default is a better approach. However, defaults have to stay "conservative", that's why I thought an interactive tool could make things more convenient for users that want to go further than just using the defaults. So maybe those are complementary :)

---

_Comment by @MichaReiser on 2025-10-30 13:22_

I'd envision that the `init` command would have some form of interactivity similar to what you said: E.g.: Do you use typed python (and how strict?), Do you want to format your code? What frameworks are you using, ... 

But we only need that interactivity for things that can't be automatially detected. Either way, I think the request is close enough so that isn't worth tracking separately (and there's a backling to this issue)

---

_Comment by @LoicRiegel on 2025-10-30 13:29_

> But we only need that interactivity for things that can't be automatially detected. Either way, I think the request is close enough so that isn't worth tracking separately (and there's a backling to this issue)

yes of course

Thanks for sharing your view :)

---

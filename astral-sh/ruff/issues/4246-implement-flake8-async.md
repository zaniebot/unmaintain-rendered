```yaml
number: 4246
title: Implement flake8-async
type: issue
state: closed
author: dacevedo12
labels:
  - good first issue
  - plugin
assignees: []
created_at: 2023-05-05T13:42:39Z
updated_at: 2023-05-15T13:15:29Z
url: https://github.com/astral-sh/ruff/issues/4246
synced_at: 2026-01-12T15:54:44Z
```

# Implement flake8-async

---

_@dacevedo12_

https://github.com/cooperlees/flake8-async

It'd be good to implement (and hopefully extend) the concept of this project in enforcing best practices for asyncio usage.

---

_Label `plugin` added by @charliermarsh on 2023-05-05 17:09_

---

_Label `good first issue` added by @charliermarsh on 2023-05-08 19:48_

---

_Comment by @charliermarsh on 2023-05-08 19:48_

Looks pretty straightforward for anyone that's interested in learning to write rules for Ruff.

---

_Comment by @inikolaev on 2023-05-14 12:34_

I want to give it a try - have a working prototype already and will push shortly.

---

_Comment by @qdegraaf on 2023-05-14 17:27_

> I want to give it a try - have a working prototype already and will push shortly.

Missed this. Had some chats about working on this the last few days in the Discord but did not mention it here. Just opened PR, but as a first time contributor I might not have come up with the most elegant solution. Happy to close for your solution if it's better, or merge the two or something else along those lines.

---

_Comment by @inikolaev on 2023-05-14 19:29_

> > I want to give it a try - have a working prototype already and will push shortly.
> 
> Missed this. Had some chats about working on this the last few days in the Discord but did not mention it here. Just opened PR, but as a first time contributor I might not have come up with the most elegant solution. Happy to close for your solution if it's better, or merge the two or something else along those lines.

I'm a first time contributor as well, feel free to peek at what I've done so far: https://github.com/charliermarsh/ruff/pull/4433

Still missing some docs for the violations

---

_Comment by @inikolaev on 2023-05-14 19:30_

I should have checked open PRs as well, but assumed nobody was working on it yet :)

---

_Comment by @qdegraaf on 2023-05-14 21:23_

Yeah I could have been more clear as well 😅 . Going to leave it to @charliermarsh to decide what's to happen for this one and be a bit more transparent on the next one.

---

_Comment by @dacevedo12 on 2023-05-14 21:41_

Please keep in mind the original one has a bug.

time.sleep -> triggers rule
from time import sleep -> doesnt

It also lacks validations for async defs that dont use await inside them.

Just pointing this out if the PRs port the code exactly as it is on that project to avoid the same mistakes

---

_Comment by @dacevedo12 on 2023-05-14 21:43_

Thanks for the contributions. I'd like to complement those myself but my skills in rust are currently lacking



---

_Comment by @qdegraaf on 2023-05-14 22:17_

> Please keep in mind the original one has a bug.
> 
> time.sleep -> triggers rule from time import sleep -> doesnt
> 
> It also lacks validations for async defs that dont use await inside them.
> 
> Just pointing this out if the PRs port the code exactly as it is on that project to avoid the same mistakes

Good catch., cheers! I'll mark mine as draft with some TODOs, but will hold off on implementing them in case https://github.com/charliermarsh/ruff/pull/4433 covers that already, in which case I can close mine.


---

_Comment by @charliermarsh on 2023-05-15 02:15_

I'm going to move forward with reviewing #4432 since it strikes me as a bit more complete (uses scoping logic to detect the async function context, includes documentation for the rules). I'm sorry @inikolaev, I know it's disappointing to put in work on a PR and not see it merged into the codebase, but we now have two similar PRs for the same scope of the work and so I have to choose between them somehow.

If you're interested in contributing, I'd love to help you find something else to work on (e.g., I'd love to finish the `flake8-pyi` rules from https://github.com/charliermarsh/ruff/issues/848), but I totally understand if you'd rather not.


---

_Comment by @inikolaev on 2023-05-15 05:31_

> I'm going to move forward with reviewing #4432 since it strikes me as a bit more complete (uses scoping logic to detect the async function context, includes documentation for the rules). I'm sorry @inikolaev, I know it's disappointing to put in work on a PR and not see it merged into the codebase, but we now have two similar PRs for the same scope of the work and so I have to choose between them somehow.
> 
> If you're interested in contributing, I'd love to help you find something else to work on (e.g., I'd love to finish the `flake8-pyi` rules from #848), but I totally understand if you'd rather not.

No worries, I will look for something else to implement 👍 

---

_Closed by @charliermarsh on 2023-05-15 13:15_

---

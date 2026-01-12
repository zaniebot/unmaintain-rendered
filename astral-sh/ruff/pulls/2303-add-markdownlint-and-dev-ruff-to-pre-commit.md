```yaml
number: 2303
title: "Add `markdownlint` and dev Ruff to `pre-commit`"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: add-markdownlint-and-prettier-to-pre-commit
created_at: 2023-01-28T17:09:55Z
updated_at: 2023-04-30T19:47:57Z
url: https://github.com/astral-sh/ruff/pull/2303
synced_at: 2026-01-12T15:55:07Z
```

# Add `markdownlint` and dev Ruff to `pre-commit`

---

_@JonathanPlasse_

Hi,
I propose adding _markdownlint_ and _Prettier_ to pre-commit.
I already added _markdownlint_ but I did not add _Prettier_ because `dev-generate-all` is incompatible with it.
We would have to run _Prettier_ inside `dev-generate-all`.
To do this we either require the developer to have node installed on their machine or use [nodeenv](https://github.com/ekalinin/nodeenv) which only requires _Python_ to create a _Node_ virtual environment where we would run _Prettier._

_Prettier_ would be really useful to format `ruff.schema.json` as it is required by `schemastore.org`.

Also maybe we should consider using for the ruff hook `cargo run` with `repo: local` instead of `ruff-pre-commit` which sometimes fails because the version for the latest release is not out yet.

---

_@not-my-profile reviewed on 2023-01-28 17:14_

---

_Review comment by @not-my-profile on `CODE_OF_CONDUCT.md`:23 on 2023-01-28 17:14_

It's a nit but I prefer `*` for bullet lists over `-`.

---

_@not-my-profile reviewed on 2023-01-28 17:16_

---

_Review comment by @not-my-profile on `python/ruff/__main__.py`:7 on 2023-01-28 17:16_

Is this change related to prettier / markdownlint?

---

_@JonathanPlasse reviewed on 2023-01-28 17:20_

---

_Review comment by @JonathanPlasse on `python/ruff/__main__.py`:7 on 2023-01-28 17:20_

No, but it was failing the Ruff hook. So I fixed it.

---

_Renamed from "Add markdownlint and prettier to pre-commit" to "Add markdownlint, prettier, and dev ruff to pre-commit" by @JonathanPlasse on 2023-01-28 17:26_

---

_Comment by @sbrugman on 2023-01-29 11:33_

Cargo format is part of our GitHub actions pipeline. If markdownlint is added, any reason to not enforce it there?

---

_Comment by @JonathanPlasse on 2023-01-29 15:37_

We could use [pre-commit.ci](https://pre-commit.ci/) that would run on pull-request to do this.

---

_Comment by @sbrugman on 2023-01-29 16:02_

> We could use [pre-commit.ci](https://pre-commit.ci/) that would run on pull-request to do this.

Certainly an option. There is also a markdownlint github action: 
https://github.com/actionshub/markdownlint (that does not require to install a github application)


---

_@sbrugman reviewed on 2023-01-29 16:09_

---

_Review comment by @sbrugman on `CODE_OF_CONDUCT.md`:23 on 2023-01-29 16:09_

Markdownlint supports the asterix style: https://github.com/markdownlint/markdownlint/blob/main/docs/RULES.md#md004---unordered-list-style

---

_Comment by @charliermarsh on 2023-01-29 18:27_

> I already added markdownlint but I did not add Prettier because dev-generate-all is incompatible with it.

Can you expand on this? How is it incompatible?


---

_Comment by @JonathanPlasse on 2023-01-29 18:39_

Dev generate all will generate the different file which will be then formated by prettier.
Pre-commit will always fail at the one of the hook as the output of prettier is different from dev generate all.
For it to be compatible it would need that dev generate all generate the different files already well formated.

---

_Comment by @not-my-profile on 2023-01-30 04:02_

>To do this we either require the developer to have node installed on their machine or use [nodeenv](https://github.com/ekalinin/nodeenv) which only requires Python to create a Node virtual environment where we would run Prettier.

I'd really rather not add Node as a dev dependency.

> Prettier would be really useful to format ruff.schema.json as it is required by schemastore.org.

I think the better approach would be to implement https://github.com/SchemaStore/schemastore/issues/2731 then our schema file would no longer have to be part of the schemastore repository.

---

_Comment by @charliermarsh on 2023-01-30 04:17_

Yeah I'd like to avoid adding Node as a dependency. I'm happy to just do this manually from time to time, even though it's not a great practice.


---

_Comment by @JonathanPlasse on 2023-01-30 11:23_

I added prettier in pre-commit.
I had to:
- Ignore formatting for the table (c.f. prettier/prettier-vscode#762)
- Use `-` as bullet as prettier does not support `*` bullet (c.f. prettier/prettier#4440).
  prettier [option philosophy](https://prettier.io/docs/en/option-philosophy.html) is to have few option so that we do not debate on which style to chose.
  > By far the biggest reason for adopting Prettier is to stop all the ongoing debates over styles.
  > Yet the more options Prettier has, the further from the above goal it gets. The debates over styles just turn into debates over which Prettier options to use.

---

_@JonathanPlasse reviewed on 2023-01-30 11:43_

---

_Review comment by @JonathanPlasse on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 11:43_

Prettier does not support it.

---

_Comment by @charliermarsh on 2023-01-30 12:14_

> I added prettier in pre-commit.

Does this require that the user have Node installed? Or how does it work?

(I don't feel strongly about those individual style decisions, like `*` vs. `-`. Even if I have preferences, it's not a blocker for me.)

---

_Comment by @JonathanPlasse on 2023-01-30 12:21_

> > I added prettier in pre-commit.
> 
> Does this require that the user have Node installed? Or how does it work?

No, it does not require Node. If pre-commit is installed it will run the prettier hook with `nodeenv` but the developer only need to have pre-commit installed and nothing else.
For the CI, I would suggest using [pre-commit.ci](https://pre-commit.ci) to run pre-commit. To avoid checking twice `fmt`, `clippy`, and `dev-generate-all`, we can edit `.pre-commit-config.yaml` to skip them when in CI.
Like this `validate-pyproject`, `prettier`, `markdownlint-fix`, and `ruff` hooks would run on the pull request.
Which is not the case currently.
Using _pre-commit.ci_ only needs `.pre-commit-config.yaml` and avoids maintaining a separate GitHub Action.

---

_Marked ready for review by @JonathanPlasse on 2023-01-30 12:27_

---

_Comment by @charliermarsh on 2023-01-30 12:32_

Candidly, while I definitely appreciate the motivation, I'm probably not going to enable pre-commit CI right now, since I don't use pre-commit personally!

---

_@charliermarsh reviewed on 2023-01-30 12:45_

---

_Review comment by @charliermarsh on `.pre-commit-config.yaml`:15 on 2023-01-30 12:45_

Should this now be removed? (Are we using Prettier instead?)

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:15 on 2023-01-30 13:00_

Prettier is only a formatter, it will not catch some lint that markdownlint has.
e.g. [MD001](https://github.com/DavidAnson/markdownlint/blob/main/doc/md001.md) heading-increment/header-increment - Heading levels should only increment by one level at a time

---

_@JonathanPlasse reviewed on 2023-01-30 13:00_

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:15 on 2023-01-30 13:02_

We should keep both.

---

_@JonathanPlasse reviewed on 2023-01-30 13:02_

---

_@charliermarsh reviewed on 2023-01-30 13:06_

---

_Review comment by @charliermarsh on `.pre-commit-config.yaml`:15 on 2023-01-30 13:06_

I see, does markdownlint do any autofixing? Would the tools ever conflict?

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:15 on 2023-01-30 13:11_

According to [this](https://github.com/DavidAnson/markdownlint/blob/main/doc/Prettier.md), no it should not.
>[`Prettier`](https://prettier.io) is a popular code formatter.
For the most part, Prettier works seamlessly with `markdownlint`.
>
>You can `extend` the [`prettier.json`](../style/prettier.json) style to disable
all `markdownlint` rules that overlap with Prettier.

---

_@JonathanPlasse reviewed on 2023-01-30 13:11_

---

_Review comment by @charliermarsh on `.pre-commit-config.yaml`:11 on 2023-01-30 13:13_

Hopefully last question - why is this omitted? 

---

_@charliermarsh reviewed on 2023-01-30 13:13_

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:11 on 2023-01-30 13:16_

Be cause `schemars` formatting conflicts with prettier.
I think it was not the case before.
Let me do a bisect to check if it is the case.

---

_@JonathanPlasse reviewed on 2023-01-30 13:16_

---

_@JonathanPlasse reviewed on 2023-01-30 13:18_

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:11 on 2023-01-30 13:18_

Generated from `schemars`:
```json
"properties": {
    "allowed-confusables": {
      "description": "A list of allowed \"confusable\" Unicode characters to ignore when enforcing `RUF001`, `RUF002`, and `RUF003`.",
      "type": [
        "array",
        "null"
      ],
```
After Prettier formatting:
```json
"properties": {
    "allowed-confusables": {
      "description": "A list of allowed \"confusable\" Unicode characters to ignore when enforcing `RUF001`, `RUF002`, and `RUF003`.",
      "type": ["array", "null"],
```

---

_@JonathanPlasse reviewed on 2023-01-30 13:24_

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:11 on 2023-01-30 13:24_

So I checked, `ruff.schema.json` was never well formatted.

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:11 on 2023-01-30 13:34_

I opened a pull request upstream.
- GREsau/schemars#200

---

_@JonathanPlasse reviewed on 2023-01-30 13:34_

---

_@JonathanPlasse reviewed on 2023-01-30 14:00_

---

_Review comment by @JonathanPlasse on `.pre-commit-config.yaml`:11 on 2023-01-30 14:00_

We could remove `ruff.schema.json` from `exclude` in a future pull request.

---

_@not-my-profile reviewed on 2023-01-30 17:36_

---

_Review comment by @not-my-profile on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 17:36_

I'd rather drop prettier than change our formatting just to cater to prettier ... and it would also help us avoid the dependency on node.

---

_@JonathanPlasse reviewed on 2023-01-30 18:37_

---

_Review comment by @JonathanPlasse on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 18:37_

There is no dependency on node.
You only need to have pre-commit installed.

---

_Review comment by @not-my-profile on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 19:05_

Doesn't pre-commit download and install node behind the scenes?

---

_@not-my-profile reviewed on 2023-01-30 19:05_

---

_@JonathanPlasse reviewed on 2023-01-30 19:10_

---

_Review comment by @JonathanPlasse on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 19:10_

Yes, it does but I do not think it is a burden on the developer experience.

---

_@not-my-profile reviewed on 2023-01-30 19:50_

---

_Review comment by @not-my-profile on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 19:50_

Having to install pre-commit is an additional step and thereby somewhat of a burden. Personally I rather avoid tools that download stuff behind the scenes.

---

_Review comment by @not-my-profile on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 19:52_

And the drawbacks of depending on an entire other ecosystem don't go away just because you install it automatically.

---

_@not-my-profile reviewed on 2023-01-30 19:52_

---

_@charliermarsh reviewed on 2023-01-30 20:03_

---

_Review comment by @charliermarsh on `CODE_OF_CONDUCT.md`:23 on 2023-01-30 20:03_

Yeah I've given it some thought... and I think I'd prefer not to add Prettier as a dev dependency. I'm happy to run Prettier as a one-time thing over the YAML files in this PR, and also to clean up the Markdown a bit (you fixed some good things in the README like trailing space, but we could roll back the `*` changes and such), but I'd rather not add Prettier on an ongoing basis.

---

_Comment by @charliermarsh on 2023-02-02 01:11_

@JonathanPlasse - I'd like to pull in some of these changes, but exclude Prettier for now. Would you like me to do it in a separate PR, or push to this branch?

---

_Comment by @JonathanPlasse on 2023-02-02 17:26_

Hi, sorry for the late response.
I rebase and remove prettier.

---

_Renamed from "Add markdownlint, prettier, and dev ruff to pre-commit" to "Add `markdownlint` and dev Ruff to `pre-commit`" by @charliermarsh on 2023-02-02 21:28_

---

_Merged by @charliermarsh on 2023-02-02 21:29_

---

_Closed by @charliermarsh on 2023-02-02 21:29_

---

_Branch deleted on 2023-04-30 19:47_

---

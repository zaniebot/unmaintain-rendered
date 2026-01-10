```yaml
number: 2501
title: Implement pydowngrade
type: issue
state: open
author: Bobronium
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-02-02T21:58:49Z
updated_at: 2025-05-30T05:57:57Z
url: https://github.com/astral-sh/ruff/issues/2501
synced_at: 2026-01-10T11:09:45Z
```

# Implement pydowngrade

---

_Issue opened by @Bobronium on 2023-02-02 21:58_

First mentioned in https://github.com/charliermarsh/ruff/issues/827#issuecomment-1414314262.

Opposite to pyupgrade: automatically find pieces of code that are incompatible with previous python versions and make them compatible.

This would allow developers to adopt features and syntax from latest python versions while maintaining compatibility with older Python versions by applying pydowngrade rules (each supported version can live in a separate branch, and the whole process can be automated with the right tooling).

While it would be hard (or even impossible) to detect all of the incompatibilities, it should be possible to detect and change common things like new union syntax (`int | str`), generic builtin types (`list[int]`), walrus operator (`if a := randint(0, 1):`), etc.

Idealy, it's not only going to make devs life easier, but also speed up adoption of new language features, which in turn should build a faster feedback loop between average CPython user and its maintainers.

Possible downgrades:
- [ ] union types: `int | str` -> `typing.Union[int, str]`
- [ ] generic builtins: `list[int]` -> `typing.List[int]`
- [ ] walrus operator: `if a := randint(0, 1):`
- [ ] pattern matching: `match foo: case 'bar':` (it should be possible to convert them to raw if statements)?
- [ ] exceptions groups
- [ ] to be continued... (if anybody wants this)

CC @PrettyWood, developer of [future-typing](https://github.com/PrettyWood/future-typing) with the similar idea in mind, but with a different (and quite elegant) approach.

---

_Comment by @Bobronium on 2023-02-02 22:12_

@colin99d, I suggest to continue discussion here, so it wouldn't be noisy to pyupgrade issue subscribers.

> This would take a while to code (probably just as long as coding all the pyupgrade features)

Indeed. I wasn't implying that it's easy to implement. But I'm sure that there's someone who recognizes the value of proposed feature and would love to spend time hacking this together.

> but with a much smaller potential use case.

I disagree. Could you elaborate on that? It potentially can be used by all python libraries that maintaining compatibility with older python versions.

> Some of the lints would not be able to be reverted, for example, if you ran the lint that removes "u" before a string, then ruff would not know which strings to add it back to.

I understand the point, however I don't see this particualr example relevant nowadays since `u` is purely a Python 2 thing. And I can't think of another example that would prove your point. Besides, it shouldn't guarantee 100% compatibility, but allow to seamlesly use most common features. The rest is still maintainer's responsibility.

> I think a much better implementation for this would be to have your CI that uploads pip take the version that works for your oldest members, and then upgrade it to the correct version for each user group downloading it. While this doesn't help the developers, it would mean all users of the code get the newest features.

I think I understood your approach (I've done [something similar](https://github.com/pydantic/pydantic/pull/4339) with the pydantic docs), however, I can't see any substential benefits of this approach vs just leaving the code as is. 

Devs still will be forced to write on the oldest version package supports, and there would be little to no difference to the end user of the library (who cares if it uses all new features, if you're just using its public API which should be the same between versions anyway)?




---

_Comment by @colin99d on 2023-02-02 23:42_

Yeah you have good points. In order for this feature to work well I think it will need to have two things:

1. Really good documentation on which new features work, and edge cases on when they will not work.
2. Some way to test each version that the code is downgraded too. This could take the form of a Github Action.

I still would be curious to know how many people would want this. I could be VERY wrong, but as a maintainer of a large package, I would be scared to have my python code changed by the CI, and then released, even if I had 100% test coverage. However, if its something that a lot of package maintainers want it would be super cool to have.

---

_Comment by @Bobronium on 2023-02-03 08:30_

Couldn't agree more. 

Another potentially less scary and more transparent workflow would include having separate branches for each supported version, that will contain the downgraded code and make release from it.

Sounds tricky to figure out and set up exact workflow, but potential benefits are quite huge, IMO. 

---

_Comment by @Jackenmen on 2023-02-03 10:55_

I think it would be incredibly annoying to work on such a package since packages installed as editable on an older version of Python wouldn't really work (after any edit, you would need to manually rebuild the older version) and personally, I typically use the oldest version when developing and testing so that I can actually ensure that it works on that oldest version.

---

_Label `plugin` added by @charliermarsh on 2023-02-03 18:16_

---

_Label `question` added by @charliermarsh on 2023-02-03 18:16_

---

_Comment by @Bobronium on 2023-02-03 20:00_

@Jackenmen, thanks you for the feedback! I think it's a perfectly reasonable reaction, considering you already have a workflow you seem to be happy with and proposed implementation would conflict with it.

I think it's important to notice such concerns and find ways to resolve them (or at least understand why they can't be resolved).

---

I acknowledge that challenges listed below may be beyond the scope of `ruff`, but nevertheless want to list them here, before I find a better place to reason about it (any suggestions are welcomed!). Besides, they are tightly related to the idea behind this feature request.

---

Overall, I see this challenges in solving this problem:

1. Understanding current approaches for supporting multiple Python versions in app development
2. Developing a new approach that retains the benefits of the current methods, such as ensuring the code works and keeping editable installations simple, while allowing to write code in the newest version
3. Developing a transition strategy to move from current methods to the new approach
4. Engineering a tool to support the new approach and transition.

---

How this might work:

Imagine a package/project/build/publish manager (like Hatch) that would orchestrate all of the release routines for each version, so the workflow might look like this:
1. Write code with newest set of features in main branch
2. In CI, run tests for every supported version
4. When it's time to deploy, run a command that does this for each supported version:
    - Switches to version specific branch, e.g. `releases/3.10`
    - Resets current changes to ones in the main branch
    - Runs `ruff --pydowngrade {version}` (or more generic ruff cmd)
    - Runs tests/mypy/etc.
    - Commits the changes to the branch
    - Builds a wheel
5. And finally a `publish` command that gathers and deploys all built wheels




---

_Comment by @patrick91 on 2023-02-19 20:23_

Just adding my use case here :) I'm working on documentation for a Python library that heavily uses type hints and I'd love to generate documentation snippets for different Python versions.

I think for us we can write snippets using the old syntax, so this it not a big deal, but it would be a nice to have :D 

if I can suggest it I'd keep the scope quite small for this PR, adding support for complex things (like match) would make the code quite complicated and would make Ruff a transpiler, which I don't think it is desirable :D

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:19_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:19_

---

_Comment by @dosisod on 2023-12-31 23:23_

If anyone is curious, I'm building a tool somewhat similar to this called [Pun](https://github.com/dosisod/pun), mainly for porting Python code to PyScript. Among other things, it can downgrade certain `match` statements to `if` statements.

In general, the `match` statement is pretty complex and does a lot of type/sanity checks behind the scenes, which is unfortunately why lots of tools/Python compilers don't support it fully (if at all).

Here are some example test cases of Pun downgrading `match` cases to `if` statements (output is somewhat verbose):

https://github.com/dosisod/pun/blob/41d0e5056784d154a7d1699a3fdb25ea9027e2ad/test/test_match.py

---

_Comment by @ntBre on 2025-01-22 16:45_

Another possible downgrade would be the opposite of UP046 and UP047:

> get linting errors for code that uses newer style generics and should be backward compatible with lower Python versions

i.e removing PEP-695-style generics in favor of standalone `TypeVar`s, as suggested in #12542.

---

_Comment by @asglover on 2025-05-30 02:23_

My use case is pretty simple, I'm writing a python library which should have support for older python versions. A related but not required package I like to have in my dev env requires a somewhat new version of python, so my dev environment uses a newish python (3.12) but ideally I want to lint my code so it all should run on an older version (3.10). 

I initially misread the purpose of the `target-version` setting. I thought it did what this issue is proposing to do. 

I would love the ability to lint to guarantee syntax validity in older python versions. 
I understand if it's not moving forward but wanted to chime in support. 

---

_Comment by @MichaReiser on 2025-05-30 05:41_

@ntBre extended Ruff so that it now detects usages of walrus operators, match statements, etc when targeting older python versions. But it's a preview only feature for now (it might get stabilized in the upcoming minor). 

`int | str` and list[int] is detected by https://docs.astral.sh/ruff/rules/future-required-type-annotation/

So, I think this is now all supported by Ruff (with the exception that we don't provide auto fixes)?



---

_Comment by @asglover on 2025-05-30 05:53_

I  heard from @ntBre  after making this post, it sounds like the detection should be coming in a release soon.

 I think asks in this issue includes  auto-fixes. (although detection is a great improvement, and greatly appreciated) 

People were just discussing somewhat complicated workflows that they wanted this feature for: auto generating different versions of documentation, CI integrations ect. I just wanted to share that there's a somewhat simple use case. Wanting to have your code be valid in a version of python older than the one in your environment. 

---

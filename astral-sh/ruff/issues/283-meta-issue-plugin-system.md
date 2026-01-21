```yaml
number: 283
title: "Meta issue: plugin system"
type: issue
state: open
author: sobolevn
labels:
  - core
assignees: []
created_at: 2022-09-29T09:42:17Z
updated_at: 2026-01-21T14:02:09Z
url: https://github.com/astral-sh/ruff/issues/283
synced_at: 2026-01-21T15:05:33Z
```

# Meta issue: plugin system

---

_@sobolevn_

I think that the main thing why Flake8 is so popular is its plugin system.
You can find plugins for every possible type of problems and tools.

Right now docs state:

```
Beyond rule-set parity, ruff suffers from the following limitations vis-à-vis Flake8:

1. Flake8 has a plugin architecture and supports writing custom lint rules.
```

I propose designing and implementing plugin API.
This way `ruff` can compete with `flake8` in terms of adoption and usability.

## Plugin API

I think that there are some flake8 problems that should be fixed and also there are some unique chalenges that should be addressed.

1. Explicit "opt-in" for plugins. Right now `flake8` suffers from a problem when you install some tool and it has a `flake8` plugin definition. This plugin is automatically enabled due to how `setuptools` hooks work. I think that all rules must be explicit. So, `eslint`'s explicit `plugins:` looks like a better way.
2. Special "fix" API and tooling: so many typical problems can be solved easily by plugin authors
3. Plugin order. Since plugins will change the source code, we must be very strict about what order they run in. Do they run in parallel while checking files? 
4. Plugin configuration: current way of configuring everything in `[flake8]` section can cause conflicts between plugins. Probably, the way `eslint` does that is better
5. Packaging: how to buld and package rust extensions? How to build wheels?

Please, share your ideas and concerns.


---

_Comment by @charliermarsh on 2022-09-29 12:11_

Strongly agree. Will write up some thoughts on this later!

---

_Comment by @charliermarsh on 2022-09-29 20:22_

The first big decision is: should plugins be written in Rust? Or in Python? I believe that either could be _possible_ (though I haven't scoped out the work at all), e.g., using [pyo3](https://pyo3.rs/latest/python_from_rust.html). It _may_ even be _easier_ to support plugins in Python given that loading code dynamically is much easier in a scripted language...

However, I'm partial to requiring plugins be written in Rust. It will lead to a more cohesive codebase, allow us to maintain a focus on performance, and avoid requiring extensive cross-language FFI. I'm open to being convinced otherwise here though.

Here are a few relevant resources on implementing a plugin system for Rust:

- https://nullderef.com/blog/plugin-tech/
- https://github.com/rodrimati1992/abi_stable_crates
- https://zicklag.github.io/rust-tutorials/rust-plugins.html

One of the main challenges seem to be around the lack of ABI stability in Rust. In many of the above write-ups, they discuss how both the plugins and calling library need to use the same versions of Rust in order to be compatible, which feels like a tall order. (From that perspective, one thing that's interesting to me is: could we compile plugins to WASM?)


---

_Comment by @sobolevn on 2022-09-29 20:33_

I think that instead of Python based plugins, it is better to provide some kind of query language to make easy plugins very easy. Like in https://github.com/hchasestevens/astpath

In my opinion, any complex stuff should be in Rust. This way it can reuse existing APIs and be fast. But, I don't know how many Python developers actually know Rust 🤔 

I think another way of dealing with it is to ask exisiting flake8 plugin authors about their prefered way of writting it. Their feedback would be very valuable!

---

_Comment by @charliermarsh on 2022-09-29 23:08_

Interesting, Fixit / LibCST has something kind of like that too. It's not quite a distinct query language, but it's effectively a DSL (in Python) to pattern-match against AST patterns.

---

_Comment by @thejcannon on 2022-10-25 15:25_

My 2c. `flake8`'s plugin support is pretty rudimentary ([operates on tokens/lines plus a handfull of metadata](https://flake8.pycqa.org/en/latest/plugin-development/plugin-parameters.html)). Therefore if you supported Python plugins, you likely could craft it in a way that supporting `flake8` plugins out-of-the-box(-ish) would be feasible. 

Then you could have most flake8 plugins available through `ruff`, and wouldn't __need__ to support new plugins by porting the code to Rust (only if you want to because yummy yummy perf).

(lightly related to https://github.com/charliermarsh/ruff/issues/414)

---

_Comment by @charliermarsh on 2022-10-27 12:21_

Another idea: we could build a plug-in system atop https://github.com/ast-grep/ast-grep. This would allow users to express lint rules in YAML or via a simple DSL.

---

_Comment by @charliermarsh on 2022-10-27 12:29_

(That tool is itself built atop tree-sitter.)

---

_Comment by @ljodal on 2022-11-08 21:18_

I’m coming to this from a position of having written a flake8 plugin for a very specific need at work, and as part of a larger project. This is not something generic, so it’d never make sense as a built-in feature in ruff. I’d love a way to write plugins to ruff in Python, mostly because it’s convenient as someone familiar with Python and not so much rust, but also because it would be nice to keep a project in pure Python even while interacting with rust.

The specific plugin in my case, [oida](https://github.com/kolonialno/oida), was first written as a standalone thing before I discovered how easy it was to add it as a plugin to flake8. It also uses LibCST, for its ability to round trip code, where we do codemodding for its. If it would be possible to expose a similar ast based Python-interface for plugins that would be awesome.

I also have use cases where I’d like to do auto fixing, which it would also be nice to support. The first thing I’d like to do is normalize import statements (relative vs absolute). In order to do that I’d need an interface where I get import statements or ast nodes and the path to the file so I can locate it in relation to other files on the system.

I understand that writing plugin in Python would be a slowdown compared to writing them in rust, but I think that tradeoff would be very much acceptable in many cases.

---

_Comment by @charliermarsh on 2022-11-08 21:28_

Very helpful and all makes sense.

Maybe just as another data point for the thread: when I was at [Spring Discovery](https://springdiscovery.com/), we wrote a few Flake8 plugins to enforce highly codebase-specific rules.

For example:
- "Always late-import TensorFlow" (i.e., import it within a function that depends on it, rather than at module top-level)
- "If you ever import module X, make sure the file _also_ imported module Y"
- "Imports to module Z should always use `import from` structure"


---

_Comment by @charliermarsh on 2022-11-08 21:29_

So in that light, I think there are different categories of plugins:

- Plugins that are custom to a codebase
- Plugins that may apply to many codebases, but don't make sense to include directly in Ruff (e.g., Django-specific stuff could qualify here)


---

_Comment by @charliermarsh on 2022-11-08 21:30_

I think most of those "custom" plugins / checks could be built atop something like [`ast-grep`](https://ast-grep.github.io/), but more complex checks (like rewriting absolute and relative imports) would be limited by that approach.

---

_Comment by @charliermarsh on 2022-11-08 21:30_

> The first thing I’d like to do is normalize import statements (relative vs absolute). In order to do that I’d need an interface where I get import statements or ast nodes and the path to the file so I can locate it in relation to other files on the system.

(Separately: this could arguably make sense to include in Ruff directly.)


---

_Comment by @ljodal on 2022-11-08 22:03_

> I think most of those "custom" plugins / checks could be built atop something like [`ast-grep`](https://ast-grep.github.io/), but more complex checks (like rewriting absolute and relative imports) would be limited by that approach.

Yeah, I wouldn’t really be able to implement any of Oida using ast-grep, as all the rules depend on the context of the project. I use in-process caching to keep that state ready between files in the current flake8 plugin btw, forgot to mention that above, so the flake8 interface isn’t ideal for that kind of plugin.


> > The first thing I’d like to do is normalize import statements (relative vs absolute). In order to do that I’d need an interface where I get import statements or ast nodes and the path to the file so I can locate it in relation to other files on the system.
> 
> (Separately: this could arguably make sense to include in Ruff directly.)

I guess some rules could be, again not for my specific case. What we’re considering at work is to enforce relative imports within a Django app and use absolute imports for everything else. Our structure will be `project.component.app` or `project.app`, so it would be very specific to our use case how that rule should be applied. I’ve already played with implementing it in `isort`, but I found that code base hard to navigate and would love a clean ast/cst based plugin interface where I could add this logic :)

---

_Comment by @peterjc on 2022-11-14 20:30_

You asked for feedback from other flake8 plugin authors, so:

* https://github.com/peterjc/flake8-black (620k downloads/month on PyPI), not needed if you can run black directly as well as running flake8, for example via the tool pre-commit or otherwise. Currently this reloads each Python file from disk (scope here to refactor to let black use its cache), it would not be possible to use the AST from flake8 directly. Does not make sense to plug into ruff.
 
* https://github.com/peterjc/flake8-rst-docstrings/ (238k downloads/month on PyPI), uses the AST to extract docstrings, which are passed as strings to the Python library ``docutils`` to be validated as RST. My code is essentially a wrapper, and since ``docutils`` is written in Python that would have to be used internally if this plugin were to be ported to ruff. 

* https://github.com/peterjc/flake8-sfs (15k downloads/month on PyPI), uses the AST directly looking for particular kinds of node. Probably could be done in either Python or Rust, although unlikely to be popular enough to deserve including in ruff itself.

---

_Comment by @charliermarsh on 2022-11-15 02:35_

Thank you @peterjc! Really appreciate your engagement here as a plugin author!

(Regarding RST: it looks like there's at least one [Rust crate](https://github.com/flying-sheep/rust-rst) for parsing RST, though it doesn't look super popular.)


---

_Comment by @ofek on 2022-11-15 03:00_

Is this possible, or supported currently? https://github.com/adamchainz/flake8-tidy-imports

---

_Comment by @charliermarsh on 2022-11-17 15:07_

@ofek - Not currently supported but it’s a pretty small surface area so should be easy to add some time in the next few days.

---

_Comment by @ofek on 2022-11-17 15:21_

Thanks! I've been enforcing absolute imports recently (except in tests) https://github.com/pypa/hatch/blob/b0911bb0eaa8d331c24eda940b97bf244ecd5ac3/.flake8#L8-L11

After that I'll switch over, and make new projects generated by Hatch use this.

---

_Comment by @charliermarsh on 2022-11-17 15:39_

Sweet! The banned relative import rule I can definitely do today.

---

_Comment by @charliermarsh on 2022-11-17 17:39_

@ofek -- `I252` (banned relative imports) just went out in `v0.0.125`.

You can use it in Hatch by adding this to your `pyproject.toml`:

```toml
[tool.ruff]
select = [
  "B",
  "C",
  "E",
  "F",
  "W",
  # Ruff doesn't have this, but it does have E722.
  # "B001",
  "B003",
  "B006",
  "B007",
  # These don't exist in newer flake8-bugbear versions IIUC.
  # "B301",
  # "B305",
  # "B306",
  # "B902",
  "Q000",
  "Q001",
  "Q002",
  "Q003",
  "I252",
]
ignore = [
  "B027",
  # "E203",
  # "E722",
  # "W503",
]
line-length = 120
# tests can use relative imports
per-file-ignores = {"tests/*" = ["I252"], "tests/**/*" = ["I252"]}

[tool.ruff.flake8-tidy-imports]
ban-relative-imports = "all"
```

Let me know if it works, or doesn't! :)


---

_Comment by @ofek on 2022-11-20 17:55_

Thank you!!! https://github.com/pypa/hatch/pull/607

---

_Comment by @ljodal on 2022-11-28 17:09_

@charliermarsh You wrote somewhere that libcst is significantly slower than the current ast implementation in ruff (can't find it right now). Do you know why? Is it because it's a cst or is it because the classes it exposes are Python "compatible"?

I'm asking because I've started looking into pyo3 and from what I see the only way to expose an ast to a Python plugin would be to make the ast classes Python classes in pyo3. If that's what's slow with libcst I guess there's not really any point in investigating that route too much, but if we could make that fast enough I guess it could be one way to make plugins work.

That doesn't resolve auto-fixing, but as I suggested in another thread I think maybe doing auto-fixing on the token level could be made to work. Maybe an interface like this:

```python
def visit_Import(node: ast.Import, tokens: list[str]) -> list[str]:
    # Check ast (or tokens) for violations and return updated token
    return ["import", " ", "foo"]
```

Or maybe have tokens as an attribute on the ast nodes 🤔



---

_Comment by @charliermarsh on 2022-11-28 18:48_

@ljodal - This was all based on LibCST as a Rust crate, with no Python FFI -- so I _think_ it's just the CST and parser, and not anything to do with the the serialization. (I also hacked in some RustPython vs. LibCST benchmarks into the existing LibCST `benches` and got similar results. As with all benchmarking, though, I could definitely be doing something wrong!)

---

_Comment by @charliermarsh on 2022-11-28 18:49_

@ljodal - I don't have great intuition for whether the PyO3 FFI would add much overhead and what the performance impact would be. I think it's worth exploring!

---

_Comment by @ljodal on 2022-11-29 08:30_

Aight, then I'll continue investigating :) 

I haven't written any rust before, so it's slow going (thinking of doing advent of code in rust to get a kickstart). My plan was to use the Python ASDL definitions to generate AST classes, but it's been years since last I touched compilers so I'll have to see how I go about the tokenization and conversion to ast

---

_Comment by @charliermarsh on 2022-11-29 21:12_

You might be interested in some of the stuff in RustPython -- they generate the Rust AST definitions from an ASDL file [here](https://github.com/RustPython/RustPython/blob/main/compiler/ast/asdl.py) and [here](https://github.com/RustPython/RustPython/blob/main/compiler/ast/asdl_rs.py).

---

_Comment by @ljodal on 2022-11-29 22:00_

Nice, thanks for the links, I'll take a look :) I guess if I base it on that and just tweak it to be Python compatible it should be fairly easy to see the overhead as well

---

_Label `core` added by @charliermarsh on 2022-12-31 18:20_

---

_Comment by @sbdchd on 2023-01-04 02:42_

> Another idea: we could build a plug-in system atop https://github.com/ast-grep/ast-grep. This would allow users to express lint rules in YAML or via a simple DSL.

something like this would be useful to replicate eslint's https://eslint.org/docs/latest/rules/no-restricted-syntax which is handy for little one off things

---

_Comment by @rmorshea on 2023-01-11 01:35_

As a flake8 plugin author myself who is not presently acquainted with Rust, I think it's important that Ruff's plugin system support plugins implemented in Python. While I understand the benefits of simplicity and performance that would come with requiring plugins to be implemented in Rust, I suspect that the eventual ecosystem of Ruff plugins would ultimately suffer for it. I think this because Ruff is a linter for Python and as such, many, and probably a significant majority of its users, will come to Ruff not knowing Rust. As a consequence, if only Rust plugins are supported, when Ruff users who are not familiar with Rust find a need or idea for a new plugin, in the case of the former they may switch to Flake8, and in the case of the latter they may do the same or just not develop a plugin at all. Chances are, prospective plugin authors care first about having something that works and only second about whether or not it is performant. Thus, it seems prudent to support the tools that Ruff's future plugin authors likely already know. That being Python.

My own personal anecdote is that I'm the author of a [Flake8 plugin](https://github.com/idom-team/flake8-idom-hooks) for another one of [my projects](https://github.com/idom-team/idom). Both these projects are small in scale at the moment, so take this with a grain of salt, but I think it's unlikely that I would author a plugin for Ruff if I had to do it in Rust unless and until Ruff became more popular than Flake8. While I like the idea of learning Rust in order to port my plugin, I just don't think I could justify taking the time to do so. Rather, I would prefer to maintain a generic set of linting heuristics that I could use in both my Flake8 plugin and a potential Ruff plugin. Doing so would both save on maintenance effort and allow users to choose the linting tool of their choice. While my plugin and project don't have many users, I expect that my rational would probably be even more applicable to large projects with maintainers who have even less time to spend supporting what is at present, a fairly niche linting tool like Ruff.

With all this said, if Ruff supported plugins in Python and Rust, I think Ruff plugins could be a gateway to learn Rust (if an unlikely one) since authors of Python plugins like myself may find a need or demand for performance in the future.

---

_Comment by @sondrelg on 2023-01-19 16:38_

I maintain a [plugin](https://github.com/snok/flake8-type-checking) that I would like to rewrite in rust, to be able to run it with `ruff`. 

I just wanted to ask, what happens to flake8-plugins ported to `ruff` that need significant upgrading/maintenance for, e.g., new python versions; who has the maintenance burden once it has been ported? :slightly_smiling_face:

Would it be possible to port it to `ruff` (if deemed generally useful), then split out as a plugin if/once the architecture for that is in place? 

---

_Comment by @charliermarsh on 2023-01-19 16:52_

Oh rad! It looks like a great plugin -- a bunch of people have asked for it (https://github.com/charliermarsh/ruff/issues/1785), I was actually gonna look into porting it soon myself given all the demand. But if you're up for owning the port, and / or willing to help out, that would be amazing, and I'd be happy to support you however I can.

> Who has the maintenance burden once it has been ported?

This is a great question. Anything that we merge into Ruff (and that stays in Ruff), I'm signing up to be backstop maintainer. If other contributors are able to help maintain, merge in improvements, fix bugs, and support those bigger upgrades, it's obviously much appreciated and welcome. But I'm not merging in plugins with the expectation that others are required to maintain them in perpetuity.

(This means there are some limits on what we'd merge into Ruff. I haven't defined those yet since, frankly, I haven't seen anything proposed yet that feels out of place.)

A little off-topic, but it might be useful to note that, because of the "bundling" that we're doing with Ruff, plugins can actually share a bunch of functionality, which in some ways makes them easier to maintain as a group. For example, `pydocstyle` and `flake8-unused-arguments` both rely on the ability to determine whether a function or method is public or private. In Ruff, we can use a single mechanism for that tracking, and share the inferred visibility with the individual rules. As another example, I'd hope that some of the stuff we have around import tracking and annotation detection would be useful for `flake8-type-checking`.

> Would it be possible to port it to `ruff` (if deemed generally useful), then split out as a plugin if/once the architecture for that is in place?

Yes, absolutely.


---

_Comment by @sondrelg on 2023-01-19 19:09_

Ah how cool! Glad I stumbled onto this then :partying_face: I'll direct remaining questions to that issue :+1: 

---

_Comment by @scop on 2023-01-23 21:36_

When I found out about ruff last week, two things resonated with me immediately:
1) The uncanny performance, obviously :rocket: 
2) No need to look for a plethora of $linter plugins, and spend time keeping them working e.g. across breaking API changes, Python versions, etc.

I realize there are good use cases for plugins (specific/private use cases perhaps the best one), but the fewer of them the merrier, as long as their functionality is available and the ecosystem thrives, all corners get the love they need, things end up in hands of users as quickly as they should etc. There are limits to how far that can scale, and not all itches can ever be scratched, but for the time being people working on ruff seem to be doing an awesome job on this front, too.

I'm not arguing against a plugin system, but rather just hoping that the proliferation of plugins there is in the traditional Python linter scene doesn't repeat itself here. Having read the above commentary, I've no reason to think it will :+1: 

---

_Comment by @yakMM on 2023-01-24 09:43_

>     2. No need to look for a plethora of $linter plugins, and spend time keeping them working e.g. across breaking API changes, Python versions, etc.

Yes! This is part of what makes ruff an amazing project imo. You can just replace so many dependencies (and their sub-dependencies) with one self-contained package that already have (almost) all the rules you need available.



---

_Comment by @mwgamble on 2023-01-25 01:12_

In the Javascript world, when using eslint, there are a myriad of plugins to support various use cases. Some of those are about enabling Typescript support, some about integrating prettier, some about strictness, etc etc

There is also a tool named `xo` that essentially wraps all of this up into a set of opinionated defaults, that automatically configures all of the above for you. I've started using this in my Typescript projects, and am loving it.

I would personally prefer to see an `xo`-style tool for python that wraps flake8, black and isort, but still provides all of the power of those things. People are inevitably going to want custom plugins for various reasons, and a good tool shouldn't block that IMO. I also don't want to be beholden to the decisions of a single dev team to decide what linting rules I should and shouldn't have, who also has to weigh up whether or not it's worth breaking peoples' builds to introduce some new checks (once things have stabalised a bit).

Unfortunately the big blocker for this happening with flake8 right now is the lack of a good configuration system. Eslint's config system is very powerful and supports presets. This empowers people to wrap it effectively without reducing the power of the underlying system.

---

_Comment by @charliermarsh on 2023-01-25 02:51_

I appreciate all the input here :) 

I'll just chime in briefly to say a make a few quick comments:

- I do see the "bundling" that's happening in Ruff as a feature and not a bug. The ability to replace multiple tools with one unified tool has resonated a lot with users! That makes me happy and has led me to continue pushing in that direction.
    - (There are some "economies of scale" here too: if two rules need a notion of public / private visibility tracking, or docstring detection, they can share that logic. This wouldn't be _impossible_ to achieve with a plugin system, but it's certainly harder to anticipate all the data a plugin might need.)
- I would still like to support third-party plugins in some form.
    - Broadly, I could see two APIs for plugins: (1) some kind of DSL, as in [ast-grep](https://ast-grep.github.io/), that doesn't require writing code, but is more limited in what you can flag; (2) a Rust and/or Python API, similar to the plugin systems you see in tools like ESLint, Babel, Rollup, etc.
- However, given the success we've had with our current approach, plugins aren't a top priority for me right now.
    - Introducing a plugin system will also put a lot of constraints on the project. As soon as we have non-Ruff code that depends on any sort of public API, we'll be far more limited in the kinds of changes and refactors we can make. So I don't even want to start hacking on a plugin system until we're confident that the API won't see significant churn.

All this is to say that I would like to support plugins at some point, but it likely won't happen soon enough for me to put any sort of timeline on it.


---

_Comment by @muglug on 2023-03-06 05:52_

Offering input from someone who doesn't use Ruff or Python, but who has rewritten a static analysis tool (with a large plugin ecosystem) in Rust that was originally in an interpreted language, I think you should stick with Rust as much as possible for plugins.

The path I took — wrapping the Rust tool in a custom wrapper that uses documented plugin hooks — probably doesn't make sense for a tool with this large a community, because it would require users recompile ruff.

SWC uses [an alternative system](https://swc.rs/docs/plugin/ecmascript/getting-started) — plugins are written in Rust and run using WASM, which is slightly slower but not as slow as firing up the Python interpreter. Colleagues have written one or two internal SWC plugins, allowing us to migrate away from slower Node-based tools.

---

_Comment by @zzzeek on 2023-03-07 19:30_

Ideas department:

[RustPython](https://github.com/RustPython/RustPython) is a full Python interpreter written Rust and is embeddable in Rust programs.   Ruff could **optionally** embed this interpreter in order to use outlier flake8 plugins.  If no such plugins are needed then the interpreter wouldn't be loaded. 

I think being able to use existing flake8 plugins as is, as well as to be able to write plugins in Python in general, would be a big win.  

---

_Comment by @smackesey on 2023-04-10 13:39_

Just chiming in here to say that over at [Dagster](https://github.com/dagster-io/dagster) the ability to write custom ruff lint rules would be extremely useful. Would also be fine if it required rust.

---

_Comment by @AngellusMortis on 2023-05-02 06:47_

I definitely think support for Python based plug-ins is critical. Probably more important then Rust plugin support (at least for prioritization and initial support, not saying never implemente Rust support). 

The repo / open source community has already showed that the tool itself is willing to accept a ton of popular plugin linting rules. So if the rules are open source, they can be written in Rust and added right into the repo without a plugin. 

However, there are going to be a lot of rules that are not open source or popular. Especially ones written by orgs. The bar to entry here probably needs to be as low as possible. What if the org/devs do not know Rust? They know Python for sure since this is a Python linter.

---

_Comment by @flying-sheep on 2023-06-19 09:12_

> [peterjc/flake8-rst-docstrings](https://github.com/peterjc/flake8-rst-docstrings/) (238k downloads/month on PyPI), uses the AST to extract docstrings, which are passed as strings to the Python library docutils to be validated as RST. My code is essentially a wrapper, and since docutils is written in Python that would have to be used internally if this plugin were to be ported to ruff.

> (Regarding RST: it looks like there's at least one [Rust crate](https://github.com/flying-sheep/rust-rst) for parsing RST, though it doesn't look super popular.)

Yeah, my crate hasn’t received much love, which I think is due to rST in general not receiving much love, especially outside the python community.

I’m not sure if it would be up to the task of validating rST grammar, since there is no formal rST grammar, so I had to do an ad-hoc one, which is not finished. My crate shares that issue with other (equally partial) rST implementations.

---

_Comment by @hajs on 2023-07-27 19:03_

Hello, the following tutorial about ruff mentions some plugin mechanism, which is not documented anywhere else: 

https://dev.to/ken_mwaura1/enhancing-python-code-quality-a-comprehensive-guide-to-linting-with-ruff-3d6g#creating-custom-linting-rules-in-ruff

Can anyone confirm or deny this feature? I doubt that this issue is outdated.


---

_Comment by @charliermarsh on 2023-07-27 19:09_

I can confirm that the mentioned feature doesn't exist. I'm not sure where that code snippet came from, but we don't have a Python API or support custom rules like that.

---

_Comment by @rmorshea on 2023-07-27 21:45_

Seems like something an AI would generate.

---

_Comment by @flying-sheep on 2023-07-28 08:23_

This reddit thread has a few approaches for plugins: https://www.reddit.com/r/rust/comments/144zmwk/how_can_i_add_dynamic_loading_to_do_plugins_for/

The options are basically

- Rust-to-Rust by using an ABI stable type crate 
- Some other language interface like WASM or C
  - There’s wrappers like [Extism](https://extism.org/docs/integrate-into-your-codebase/rust-host-sdk/) for WASM
- Some embedded scripting language like Lua or one of the Rust ones like Rhai or dyon

---

_Comment by @obi1kenobi on 2023-07-28 15:56_

Another option is to embed just a query engine, as a lighter-weight option than a full scripting language. I've built such an engine ([Trustfall](https://github.com/obi1kenobi/trustfall)) and several linters like this ([cargo-semver-checks](https://github.com/obi1kenobi/cargo-semver-checks) for linting semver in Rust, [company-internal linter](https://www.hytradboi.com/2022/how-to-query-almost-everything) for internal lints).

The query engine route makes it possible to separate lints (business logic) from implementation details (how is the AST stored under the hood, etc.). Specifying lints as declarative queries makes maintainability and performance optimization easier: `cargo-semver-checks` recently [became 2000x faster without any changes to lints](https://predr.ag/blog/speeding-up-rust-semver-checking-by-over-2000x/).

It also makes it easy to spin up a playground without worrying about someone injecting `eval()`. Here's a playground where you can query the contents of popular Rust crates as well as internals like `std` and `core`: https://play.predr.ag/rustdoc
![image](https://github.com/astral-sh/ruff/assets/2348618/b3c0b5c2-ab4f-4b2b-b85c-8b70b78f29c1)

The `oxc` Javascript linter also [recently adopted Trustfall](https://github.com/web-infra-dev/oxc/tree/main/crates/oxc_query) as an approach for both internal and custom lints. Here's an example Trustfall-based lint from `oxc`: https://github.com/web-infra-dev/oxc/pull/627/files#diff-23497bb392f82d20572ed7744d6d0b80061ff480d9e4c913792cf2c911bde5cd



---

_Comment by @zanieb on 2023-07-28 22:18_

There is also a lot of interesting discussion about plugins in the Rust ecosystem over at https://github.com/helix-editor/helix/discussions/3806

It may be essential for us to allow plugins to be authored in Python, regardless of the machinery between the Python API and our Rust API.

---

_Comment by @adam-azarchs on 2023-09-25 22:08_

Definitely not a mainstream idea but just wanted throw this out there for consideration: you could consider using [starlark](https://github.com/bazelbuild/starlark) for plugins.

The language is essentially a [non-Turing-complete subset of python](https://github.com/bazelbuild/starlark/blob/master/design.md) with strong sandboxing and safe multithreaded execution, designed for having an interpreter embedded in another hosting process.  There is in fact an [interpreter implementation for rust](https://github.com/facebookexperimental/starlark-rust).

In some ways this would provide the "best of both worlds" in allowing you to keep a near-python syntax while avoiding many of the performance and maintenance issues inherent to supporting python plugins.  For one thing, you don't need to be ABI-compatible with python, since you'd be self-hosting the interpreter.  The main downside would be lack of availability of arbitrary python packages, but from a performance perspective that's probably a good thing.

The main differences between starlark and python:
1. Top-level variables (including functions) are frozen after import, and are single-assignment.  This makes concurrent execution easy, since there can be no mutable state shared between invocations of a function; something that `ruff` would I think very much want to be able to take advantage of.  It also permits certain forms of ahead of time "compilation" and static checking which are impossible to do reliably in a language as dynamic as python, e.g. checking for undefined names during load rather than at runtime on every reference.
2. No try/except.  Errors are always hard failures.  This significantly simplifies the runtime and again allows for more "precompilation".
3. Disallowing of various bug-prone patterns like modification during iteration.
4. No unbounded loops.  `for x in y` is allowed but no `while` loops and, by default, no recursion.  This is a little awkward at times but allows the runtime to ensure that all starlark programs will eventually terminate.
5. No OS access out of the box.  While the hosting interpreter can expose methods for things like reading files, none is provided by default, meaning it should be safe to run untrusted starlark code.  It also prevents plugin authors from "going around" your provided APIs.

Ultimately, I don't think any of this would provide much of a benefit over and above using WASM for plugins.  WASM already enables users to write their code in any language of their choice that supports wasm as a compilation target.  However, python is not one of those languages, and as has been pointed out, most `ruff` users work primarily, if not exclusively, in python, so having something at least near-python may have some value. It still wouldn't enable things like `flake8-rst-docstrings` delegating out to a python `rst`-parsing library, but personally I would consider that to be a good thing, as python dependency trees can quickly grown out of control and become difficult to maintain and keep up to date.

---

_Comment by @ofek on 2023-09-25 22:22_

If we do go down the Starlark route, the PyOxidizer project(s) can serve as an extensive example of usage in Rust.

---

_Comment by @obi1kenobi on 2023-09-25 22:27_

Small update if you might be considering Trustfall: at RustConf last week, @estebank and a few other folks expressed interest in using Trustfall to query Rust HIR as a way to support custom lints for Rust 👀

---

_Comment by @Gnosnay on 2023-12-14 10:00_

learned a lot from this long thread. May i know for now, if we wanna define our own syntax linter check with ruff, how should we do? 
If anyone can give one way, i will very appreciate it

---

_Comment by @monotkate on 2024-02-13 23:00_

It sounds like the jury is still out when it comes to creating custom rules, is that correct? I've seen custom linting rules be a valuable tool when modularizing a monolith, with rules very customized for the codebase you're working in. As far as I can tell, Ruff does not allow you to develop custom rules at this time, so we'd have to run another linter alongside Ruff for that ability.

We just switched our codebase to using Ruff, and are also looking to start modularizing. I'm trying to figure out if I need to chose a second tool alongside Ruff for customizations.

---

_Comment by @adam-azarchs on 2024-02-13 23:25_

I think the main point of contention is not whether it should be allowed but rather _how_.

IMO if people want to author a plugin in python they should probably use a python-based tool (e.g. `flake8` or `pylint`) to run it.  One of the things I like about `ruff` is that it doesn't have a dependency on a python runtime, and not unrelatedly that it is very fast.  A plugin architecture for `ruff` would be nice, certainly, but I'd advocate for it being either native plugins (e.g. `.so` libs that can be dynamically loaded into the process and register themselves), WASM, or maybe some kind of DSL (ast-grep was mentioned earlier).

I strongly suspect that a DSL would be sufficient for 80+% of the kind of use cases people are describing where a repo has rules very specific to their code base that wouldn't be sufficiently broadly applicable to upstream.  Especially nice about that is that such custom rules could just be included in the `pyproject.toml` (toml is isomorphic to yaml, though it does get awkward compared to yaml for more deeply nested structures).

---

_Comment by @morgante on 2024-02-21 12:02_

We've been working with Biome to [integrate GritQL](https://github.com/biomejs/biome/discussions/1762) as an extension/plugin system and I'd love to offer the same for Ruff. The problem space is similar and I think GritQL provides a few advantages:
- Preserve some of the best things about Ruff: no runtime Python dependency, pure Rust, no separate installation steps, etc.
- Most custom/codebase-specific rules can be expressed as simple AST-based transforms.
- Traversal still happens in pure Rust and, because declarative queries are used, they could be optimized to maintain Ruff's excellent performance

Here's a few example of how @charliermarsh's earlier custom suggestions could be implemented directly:

- Always late import TensorFlow - [studio](https://app.grit.io/studio?key=C0EFJmLny6mTzygd_fFkI&preset=print_to_log)
  ```
  `import $import` where {$import <: contains `tensorflow`, $import <: not within block()}
  ```
- If you ever import module X, make sure the file also imported module Y - [studio](https://app.grit.io/studio?key=xriEV93BGjz0RUZwpNUET)
  ```
  `import $import` where {
      $import <: contains `moduleX`,
      $program <: not contains `import moduleY`
  }
  ```
- Imports to module Z should always use import from structure - [studio](https://app.grit.io/studio?key=8NZm7iMaG7noDYqh_NgCd)
  ```
  `import $import` where {$import <: contains `module`, $import <: not within `from $_ import $_`}
  ```

---

_Comment by @JobaDiniz on 2024-05-14 12:57_

I'm looking to write a few [fitness functions](https://www.thoughtworks.com/insights/articles/fitness-function-driven-development) by extending ruff linters. I think that would be ideal, and I'd like to use `python`.

Since ruff does not support plugins, I'm writing these functions as tests that run on CI using `pytest`, but these specific fitness functions I'm writing are linters, so it would make sense to write them as part of ruff linters.

---

_Comment by @jhosmanfriasbravo on 2024-05-22 16:25_

@charliermarsh hi! do you think that ruff will consider a plugin system in the short-medium term?

---

_Comment by @MichaReiser on 2024-05-28 13:40_

Thank you, @morgante, for offering your support to help us build a GritQL-based plugin system. 

GritQL is undoubtedly at the top of my mind when it comes to designing a plugin system for Ruff, and I'm following the work in the Biome repository from a distance (but I must admit, not very closely). 

It will probably be a while before we evaluate solutions for a plugin system because we're currently in the middle of rewriting Ruff's compiler infrastructure to support multifile analysis (and more ;)).  But I'll come back to your offer when we're ready to explore Ruff plugins.

---

_Comment by @ssbarnea on 2024-06-26 07:34_

One flake8 plugin that could be very useful to be covered by ruff would be [pydoclint](https://github.com/jsh9/pydoclint/). Even having external plugins, like called as shell commands would prove very useful, speed should not be no1 priority. In time plugin authors might rewrite them in rust, but for start a way to hook external ones would prove very useful.

---

_Comment by @adam-azarchs on 2024-06-26 08:20_

I am far from convinced that there's value in having `ruff` launch such external processes, including hosting a python runtime (which while technically could be in-process, there wouldn't really be much of an advantage over forking it as a subprocess).  The primary value that `flake8` gets out of integrating a lot of plugins is that they all can share the same parsed AST representation, to avoid redundant work.  Likewise with `ruff`'s integrated linters.  That would not be the case for `ruff` plugins unless those plugins were also written in rust, or some other language that could consume ruff's in-memory representations with limited or no transformations.

It sort of sounds to me that what some people are looking for is a way to run a bunch of arbitrary checkers on python files in a repository, and they don't _really_ care whether those commands are integrated into the same executable or not. If that's what you're looking for, maybe look at something like `pre-commit`?  `pre-commit` is quite good at that job.  `ruff` just feels like the wrong layer for doing that - it's a thing that gets run by pre-commit, and shouldn't be trying to do the same job.

---

_Comment by @flying-sheep on 2024-07-25 21:16_

A new blog post investigating Rust plugin systems, probably helpful! https://benw.is/posts/plugins-with-rust-and-wasi

---

_Comment by @2bndy5 on 2024-08-30 19:07_

FYI, using external processes (like a rust-based standalone binary or a python-based `entrypoint` script) is how [mdbook implements plugins for preprocessing Markdown](https://rust-lang.github.io/mdBook/for_developers/preprocessors.html#hooking-into-mdbook). This is how mdbook also supports [plugins implemented in python](https://rust-lang.github.io/mdBook/for_developers/preprocessors.html#implementing-a-preprocessor-with-a-different-language) or possibly other languages. If designed well enough, one could write a ruff plugin that also acts as a standalone linter. This means adding unsupported rules (from linters that ruff is non-compliant with) could be implemented externally in the (unsupported) third-party linter.

---

_Comment by @carlpaten-ivadolabs on 2024-09-04 13:42_

Members of my team have been agitating for Ruff, but without support for custom rules it's a tough sell. We need to be able to implement bespoke rules to support our own coding style - stuff like forbidding top-level statements not guarded by `if __name__ == "__main__":`, for example. We have the choice between maintaining our own fork of Ruff, with limited in-house Rust experience, or use Flake8 + Black.

---

_Comment by @Dreamsorcerer on 2024-09-04 14:06_

> We have the choice between maintaining our own fork of Ruff, with limited in-house Rust experience, or use Flake8 + Black.

Or (and I don't necessarily recommend it), Ruff + Flake8 (using the external config option for rules handled by Flake8).

---

_Comment by @muglug on 2024-09-04 14:24_

> Members of my team have been agitating for Ruff, but without support for custom rules it's a tough sell.

If they want to monetise what they’ve built (and add this as a paid feature) then Astral might welcome this sort of feedback, but otherwise it feels a little off-key for a free tool with a permissive open-source license.

---

_Comment by @bverhoeve on 2024-09-17 09:18_

I have the same problem as @Dreamsorcerer for my team. `flake8` + `black` works fast enough for us, so the speed of `ruff` isn't a good enough trade off for the lack of custom plugins.

---

_Comment by @jd-solanki on 2024-10-07 05:38_

Along with plugin system I would like to share great idea for beginners to lint their code who don't know how to write linting rules using AST or something else.

"Lint using regex"

In JS, ESLint has plugin system and we used to write custom rules for our solutions. We wrote some rules using official rules docs but using regex to lint the code allowed all of us in our team to writing linting rules without any additional learning.

Regex plugin that allowed beginner devs to write linting rules: https://www.npmjs.com/package/eslint-plugin-regex

---

_Comment by @chadrik on 2024-10-12 00:12_

I had a look at ast-grep and it's pretty fantastic.  It is fast, it is written in rust and provides multiple high-level APIs, including both python and YAML.  But it has one giant problem:  it doesn't preserve comments, making it pretty _useless_ for real-world code manipulation.  

Would it be possible to publish ruff's python ast parser as a crate, which ast-grep could be modified to use, and then ruff could use ast-grep's high level APIs for its plugin system?



---

_Comment by @dgilmanAIDENTIFIED on 2024-11-19 15:27_

It hasn't been mentioned explicitly in this thread but a plugin system gives the opportunity to lint non-Python languages that have been embedded in a Python file. This does show up in Python files somewhat often (rST in docstrings, SQL string literals) but implementing those formatters is clearly out of scope for ruff itself.

It still makes sense to have ruff as the host for these linters, though: you'd get to take advantage of ruff's configuration (file exclude rules), you could piggyback on the `# noqa` functionality, and I think there's even an opportunity to extend the linter comment syntax for string literals: something like `# lint: mysqlplugin` to indicate which string literals you want linted.

Even when you're just linting a string literal you may want to expose the AST to that linter. A SQL statement might want to know what level of indentation a string literal was declared at to match vertical alignment:

```
def foo():
    x = """
select *
    """
```

could be linted/formatted into

```
def foo():
    x = """
    select *
    """
```
 
But maybe there is also opportunity to have a simpler "if this string literal is annotated for my linter (`# lint: mysqlplugin`), pass just the string literal's body to the plugin" API, or at least provide the AST-consuming glue for that as sample code.

I see a bit of discussion in this thread over forking vs in-process processing: if you are providing an AST or AST-lite API you can let the plugin decide if they want to fork or not. It would be unfortunate for performance if they had to fork but it is entirely possible that a plugin might want to use an existing linter and be practically constrained to fork to run it.

---

_Comment by @MichaReiser on 2024-11-19 16:29_

@dgilmanAIDENTIFIED supporting embedded languages that are commonly used with Python is a long-term goal. See https://github.com/astral-sh/ruff/issues/8237

---

_Comment by @bverhoeve on 2024-11-20 07:54_

Maybe it's a bit too naive of an idea - but wouldn't the embedding also immediately solve the issue of needing Python linting/formatting plugins to be written in Rust? I.e. if you have a `flake8` plugin that uses the AST as an interface you could use `ruff` which interprets and provides the AST to the plugin. 

---

_Comment by @adam-azarchs on 2024-11-20 08:16_

That sort of implies that there's some universal representation for ASTs, which isn't the case.  `ruff`'s in-memory representation of the AST is not something that python code could consume without conversion into python objects, and in particular python objects using the representation that flake8 wants.  It doesn't seem obvious to me that such conversion would be significantly less time-consuming than simply having flake8 parse the source itself, and would certainly be a lot more effort.

---

_Comment by @alexisdrakopoulos on 2024-12-03 12:37_

Has there been any progress on this? We'd love to be able to write custom rules, eg we like Pylints E1101 and want to have that functionality.

---

_Comment by @HerringtonDarkholme on 2025-05-13 04:00_

Hi ast-grep author here. Recently ast-grep supports non tree-sitter parser like OXC, example POC https://github.com/ast-grep/ast-grep/pull/1970. 

Techinically, Ruff's [`AnyNodeRef`](https://github.com/astral-sh/ruff/blob/0fb94c052e8e5e48743bf70931c4810bcc1fe53e/crates/ruff_python_ast/src/node.rs#L661) should be easily integrated to ast-grep's rule and config. 

While Ruff is not shipping plugin system recently, and also the parser is [not available in crates.io](https://github.com/astral-sh/ruff/issues/43), I cannot give a POC like above. But if one day Ruff is planning to prioritize plugin, I am very happy to help!

---

_Comment by @blankform-co on 2025-06-05 17:08_

Bump, had to switch off ruff because our team wanted features not supported by ruff but were already plugins for flake8.

Sad because I loved ruff's speed and user experience.

---

_Comment by @phitoduck on 2025-06-24 03:32_

I run an ML Platform team that serves several data scientists. 

We want to add a rule that requires calls to `pd.merge(...)` or `df.merge(...)` to include the `validate=...` parameter where `validate` is one of `1:1`, `m:1`, `1:m` or `m:m`.

We think this would solve a huge portion of the data quality issues we get in our pipelines.

This is a bit of a wacky, esoteric rule, so I don't imagine most teams would want to enable it.

It's only for our monorepo, so it doesn't have to be all that fast.

Hence, we'd actually be fine (even *prefer*) if the `ruff` plugin system allowed for contributing plugins in Python. We'd sacrifice speed, but we could also develop it more easily.

So, this is essentially an anecdote to say:

1. I think plugins in Python are good for many teams who have esoteric use-cases
2. If it "took off" I could imagine us contributing this directly to `ruff` in rust. Then we'd get speed.
3. ... or maybe I should ask for the whole world: `ruff` should support a plugin system in both languages, so low-performance plugins can be ported to high-performance plugins if they turn out to be valuable

---

_Comment by @notatallshaw on 2025-06-24 03:52_

@phitoduck given speed isn't an issue for you, and writing Python is a plus, what advantage do you think ruff would provide over writing a flake8 plugin or using any other Python framework to do linting checks? 

---

_Comment by @adam-azarchs on 2025-06-24 04:59_

That particular lint rule also falls neatly into ast-grep or similar.

I continue to assert that if you want lint rules written in python, use a python-based linter.  If you want to use `ruff` with such a thing, frameworks like pre-commit are pretty good for running multiple linters on a file, one of which can be `ruff` - we have a very large monorepo and we use pre-commit to run ruff plus a couple of other things.  Rust doesn't really have any advantage over python for the I/O-bound task of finding relevant files, so a framework written in python that invokes `ruff` plus other linters is fine, and probably makes more sense than trying to shoehorn a python interpreter into `ruff`.

---

_Comment by @KelSolaar on 2025-08-06 01:32_

While I understand the reticence to support Python based linting inside Ruff, I feel like asking people to use flake8 is backwards. Most people using Ruff nowadays have dropped a myriad of different linters in favour of Ruff to have a single and consistent linting interface. I'm certainly not looking at re-adding any linters I used in the past.

---

_Comment by @MarthinusBosman on 2025-08-08 08:42_

Seeing as this decision is taking years to make progress, just my 2 cents:

At the very least, supporting Rust plugins feels like it's the most fundamental feature needed. If the goal for Ruff is to be able to be the only linter/formatter needed for a project, you don't have an option of not supporting custom rules. And if plugins are widely available, you definitely don't want those plugins limited to be much slower than the rest of Ruff.

Of course python / some other query syntax would be helpful for defining project-specific rules, and is the feature I'm looking for that led me to this issue, but not having any option of expandability in the meantime is a pretty pressing issue?

Ideally I don't see a reason for not supporting both Rust- and Python- based. But I'd rather have one than neither.

---

_Comment by @adam-azarchs on 2025-08-08 20:37_

I think support for custom rules is very important.  Rust-based plugins would be nifty, though I strongly suspect that 95% of use cases, at least for things that wouldn't make sense to upstream directly into ruff itself, would be satisfied by allowing rules based on ast-grep or similar DSL.

The problem with python plugins is that now you're talking about starting up a python interpreter.  And in order to really be useful, you're almost certainly going to need to associate that interpreter with a python environment (venv or otherwise). Aside from the runtime performance problems inherent to python (the avoidance of which was the main point of ruff in the first place), now ruff is no longer just a standalone static-linked binary you can just download and run wherever.

> If the goal for Ruff is to be able to be the only linter/formatter needed for a project, you don't have an option of not supporting custom rules.

I question whether that goal makes sense in this context.  A user wants to have one command that they run, yes, but they don't actually care about the internals of what that command is doing - they only care about the quality of the results and how long it takes to get them.  There isn't really any practical difference from that perspective between `ruff` starting a python interpreter to run a plugin vs. some other tool (e.g. pre-commit or flake8) being the one to launch `ruff`.  Either way you are already stuck with redundant parsing.  We could define a protocol whereby `ruff` is able to launch arbitrary subprocesses (why just python?) and send the files to be linted to them, but there are already programs that do that and they know how to use `ruff`, so why not just use one of those?

Personally I'm a fan of `pre-commit` for this sort of thing.  It's rare that a repo is _purely_ python.  At the very least you have a `pyproject.toml`, which you might want to apply an auto-formatter to, not to mention probably some markdown and yaml. There are `pre-commit` hooks to handle all of that stuff.  That way your "one command to run all linting" really does cover _all_ linting, not just python.  And it handles downloading the specific version of `ruff` and other tools that you want people to be using so there's consistency between team members.

---

_Comment by @gpshead on 2025-08-18 22:12_

We want to get rid of _all_ of our Python based custom rules because they are slow and flake8 or pylint or whatnot are additional similar tooling ecosystems to have to support. A way to add these within the framework of ruff without requiring Rust based patches + builds + custom-distribution would be great.  Even if such custom rules were "slow" from ruff's perspective by being implemented in, for example, a ruff embeded starlark or similar runtime.

Similarly a pre-made way to make that ruff Rust patching for feature additions + builds pain be easy could work.  I can imagine this being done within a ruff-pre-commit environment, it comes down to a trusted build system and build caching behind the scenes to avoid repeated duplicate work.  But what that looks like is going to vary _a lot_ depending on people's supported environments.

---

_Comment by @ashrub-holvi on 2025-11-14 16:11_

It's already mentioned by issue creator, but I will repeat it in a bit different words - perhaps this issue can be considered as two separate features:
1. Lightweight custom rules
2. Fully-featured plugins

Lightweight rules are just text files or blocks in config file:
```
[lint.custom-rules]
XX01 = {  # XX01 - code of custom check
    query = "query for match AST, some DSL like xpath or tree-sitter's query lang",
    match-type = "match",  #  can be "not match"
    error = "Message to user here",
    # filename regex?
    # how to implement noqa? ideally query should be able to match comments
}
```
Link to [tree-sitter's query lang](https://tree-sitter.github.io/tree-sitter/using-parsers/queries/index.html), I am not really used it yet, just an example, but it's even has [language server](https://github.com/ribru17/ts_query_ls) and "standard" file extension `.scm`.
So, if code "match" or "not match" to query, you will get an error message, simple and should cover many scenarios.
The idea to simplify creating checks for simple cases - I played a bit with CST and my feeling that it's hard to write and even harder to read the code. For checks I want to add it's too low-level (yes, it's a CST, but I am sure all said applied to AST as well).
I would be happy even if it would be double parsing by, for example, tree-sitter, which has Rust bindings, so, should be easy to add initial version just by using tree-sitter as it's and think about getting rid of double parsing later, it's just should be disabled by default. People who want it, should set `make-ruff-slower-by-enabling-custom-checks` option.

Fully-featured plugins is just some compiled code which can do everything what is not possible by simple matching, it's either Rust-only or any language compiled to WebAssembly (dprint has plugins in webasm if I understand it correctly).

---

_Comment by @jcampbell05 on 2026-01-09 08:35_

Just a thought but would it be possible for Ruff to develop a API for Rust or even just WASM plugins, then this idea of lightweight syntax / DSL could be served by such a plugin ?

https://tartanllama.xyz/posts/wasm-plugins/

---

_Comment by @leandrodamascena on 2026-01-21 12:59_

Hey folks, I just wanted to add another use case here. I work on an SDK that needs custom lint rules to catch domain-specific issues - things that don't make sense as upstream rules but are critical for our users.

I use Ruff daily and love it - it's become essential for my workflow. But without plugin support, I had to fall back to Flake8 just for these custom rules. It works, but now users need two linters instead of one.

I get that plugin support is complex (performance, sandboxing, API stability), but the lack of it is a real blocker for teams with internal standards or SDK/framework-specific rules. Would love to see this move forward, even if it's a limited/experimental API to start.

---

_Comment by @CarrotManMatt on 2026-01-21 13:22_

> I had to fall back to Flake8 just for these custom rules

For temporary plugin support [Fixit](https://fixit.readthedocs.io) using [LibCST](https://libcst.readthedocs.io) has been much more ergonomic to develop with rather than Flake8

---

_Comment by @leandrodamascena on 2026-01-21 13:27_

> > I had to fall back to Flake8 just for these custom rules
> 
> For temporary plugin support [Fixit](https://fixit.readthedocs.io) using [LibCST](https://libcst.readthedocs.io) has been much more ergonomic to develop with rather than Flake8

Hi @CarrotManMatt, thanks for the reply. I wasn't familiar with `Fixit`, it looks good, but adoption seems quite low yet, doesn't it? I'm just looking at pypi stats numbers..

The supply chain is important in this case because it's not about internal customers that I can control, but rather an external audience, and Flak8 is much more popular.

---

_Comment by @flying-sheep on 2026-01-21 13:35_

Flake8 still doesn’t have pyproject.toml support, right? Idk, I’d rather trust something that follows established community standards than something that’s only relevant because it was there first and has inertia.

It’s different for something that’s integrated into your code (like e.g. the testing framework), but the only issue with linters is that each added linter has to parse and traverse your codebase independently, so I wouldn’t add Fixit if I’d *also* need Flake8, but next to Ruff, I’d definitely prefer Fixit if necessary.

PS: for anyone still caring about Flake8 and consequently pyproject.toml support, technically there are no blockers anymore according to this: https://github.com/PyCQA/flake8/issues/234#issuecomment-812800722
- [Python 3.11+ has tomllib](https://docs.python.org/3/library/tomllib.html)
- [pip 25.3 removed setup.py support](https://pip.pypa.io/en/stable/news/#v25-3)

---

_Comment by @leandrodamascena on 2026-01-21 13:57_

You're right about `pyproject.toml` - Flake8 doesn't support it natively. My plugin uses pyproject.toml for packaging (hatch/uv), but yeah, Flake8 config still needs .flake8 or setup.cfg. Not perfect.

I went with `Flake8` mainly because of reach. Most Python projects I've seen already have Flake8 in their CI or pre-commit setup, even IDE with plugin, so adding a plugin is just one extra dependency. Since I can't control what external users have installed, meeting them where they are seemed like the safer bet.

`Fixit` looks interesting - I'll check it out. My only hesitation is that it's less common, so shipping a `Fixit` plugin means asking users to add another tool to their setup. With `Flake8`, we have more people already have it somewhere in their workflow.

---

_Comment by @MichaReiser on 2026-01-21 14:01_

Thanks for raising awareness for your use case. I'd like to ask you to keep discussions on this issue limited because many users are subscribed to it. Feel free to open a new discussion to discuss flake8/fixit and other alternatives.

---

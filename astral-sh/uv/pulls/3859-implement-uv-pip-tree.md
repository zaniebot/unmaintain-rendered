```yaml
number: 3859
title: "implement `uv pip tree`"
type: pull_request
state: merged
author: ChannyClaus
labels: []
assignees: []
merged: true
base: main
head: uv-pip-tree
created_at: 2024-05-27T06:46:45Z
updated_at: 2024-06-21T19:48:31Z
url: https://github.com/astral-sh/uv/pull/3859
synced_at: 2026-01-10T13:48:28Z
```

# implement `uv pip tree`

---

_Pull request opened by @ChannyClaus on 2024-05-27 06:46_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
resolves https://github.com/astral-sh/uv/issues/3272

1. added it as a new subcommand rather than a flag on an existing command since that seems more consistent with `cargo tree` + cleaner code organization, but can make changes if it's preferred the other way.
2. throws an error on finding a cyclic dependency, which is more similar to `cargo` than `pipdeptree` (`pipdeptree` seems to ignore the packages that are part of the dependency cycle and display the rest).

## Test Plan
added tests 🌵



---

_Comment by @codspeed-hq[bot] on 2024-05-27 13:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ChannyClaus:uv-pip-tree)

### Merging #3859 will **not alter performance**

<sub>Comparing <code>ChannyClaus:uv-pip-tree</code> (828fc7d) with <code>main</code> (e0ad649)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Converted to draft by @ChannyClaus on 2024-05-29 03:11_

---

_Comment by @ChannyClaus on 2024-05-29 03:13_

discovered a bit of an issue - the current implementation goes through all extras when constructing the graph, resulting in a cycle way more often. seems like `pipdeptree` just ignores extras entirely; let me know if y'all have any thoughts but probably going to look into doing the same here.

---

_Comment by @eth3lbert on 2024-05-29 06:42_

> discovered a bit of an issue - the current implementation goes through all extras when constructing the graph, resulting in a cycle way more often. seems like `pipdeptree` just ignores extras entirely; let me know if y'all have any thoughts but probably going to look into doing the same here.

I've actually tried adding support for extras to pipdeptree, and it partially works. However, I haven't found the time to clean it up and submit a pull request.

My approach is to first consider all extras, but respect the marker (excluding the extra part). Then, while building the extra map, prune impossible extra branches that lack required packages or are unreachable nodes.

However, this approach isn't perfect. It determines extra branches based on the installed packages and tries to infer backwards, leading to situations where it can't determine the root package requesting the extra installation correctly. This can happen when two separate packages have similar extra packages.

Another approach for the tree might be to leverage the manifest/lock file, which is also used by multiple package management tools like Cargo, pnpm, Poetry, etc. This might be more accurate.

My other concern is the tree display for packages with extras, especially those with multiple extras. How should we handle them: aggregate or split them in the display?

---

_Comment by @ChannyClaus on 2024-05-30 02:02_

> > discovered a bit of an issue - the current implementation goes through all extras when constructing the graph, resulting in a cycle way more often. seems like `pipdeptree` just ignores extras entirely; let me know if y'all have any thoughts but probably going to look into doing the same here.
> 
> I've actually tried adding support for extras to pipdeptree, and it partially works. However, I haven't found the time to clean it up and submit a pull request.
> 
> My approach is to first consider all extras, but respect the marker (excluding the extra part). Then, while building the extra map, prune impossible extra branches that lack required packages or are unreachable nodes.
> 
> However, this approach isn't perfect. It determines extra branches based on the installed packages and tries to infer backwards, leading to situations where it can't determine the root package requesting the extra installation correctly. This can happen when two separate packages have similar extra packages.
> 
> Another approach for the tree might be to leverage the manifest/lock file, which is also used by multiple package management tools like Cargo, pnpm, Poetry, etc. This might be more accurate.
> 
> My other concern is the tree display for packages with extras, especially those with multiple extras. How should we handle them: aggregate or split them in the display?

hmm yeah, given the complexity, maybe we shall go with what pipdeptree does for the time being then?

---

_Marked ready for review by @ChannyClaus on 2024-05-31 04:10_

---

_Review requested from @charliermarsh by @konstin on 2024-05-31 10:00_

---

_Comment by @ChannyClaus on 2024-06-09 16:32_

handled the case where there's a dependency cycle (test at https://github.com/astral-sh/uv/pull/3859/files#diff-ada61e3d2726ed1b05e633096131a477c895c737c6350b8238d9e81b2745e8eaR165)


let me know if there's anything else that y'all think should be changed 🌵

---

_Comment by @ChannyClaus on 2024-06-10 23:51_

@charliermarsh let me know if this is kinda low-prio to get in now - wasn't sure if i should keep rebasing on main 😅 

---

_Comment by @zanieb on 2024-06-15 02:49_

Charlie's out next week, if I have time I'll take a look!

---

_Comment by @ChannyClaus on 2024-06-17 03:04_

> Charlie's out next week, if I have time I'll take a look!

thanks! seems like something strange is going on with windows test after rebasing - will try to take a look more this week...

---

_Review requested from @zanieb by @zanieb on 2024-06-17 10:46_

---

_Comment by @ChannyClaus on 2024-06-19 02:47_

i just noticed the failure is from `uv pip install` - it _seems_ like it's handling cyclic dependency differently for windows. excluded the test for windows for now, but can continue to look but if y'all have thoughts as to what's going on, let me know!

---

_Comment by @zanieb on 2024-06-19 02:51_

Hey @ChannyClaus, it's overflowing the stack which is common on our test builds on Windows. I'd recommend setting up your test invocations like this

https://github.com/astral-sh/uv/blob/b22ee82f0d3af607cc49d3f3870805769ea0b6e6/crates/uv/tests/common/mod.rs#L261-L294

https://github.com/astral-sh/uv/blob/b22ee82f0d3af607cc49d3f3870805769ea0b6e6/crates/uv/tests/pip_install.rs#L321-L326

Note the `UV_STACK_SIZE` tweak on Windows.

I just gave this a quick look so lmk if you're accounting for that already.

---

_Comment by @ChannyClaus on 2024-06-19 03:41_

> Hey @ChannyClaus, it's overflowing the stack which is common on our test builds on Windows. I'd recommend setting up your test invocations like this
> 
> https://github.com/astral-sh/uv/blob/b22ee82f0d3af607cc49d3f3870805769ea0b6e6/crates/uv/tests/common/mod.rs#L261-L294
> 
> https://github.com/astral-sh/uv/blob/b22ee82f0d3af607cc49d3f3870805769ea0b6e6/crates/uv/tests/pip_install.rs#L321-L326
> 
> Note the `UV_STACK_SIZE` tweak on Windows.
> 
> I just gave this a quick look so lmk if you're accounting for that already.

ahh, that must be it, i think i removed that without knowing what it did while adding the extra index :.) 

> Hey @ChannyClaus, it's overflowing the stack which is common on our test builds on Windows. I'd recommend setting up your test invocations like this
> 
> https://github.com/astral-sh/uv/blob/b22ee82f0d3af607cc49d3f3870805769ea0b6e6/crates/uv/tests/common/mod.rs#L261-L294
> 
> https://github.com/astral-sh/uv/blob/b22ee82f0d3af607cc49d3f3870805769ea0b6e6/crates/uv/tests/pip_install.rs#L321-L326
> 
> Note the `UV_STACK_SIZE` tweak on Windows.
> 
> I just gave this a quick look so lmk if you're accounting for that already.

this was absolutely the case (sorry removed that bit when i was writing out the invocation cuz i had to add the extra index arg) - should be ready for review again now 😅 

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:181 on 2024-06-20 15:14_

Nit:
```suggestion
                let visited_lines = self.visit(self.dist_by_package_name[&required.name], visited, path)?;
                lines.extend(visited_lines);
```

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:79 on 2024-06-20 15:17_

What's the reason for filtering these out?

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:109 on 2024-06-20 15:18_

Nit:
```suggestion
        line.push_str("└── ");
```

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:111 on 2024-06-20 15:21_

No need to allocate here, you can simplify all of this with `write!`.

```
write!(&mut line, "{} v{}" installed_dist.name(), installed_dist.version()).unwrap();
```

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:151 on 2024-06-20 15:22_

`PackageName` is already a `String`, no need to allocate.

```suggestion
        let package_name = installed_dist.name().as_ref();
```

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:196 on 2024-06-20 15:24_

Should we be reusing the same `path` for all dependencies? Cycles can only occur in `visit`, so it seems like we should be clearing the `Vec` or creating a new `path` for each dependency.

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_tree.rs`:222 on 2024-06-20 15:29_

Can we give a better error message here? Something like:

```
package `a` depends on package `b`, which depends cyclically on package `a`

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:42 on 2024-06-20 15:33_

I believe this needs to be updated with the new internal toolchain changes.

```suggestion
    let environment = PythonEnvironment::from_toolchain(Toolchain::find(
        python.map(ToolchainRequest::parse),
        EnvironmentPreference::from_system_flag(system, true),
        ToolchainPreference::from_settings(preview),
        &cache,
    )?);
```

---

_@ibraheemdev requested changes on 2024-06-20 15:37_

Some small nits and questions. Otherwise this looks good!

---

_@zanieb reviewed on 2024-06-20 15:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/tree.rs`:42 on 2024-06-20 15:45_

I'm happy to do this rebase. I keep changing things :)

---

_@ChannyClaus reviewed on 2024-06-20 20:24_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:79 on 2024-06-20 20:24_

cycles become way too common if extras are included (in which case the current implementation throws an error). `pipdeptree` seems to just remove the extras when building the tree, and it seemed like the right choice for usability.

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:111 on 2024-06-20 20:57_

this is actually what i had previously i believe - i changed it because this would then print the tree partially when there's a cycle (and thought it looked better if no tree gets printed at all if an error was going to be thrown)

---

_@ChannyClaus reviewed on 2024-06-20 20:57_

---

_@ibraheemdev reviewed on 2024-06-20 21:57_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:79 on 2024-06-20 21:57_

I see, that seems reasonable. But why filter only the extras that are included by default, and not all extras installed?

---

_@ibraheemdev reviewed on 2024-06-20 22:03_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:111 on 2024-06-20 22:03_

GitHub doesn't let me edit the selected lines, but I'm just suggesting to use `write!` for an individual package. Makes it a little nicer:

```diff
+    write!(&mut line, "{} v{}" installed_dist.name(), installed_dist.version()).unwrap();
-    line.push_str((*installed_dist.name()).to_string().as_str());
-    line.push_str(" v");
-    line.push_str((*installed_dist.version()).to_string().as_str());
```

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:151 on 2024-06-20 22:06_

Hmm looks like this was marked as resolved but didn't get changed? 

---

_@ibraheemdev reviewed on 2024-06-20 22:06_

---

_Comment by @zanieb on 2024-06-20 23:01_

So I gave this a try today, just to get a sense for the user experience and the output for a small project looks like this

```
❯ uv pip tree
editables v0.5
packse v0.0.0
└── chevron-blue v0.2.1
└── hatchling v1.24.2
    └── packaging v24.1
    └── pathspec v0.12.1
    └── pluggy v1.5.0
    └── trove-classifiers v2024.5.22
└── msgspec v0.18.6
└── pyyaml v6.0.1
└── setuptools v70.0.0
└── twine v5.1.0
    └── pkginfo v1.11.1
    └── readme-renderer v43.0
        └── nh3 v0.2.17
        └── docutils v0.21.2
        └── pygments v2.18.0
    └── requests v2.32.3
        └── charset-normalizer v3.3.2
        └── idna v3.7
        └── urllib3 v2.2.1
        └── certifi v2024.6.2
    └── requests-toolbelt v1.0.0
        └── requests v2.32.3 (*)
    └── urllib3 v2.2.1 (*)
    └── importlib-metadata v7.1.0
        └── zipp v3.19.2
    └── keyring v25.2.1
        └── jaraco-classes v3.4.0
            └── more-itertools v10.3.0
        └── jaraco-functools v4.0.1
            └── more-itertools v10.3.0 (*)
        └── jaraco-context v5.3.0
    └── rfc3986 v2.0.0
    └── rich v13.7.1
        └── markdown-it-py v3.0.0
            └── mdurl v0.1.2
        └── pygments v2.18.0 (*)
psutil v5.9.8
pypiserver v2.1.1
└── pip v24.0
└── packaging v24.1 (*)
ruff v0.1.15
syrupy v4.6.1
└── pytest v8.2.2
    └── iniconfig v2.0.0
    └── packaging v24.1 (*)
    └── pluggy v1.5.0 (*)
watchfiles v0.21.0
└── anyio v4.4.0
    └── idna v3.7 (*)
    └── sniffio v1.3.1
```

A things I notice here.

1. The tree is not drawn with properly "connected" branches. As a contrasting example, here's the packse tree for a scenario:

```
❯ uv run -- packse view scenarios/examples/example.json
example-961b4c22
├── environment
│   └── python3.8
├── root
│   └── requires a
│       └── satisfied by a-1.0.0
├── a
│   └── a-1.0.0
│       └── requires b>1.0.0
│           ├── satisfied by b-2.0.0
│           └── satisfied by b-3.0.0
└── b
    ├── b-1.0.0
    ├── b-2.0.0
    └── b-3.0.0
        └── requires c
            └── unsatisfied: no versions for package
```

2. The lack of extras display is very confusing, e.g. most of these dependencies are from `packse` and it's weird that the extras are not shown as sources. I know this was discussed elsewhere, but this seems like a big deal to me. I'm not sure I fully understand why this is hard to do.

3. There's no indication of what the `(*)` means

4. The versions seem superfluous and distracting when inspecting a dependency tree — should we make display of those opt-in?

For comparison purposes, here's the `pipdeptree` output for the same project:

```
❯ uv tool run pipdeptree --python .venv/bin/python
------------------------------------------------------------------------
packse==0.0.0
├── chevron-blue [required: >=0.2.1, installed: 0.2.1]
├── hatchling [required: >=1.20.0, installed: 1.24.2]
│   ├── packaging [required: >=23.2, installed: 24.1]
│   ├── pathspec [required: >=0.10.1, installed: 0.12.1]
│   ├── pluggy [required: >=1.0.0, installed: 1.5.0]
│   └── trove-classifiers [required: Any, installed: 2024.5.22]
├── msgspec [required: >=0.18.4, installed: 0.18.6]
├── PyYAML [required: >=6.0.1, installed: 6.0.1]
├── setuptools [required: >=69.1.1, installed: 70.0.0]
└── twine [required: >=4.0.2, installed: 5.1.0]
    ├── importlib_metadata [required: >=3.6, installed: 7.1.0]
    │   └── zipp [required: >=0.5, installed: 3.19.2]
    ├── keyring [required: >=15.1, installed: 25.2.1]
    │   ├── jaraco.classes [required: Any, installed: 3.4.0]
    │   │   └── more-itertools [required: Any, installed: 10.3.0]
    │   ├── jaraco.context [required: Any, installed: 5.3.0]
    │   └── jaraco.functools [required: Any, installed: 4.0.1]
    │       └── more-itertools [required: Any, installed: 10.3.0]
    ├── pkginfo [required: >=1.8.1, installed: 1.11.1]
    ├── readme_renderer [required: >=35.0, installed: 43.0]
    │   ├── docutils [required: >=0.13.1, installed: 0.21.2]
    │   ├── nh3 [required: >=0.2.14, installed: 0.2.17]
    │   └── Pygments [required: >=2.5.1, installed: 2.18.0]
    ├── requests [required: >=2.20, installed: 2.32.3]
    │   ├── certifi [required: >=2017.4.17, installed: 2024.6.2]
    │   ├── charset-normalizer [required: >=2,<4, installed: 3.3.2]
    │   ├── idna [required: >=2.5,<4, installed: 3.7]
    │   └── urllib3 [required: >=1.21.1,<3, installed: 2.2.1]
    ├── requests-toolbelt [required: >=0.8.0,!=0.9.0, installed: 1.0.0]
    │   └── requests [required: >=2.0.1,<3.0.0, installed: 2.32.3]
    │       ├── certifi [required: >=2017.4.17, installed: 2024.6.2]
    │       ├── charset-normalizer [required: >=2,<4, installed: 3.3.2]
    │       ├── idna [required: >=2.5,<4, installed: 3.7]
    │       └── urllib3 [required: >=1.21.1,<3, installed: 2.2.1]
    ├── rfc3986 [required: >=1.4.0, installed: 2.0.0]
    ├── rich [required: >=12.0.0, installed: 13.7.1]
    │   ├── markdown-it-py [required: >=2.2.0, installed: 3.0.0]
    │   │   └── mdurl [required: ~=0.1, installed: 0.1.2]
    │   └── Pygments [required: >=2.13.0,<3.0.0, installed: 2.18.0]
    └── urllib3 [required: >=1.26.0, installed: 2.2.1]
pipdeptree==2.23.0
├── packaging [required: >=23.1, installed: 24.1]
└── pip [required: >=23.1.2, installed: 24.1]
psutil==5.9.8
pypiserver==2.1.1
├── packaging [required: >=23.2, installed: 24.1]
└── pip [required: >=7, installed: 24.1]
syrupy==4.6.1
└── pytest [required: >=7.0.0,<9.0.0, installed: 8.2.2]
    ├── iniconfig [required: Any, installed: 2.0.0]
    ├── packaging [required: Any, installed: 24.1]
    └── pluggy [required: >=1.5,<2.0, installed: 1.5.0]
watchfiles==0.22.0
└── anyio [required: >=3.0.0, installed: 4.4.0]
    ├── idna [required: >=2.8, installed: 3.7]
    └── sniffio [required: >=1.1, installed: 1.3.1]
```

Note they display the tree correctly as I requested. It's nice that they show version requirements as well as the installed version, but I still feel like it's way too hard to read as a default output. It seems entirely fine to add support for that separately.

---

_Comment by @zanieb on 2024-06-20 23:06_

I also find the cargo behavior around cycles pretty unhelpful as it makes it very difficult to understand how the cycle is happening. What's the problem with displaying cycles? It seems like if `a` depends on `b` and `b` depends on `c` which depends on `a` we'd just display something like

```
- a
    - b
       - c
          - a
```

Where if a package is already present at the top-level we omit display of its tree?

---

_Comment by @ChannyClaus on 2024-06-20 23:24_

>The tree is not drawn with properly "connected" branches.

ah my bad - let me try to fix this today.

>There's no indication of what the (*) means

i can definitely add some sort footnote for this in the output; was just following what `cargo tree` does.

>The versions seem superfluous and distracting when inspecting a dependency tree — should we make display of those opt-in?

i'd guess it would be more common default to display the versions but putting it behind a flag seems also fine.

>The lack of extras display is very confusing, e.g. most of these dependencies are from packse and it's weird that the extras are not shown as sources. I know this was discussed elsewhere, but this seems like a big deal to me. I'm not sure I fully understand why this is hard to do.

i'll make a separate comment on this with an example, when i was playing around with the version that includes extra, installing a few popular packages would create a dependency cycle. seemed like it would be too limiting as a feature to fail on common set of installed packages.

>we'd just display something like
```
a
|--> b
        |->> c
```
this is actually easier; if preferred, i can revert the code to print the above output? (and error underneath?)

---

_@ChannyClaus reviewed on 2024-06-20 23:34_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:111 on 2024-06-20 23:34_

sorry i think i was not clear - what i was saying was, with `write!`, the output would look like
```
package1
|--> package 2
package3
...
cycle_package1
|--> cycle_package2
         |--> cycle_package1
Error: cycle exists: cycle_package1 -> cycle_package2
```
rather than 
```
Error: cycle exists: cycle_package1 -> cycle_package2
```
i thought the latter was preferred but maybe the former is preferred based on the comment @zanieb made below? https://github.com/astral-sh/uv/pull/3859#issuecomment-2181687503

i can make the change if the former is preferred!

---

_Comment by @zanieb on 2024-06-20 23:34_

> ah my bad - let me try to fix this today.

Note this is pretty painful. The [packse implementation](https://github.com/astral-sh/packse/blob/main/src/packse/view.py#L67) may be a helpful reference.

>  re `(*)` was just following what cargo tree does.

What does it mean though? :)

Regarding cycles in extras: I'm not sure why they need to be an error though? It seems like we should be able to display them.


---

_@ChannyClaus reviewed on 2024-06-20 23:35_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:79 on 2024-06-20 23:35_

hmm the code looks like filtering all extra packages altogether, but maybe i'm missing something?

---

_Comment by @ibraheemdev on 2024-06-20 23:37_

> The tree is not drawn with properly "connected" branches. As a contrasting example, here's the packse tree for a scenario:

Another thing that is missing is the current package as the root of the tree.

> There's no indication of what the (*) means

`(*)` is taken from cargo to represent a duplicated package that has already been expanded elsewhere. Cargo has the `--no-dedupe` flag to fully expand these nodes. De-duplicating by default would likely be far too noisy. The other alternative is to omit the `(*)` entirely, but I think that would end up being more confusing.

Cycles could similarly use `(*)` and not be special-cased, I think.

---

_Comment by @ibraheemdev on 2024-06-20 23:40_

`cargo tree` has some other options, such as `--prune` and especially `--depth`, that we likely will want to add eventually.

---

_Comment by @zanieb on 2024-06-20 23:42_

Ah thank you @ibraheemdev. Yeah I'm suggesting something like using `(*)` for the cycles. We could do `(**)` as well. Notably we couldn't expand them though.


---

_@ChannyClaus reviewed on 2024-06-20 23:46_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:111 on 2024-06-20 23:46_

oh never mind, i see what you mean

---

_@ChannyClaus reviewed on 2024-06-21 00:16_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:151 on 2024-06-21 00:16_

oh oops, yah i think i reverted this while working on other comments; this change makes compiler to complain about a flurry of other things and my tiny brain was not strong enough to win that battle - will look again after fixing the display issue 😅 

---

_Comment by @ChannyClaus on 2024-06-21 02:38_

alright, handled:
1. the incorrect rendering issue https://github.com/astral-sh/uv/pull/3859/files#diff-ada61e3d2726ed1b05e633096131a477c895c737c6350b8238d9e81b2745e8eaR223-R259
2. allowing cycles https://github.com/astral-sh/uv/pull/3859/files#diff-ada61e3d2726ed1b05e633096131a477c895c737c6350b8238d9e81b2745e8eaR324-R328

what's left now?
- add a footnote type output for (*) for de-duplication?
- (**) for cycle?

i'll look at https://github.com/astral-sh/uv/pull/3859/files#r1647760723 in the meantime.

---

_@ibraheemdev reviewed on 2024-06-21 02:56_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:151 on 2024-06-21 02:56_

Ah you end up needing the `String` later anyways. You could use a `HashSet<&str>` and `Vec<&str>` for the `visited` and `lines` types to avoid the allocations, but it's fine how it is now.

---

_Comment by @zanieb on 2024-06-21 03:36_

The branching looks great now! A footnote seems reasonable for `*` I think we can skip using `**` and just use `*` with a different footnote when a `--no-dedupe` flag is used.

Thoughts on extras? Should we pursue it as a follow-up?

---

_Comment by @ChannyClaus on 2024-06-21 03:56_

> The branching looks great now! A footnote seems reasonable for `*` I think we can skip using `**` and just use `*` with a different footnote when a `--no-dedupe` flag is used.
> 
> Thoughts on extras? Should we pursue it as a follow-up?

yah, follow-up sounds good. i think it's confusing both ways. if we do show extras, it would be confusing if the user didn't install it with extras. if we do _not_ show extras, as you pointed out, the user will be surprised by them _not_ being linked in the tree output.

i'll add a footnote shortly!

---

_Comment by @ChannyClaus on 2024-06-21 04:34_

done 🌵 
```
$ cargo run -q pip tree
uv-cyclic-dependencies-c v0.1.0
└── uv-cyclic-dependencies-a v0.1.0
    └── uv-cyclic-dependencies-b v0.1.0
        └── uv-cyclic-dependencies-a v0.1.0 (*)
Note: (*) indicates the package has been `de-duplicated`.
The dependencies for the package have already been shown elsewhere in the graph, and so are not repeated.
```
~should i use a period not a semi-colon - i feel like i overuse it~

---

_@zanieb approved on 2024-06-21 16:23_

Thanks for your work on this (and your patience!)

---

_Review requested from @ibraheemdev by @ChannyClaus on 2024-06-21 16:53_

---

_@ChannyClaus reviewed on 2024-06-21 16:54_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:151 on 2024-06-21 16:54_

got it, yah maybe better to leave it as is for now then (was iffy around adding all the lifetimes annotation etc 😅 )

---

_@ibraheemdev approved on 2024-06-21 19:44_

Great work! Some of the other things we discussed can be addressed in follow-up PRs.

---

_Merged by @ibraheemdev on 2024-06-21 19:48_

---

_Closed by @ibraheemdev on 2024-06-21 19:48_

---

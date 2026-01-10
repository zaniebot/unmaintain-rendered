```yaml
number: 13264
title: "Add `--show-with` to `uv tool list` to list packages included by `--with`"
type: pull_request
state: merged
author: gaardhus
labels:
  - cli
assignees: []
merged: true
base: main
head: show-extras
created_at: 2025-05-02T12:36:00Z
updated_at: 2025-05-06T20:23:50Z
url: https://github.com/astral-sh/uv/pull/13264
synced_at: 2026-01-10T11:10:41Z
```

# Add `--show-with` to `uv tool list` to list packages included by `--with`

---

_Pull request opened by @gaardhus on 2025-05-02 12:36_

## Summary

Add a `--show-extras` argument to the `uv tool list` cli, to show which extra dependencies were installed with the tool.

i.e.

```bash
$ uv tool install fastapi --with requests --with typer==0.14
```

```bash
$ uv tool list --show-extras
fastapi v0.115.12 [extras: requests, typer==0.14]
- fastapi
```

## Test Plan

Added a new test function based on the others in the same file, with the other arguments tested with the new argument as well.

---

_Comment by @zanieb on 2025-05-02 13:14_

Thanks for contributing! Is there an issue discussing this feature?

---

_Comment by @gaardhus on 2025-05-02 13:18_

Tried looking but I couldn't find any; I needed the functionality my self so I just went ahead an implemented it.

---

_Comment by @zanieb on 2025-05-02 13:24_

Thanks for checking. Given that there wasn't prior discussion, we'll need to figure out what we want the user experience to look like here.

e.g., for:

> [extras: requests, typer==0.14]

Should we show the extras _after_ the `--with` dependencies? or add `with: `? It seems confusing as-is.

Another option... should we just render it as `fastapi[requests] v0.115.12`?

What's the motivation for making this opt-in? Should we always show these?


---

_Comment by @gaardhus on 2025-05-02 13:34_

Sure thing!

> Thanks for checking. Given that there wasn't prior discussion, we'll need to figure out what we want the user experience to look like here.
> 
> e.g., for:
> 
> > [extras: requests, typer==0.14]
> 
> Should we show the extras _after_ the `--with` dependencies? or add `with: `? It seems confusing as-is.

Currently I thought the terminology was that you installed `extras` with `with` in the case of `uv tool install`, if that makes sense?

https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies

However, renaming my implementation to `--show-with` and `[with: requests]` I think is fine. 

> Another option... should we just render it as `fastapi[requests] v0.115.12`?
> 
> What's the motivation for making this opt-in? Should we always show these?

I made it optional since I mainly needed it for debugging I guess? So if I just need to know which tools I have installed and are available, I don't really need to know the specific requirements, (i.e. also why I guess the `--show-version-specifiers` is also an argument). Also, it might get super cluttered if you have a lot of specified extras.

---

_Comment by @zanieb on 2025-05-02 13:40_

Oh, sorry I totally misunderstood. We can't say `extras` here as that term is a bit overloaded, i.e., it means a set of optional dependency for a package (see https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#dependencies-optional-dependencies and https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-extras). I should have read more carefully.

I think it's okay to do `[with: ...]` excluding version specifiers by default? `--show-version-specifiers` is indeed behind a flag because it adds a lot of visual noise, but `[with: ]` seems less likely to do that? My main concern would be users that do like... `--with-requirements <file>` and include a _lot_ of additional dependencies. I'm not sure how we want to render that, even if it's opt-in.


---

_Comment by @zanieb on 2025-05-02 13:41_

(If we leave it opt-in, we need another flag name. Maybe just `--show-with` but that feels clunky to me for some reason)

---

_Comment by @gaardhus on 2025-05-02 13:46_

> Oh, sorry I totally misunderstood. We can't say `extras` here as that term is a bit overloaded, i.e., it means a set of optional dependency for a package (see https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#dependencies-optional-dependencies and https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-extras). I should have read more carefully.

Yeah, I was just about to say that: I see that `extra`(s) may not be that precise, since they are more directly tied with the actual dependencies of the package (https://docs.astral.sh/uv/pip/dependencies/#using-pyprojecttoml https://docs.astral.sh/uv/guides/integration/fastapi/#migrating-an-existing-fastapi-project).

> I think it's okay to do `[with: ...]` excluding version specifiers by default? `--show-version-specifiers` is indeed behind a flag because it adds a lot of visual noise, but `[with: ]` seems less likely to do that? My main concern would be users that do like... `--with-requirements <file>` and include a _lot_ of additional dependencies. I'm not sure how we want to render that, even if it's opt-in.

For my use cases I would still think it should be an argument rather than default behavior. Running `uv tool list` to get a very brief and minimal overview of the tools and their executeables is nice, and when I need more information (which will generally be for some form of debugging?) its nice to have the arguments.

> (If we leave it opt-in, we need another flag name. Maybe just `--show-with` but that feels clunky to me for some reason)

Agree, maybe that's also why I went with `--show-extras`. Could be `--show-with-requirements` or `--show-extra-requirements`?

---

_Comment by @gaardhus on 2025-05-02 13:54_

> > I think it's okay to do `[with: ...]` excluding version specifiers by default? `--show-version-specifiers` is indeed behind a flag because it adds a lot of visual noise, but `[with: ]` seems less likely to do that? My main concern would be users that do like... `--with-requirements <file>` and include a _lot_ of additional dependencies. I'm not sure how we want to render that, even if it's opt-in.
> 
> For my use cases I would still think it should be an argument rather than default behavior. Running `uv tool list` to get a very brief and minimal overview of the tools and their executeables is nice, and when I need more information (which will generally be for some form of debugging?) its nice to have the arguments.

Also, I have a hard time imagining the new argument to show less visual noise than `--show-version-specifiers` does?

> > (If we leave it opt-in, we need another flag name. Maybe just `--show-with` but that feels clunky to me for some reason)
> 
> Agree, maybe that's also why I went with `--show-extras`. Could be `--show-with-requirements` or `--show-extra-requirements`?

Or maybe the super verbose `--show-with-extra-requirements` 

---

_Assigned to @zanieb by @konstin on 2025-05-02 15:03_

---

_Comment by @gaardhus on 2025-05-02 15:39_

Just throwing in another solution: To merge the `--show-version-specifiers` with my argument into `--show-specifiers` or `--show-all-specifiers`

---

_Comment by @zanieb on 2025-05-02 15:42_

I don't think `--show-specifiers` makes sense for including these "additional" package names.

I _think_ `--show-with` makes the most sense for now. I think we can easily rename it in the future if we want.

---

_Comment by @gaardhus on 2025-05-02 15:51_

Last suggestion would be to reword extra to additional, also in `uv tool install --help` 
```
      --with <WITH>
          Include the following additional requirements
```

and then for `uv tool list --help`
```
  --show-additional-requirements              Whether to display the additional requirements installed with
                             each tool
```

and maybe still `[with: ...]`  in the output of the actual command?

I don't feel strong about either, so up to you what I should implement.

---

_@zanieb reviewed on 2025-05-06 12:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:84 on 2025-05-06 12:53_

Can you rename this variable to match, e.g., `with_requirements`?

---

_@zanieb reviewed on 2025-05-06 12:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_list.rs`:447 on 2025-05-06 12:54_

How does `with:` interact with `required:`? Can you add a test case for that?

---

_@zanieb reviewed on 2025-05-06 13:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:90 on 2025-05-06 13:01_

Is the requirement source display ever empty?

---

_@zanieb reviewed on 2025-05-06 13:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 13:04_

I think you could rewrite this entire block as something like

```rust
        let extra_requirements = (show_with && !tool.requirements().is_empty())
            .then(|| {
                let requirements = tool
                    .requirements()
                    .iter()
                    // Exclude the package itself
                    .filter(|req| req.name != name)
                    .map(|req| format!("{}{}", req.name, req.source))
                    .join(", ");
                format!(" [with: {requirements}]")
            })
            .unwrap_or_default();
```

---

_@gaardhus reviewed on 2025-05-06 13:12_

---

_Review comment by @gaardhus on `crates/uv/tests/it/tool_list.rs`:447 on 2025-05-06 13:12_

My bad, pretty sure that black test was supposed to do that, so I updated it to include an `--with requests` argument

---

_@gaardhus reviewed on 2025-05-06 13:44_

---

_Review comment by @gaardhus on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 13:44_

Okay that broke some stuff, need to add a check that requirements exists, I guess since installing without any --with requirements still returns the original package, and hence returns an empty `[width: ]` string. Will add the original test without any --with requirements which I was supposed to test this back in the test suite.

---

_@gaardhus reviewed on 2025-05-06 14:27_

---

_Review comment by @gaardhus on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 14:27_

I'm super new to rust, so sorry if there is something I don't ace.

Since the requirements array may be empty, I think we do need an if else clause here before inserting the values?

So currently working implementation would be 

```rs
        let with_requirements = (show_with && !tool.requirements().is_empty())
            .then(|| {
                let requirements = tool
                    .requirements()
                    .iter()
                    .filter(|req| req.name != name)
                    .map(|req| format!("{}{}", req.name, req.source))
                    .join(", ");
                if requirements.is_empty() {
                    String::new()
                } else {
                    format!(" [with: {requirements}]")
                }
            })
            .unwrap_or_default();
```

My initial implementation was based on the version_specifier parsing on the lines above. So if you prefer unwrap_or_default should I also change that chunk to mirror that, ie.

```rs
        let version_specifier = show_version_specifiers
            .then(|| {
                let specifiers = tool
                    .requirements()
                    .iter()
                    .filter(|req| req.name == name)
                    .map(|req| req.source.to_string())
                    .filter(|s| !s.is_empty())
                    .join(", ");
                if specifiers.is_empty() {
                    String::new()
                } else {
                    format!(" [required: {specifiers}]")
                }
            })
            .unwrap_or_default();
```

---

_@gaardhus reviewed on 2025-05-06 14:57_

---

_Review comment by @gaardhus on `crates/uv/src/commands/tool/list.rs`:90 on 2025-05-06 14:57_

I just copied the filtering from the code block above, but removing any if them doesn't change the output or the status of the tests? So I guess either remove them, or if the are required ideally add test cases to capture that?

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 15:02_

You could just do `(show_with && tool.requirements().len() > 1)`, though it's a bit less elegant since it doesn't capture that after the filter.

The prettier way is like..


```rust
        let extra_requirements = show_with
            .then(|| {
                tool.requirements()
                    .iter()
                    // Exclude the package itself
                    .filter(|req| req.name != name)
                    .peekable()
            })
            // Omit the `[with: ...]` if there are no extra requirements
            .filter(|requirements| requirements.peek().is_some())
            .map(|requirements| {
                let requirements = requirements
                    .iter()
                    .map(|req| format!("{}{}", req.name, req.source))
                    .join(", ");
                format!(" [with: {requirements}]")
            })
            .unwrap_or_default();
```

---

_@zanieb reviewed on 2025-05-06 15:02_

---

_@zanieb reviewed on 2025-05-06 15:02_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 15:02_

You could just do `(show_with && tool.requirements().len() > 1)`, though it's a bit less elegant since overlaps with the logic encoded in the filter.

The prettier way is like..


```rust
        let extra_requirements = show_with
            .then(|| {
                tool.requirements()
                    .iter()
                    // Exclude the package itself
                    .filter(|req| req.name != name)
                    .peekable()
            })
            // Omit the `[with: ...]` if there are no extra requirements
            .filter(|requirements| requirements.peek().is_some())
            .map(|requirements| {
                let requirements = requirements
                    .iter()
                    .map(|req| format!("{}{}", req.name, req.source))
                    .join(", ");
                format!(" [with: {requirements}]")
            })
            .unwrap_or_default();
```

---

_@zanieb reviewed on 2025-05-06 15:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 15:03_

(The `show_version_specifiers` implementation isn't as clean as it could be, it was also externally contributed)

---

_@gaardhus reviewed on 2025-05-06 17:43_

---

_Review comment by @gaardhus on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 17:43_

Thank you for your suggestions and detailed review!

I couldn't make the peekable work, the compiler kept throwing errors about mutability.

So I tried reimplementing and unifying the two functions like this 

```rs
        let tool_requirements = (show_version_specifiers | show_with)
            .then(|| tool.requirements())
            .unwrap_or_default();

        let version_specifier = show_version_specifiers
            .then(|| {
                let specifiers = tool_requirements
                    .iter()
                    .filter(|req| req.name == name)
                    .map(|req| req.source.to_string())
                    .filter(|s| !s.is_empty())
                    .join(", ");
                (!specifiers.is_empty())
                    .then(|| format!(" [required: {specifiers}]"))
                    .unwrap_or_default()
            })
            .unwrap_or_default();

        let with_requirements = (show_with && tool_requirements.len() > 1)
            .then(|| {
                let requirements = tool_requirements
                    .iter()
                    .filter(|req| req.name != name)
                    .map(|req| format!("{}{}", req.name, req.source))
                    .join(", ");
                format!(" [with: {requirements}]")
            })
            .unwrap_or_default();
```

---

_@zanieb reviewed on 2025-05-06 17:51_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:90 on 2025-05-06 17:51_

I don't think it's possible for it to be empty from looking at the implementation 🤷‍♀️ not sure why it's that way.

---

_@zanieb reviewed on 2025-05-06 17:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 17:58_

Ah sorry about that, the mutable reference thing is annoying... I'm not sure if there's a fix for that (but I'll ask someone who knows more than me).

---

_@zanieb reviewed on 2025-05-06 17:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 17:59_

Ah `.take_if(|requirements| requirements.peek().is_some())` works instead of `.filter(...)`

---

_@zanieb reviewed on 2025-05-06 18:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 18:03_

(Rust's idioms around options take a while to learn, but they are nice once you get the hang of it)

---

_Review comment by @gaardhus on `crates/uv/src/commands/tool/list.rs`:105 on 2025-05-06 18:54_

Then they become

```rs
        let version_specifier = show_version_specifiers
            .then(|| {
                tool.requirements()
                    .iter()
                    .filter(|req| req.name == name)
                    .map(|req| req.source.to_string())
                    .filter(|s| !s.is_empty())
                    .peekable()
            })
            .take_if(|specifiers| specifiers.peek().is_some())
            .map(|mut specifiers| {
                let specifiers = specifiers.join(", ");
                format!(" [required: {specifiers}]")
            })
            .unwrap_or_default();

        let with_requirements = show_with
            .then(|| {
                tool.requirements()
                    .iter()
                    .filter(|req| req.name != name)
                    .peekable()
            })
            .take_if(|requirements| requirements.peek().is_some())
            .map(|requirements| {
                let requirements = requirements
                    .map(|req| format!("{}{}", req.name, req.source))
                    .join(", ");
                format!(" [with: {requirements}]")
            })
            .unwrap_or_default();
```

if I'm not mistaken?

---

_@gaardhus reviewed on 2025-05-06 18:54_

---

_Renamed from "Add `--show-extras` argument to `uv tool list` cli" to "Add `--show-with` to `uv tool list` to list packages included by `--with`" by @zanieb on 2025-05-06 19:03_

---

_@zanieb reviewed on 2025-05-06 19:05_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_list.rs`:428 on 2025-05-06 19:05_

Do you think these should be separate blocks? Should it be `[required: ..., with: ...]`?

There's no need for this stylistic nit to block this pull request, we can address this afterwards if we want.

cc @charliermarsh curious if you have a stance.

---

_@zanieb approved on 2025-05-06 19:05_

Thanks for your patience!

---

_Label `cli` added by @zanieb on 2025-05-06 19:05_

---

_@zanieb reviewed on 2025-05-06 19:06_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4337 on 2025-05-06 19:06_

We might want to say "additional" here (as discussed elsewhere). If you want to update that language all at once, it can happen here or in a separate pull requests.

---

_@zanieb reviewed on 2025-05-06 19:06_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4337 on 2025-05-06 19:06_

I think it's also fine to just say "requirements" without the floating (s)

---

_@zanieb reviewed on 2025-05-06 19:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4337 on 2025-05-06 19:07_

```suggestion
    /// Whether to display the additional requirements installed with each tool.
```

(will require generated reference update)

---

_Merged by @zanieb on 2025-05-06 20:23_

---

_Closed by @zanieb on 2025-05-06 20:23_

---

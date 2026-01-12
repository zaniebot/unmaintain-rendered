```yaml
number: 2088
title: "refactor: Introduce crates folder"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: refactor/create-crates-folder
created_at: 2023-01-22T16:01:20Z
updated_at: 2023-02-05T21:47:48Z
url: https://github.com/astral-sh/ruff/pull/2088
synced_at: 2026-01-12T15:55:07Z
```

# refactor: Introduce crates folder

---

_@MichaReiser_

This PR introduces a new `crates` directory and moves all "product" crates into that folder. 

Part of #2059 

**TODO**

* Test Playground

---

_Comment by @MichaReiser on 2023-01-22 16:04_

@chammika-become what 's the best way to verify that my PR doesn't introduce any regressions?

It seems that the `resources` folder is only used by tests in the `ruff_analyze` crate. Should I move it to `crates/rust_analyze/tests/resources`

---

_Comment by @charliermarsh on 2023-01-22 16:40_

Maybe you meant to tag me? :joy:

I think there are a few things we can do here from a testing perspective:

1. Get CI passing (`fmt`, `clippy`, `test --all`).
2. Ensure that the built wheel is unchanged (modulo incidental file-size changes due to README or whatever else). This would involve installing [maturin](https://maturin.rs/), running `maturin build`, and then looking at the built wheel (which is just a zip archive containing the Ruff binary plus metadata):

```
❯ maturin build
🍹 Building a mixed python/rust project
🔗 Found bin bindings
📡 Using build options bindings from pyproject.toml
💻 Using `MACOSX_DEPLOYMENT_TARGET=11.0` for aarch64-apple-darwin by default
   Compiling ruff v0.0.229 (/Users/crmarsh/workspace/ruff)
   Compiling ruff_cli v0.0.229 (/Users/crmarsh/workspace/ruff/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 18.11s
📦 Built wheel to /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.229-py3-none-macosx_11_0_arm64.whl

❯ unzip -l /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.229-py3-none-macosx_11_0_arm64.whl
Archive:  /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.229-py3-none-macosx_11_0_arm64.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
   127520  01-22-2023 16:38   ruff-0.0.229.dist-info/METADATA
      103  01-22-2023 16:38   ruff-0.0.229.dist-info/WHEEL
     1070  01-22-2023 16:38   ruff-0.0.229.dist-info/license_files/LICENSE
        0  01-22-2023 16:38   ruff/__init__.py
      193  01-22-2023 16:38   ruff/__main__.py
 40091538  01-22-2023 16:38   ruff-0.0.229.data/scripts/ruff
      540  01-22-2023 16:38   ruff-0.0.229.dist-info/RECORD
---------                     -------
 40220964                     7 files
```

This artifact should be effectively identical before and after this change. (It's the thing that gets uploaded to PyPI and installed via `pip install ruff`.) It should also be possible to `pip install /Users/crmarsh/workspace/ruff/target/wheels/ruff-0.0.229-py3-none-macosx_11_0_arm64.whl` in a new virtual environment, then run `ruff /path/to/file.py`. I can help with that if your Python env isn't fully setup.

3. Ensure that the playground works as expected? I guess we can just validate that locally for the most part via running the documented `wasm-pack` commands, then `npm run dev` from `playground`.


---

_Comment by @charliermarsh on 2023-01-22 16:41_

> It seems that the resources folder is only used by tests in the ruff_analyze crate. Should I move it to crates/rust_analyze/tests/resources

I _think_ this would be a reasonable thing to do. That folder mostly contains Python text fixtures, some of which are used by the unit tests (for snapshot testing with `insta`), and some of which just exist for manual testing right now (e.g., the stuff in `resources/test/project`, which lets you test some of the config-resolution logic manually).

There may well be a better convention for this altogether. It's one of the things I ran with at the start of the project, and hasn't changed much since. (I _do_ like having the fixtures as Python files and not embedded as strings in Rust code, so I wouldn't want to change _that_ aspect of it, but e.g., I'd guess that `resources` is not the conventional name for a folder like this.)


---

_Comment by @not-my-profile on 2023-01-22 20:15_

> This PR further renames the ruff package to ruff_analyze, mainly to avoid the CLI and the previous ruff crate having the same name. I don't know if that's desired but I found it confusing that the ruff binary comes from the CLI and not the ruff crate.

I intentionally named the CLI crate this way when I split it from the library. I think it only makes sense that the ruff library is called `ruff` and not something random like ruff_analyze[^1]. Also there is precedence for this naming convention from very popular projects e.g. [diesel](https://crates.io/crates/diesel) & [diesel_cli](https://crates.io/crates/diesel_cli) and [wasm-bindgen](https://crates.io/crates/wasm-bindgen) & [wasm-bindgen-cli](https://crates.io/crates/wasm-bindgen-cli). I think naming the library crate something other than ruff very much implies that the library is a second-class citizen, which might be true right now but I very much expect that to change as ruff stabilizes.

[^1]: Also sounds weird given the very popular [rust-analyzer](https://github.com/rust-lang/rust-analyzer) ... we don't analyze ruff, we analyze python code.

---

_Comment by @charliermarsh on 2023-01-22 21:40_

I remain happy to update the crate structure, though I agree that we should retain the `ruff` name for now rather than renaming to `ruff-analyzer`. There's some precedent for this pattern amongst other projects, and it feels like a reasonable structure from my perspective.

(Also: we can always make that change in the future -- it's harder to undo than to do -- and in a separate PR, rather than bundling with the crate reorganization.)

---

_@charliermarsh reviewed on 2023-01-22 21:41_

---

_Review comment by @charliermarsh on `README.md`:1783 on 2023-01-22 21:41_

These _may_ have been updated accidentally.

---

_Marked ready for review by @MichaReiser on 2023-01-26 15:21_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-01-26 17:34_

---

_Review comment by @charliermarsh on `ruff_dev/Cargo.toml`:12 on 2023-01-27 00:08_

I believe `ruff_dev` needs to be moved to `./crates/ruff_dev`.

---

_@charliermarsh reviewed on 2023-01-27 00:08_

---

_Comment by @charliermarsh on 2023-01-27 00:33_

I think we need to move `ruff_dev` as well. Apart from that, and a rebase, this looks good to me. I went ahead and tested both the playground, and `maturin build` + the installed wheel too.

---

_@charliermarsh reviewed on 2023-01-27 00:34_

---

_Review comment by @charliermarsh on `.github/workflows/playground.yaml`:31 on 2023-01-27 00:34_

I was confused by this `../../` path, but I guess it's relative to `crates/ruff`, rather than the `cwd`? Seems to work!

---

_Review comment by @MichaReiser on `.github/workflows/playground.yaml`:31 on 2023-01-27 09:42_

Yeah, that's confusing. It seems that `wasm-pack` interprets the out dir path relative to the build target... 

---

_@MichaReiser reviewed on 2023-01-27 09:42_

---

_Comment by @MichaReiser on 2023-01-27 09:52_

> I think we need to move `ruff_dev` as well. Apart from that, and a rebase, this looks good to me. I went ahead and tested both the playground, and `maturin build` + the installed wheel too.

Awesome, and thanks for testing! 

I haven't moved `ruff_dev` because we haven't yet reached a consensus in #2059 on crates that are build scripts. I proposed to move code-gen/rust-scripts into the `xtask` folder to separate them from the product code and also to avoid accidentally publishing such crates in the future (consider code in the `crates` folder as the "public" crates and crates in the `xtask` folder as private). However, I learned more about ruff's code structure when doing this refactor and realized that there are also many scripts written in Python, and it makes sense for ruff not to write all scripts in rust to dogfood ruff. 

I would prefer not to move `ruff_dev` for now and first get a shared understanding of where we want to place build scripts (use a single folder, keep the python and rust scripts separate, ...). 

I've also decided not to move the `resources` folder yet because I first want to figure out a way to reuse the test-setup code between tests (avoid the repetition of the `../../resources` path in every test. 



---

_Comment by @MichaReiser on 2023-01-27 18:04_

I'll be out for the next few days. Feel free to land this anytime or take over. I've given Maintainers push access in case some merge conflicts need to be solved (or other changes)

---

_Comment by @charliermarsh on 2023-01-27 23:04_

👍 Will do!

---

_Comment by @charliermarsh on 2023-01-31 23:21_

@not-my-profile - I'm hoping to rebase and merge this in the next release (not today's release, the following). Did you have any further comments on changing the structure? I know you'd mentioned somewhere (though I can't find it in this PR) creating a symlink or similar to `src`.

---

_Comment by @not-my-profile on 2023-02-01 04:23_

I think the creation of that symlink can wait till we improve the structure of the `ruff` library.

Regarding this PR .. I think it would be nice to perform the change in 2 commits:

1. The first commit only moves the files and changes nothing else.
2. The second commit updates all path references that need to be updated.

---

_Comment by @not-my-profile on 2023-02-01 04:27_

And to be honest I'd like to avoid changes such as:

```diff
-           Path::new("./resources/test/fixtures/flake8_simplify")
+           Path::new("../../resources/test/fixtures/flake8_simplify")
```

I think it would be better to firstly refactor our code to have the `test_path` function automatically prefix `./resources/test/fixtures/` ... then we wouldn't have to update all of these paths for performing the move.

(Seeing that `./resources/test/fixtures` is also referenced in `Settings` structs in the tests ... this should really be a constant).

---

_Comment by @charliermarsh on 2023-02-01 04:27_

@not-my-profile - Ok, I can make that happen. It needs a big rebase anyway.

---

_Comment by @not-my-profile on 2023-02-01 04:33_

Btw I have finally fully implemented the many-to-many mapping ... I'll only have to clean up my commits, update the `rule` command and add some tests :) I think I'll wait with doing the PR till this migration is over.

> I think it would be better to firstly refactor our code to factor out `./resources/test/fixtures` to a constant

Should I get started with that?

---

_Comment by @charliermarsh on 2023-02-01 04:35_

> Should I get started with that?

Yeah that'd be great! I'm not going to rebase and merge this until tomorrow anyway (getting late here).

---

_Comment by @not-my-profile on 2023-02-01 10:11_

> I haven't moved ruff_dev because we haven't yet reached a consensus in https://github.com/charliermarsh/ruff/issues/2059 on crates that are build scripts. I proposed to move code-gen/rust-scripts into the xtask folder to separate them from the product code

If we have a `crates` directory I think it should certainly contain all of our crates ... no matter if they're internal or public. I don't like the name `xtask` since it's very nondescript (what tasks?).

> to avoid accidentally publishing such crates in the future

We can simply set `publish = false` in `Cargo.toml` for crates that shouldn't be published, see [the Cargo documentation](https://doc.rust-lang.org/cargo/reference/manifest.html#the-publish-field).

Since this PR has gotten a bit messy, I have started a new PR #2435 for this migration ... I refactored the code beforehand in #2433 so that the actual migration doesn't require us to update hundreds of paths.

Looking at the other commits you pushed here Charlie:

> Tweak Cargo.toml files 2b9388b63b6577dba2d56719194af5dfe17592a0)

I really think we should keep unrelated changes out of the PR we use for such a migration. I was really confused why this PR removed 3k lines ... until I realized it's because you deleted a no longer needed `Cargo.lock` file in that commit ... I think that's better done after the migration.

> Fix contributing references c01fe8c8ff068959356354b15d1c8948a12c4a49

I think prefixing `crates/ruff/` to every path makes the file less readable ... I'd rather move the example sections to `crates/ruff/CONTRIBUTING.md` and link that file from the main `CONTRIBUTING.md` file.

>  I think it would be nice to perform the change in 2 commits

I was mostly concerned about making many changes in a huge commit that moves all these files. Now that all of these path updates aren't necessary anymore I think it makes more sense to do the `crates/` migration in a single atomic commit.

---

_Comment by @not-my-profile on 2023-02-01 11:20_

After looking at the resulting file structure for a while:

```
├── crates/
│   ├── flake8_to_ruff/
│   ├── ruff/
│   ├── ruff_cli/
│   ├── ruff_dev/
│   └── ruff_macros/
```

I think I don't like it ... in particular that `ruff_cli` and `ruff_dev` are between `ruff` and `ruff_macros`. (Yes moving `ruff_dev` out of `crates/` would improve that a bit but I think it would be counter-intuitive to have a crate outside of `crates/`). I have therefore updated my PR #2435 to instead implement the following structure:

```
├── frontends/
│   ├── flake8_to_ruff/
│   ├── playground/
│   └── ruff_cli/
├── lib/
│   ├── ruff/
│   └── ruff_macros/
├── ruff_dev/
│   ├── src/
│   └── Cargo.toml
```


---

_Comment by @charliermarsh on 2023-02-01 12:36_

Yeah I generally agree with keeping `ruff_dev` alongside other crates, regardless of the resulting structure.

I understanding your reasoning and appreciate the argument put forth here, but my current preference is:

```
├── crates/
│   ├── flake8_to_ruff/
│   ├── ruff/
│   ├── ruff_cli/
│   ├── ruff_dev/
│   └── ruff_macros/
├── playground/
│   └── ....
```

I'd like all of the crates to be in `crates`. I think it's intuitive and follows a known convention that will be familiar to newcomers with Rust experience. (Note that I've included `ruff_dev` in there too.)

There's another issue here, which is that the [originating blog post](https://matklad.github.io/2021/08/22/large-rust-workspaces.html) explicitly mentions using `libs` as distinct from `crates` to indicate published vs. unpublished packages, so using `lib` also may be confusing in that light.

(It may make sense to wait for @MichaReiser to get back before moving forward either way though so that we can build more consensus of the right structure -- I'll try not to rush a decision that will hopefully stand the test of time.)


P.S. I removed the unused `Cargo.lock` in #2437.

---

_Comment by @not-my-profile on 2023-02-01 13:44_

> I generally agree with keeping ruff_dev alongside other crates, 

I only said that *if* we have a `crates` directory it should contain all crates. But I in fact agree with Micha that `ruff_dev` should be separate from the other crates, since it's only our internal tooling.

I think it makes much more sense to structure code after its function than blindly putting all crates in one directory, even if that is a widely used convention.

I see the following advantages to separating `frontends` from `lib`raries:

* It makes the codebase more accessible to newcomers, e.g. compare the following two paths
  
      crates/ruff/src/flake8_to_ruff/mod.rs
      crates/flake8_to_ruff/src/main.rs
  
  with:

      lib/ruff/src/flake8_to_ruff/mod.rs
      frontends/flake8_to_ruff/src/main.rs

* You are more likely to be looking for a specific frontend or a specific library than for *any crate*. With my suggested structure we could even put a `README.md` in both `frontends` and `lib` to further explain the structure. E.g. `frontends/README.md` could also link [ruff-lsp](https://github.com/charliermarsh/ruff-lsp).
* Our library crates should not depend on our frontend crates ... separating the crates into two directories makes it easier to conceptualize that.
* The number of our library crate will only grow as we try to split them up in order to improve our compile time ... putting all crates in one directory would make the frontend crates harder to spot since all the software simply lists directory contents alphabetically.
* The library crates depend on each other while the frontend crates don't. You are more likely to have to edit multiple crates in the case of libraries as opposed to frontend crates, and I think it makes sense to group code you're likely to edit together.

> the [originating blog post](https://matklad.github.io/2021/08/22/large-rust-workspaces.html) explicitly mentions using libs as distinct from crates to indicate published vs. unpublished packages

I have not seen a single project following that convention, so I wouldn't pay that much attention to it. Also choosing `libs` to refer to unpublished packages strikes me as very counter-intuitive since published libraries are also libs (and technically even unpublished libraries are crates!).

---

_Comment by @not-my-profile on 2023-02-01 15:07_

In case you nonetheless decide to go with the all crates in one directory approach, I just opened #2441 for that (since I had already implemented that before changing the approach).

---

_Comment by @charliermarsh on 2023-02-01 15:17_

Thank you for writing this up :) All good points, and I especially appreciate that you're looking forward to a world in which we have many more library crates, in which case, the split becomes more impactful (both by way of isolating the `frontend` crates _and_ providing clean definitions for allowed and disallowed inter-dependencies). Let me think on it for a bit... and I'd love to get @MichaReiser's take on it too as the original author.

> It makes the codebase more accessible to newcomers.

I think the impact here is hard to predict... I can imagine that as a newcomer, I'd find it helpful to know that everything in `crates` is a crate, and that all the crates are in `crates`; whereas I may not understand the distinction between `lib` and `frontends`, or what a `frontend` is in this context (Is it a web frontend, like `playground`? Is it a frontend for invoking Ruff, like `ruff-cli`? But `flake8-to-ruff` arguably wouldn't fit that description). I don't think those names are wrong or bad, only that they may not be as crystal-clear to newcomers as they would be to us.


---

_Comment by @not-my-profile on 2023-02-01 15:24_

Somewhat related: I think `ruff_dev` and `scripts` belong together since they're both our internal tooling.

I think a structure like the following would make sense no matter if we decide to go with the `crates/` or the `frontends/` / `lib` approach:

```
├── utils/
│   ├── benchmarks/
│   ├── ruff_dev/
│   ├── add_plugin.py*
│   ├── add_rule.py*
│   ├── generate_mkdocs.py
│   ├── pyproject.toml
│   ├── transform_readme.py
│   └── _utils.py

```

While this does make the `ruff_dev` crate harder to find, I actually think that's a pro ... since most contributors should never have to deal with its source code.

>  a newcomer, [..] may not understand the distinction between lib and frontends, or what a frontend is in this context

I think this could be well explained in `CONTRIBUTING.md` and `frontends/README.md` / `lib/README.md`. The benefit is that once you have understood these terms you benefit form the more organized structure than all-crates-in-one-dir. Another point is that I think it's a bit weird if `playground` is in the directory hierarchy a level above all the Rust code despite it arguably being less important.

---

_Comment by @andersk on 2023-02-01 20:43_

It seems to me that `frontends` vs. `lib` is hierarchy for the sake of hierarchy. If a newcomer is looking for the code of `flake8_to_ruff`, they might not know whether we decided to call it a “lib” or a “frontend”. They might even ignore “frontend” completely because they think of themselves as a backend developer and think “frontend” is for web stuff. And even for experienced contributions, it will always be one extra thing to think about when navigating the code.

---

_Comment by @not-my-profile on 2023-02-02 02:02_

> hierarchy for the sake of hierarchy

This hierarchy is defined via our dependency graph no matter where we put our crates.

![architecture](https://user-images.githubusercontent.com/73739153/216212069-057b8550-ee93-4a56-b22c-accf5e36f369.svg)

(lib-only crates are indicated via a square)

I'd rather have our directory hierarchy reflect our component hierarchy than having to conceptualize the latter from a flat directory structure, where everything is listed alphabetically in an order that does not carry any meaning beyond that.

> And even for experienced contributions, it will always be one extra thing to think about

I'd argue that you have to think about it in any case ...  because just `ruff` is ambiguous since it's the name of both the binary of our CLI and the name of our Rust library. (Yes `ruff_cli` is currently easy to spot but that won't be the case when there are much more crates.)

---

_Comment by @charliermarsh on 2023-02-03 21:47_

I've been thinking on this and my personal preference is still to use a single flat `crates` directory. I understand that it will fail to capture the hierarchy, but I think it's simplest structure for us to manage and still the clearest to newcomers. But I'd like to get @MichaReiser's take prior to making a final decision (I believe he's back next week) -- just didn't want to leave this thread idle, so thought I'd chime in before the weekend.

> I'd argue that you have to think about it in any case ... because just ruff is ambiguous since it's the name of both the binary of our CLI and the name of our Rust library.

Separately: this could change, right? The current top-level crate will certainly be broken down, but it could also end up being renamed as `ruff_core` or something (like Ripgrep, which has `core`).


---

_Comment by @MichaReiser on 2023-02-05 11:16_

Thanks for the feedback on the PR. I'll split my reply into two sections because some input is related to the code changes, and others discuss the project structure. 

## Code changes
> And to be honest I'd like to avoid changes such as:

Not having this change across all files would be nice. I also noted in my comment that I plan to refactor the repetitive test pattern in a separate PR to avoid such changes in the future (and keep this PR pure mechanical, as you mentioned). I made this suggestion because I only learned very late in the refactoring that I had to change all tests (the tests didn't run on windows), and stacked PRs are painful, to say the least. I recommend doing the refactor after because I don't think the extra history entry is bothersome enough to justify someone investing additional time avoiding it.

> Regarding this PR .. I think it would be nice to perform the change in 2 commits:

I planned to squash all commits into a single commit. We could even hide it from the git history (with gitattributes). 

## Structure
> I see the following advantages to separating frontends from libraries:

I see a few challenges with this approach:
Unclear terminology: As brought up by others, it's unclear what frontend refers to. The frontend in a compiler parses the source code and lowers it to a machine-independent representation. But that's not it. As a frontend engineer, I might think this folder only contains assets for the website or playground. 
It adds additional complexity to new contributors and navigating the project structure. As a new contributor, I may not know which crates are frontends, and it's likely not relevant to the change I make. Navigating the code also becomes more complicated because the "frontend" isn't encoded in the Rust path of a module (its `ruff_cli::` and not `frontend::ruff_cli::`). 
These are also the reasons that [matklad](https://matklad.github.io/2021/08/22/large-rust-workspaces.html#Flat-Is-Better-Than-Nested) brings up why flat is better than hierarchical. 

> Somewhat related: I think ruff_dev and scripts belong together since they're both our internal tooling.

That makes sense, but I would prefer not to name the directory `utils`. Maybe commands, tasks, or scripts (build scripts). 

> While this does make the ruff_dev crate harder to find, I actually think that's a pro ... since most contributors should never have to deal with its source code.

I wouldn't take that as a given. Rome received many excellent contributions to the code gen. 

## Next steps
This PR intentionally didn't move the `ruff_dev` crate because we haven't reached a consensus on its placement yet. I recommend keeping it this way to unblock this PR and continue the discussion around the xtasks folder in #2059. 

That leaves us with the sole decision if we prefer a single `crates` folder or the`frontend`/`libs` folders. I recommend going with a single `crates` folder for simplicity reasons. We'll still have the option to split the folder if there's a need for it. 

I'll rebase the branch when we reached consensus. 



---

_Comment by @charliermarsh on 2023-02-05 12:48_

Thanks @MichaReiser -- and welcome back :)

Given the above, my preference is to move forward with the `crates` approach.

My only point of disagreement is that I'd still like to move `ruff_dev` into `crates` for now. We can always revert that decision later if we choose to change the "tasks" structure further, but as-is, I worry that it leaves the codebase in an inconsistent state on an unclear timeline. It's hard to know if/when we'll end up changing `ruff_dev` further, and so I'd rather bias towards applying the updated convention. (Moving the crate twice isn't ideal, but it's a small cost IMO.)

(Procedural comment: @not-my-profile also opened #2441, which is ~close to a rebase of this PR. I'd like to merge this one if we can, since it's attached to all the discussion history, and that one needs to be rebased anyway now, but note that it exists and may be useful to look at. Another change that was made in that PR vis-a-vis this version is that `resources` was moved into `crates/ruff`, which I think is also a fine thing to do too.)

Finally: we may not reach complete consensus -- that's ok. Every decision has tradeoffs. The most important thing (to me) is that we've laid them out here and given them thoughtful consideration.

P.S. On this point:

> I planned to squash all commits into a single commit. We could even hide it from the git history (with gitattributes).

My preference within the project is to always "squash and merge". We sometimes "rebase and merge" in the event that a PR contains multiple atomic commits with a clean history. But as much as possible, I try to adhere to the Phabricator-like model.


---

_Comment by @MichaReiser on 2023-02-05 13:51_

> My only point of disagreement is that I'd still like to move ruff_dev into crates for now. 

Sound's good to me. 

>  Another change that was made in that PR vis-a-vis this version is that resources was moved into crates/ruff, which I think is also a fine thing to do too.)

I can do that in t his PR too. 

---

_@MichaReiser reviewed on 2023-02-05 16:19_

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:157 on 2023-02-05 16:19_

I hope I got all these references correctly

---

_Comment by @MichaReiser on 2023-02-05 16:35_

@charliermarsh I moved `ruff_dev` and the `resources` folders. But it's not clear to me why the ubuntu tests are failing now (they pass fine on my linux machine)

---

_Comment by @charliermarsh on 2023-02-05 16:45_

Hmm, I wonder if it has to do with the `docs/` in the `.gitignore`. The failing test (`packaging::tests::package_detection`) is relying on `"./resources/test/project/examples/docs/docs"`, which was introduced prior to that addition to the `.gitignore` -- but if that directory is being moved, Git might think it's being deleted.

(Will check locally -- this is just first instinct.)

---

_@charliermarsh reviewed on 2023-02-05 16:50_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:138 on 2023-02-05 16:50_

I think this is now say `crates/ruff/src/flake8_to_ruff/converter.rs`. (The path listed here was incorrect, this got moved around.)

---

_@charliermarsh reviewed on 2023-02-05 16:54_

---

_Review comment by @charliermarsh on `scripts/add_plugin.py`:48 on 2023-02-05 16:54_

Was the inclusion of `./resources/test/fixtures/` here wrong before? (I think so?)

---

_@MichaReiser reviewed on 2023-02-05 16:57_

---

_Review comment by @MichaReiser on `scripts/add_plugin.py`:48 on 2023-02-05 16:57_

Uh, I didn't notice that I removed the `test/fixtures` too. I just searched for the committed code and inferred the pattern from there. 

https://github.com/charliermarsh/ruff/blob/c6887fa4d35d67971930b7d681f70bb561423ef5/crates/ruff/src/rules/tryceratops/mod.rs#L26-L29

---

_Review comment by @charliermarsh on `scripts/add_plugin.py`:48 on 2023-02-05 16:58_

Yeah since `test_path` was introduced, I think the pattern we have now is correct. (IIRC, we do test these scripts on CI, but only that the code still compiles after running them, which wouldn't catch this issue.)

---

_@charliermarsh reviewed on 2023-02-05 16:58_

---

_Comment by @MichaReiser on 2023-02-05 16:59_

> Hmm, I wonder if it has to do with the `docs/` in the `.gitignore`. The failing test (`packaging::tests::package_detection`) is relying on `"./resources/test/project/examples/docs/docs"`, which was introduced prior to that addition to the `.gitignore` -- but if that directory is being moved, Git might think it's being deleted.
> 
> (Will check locally -- this is just first instinct.)

Yeah, that was it. Took me a while to figure out that the failure indicated a test failure and not that some command failed to run... 

---

_Comment by @charliermarsh on 2023-02-05 17:00_

I _think_ the CI output used to be clearer, and should be improved once #344 is released (we use `cargo insta test` to help us detect unreferenced snapshots).

---

_@charliermarsh approved on 2023-02-05 17:20_

This looks good to me! Thanks for rebasing, and for putting this together in the first place. It puts us in a much better position to start introducing some new crates.

Once again I checked out locally and ran `pip install .` to verify that the Python package is building + installing as expected.

I'll look to merge this later today.

---

_Comment by @charliermarsh on 2023-02-05 21:18_

(Rebasing...)

---

_Merged by @charliermarsh on 2023-02-05 21:47_

---

_Closed by @charliermarsh on 2023-02-05 21:47_

---

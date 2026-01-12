```yaml
number: 3756
title: GH-Action
type: pull_request
state: closed
author: brucearctor
labels: []
assignees: []
base: main
head: ghaction
created_at: 2023-03-27T04:29:59Z
updated_at: 2023-03-30T18:14:34Z
url: https://github.com/astral-sh/ruff/pull/3756
synced_at: 2026-01-12T15:55:13Z
```

# GH-Action

---

_@brucearctor_

Following up on the wish-list item found: https://github.com/charliermarsh/ruff/issues/3289

This addresses a functioning GitHub action to potentially increase usability/user-base of ruff.  This can be extended to support more, but addresses the basics.  

This is currently a draft PR.  I'd be happy for this to be merged as-is, and I can follow up with docs.  I am willing to do docs as part of the PR, but would want some verification that this is desired/on-the-right-track before devoting more effort into docs.  This was a good learning exercise into creating composite GH Actions.  

---

_Comment by @github-actions[bot] on 2023-03-27 04:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.1±0.77ms     2.3 MB/sec    1.00     18.1±0.86ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.6±0.15ms     3.6 MB/sec    1.00      4.5±0.15ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.10   627.8±30.14µs     4.7 MB/sec    1.00   568.6±71.64µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.09      8.4±0.42ms     3.0 MB/sec    1.00      7.8±0.30ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.02      9.6±0.46ms     4.2 MB/sec    1.00      9.4±0.24ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1929.4±62.43µs     8.6 MB/sec    1.08      2.1±0.10ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.01   253.9±15.35µs    11.6 MB/sec    1.00   252.2±13.66µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.15ms     6.0 MB/sec    1.11      4.7±0.39ms     5.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     18.4±0.87ms     2.2 MB/sec    1.00     17.4±0.52ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.8±0.22ms     3.4 MB/sec    1.00      4.6±0.13ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.03   583.1±24.50µs     5.1 MB/sec    1.00   564.1±22.72µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.8±0.19ms     3.3 MB/sec    1.00      7.6±0.26ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.08      9.5±0.42ms     4.3 MB/sec    1.00      8.8±0.18ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.12      2.2±0.17ms     7.6 MB/sec    1.00  1961.2±51.66µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    223.7±7.52µs    13.2 MB/sec    1.09   243.0±12.94µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.25      5.1±0.68ms     5.0 MB/sec    1.00      4.1±0.14ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `action/main.py`:23 on 2023-03-28 11:45_

What about using `pipx install --suffix @ruff-github-action ruff=={version}` here? pipx is [preinstalled](https://github.com/actions/runner-images/blob/main/images/linux/Ubuntu2204-Readme.md) and at least for me locally the entire `pipx intall ruff` is faster than `python -n venv .ruff-env` (1.5s vs. 3.5s)

---

_Review comment by @konstin on `action/main.py`:17 on 2023-03-28 11:47_

given that `packaging` is not in std, you could check that only `[a-zA-Z0-9_-.]` are used

---

_Review comment by @konstin on `action/main.py`:36 on 2023-03-28 11:47_

this is a neat way to deal with the arguments!

---

_Review comment by @konstin on `action.yml`:16 on 2023-03-28 11:49_

```suggestion
    description: 'The version of ruff to use, e.g. "0.0.259"'
```

most people don't really know about python version specifier or specs behind them and i think "just copy the version you want to pin" is better advice here

---

_@konstin approved on 2023-03-28 11:50_

looks good!

---

_Review comment by @konstin on `action.yml`:10 on 2023-03-28 11:52_

one more thing, i think `--format github` should be in here in the defaults somewhere

---

_@konstin reviewed on 2023-03-28 11:52_

---

_Review comment by @brucearctor on `action.yml`:16 on 2023-03-28 16:49_

makes sense towards usability [ unless we want to also nudge to educate ].  Accepted.  

---

_@brucearctor reviewed on 2023-03-28 16:49_

---

_Review comment by @brucearctor on `action.yml`:16 on 2023-03-28 16:49_

FYI - IIRC, in my tests, "v..." also worked, it wasn't required to not have the 'v'

---

_@brucearctor reviewed on 2023-03-28 16:49_

---

_@brucearctor reviewed on 2023-03-28 16:52_

---

_Review comment by @brucearctor on `action/main.py`:17 on 2023-03-28 16:52_

This doesn't concern me greatly, but feels a potentially nice to have.  I sorta take issue with a cruder match that might not actually be a version.  Not sure whether worth adding

---

_@brucearctor reviewed on 2023-03-28 16:53_

---

_Review comment by @brucearctor on `action.yml`:10 on 2023-03-28 16:53_

Agreed here.  Since this is a GitHub action, I'd go further and suggest isn't an input option with default... but rather that `--format github` is just to be part of the/this github action.  

---

_@charliermarsh reviewed on 2023-03-28 17:16_

---

_Review comment by @charliermarsh on `action.yml`:10 on 2023-03-28 17:16_

We also support use of an env var for format, which might be easier.

---

_Review comment by @konstin on `action.yml`:10 on 2023-03-28 17:16_

either way is fine

---

_@konstin reviewed on 2023-03-28 17:16_

---

_Comment by @brucearctor on 2023-03-28 17:21_

Ah, duh ... Should be doing dev on another branch in my repo, merge into this one with the open PR when ready, and then have it reviewed for merge into https://github.com/charliermarsh/ruff/ ... I'll comment back in here when I think I'm done and make it a regular not draft PR.  

---

_Marked ready for review by @brucearctor on 2023-03-28 18:20_

---

_Comment by @brucearctor on 2023-03-28 18:21_

@konstin , @charliermarsh ... looks good to go

---

_Review comment by @brucearctor on `action.yml`:10 on 2023-03-28 18:23_

used env var.  that looked cleaner [ is in the action.yml file, rather than being in the command/py file ]

---

_@brucearctor reviewed on 2023-03-28 18:23_

---

_@brucearctor reviewed on 2023-03-28 18:24_

---

_Review comment by @brucearctor on `action/main.py`:23 on 2023-03-28 18:24_

Skipped the pipx install , and just went with a pipx run with version/options as needed

---

_Comment by @brucearctor on 2023-03-28 18:55_

And, do advise -- do you want docs in this PR, or in a follow up PR?  

---

_Comment by @konstin on 2023-03-28 18:56_

docs in the same pull request, please

---

_@konstin reviewed on 2023-03-28 18:57_

---

_Review comment by @konstin on `action/main.py`:17 on 2023-03-28 18:57_

yeah please add an assert here that there are no strange characters

---

_@brucearctor reviewed on 2023-03-28 22:42_

---

_Review comment by @brucearctor on `action/main.py`:17 on 2023-03-28 22:42_

I added a regex that should work with the expected pattern of ruff versions.  

---

_Comment by @brucearctor on 2023-03-28 22:44_

I think all things are addressed.  Will assume this is done, without hearing more comments/suggestions-for-short-term/now- improvements.  

---

_Comment by @brucearctor on 2023-03-28 23:56_

> docs in the same pull request, please


Good call --> I would have done [ but anyone hopefully would at least say that ]; in the same PR makes sure it gets addressed!



---

_Review comment by @charliermarsh on `docs/usage.md`:62 on 2023-03-29 00:02_

How does the versioning work? Where is the current version expressed?

---

_@charliermarsh reviewed on 2023-03-29 00:02_

---

_@charliermarsh reviewed on 2023-03-29 00:03_

---

_Review comment by @charliermarsh on `docs/usage.md`:77 on 2023-03-29 00:03_

Do you mind adding one example that passes a command-line option to Ruff? (E.g. `--select B`, just as an arbitrary example.)

---

_@brucearctor reviewed on 2023-03-29 00:07_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 00:07_

https://docs.github.com/en/actions/creating-actions/about-custom-actions#using-tags-for-release-management

Ultimately, I'd likely either use v0 for now [ until ruff is v1 ] where action will then keep with ruff versions, or can pin it as tied to release tags via existing process.  Version can also be specified [ version of ruff, which can be different from the version of action ... I assume almost always these will be the same ] in the with: options.  

---

_@brucearctor reviewed on 2023-03-29 00:07_

---

_Review comment by @brucearctor on `docs/usage.md`:77 on 2023-03-29 00:07_

Let me get to that ... 

---

_@brucearctor reviewed on 2023-03-29 00:07_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 00:07_

Does that make sense to you?

---

_Review comment by @charliermarsh on `docs/usage.md`:62 on 2023-03-29 00:09_

Okay thanks, I might just need to spend a few mins reading through these docs before merging to make sure I understand what's going on. But I'll try not to bother you further :)

---

_@charliermarsh reviewed on 2023-03-29 00:09_

---

_@brucearctor reviewed on 2023-03-29 00:17_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 00:17_

EDIT:  Happy to help, if I can be useful.  If you wanted to talk through, even could hop on a quick call to share pointers.

---

_@brucearctor reviewed on 2023-03-29 00:21_

---

_Review comment by @brucearctor on `docs/usage.md`:77 on 2023-03-29 00:21_

FYI ... many different formats for version work.

`version: v0.0.259`
`version: "v0.0.259"`
`version: 0.0.259`
`version: "0.0.259`

[ YAML ]

---

_Review comment by @brucearctor on `docs/usage.md`:75 on 2023-03-29 00:24_

```suggestion
    version: 0.0.259
    options: --select B
```

---

_Review comment by @brucearctor on `docs/usage.md`:77 on 2023-03-29 00:25_

added as suggestion 

---

_@brucearctor reviewed on 2023-03-29 00:25_

---

_@brucearctor reviewed on 2023-03-29 00:25_

---

_Review comment by @brucearctor on `docs/usage.md`:77 on 2023-03-29 00:25_

and now committed

---

_@brucearctor reviewed on 2023-03-29 00:33_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 00:33_

Probably the thing to really be aware of is:

* The action can be released as part of tag/release process [ that you already have ].  
* The action version can differ from the ruff version used by the action, if it is specified.  

And, I added you (@charliermarsh ) as collaborator in https://github.com/brucearctor/testing_repo/actions ... to be able to see logs of prior runs, so you can have some more info on how this worked.  [ that repo was running actions based on tags/releases from https://github.com/brucearctor/ruff/ ex: https://github.com/brucearctor/ruff/tree/v0.0.009  ]

---

_@charliermarsh reviewed on 2023-03-29 03:03_

---

_Review comment by @charliermarsh on `docs/usage.md`:62 on 2023-03-29 03:03_

Oh okay, so would folks need to use (e.g.) `@v0.0.260` (our next release) or later?

---

_Comment by @charliermarsh on 2023-03-29 04:04_

Will plan to merge this tomorrow, just need to do a read over the docs etc.

---

_@brucearctor reviewed on 2023-03-29 04:04_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 04:04_

Well, they would need to use an action based on a tag [ once merged, an `action.yml` file -- along with associated code for the composite action -- will exist in the repo, and at some point you'll wind up tagging a specific point with a version number ].  So, the action called would first be available with that tag.  

Someone could use the action based on that tag with a prior version of ruff ->

```yaml
name: Ruff

on: [push, pull_request]

jobs:
  ruff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: charliermarsh/ruff@v0.0.260
        with:
          version: v0.0.249
          options: --select ANN
```

^^ Would be the action of version 0.0.260, which would run ruff version 0.0.249.  

By default, the action always installs/runs on the most recent version of ruff.  Using the with: version: is the way to allow users to pin a specific version [ ex: I keep the same version pinned in pre-commit, and our codebases ].  


---

_Review comment by @charliermarsh on `docs/usage.md`:62 on 2023-03-29 04:10_

Yeah makes sense. So the _action_ version will be available starting in the first tag that's created after this is merged. But they can specify any Ruff version within the action, as above. (I assume Black works similarly?)

---

_@charliermarsh reviewed on 2023-03-29 04:10_

---

_@brucearctor reviewed on 2023-03-29 04:17_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 04:17_

> Yeah makes sense. So the _action_ version will be available starting in the first tag that's created after this is merged. But they can specify any Ruff version within the action, as above. (I assume Black works similarly?)

That is correct.


Black:  I *think* yes.  But, I would need to check to be 100%.  Personally, I trust black enough not to have bothered with minor versions [ I just rely on `uses: psf/black@stable` ].  It is my understanding, based on ruff SemVer, that we aren't thinking about a v1 release yet, and therefore somewhat early to be relying on a `stable` tag [ which I think would be good to eventually sort out, but likely outside the needed scope of just this little code contribution ].  The pace of ruff development makes me think pinning versions at this point is rather prudent [ esp. in an action, so the development in ruff doesn't introduce a new rule that gets added which then fails the pipeline, even though not related to code change ... getting upgraded versions in something like pre-commit is less an issue because that should help while developing the code ].  

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 04:37_

Added a little more to the docs

---

_@brucearctor reviewed on 2023-03-29 04:37_

---

_Review comment by @MichaReiser on `docs/usage.md`:62 on 2023-03-29 07:21_

Thanks for expanding the documentation. I prefer versioning the action independently of ruff because users may get notified by dependentbot that their action is outdated and needs upgrading, even though nothing has changed. It's also unclear if I could use the ruff action 0.0.259 to install ruff 0.0.258 or 0.0.260. 

What I've seen other repositories doing is that they don't have a `version` option but instead use the action tag `@0.0.259` to determine the ruff version to install. I'm undecided if that would be a good idea (dependentbot strikes again). 

We currently promote using a package manager like `pip` to install ruff because it solves the versioning problem; All users use the `ruff` version configured for the project. What's your opinion on whether we should fallback to the ruff version specified in the repository rather than the latest ruff version?



---

_@MichaReiser reviewed on 2023-03-29 07:21_

---

_Review comment by @MichaReiser on `action.yml`:14 on 2023-03-29 07:25_

What's your opinion on using/allowing an array for src and `options`? For example, github uses an array for the arguments of a docker container:  https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions#runsargs

Nit: Should we rename `options` to arguments?

---

_@MichaReiser reviewed on 2023-03-29 07:25_

---

_@MichaReiser reviewed on 2023-03-29 07:36_

Thanks for working on this. 

Do you know if the action creates github annotations automatically if we use the [github](https://beta.ruff.rs/docs/settings/#format) format except if specified differently? If not, do you know how we could add support for it. 

Could you add a workflow to this repository that uses the GitHub action and runs on changes to this file or at least once per day (similar to [this](https://github.com/rome/setup-rome/blob/main/.github/workflows/ci.yml))?



---

_Comment by @charliermarsh on 2023-03-29 13:44_

> Do you know if the action creates github annotations automatically if we use the [github](https://beta.ruff.rs/docs/settings/#format) format except if specified differently? If not, do you know how we could add support for it.

(It should add annotations automatically since we're setting the `RUFF_FORMAT: github` environment variable in the action YAML.)


---

_@charliermarsh reviewed on 2023-03-29 13:46_

---

_Review comment by @charliermarsh on `docs/usage.md`:62 on 2023-03-29 13:46_

I also prefer versioning the action independently, but would we then need to release it as its own repo to avoid coupling it to our own release tags?

---

_@brucearctor reviewed on 2023-03-29 17:41_

---

_Review comment by @brucearctor on `action.yml`:14 on 2023-03-29 17:41_

rename 'options' to 'arguments':  Doesn't matter to me.  They are pretty synonomous -- while 'arguments' might be the more proper engineering term also arguments can have the negative connotation of 'arguing', 'lawyers', etc...  :-) ...  again, as long as clear enough [ i think both are ] it doesn't matter to me

Array-v-String:  I believe it would be easier for those wanting to use an action to rely on the string that they would type in their local shell and put that in the `src` field.  Otherwise, they'll need to translate to alternate syntax.  Again, not necessarily a problem [ and actually somewhat cleaner with array from a code/type/spacing perspective ], but again, I'm thinking about this action as being something as easy as possible for people to be able to use.  I think this would be harder for people to use array for src, and definitely harder for options/args [ there ordering is important, etc ].  But ...?


---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 17:55_

I can also host an action in a separate repo if separate versions preferred, ex like: https://github.com/jakebailey/pyright-action ...   Ruff could then decide it is the community's go-to action in that case and document accordingly:  https://github.com/microsoft/pyright/blob/main/docs/ci-integration.md 

I strongly believe that to be not as good of an outcome for the goal of increasing ruff's usage, and ease-of-onboarding new people.  The visibility of the action IN the repo will mean increased confidence in its usage.  

That said, whatever works.  I'll follow this with some additional thoughts.


---

_@brucearctor reviewed on 2023-03-29 17:55_

---

_@brucearctor reviewed on 2023-03-29 17:56_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 17:56_

@MichaReiser : Does dependabot actually highlight if using not the latest action?  I have not experienced that [ but I also do not specify minor versions of actions.  yes, specify minor versions of the version of code that they call certainly  ]

Ex, the following is what someone would place into something like `.github/workflows/ruff.yml` which just specifies to use the most recent v0 release with the latest version of ruff  -->

```yaml
name: Ruff
on: [push, pull_request]
jobs:
  ruff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charliermarsh/ruff@v0
``` 

Ex, the following is what someone would place into something like `.github/workflows/ruff.yml` which just specifies to use the most recent v0 release along with the 0.0.260 version of ruff -->

```yaml
name: Ruff
on: [push, pull_request]
jobs:
  ruff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charliermarsh/ruff@v0
        with:
            version: v0.0.260
``` 


I do not know whether I have ever seen minor versions of actions suggested to be actually used as part of workflows.



---

_@brucearctor reviewed on 2023-03-29 17:57_

---

_Review comment by @brucearctor on `docs/usage.md`:62 on 2023-03-29 17:57_

@MichaReiser --> do you have a sense of how docs can be improved as it relates to ruff versions?  It sounds like you were unclear on "if I could use the ruff action 0.0.259 to install ruff 0.0.258 or 0.0.260."

```yaml
        with:
            version: v0.0.260
```

As written, if specified, ANY version of action can install any valid [ pip ] installable version of ruff.  

So, in that case, yes, ruff action 0.0.259, or any other version could install 0.0.258, 0.0260, or any other valid ruff.  I would imagine that to be the desired behavior [ at least give the user the ability to configure ].  

---

_Comment by @brucearctor on 2023-03-29 18:01_

> > Do you know if the action creates github annotations automatically if we use the [github](https://beta.ruff.rs/docs/settings/#format) format except if specified differently? If not, do you know how we could add support for it.
> 
> (It should add annotations automatically since we're setting the `RUFF_FORMAT: github` environment variable in the action YAML.)

Exactly

---

_Comment by @brucearctor on 2023-03-30 00:07_

@ssweber -- as one of the first to-be users of the GH Action, how do the docs look?  Seem usable as you'd expect [ was designed in the style of black's action ]?  

The relevant docs found at end of --> https://github.com/charliermarsh/ruff/blob/ea44ea916ee2edbfdff07217ea942ac33cc0bede/docs/usage.md

---

_Comment by @ssweber on 2023-03-30 17:20_

@brucearctor , looking forward to using it! Really fresh GitHub user, learning black, Ruff, GitHub actions for the first time past week. Not quite ready to wrap my head around pre-commit, so a light-weight GitHub action is really appealing.

there might be some opportunity to simplify/clarify doc… an example rewrite:

———————-

```
Ruff can be used as a [GitHub Action](https://github.com/features/actions) (check, linting only? Ie, —fix won’t work right?):

Create a file named .github/workflows/ruff.yml inside your repository with:

name: Ruff

on: [push, pull_request]

jobs:
  ruff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charliermarsh/ruff@v0

You can configure the Ruff version and arguments passed to Ruff via:

- version: Must be release available on PyPI. default, latest release. You can pin a version, or use any valid version specifier.
- options: default,`check`
- src: default, '.'

- uses: charliermarsh/ruff@v0
  with:
    version: "0.0.259" or (version specifier here)
    options: example option here
    src: "./src"

See Configuring Ruff for further details


---

_Comment by @brucearctor on 2023-03-30 17:28_

@ssweber - thanks, let me look at what to incorporate!

`--fix` as part of the GH Action would work ( at least it is callable, there would be a bit of complexity if wanting it to serve the function you desire - actually fixing the codebase and that getting committed as part of CI ).  That's a more advanced use case, and one I [ personally ] would not advocate for as part of a GH Action, albeit others might be in favor [ so isn't explicitly called out, but docs on how to use it in that way could be added ].  

Additional context:  Really, the action as submitted is just a wrapper for `pipx run ruff{$maybe_specific_versio} ...` and you could pass any options into that.  For fix to work ( outside the scope of this PR ), you'd need to grant your action relying on it more permissions and steps to be able to modify code and commit.  Suggesting one step at a time.  

---

_Comment by @ssweber on 2023-03-30 17:36_

Thanks. I agree to keeping it as a lint-only.

For other newbies like me, I think it’s helpful to clarify the behavior.

Eg:

```
Ruff can be used as a [GitHub Action] pass/fail lint test:

---

_Comment by @brucearctor on 2023-03-30 17:44_

> 



---

_Closed by @brucearctor on 2023-03-30 17:44_

---

_Comment by @brucearctor on 2023-03-30 17:46_

> Thanks. I agree to keeping it as a lint-only.
> 
> For other newbies like me, I think it’s helpful to clarify the behavior.
> 
> Eg:
> 
> ```
> Ruff can be used as a [GitHub Action] pass/fail lint test:
> ```

I added the line on pass/fail.  

I don't want to limit behaviour of what people can do.  But, we need to start somewhere, and that place [ for me ] is the easy path for newcomers to see value.  We can continue to show people how to handle the more complex use cases [ lol, but that exercise could be never ending ] and/or naturally those inclined can use how they see fit.  The tool as written is sufficiently flexible.  

---

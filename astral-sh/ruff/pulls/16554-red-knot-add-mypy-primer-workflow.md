```yaml
number: 16554
title: "[red-knot] Add 'mypy_primer' workflow"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/mypy-primer
created_at: 2025-03-07T14:22:43Z
updated_at: 2025-03-10T10:14:36Z
url: https://github.com/astral-sh/ruff/pull/16554
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add 'mypy_primer' workflow

---

_Pull request opened by @sharkdp on 2025-03-07 14:22_

## Summary

Run Red Knot on a small selection of ecosystem projects via a [forked version of `mypy_primer`](https://github.com/astral-sh/mypy_primer/tree/add-red-knot-support). I think it's probably fine to run this from a fork as long as the red knot CLI is not stable, and as long as we're still experimenting with this?

There's probably a lot still to do both on the `mypy_primer` side as well as on the pipeline side here (we might want to have it comment on PRs, for example), but maybe this is a good first step to have this as a non-blocking pipeline that simply runs on red knot PRs? For you to look at, if you're interested...

Example output with a negative diff for a dummy change (commented out `unresolved-attribute` diagnostics):

![image](https://github.com/user-attachments/assets/c6940d77-843f-4664-83af-5664436bbb77)


## Test Plan

* Successful run: https://github.com/astral-sh/ruff/actions/runs/13725319641/job/38390245552?pr=16554
* Intentially failed run where I commented out `unresolved-attribute` diagnostics reporting: https://github.com/astral-sh/ruff/actions/runs/13723777105/job/38385224144

<!-- How was it tested? -->


---

_Label `red-knot` added by @sharkdp on 2025-03-07 14:22_

---

_Marked ready for review by @sharkdp on 2025-03-07 16:55_

---

_@sharkdp reviewed on 2025-03-07 16:57_

---

_Review comment by @sharkdp on `.github/workflows/mypy_primer.yaml`:61 on 2025-03-07 16:57_

Just a few hand-selected projects that we don't panic on :upside_down_face:. I will look into extending this list.

---

_Comment by @MichaReiser on 2025-03-07 17:03_

This is only an understanding question from my side: The check doesn't comment on the PR, unlike Ruff's ecosystem check. Anyone interested would have to "actively" check the results. Is that understanding correct?

---

_Comment by @sharkdp on 2025-03-07 18:29_

> This is only an understanding question from my side: The check doesn't comment on the PR, unlike Ruff's ecosystem check. Anyone interested would have to "actively" check the results. Is that understanding correct?

Yes, see PR description: *"There's probably a lot still to do […] (we might want to have it comment on PRs, for example), but maybe this is a good first step to have this as a non-blocking pipeline that simply runs on red knot PRs? For you to look at, if you're interested..."*

Mypy, pyright, and typeshed all have similar pipelines (see e.g. https://github.com/python/mypy/blob/master/.github/workflows/mypy_primer.yml and https://github.com/python/mypy/blob/master/.github/workflows/mypy_primer_comment.yml), so we should be able to set this up easily once we're confident that it has an actual benefit. I just thought it would make sense to run this "in the background" in the beginning to experiment with it.

---

_Comment by @MichaReiser on 2025-03-07 20:40_

Ruff's ecosystem also supports this. There are two parts to it:

* The ecosystem check itself. It [writes the output](https://github.com/astral-sh/ruff/blob/bb15c7653a358dda11cd590c57f7f5e9a8b9178d/.github/workflows/ci.yaml#L456-L461) and [the PR number](https://github.com/astral-sh/ruff/blob/bb15c7653a358dda11cd590c57f7f5e9a8b9178d/.github/workflows/ci.yaml#L517-L525) to a file
* There's a second job that [creates or updates the PR comment](https://github.com/astral-sh/ruff/blob/3a08570a684d28800163288ab53986534c9205a6/.github/workflows/pr-comment.yaml#L10)

The main downside is that GitHub has an upper limit on the comment size :(

Either way. I think this is useful as it is. It being invisible has the downside that it's unlikely that people will check it, unless we introduce a *I did review the red knot ecosystem changes* checkbox to the test plan



---

_Comment by @sharkdp on 2025-03-10 09:05_

> Either way. I think this is useful as it is. It being invisible has the downside that it's unlikely that people will check it, unless we introduce a _I did review the red knot ecosystem changes_ checkbox to the test plan

I mainly wanted to keep it in the background for a few days(?) to test it before we start commenting on everyone's PRs.

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:8 on 2025-03-10 09:07_

```suggestion
      - "crates/red_knot*/**"
      - "crates/ruff_db"
```

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:26 on 2025-03-10 09:08_

We had issues in the past where GitHub up and downgraded our ubuntu runners which resulted in broken CI pipelines. I suggest fixing it to ubuntu 24 (which I think is the same as latest)

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:68 on 2025-03-10 09:10_

This seems magic to me. How does mypy_primer setup the environment? Where does it install the dependencies? How does it configure the python version? How does it specify which files need checking?

I'm asking because I'm wondering how it ensures that knot knows about third party dependencies.

---

_@MichaReiser approved on 2025-03-10 09:11_

This is cool and a very nise surprise for the new month :)

---

_Label `ci` added by @MichaReiser on 2025-03-10 09:11_

---

_@MichaReiser reviewed on 2025-03-10 09:12_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:6 on 2025-03-10 09:12_

Maybe consider moving it to the `ci.yaml`? It's where we have all (or the vast majority) of all jobs that run as part of pull requests and is the first place where I'd be looking for this job

---

_@sharkdp reviewed on 2025-03-10 09:20_

---

_Review comment by @sharkdp on `.github/workflows/mypy_primer.yaml`:6 on 2025-03-10 09:20_

I did consider moving it there, but kept it in a separate file for now because (1) I still consider it experimental; I'm not even sure if we want to go with `mypy_primer` for our ecosystem checks or build something of our own and (2) I wanted to keep it close to the existing `mypy_primer` pipelines that exist for mypy/pyright/typeshed.

I would definitely be in favor of moving it to `ci.yaml` if this turns out to be the approach we want to take.

---

_@sharkdp reviewed on 2025-03-10 09:28_

---

_Review comment by @sharkdp on `.github/workflows/mypy_primer.yaml`:68 on 2025-03-10 09:28_

> How does mypy_primer setup the environment?

mypy_primer has a large list of hard-coded projects. For example, here's the [definition of `black`](https://github.com/sharkdp/mypy_primer/blob/ce9b172ca91dbf4c2e582959f544525bfcdb3580/mypy_primer/projects.py#L90-L97).

For some of these projects, dependencies are specified in a `deps` list. This is very simplistic. There are no version constraints, for example. mypy_primer will then install those dependencies using `pip` (or `uv pip`) into a virtual environment [here](https://github.com/sharkdp/mypy_primer/blob/ce9b172ca91dbf4c2e582959f544525bfcdb3580/mypy_primer/model.py#L154-L167).

Later, I am passing the path to the created venv to Red Knot [here](https://github.com/sharkdp/mypy_primer/blob/ce9b172ca91dbf4c2e582959f544525bfcdb3580/mypy_primer/model.py#L298-L309).

> How does it configure the python version?

It is [hard-coded to 3.13](https://github.com/sharkdp/mypy_primer/blob/ce9b172ca91dbf4c2e582959f544525bfcdb3580/mypy_primer/model.py#L304-L305) at the moment.

> How does it specify which files need checking?

This is another thing that is very rudimentary. As you can see in the definition of `black`, mypy_primer passes the `src` directory as an explicit argument to mypy and pyright. For Red Knot, I added an optional `knot_paths` argument where we can pass directories, in case we don't want to run it from the project root.

---

_Review comment by @sharkdp on `.github/workflows/mypy_primer.yaml`:61 on 2025-03-10 09:29_

I did so now and went through the project list again. I chose a few projects without a lot of dependencies, because we do not understand `-stubs` imports yet.

---

_@sharkdp reviewed on 2025-03-10 09:29_

---

_@MichaReiser reviewed on 2025-03-10 09:30_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer.yaml`:68 on 2025-03-10 09:30_

Ohhh... I didn't realize that you use your own fork. But that's on me for not reading your summary carefully.

---

_Merged by @sharkdp on 2025-03-10 10:14_

---

_Closed by @sharkdp on 2025-03-10 10:14_

---

_Branch deleted on 2025-03-10 10:14_

---

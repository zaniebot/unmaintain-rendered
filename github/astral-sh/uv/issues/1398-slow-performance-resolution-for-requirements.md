---
number: 1398
title: "\"Slow\" performance resolution for requirements"
type: issue
state: closed
author: notatallshaw
labels:
  - performance
  - resolver
assignees: []
created_at: 2024-02-16T00:07:41Z
updated_at: 2024-04-17T00:18:31Z
url: https://github.com/astral-sh/uv/issues/1398
synced_at: 2026-01-07T13:12:16-06:00
---

# "Slow" performance resolution for requirements

---

_Issue opened by @notatallshaw on 2024-02-16 00:07_

Honestly, this isn't _that_ slow, feel free to close this as "working as expected", but throwing all the difficult real world resolution cases I have at uv this is the slowest set of requirments I have found so far (at least until https://github.com/astral-sh/uv/issues/1342 is fixed):

```
uv pip compile requirements.in --exclude-newer 2023-10-01T00:00:00Z --no-cache
```

Where `requirements.in` is:

```
click >= 7.0
click-loglevel ~= 0.2
dandi >= 0.24.0
psutil ~= 5.9
pyyaml
selenium
```

On my machine takes 1m 21s.

If the issue is similiar to why Pip is slow for these requirements then the issue may be that urllib3 2.0 is pinned early in the resolution process and `boto3`/`botocore` put an upper bound on `urllib3`, forcing backtracking on increasingly early versions of `boto3`/`botocore`  on which there are hundreds to collect. 

If uv instead backtracked on urllib3 then it would save having to collect metadata from hundreds of versions of `boto3`/`botocore`. I do wonder if there's some heuristic that could help here.

For Pip I have a speculative solution for encountering this kind of issue https://github.com/pypa/pip/issues/12523, although I have not had chance to make a PR so I don't yet have evidence it actually helps. And I've not looked at uvs resolution process so I don't know if this idea translates at all.

---

_Comment by @zanieb on 2024-02-16 00:10_

Thanks for the report! We love some slow resolutions to dig into :)

I think we were just talking about this kind of heuristic with @Eh2406 — it'd be great to have. cc @BurntSushi 

---

_Label `performance` added by @zanieb on 2024-02-16 00:10_

---

_Comment by @dimbleby on 2024-02-16 00:29_

classic slow combo `uv pip install urllib3 boto3`

if you pick a recent urllib3 first you will do a lot of backtracking before you find a boto3 that is compatible with it..

---

_Comment by @charliermarsh on 2024-02-16 00:31_

Hah -- we've actually looked at this `boto` / `urllib3` problem before. We had some (brief) discussion here about it: https://github.com/astral-sh/uv/issues/170. I would love to make this case faster.

---

_Comment by @dimbleby on 2024-02-16 00:33_

poetry fairly recently adopted the heuristic "first try to decide the package with the most available versions", motivated by this case

this is the opposite of what the pubgrub paper suggests, some discussion at https://github.com/python-poetry/poetry/pull/8255

(it doesn't always work, there are still examples where the solver resolves urllib3 before it even learns that boto3 is going to be wanted)

---

_Comment by @notatallshaw on 2024-02-16 00:49_

Have you tried the idea of a heuristic of when a package specifies an upper bound on a requirement, preferring backtracking on the requirement that is upper bounded?  

I planned to create a PR for Pip soon that tried this to see if it improved the situation, I can report back if it works for Pip at least (but I don't have much time to do dev work on OSS so it might be at couple of weeks).

---

_Comment by @notatallshaw on 2024-02-16 01:43_

A little bit of attribution, this upper bound idea was recently mentioned by rip devs thinking about it (https://github.com/prefix-dev/rip/issues/191), and the example I give above comes from a real world use case reported to Pip (https://github.com/pypa/pip/issues/12274), although issues with boto3 and many other packages have come up many times, this was fairly easy to reproduce and particularly egregious.

---

_Comment by @dimbleby on 2024-02-16 07:24_

No.  But it seems a plausible idea.

Apart from anything else there is an argument that backtracking boto3 (say) in the hope of finding a version that doesn't declare itself incompatible with recent urllib3 (say) is likely to give bad answers - Is ye-olde-version of boto3 _really_ compatible with recent urllib3, or is it just that they didn't know at the time that it would turn out not to be?  

---

_Comment by @notatallshaw on 2024-02-16 14:48_

> Apart from anything else there is an argument that backtracking boto3 (say) in the hope of finding a version that doesn't declare itself incompatible with recent urllib3 (say) is likely to give bad answers - Is ye-olde-version of boto3 _really_ compatible with recent urllib3, or is it just that they didn't know at the time that it would turn out not to be

This is actually my main argument in https://github.com/pypa/pip/issues/12523 to add this to Pip, I have seen this issue in the real world break many user environments (including my own), i.e. libraries gets installed where the metadata of the libraries are compatible but the libraries are not actually functionally compatible.

On that side, I am not sure resolvelib (Pip's resolver) is flexible enough to actually support this in the example I give in this thread, as I say I'm going to attempt to implement a PR on that side and see if it works or not. If uv has not already made progress on this by the time I do that I'll report back here if it worked.



---

_Label `resolver` added by @zanieb on 2024-02-16 14:57_

---

_Referenced in [astral-sh/uv#1560](../../astral-sh/uv/issues/1560.md) on 2024-02-17 03:27_

---

_Referenced in [astral-sh/uv#1575](../../astral-sh/uv/issues/1575.md) on 2024-02-18 02:43_

---

_Comment by @Eh2406 on 2024-02-19 15:03_

> poetry fairly recently adopted the heuristic "first try to decide the package with the most available versions", motivated by this case
> this is the opposite of what the pubgrub paper suggests, 

The pubgrub paper suggestion is based on advice in "answer set programming", which itself is generalized common wisdom. Generally when looking at a backtracking algorithm "fewest versions first" is exponentially faster than "most versions first", but "generally" is doing a lot of work in that statement. For one thing loading in data is it most linearly expensive, but in this use case the linear term is often bigger than the potentially exponential term.

You absolutely want to make sure "no available options" is resolved first, as its conflict. There is no advantage in delaying "one available version" as it's inevitable. It is easy to generalize that to "fewest versions first", but beyond those first two cases there's lots of good room for experimentation.

`pubgrub-rs` should make it extremely easy for `uv` to experiment with this based on any index based heuristic it would like. It does not currently provide useful diagnostics about which packages are "causing problems" nor a method for a user to say "let's try backtracking further than strictly necessary". I would love to add such API/diagnostics but it still at the brainstorming state.

---

_Comment by @notatallshaw on 2024-02-19 19:00_

Okay, I've made a very hacky experimental proof of concept branch on pip side that implements the idea of preferring upper bounded requirements: https://github.com/pypa/pip/compare/main...notatallshaw:pip:backjump-to-upper-bounded

For the set of requirements `click >= 7.0 click-loglevel ~= 0.2 dandi >= 0.24.0 psutil ~= 5.9 pyyaml selenium` on midnight of 1st October 2023 it reduces the number of packages pip has to collect backtracking down from 831 to 111, on my machine it reduces the amount of time pip has to resolve from ~6 mins 20 seconds to ~1 min 20 seconds. Bear in mind this is via `pypi-timemachine` which adds a huge amount of latency on to every HTTP call, so in effect for this set of requirements on this date this branch of pip is faster than uv without cache.

I did some mild testing for other requirements and found that this branch was at least able to successfully resolve them.

That said, to get this to work for resolvelib I had to add a completely new API, which substantially changes the path of the algorithm, and it would need to be carefully checked it can still soundly resolve any given requirement, I don't expect I'll ever be able to contribute this to either the resolvelib or pip project.

Edit: Also I should mention I did have to hack resolvelib into understanding a bit about extras, as part of the core issue here is both the requirement on `urllib3` and `urllib3[socks]`. Not sure how you handle extras and optimizations in resolutions.

---

_Referenced in [astral-sh/uv#2062](../../astral-sh/uv/issues/2062.md) on 2024-02-29 20:42_

---

_Referenced in [pubgrub-rs/pubgrub#191](../../pubgrub-rs/pubgrub/issues/191.md) on 2024-03-18 16:05_

---

_Comment by @notatallshaw on 2024-03-21 19:40_

Okay, I have a high level idea that should work for most resolution algorithms to implement preferring upper bounded requirements:

If during resolution when trying to pin `foo` and finding `foo` depends on `bar<n` and `bar` is already pinned to some version greater than or equal `n`, then:

1. "Unpin" `bar`
2. Add a permanent preference that from now on `bar` should be not be prefered when resolving, at least so that `foo` will always be picked over it (and probably most other packages)
3. Optionally, pin `foo` immediately if possible
4. Continue resolution

If conditions of the scenario are met later but in reverse (swap `foo` and `bar`) then none of these actions are taken. The term unpin is left vague here, in its simplest form it would be to undo all the steps that happened after and including `bar` being pinned, but this could be optimized by some algorithm that is able to approach this more surgically. Very similiar, if not identical in some algorithms, to "backjumping".

I think this idea is stable, and does not introduce any infinite loops. Although I only think about its performance in real world scenarios, it may be possible to intentionally attack it and find it can produce even more extreme pathological cases.

Currently resolvelib has an internal implementation of unpinning, but doesn't expose an API to do it. I don't know if pubgrub-rs has anything equivalent. If I can come up with a relatively sane way to expose this in resolvelib I am going to try and push to implement it.

It also may be very expensive to unpin, but in general if you end up in a situation where you are trying to pin `foo` that has an upper bound on `bar` and you have already pinned `bar` then you've already likely hit a very pathological case and the cost of trying to back out of this state and find a happier path is _likely_ going to save you time. I feel this would be even more the case with uv because of it's excellent cache use.

---

_Comment by @mpizenberg on 2024-03-21 23:06_

If I try to generalize a bit your proposition, one idea could be to exponentially de-prioritize one package pinning everytime it caused a conflict. I believe this should be possible with the version of pubgrub used by uv. Pubgrub let's you choose the priority of any given package. I imagine it might be possible, when identifying the package causing the backtracking to ask the user re-evaluate it's priority.

Packages that need a reprioritization are re-asked anyway. So I think if the algorithms pins one version of `bar`, then exhaust all versions of `foo`, it should backtrack before pinning of `bar` and re-ask `bar` priority (as well as other packages). At that moment, I believe the caller should be able to give a lower priority to `bar`. That implies tracking when being re-asked to prioritize a given package from the dependency provider.

Does this sounds plausible @Eh2406 ?

---

_Comment by @notatallshaw on 2024-03-21 23:47_

> So I think if the algorithms pins one version of `bar`, then exhaust all versions of `foo`, it should backtrack before pinning of `bar` and re-ask `bar` priority (as well as other packages).

The idea is to avoid exhausting all versions of `foo`, as it could be 1000s of versions where collecting each version of `foo` involves a non-zero cost (in Python package land at least one HTTP call, if not a large download, or worse building the package from source).

I'm suggesting to use this specific scenario (collecting package which has an upper bound dependency on an a package already pinned package to a higher version) as a flag that this is likely a pathological case, and to unpin (or backjump) and make different choices about collecting versions hopefully without having to exhaust all versions of `foo`.

And as discussed, this has the added benefit it's probable even if a version of `foo` is found which doesn't upper bound `bar`, that's because upper bounding is not considered best practise, but functionally the older version of `foo` is not compatible with the new version of `bar`. So you should end up with "better" version choices.



---

_Comment by @zanieb on 2024-03-21 23:50_

A big problem is that we _won't_ necessarily exhaust all versions of `foo`, in some cases we will just backtrack far enough that we reach a version of `foo` that is so outdated that it doesn't have requirements overlapping with `bar`. Then we find a "valid" resolution but the user much rather would have recent versions of both `foo` and `bar. 

---

_Comment by @notatallshaw on 2024-03-22 00:01_

> in some cases we will just backtrack far enough that we reach a version of `foo` that is so outdated that it doesn't have requirements overlapping with `bar`. Then we find a "valid" resolution but the user much rather would have recent versions of both `foo` and `bar`

Small correction, a newer version of `foo`, but a slightly older version of `bar`.

As I'm sure you know, this comes up outside boto3 / urllib3, this bit me one evening in production when we installed an older version of jinja2 but a newer version of markupsafe and got https://github.com/pallets/jinja/issues/1585. The backtracking by pip fell into this exact problem.



---

_Comment by @mpizenberg on 2024-03-22 07:41_

Ah I see.

First I think it would not be easy to have a more flexible backtracking algorithm in ways that enables it to go further back than necessary, which is what you need if you don't want to exhaust `foo` before unpinning `bar`. We want to guarantee two properties when backtracking (https://pubgrub-rs-guide.netlify.app/internals/conflict_resolution), such that the algorithm is sound and guaranteed to terminate. It needs to go back to a state where the cause of the conflict is "almost satisfied", meaning the algorithm can continue unit propagation (the heart of pubgrub). And it needs to go back to a state where the next decisions are guaranteed to be different than before.
I think, if we allow more "flexible" backtracking, we could invalidate these rules and so the resolver may behave in unpredictable ways. Maybe it's possible, but Jacob and I have not studied the theory of behind pubgrub (CDNL-ASP) enough to guarantee it I think.

However there is another solution to this problem IMO. Pubgrub has two sources of information, one we could call "knowledge" and one "deduction". Knowledge is what uv provides it when a new package is pinned and pubgrub requires the dependencies of that package. Deductions are self-learned through unit propagation and conflict resolution.

All that information is gathered in the form of "incompatibilities", which are sets of package versions that cannot be observed simultaneously. For example, if `foo@1` depends on `bar@1` we can craft the following incompatibility `[foo @ 1, not bar @ 1]`. This means it is forbidden to have both `foo@1` in the current solution and not have `bar @ 1` in the current solution. If the situation we want to avoid is the following:
- there are thousands of versions of `foo`, all incompatible with recent the latest versions of `bar` (v5, v6, v7).
- but the previous version of `bar` (v4) is compatible with a recent `foo`
In that case it we can avoid going through thousands of versions of `foo` by added the "knowledge" incompatibility `[foo @ anyversion, bar @ 5 <= v <= 7]`.

The issue is that currently, pubgrub does not allow the caller to provide arbitrary incompatibilities. We restrict the knowledge to only things of the shape "a depends on b". But since `uv` is working with a slightly modified pubgrub, I think they could allow providing raw incompatibilities instead. And in that case, they could provide additional "knowledge" incompatibilities such as the one we just described.

I think adding more versatile knowledge capabilities to pugrub is something quite interesting and I would have liked exploring that but haven't had the time to.

---

_Comment by @zanieb on 2024-03-22 14:49_

There may be some world where we can run multiple resolutions with lower bounds added as well... like start them concurrently then take the one that finishes with a favorable resolution first?

---

_Comment by @mpizenberg on 2024-03-22 15:41_

> There may be some world where we can run multiple resolutions with lower bounds added as well... like start them concurrently then take the one that finishes with a favorable resolution first?

That sounds like a good heuristic too. Artificially over constraint some of the packages that present the most number of versions, and run resolution in parallel. For example, if a package has 1000 different versions, run one solver where it is artificially constrained to 8 versions, another where it is artificially constrained to 32 versions, and another with 128 versions, or constraining the bounds as you suggested. Keep the first resolution that ends.

Back to the "incompatibilities" I was talking about earlier. If you enable more flexible dependency retrieval, while the solver is running, you could gather dependencies of other versions and regroup them, next time the solver asks for dependencies of a give package. For example, I'm seeing that botocore publishes almost one version per day (https://pypi.org/project/botocore/#history), but actual dependencies change much less often (https://github.com/boto/botocore/commits/develop/setup.py). So while the solver is trying stuff with v1.34.68, uv could download metadata for versions 1.34.64 to 1.34.67 that share the exact same dependencies. Then when uv backtracks and tries 1.34.67, instead of providing the incompat `[botocore=1.34.67, not urllib3>=1.25.4,!=2.2.0,<3]`, it could provide the one `[botocore>=1.34.64,<=1.34.67, not urllib3>=1.25.4,!=2.2.0,<3]` (multiple versions of botocore regrouped).
Like this, when it fails, it will skip all versions of the botocore cluster that share these exact same dependencies. This effectively reduces the cost of solving to only backtracking once per group of versions that share the same dependencies. With of course more network cost as you download more metadata while the solver is doing its thing.

Remark that all these approaches are orthogonal to each other and could all be implemented:
- artificial over-constraints for parallel resolution
- before-resolution knowledge incompatibilities (like uv knowing that `foo@anyversion` is incompatible with `bar@5,6,7`
- live regrouping of dependency incompatibilities by downloading more versions metadata while the solver is busy

---

_Comment by @notatallshaw on 2024-03-22 20:08_

> We want to guarantee two properties when backtracking (https://pubgrub-rs-guide.netlify.app/internals/conflict_resolution), such that the algorithm is sound and guaranteed to terminate. It needs to go back to a state where the cause of the conflict is "almost satisfied", meaning the algorithm can continue unit propagation (the heart of pubgrub). And it needs to go back to a state where the next decisions are guaranteed to be different than before.
> I think, if we allow more "flexible" backtracking, we could invalidate these rules and so the resolver may behave in unpredictable ways.

Yeah, I think the approach I outline breaks this "almost satisfied" rule.

Although once I fill in the rules for how permanent preferences are added, e.g. they must never break strict ordering  (so if you add `foo` < `bar` and `bar` < `baz` you can not add that `bar` < `foo`), then I it's possible to prove the stability of the resolver (i.e. it eventually comes to a halt finding a solution or unresolvable incompatibility). Though it sounds unlikely my approach can be adapted to pubgrub-rs, I will read through the design documents and see if I can adapt my approach.

All that said, I'm curious, does this mean pubgrub-rs completly forgoes [backjumping](https://en.wikipedia.org/wiki/Backjumping)?

> * but the previous version of `bar` (v4) is compatible with a recent `foo`
>   	In that case it we can avoid going through thousands of versions of `foo` by added the "knowledge" incompatibility `[foo @ anyversion, bar @ 5 <= v <= 7]`.
>
>
> The issue is that currently, pubgrub does not allow the caller to provide arbitrary incompatibilities. We restrict the knowledge to only things of the shape "a depends on b".

I'm not sure I follow, Python dependencies already have such restrictions of lower and upper bounds and not equal to, uv must pass these somehow? Is the restriction that pubgrub-rs doesn't allow "anyversion"? I suppose though that uv does know every version and could add all of them.

The problem though, is uv does **not know** this is actually true, it can only infer it's probably a good constraint. That’s why my idea revolves around preferences, not constraints.

> There may be some world where we can run multiple resolutions with lower bounds added as well... like start them concurrently then take the one that finishes with a favorable resolution first?

I think you would at least need to start a single resolution first, to extract the upper bound constraints that will conflict with existing pins.

Then I suppose you can adapt my idea into multiple resolutions, you could start a resolution normally, then when you hit this scenario start a second resolution from scratch that adds this upper bound constraint from the beginning, then:

* If the second resolution completes before the first one, cancel the first one and take the solution of the second resolution
* If the second resolution fails discard it
* If the first resolution completes first but the second resolution completes within some reasonable time frame afterwards (e.g. 100ms) evaluate which solution found a higher version of `foo` and pick that one.

It's a little convoluted but it avoids having to make sweeping changes to the resolver. And assuming all the metadata is shared in memory in a way both resolvers could use, probably not significantly expensive?

> That sounds like a good heuristic too. Artificially over constraint some of the packages that present the most number of versions, and run resolution in parallel. For example, if a package has 1000 different versions, run one solver where it is artificially constrained to 8 versions, another where it is artificially constrained to 32 versions, and another with 128 versions, or constraining the bounds as you suggested. Keep the first resolution that ends.

I think the number of versions is a bad heuristic, it solves this one example of boto3/urllib3, but if that's all uv is trying to solve it would make sense to just have uv hard code that boto3 should prefer to be resolved first (pip’s resolver did this for setuptools when it was first added, so it’s not unheard of in the Python package resolver ecosystem).

Number of versions not solve the general problem of uv picking bad versions, like the example I gave with jinja2/markupsafe, or just an "old version" which simply fails to compile, e.g. https://github.com/astral-sh/uv/issues/1560

Number of versions also doesn’t solve the problem when there may be only a few versions to solve but they are arbitrarily expensive (giant downloads and/or long build times).

> So while the solver is trying stuff with v1.34.68, uv could download metadata for versions 1.34.64 to 1.34.67 that share the exact same dependencies. Then when uv backtracks and tries 1.34.67, instead of providing the incompat `[botocore=1.34.67, not urllib3>=1.25.4,!=2.2.0,<3]`, it could provide the one `[botocore>=1.34.64,<=1.34.67, not urllib3>=1.25.4,!=2.2.0,<3]` (multiple versions of botocore regrouped).

I agree, this all sounds like it could solve speed ups in uv, but doesn't solve picking bad combinations of versions of packages nor does it avoid collecting large amounts of metadata (which can be arbitrarily expensive for Python packages).

Whereas I believe some implementation of the upper bound idea would yield significant efficiency, as well as speed ups, as my naive implementation in pip implies would work.


---

_Comment by @mpizenberg on 2024-03-22 21:07_

> I'm not sure I follow, Python dependencies already have such restrictions

So the important thing here is not what you think, it's the absence of "not" in the second part of the incompatibility. I very briefly explained why when explaining how a dependency is expressed as an incompatibility in the previous comment. But I admit all this is very abstract to someone not initiated to the dark arts of CDNL incompatibilities. I can only suggest reading more of the advanced section of pubgrub-rs guide and the original dart docs if you want to familiarize yourself more with the subject.

Also "almost satisfied incompatibilities" is probably not what you think, and is not preventing pubgrub to "backjump", which is common. I'm just using the word "backtracking" without strictly differentiating if it's just by one step or more, because pubgrub doesn't care. Here the important info is that if you can backjump "further than strictly necessary", I'm not sure we can guarantee termination. It might end up in an infinite backtrack loop.

---

_Comment by @notatallshaw on 2024-03-23 15:02_

> But I admit all this is very abstract to someone not initiated to the dark arts of CDNL incompatibilities. I can only suggest reading more of the advanced section of pubgrub-rs guide and the original dart docs if you want to familiarize yourself more with the subject.

Yeah, as soon as I get a  chanced I'll read more.

> because pubgrub doesn't care. Here the important info is that if you can backjump "further than strictly necessary", I'm not sure we can guarantee termination. It might end up in an infinite backtrack loop.

If you think of my idea as an extention to any given dependency resolution algorithm that pins and has to make choices on what to pin, and we take n to be the total possible number of packages (packages, not individual versions), and the time complexity of the original algorithm is O(f(n)), then:

 * Unpinning can only happen at most n - 1 times (due to there being no way to increasingly be more specific about ordering n elements, while following strict ordering, more than n - 1 times)
 * k unpinnings has time complexity k times the original time complexity, as the unpinning worst case is that it unpins all packages
 * Therefore the extension at worst makes the new algorithm time complexity O(nf(n))
 * And hence, if the original algorithm can guarantee termination so can the extension

I understand though, in the general case, of making pubgrub-rs more flexible that cannot be guaranteed, and this approach may not seem like a strong enough case to support it in either the core library or the fork. I will at some point try and get myself sufficently familiar with the algorithm and implementation to see if I can make a proof of concept.

---

_Referenced in [astral-sh/uv#2821](../../astral-sh/uv/issues/2821.md) on 2024-04-04 18:08_

---

_Comment by @notatallshaw on 2024-04-09 20:50_

uv seems to have made significant speed improvements since this was originally posted, running my original command on different versions of uv today:

0.1.1:   ~72s
0.1.29: ~27s
0.1.31: ~11s

That's a ~6.5x performance improvement since I opened this ticket, with the latest jump coming from https://github.com/astral-sh/uv/pull/2452, that's very impressive from the uv team!

At this point I'm not sure what to do with _this_ issue, while I feel that resolution could be improved a lot by 1) Intelligently collecting less packages, and 2) Avoiding picking "bad" resolutions, my original example has at least mitigated if not in fact fixed.

Upon finding a good example I will open a new issue for "2)", so the nuance of what is “bad” can be discussed more directly, and if I can find an even more pathological example of my original requirements I may open a new issue for "1)".


---

_Comment by @konstin on 2024-04-10 10:41_

Glad to here we're getting faster!

On my machine, i get the following numbers:

| uv       | no output file (or upgrade) | output file |
|----------|-----------------------------|-------------|
| cached   | 450ms                       | 150ms       |
| uncached | 8s                          | 1.5s        |

Is using an output file or doing a cached run to generate the locked requirements.txt an option for you?

The culprit for the bad performance is nwbinspector, which doesn't constraint it's dependencies (see their [requirements.txt](https://github.com/NeurodataWithoutBorders/nwbinspector/blob/37b6e5bfc1588feca12f5abd24dc0eac44be6450/requirements.txt) which is loaded into setup.py). It requires s3fs, which depends on aiobotocore, which causes us building many old, source dist only aiobotocore versions, which is where we spend the majority of the uncached resolution time (each bar is a aiobotocore build we later discard, the x axis is 8s, [full trace](https://gist.github.com/konstin/535d94664a016090273520016c65a060)):

![image](https://github.com/astral-sh/uv/assets/6826232/901ed8e8-cf41-4e42-aeb6-3510ec0e79ff)

While i agree that package selection can and should be improved, i urge package authors to add bounds to their version requirements, not for performance but because we otherwise can't guarantee that we don't pick an ancient unsupported version.

---

_Comment by @konstin on 2024-04-10 11:00_

Another option is "patching" nwbinspecter with `s3fs~=0.6.0`, the input below takes <2s uncached.

```
click >= 7.0
click-loglevel ~= 0.2
dandi >= 0.24.0
psutil ~= 5.9
pyyaml
s3fs~=0.6.0
selenium
```

The remaining time is split between the requests for all the packages (left), waiting for the  asciitree 0.3.3 build (exceptionally long bar in the middle, the build is ~650ms) and boto3 backtracking (right)

![image](https://github.com/astral-sh/uv/assets/6826232/cddb7b1e-8443-41b8-b62d-7f5a5367133c)



---

_Comment by @notatallshaw on 2024-04-10 23:14_

> Glad to here we're getting faster!
>
> On my machine, i get the following numbers:
> uv 	no output file (or upgrade) 	output file
> cached 	450ms 	150ms
> uncached 	8s 	1.5s
>
> Is using an output file or doing a cached run to generate the locked requirements.txt an option for you?

~~I think there's a flaw in your testing, it's only faster when there is already an out file, I've made an issue https://github.com/astral-sh/uv/issues/2983.~~

**Edit:** I misunderstood what you were asking here, now I get what you're saying. As I explain below, I'm trying to find real world cases that challenges uv's resolver, so I'm giving it the worst possible starting point. This does have real world use cases though, such as when installing Python packages in a docker image's dockerfile which is best practise not to use the tools cache and generally can't have a starting point. Or just a novice user dealing with requirements and installing for the first time.

But regardless, I don't actually have this use case, this is just a real world set of requirements that came up (on a pip issue) that I used to test how well uv's performance is, and at the time I created this issue the commands I posted seemed like the easiest way to reproduce it.

> While i agree that package selection can and should be improved, i urge package authors to add bounds to their version requirements, not for performance but because we otherwise can't guarantee that we don't pick an ancient unsupported version.

While I strongly agree with the sentiment, the problem is the existing Python ecosystem isn't going away and there are legitimate reasons to at least not put aggressive lower bounds on a dependency:

1. Aggressive lower bounds create unnecessary incompatibilities, a user may be using your library and many others, for reasons that many other languages don't have to deal with Python libraries must share the same transitive dependencies, if I require `numpy >= 2` just because it's new I'm going to make my library incompatible with a lot of the ecosystem
2. Libraries may have a very wide Python support policy, e.g. over 5 versions of Python, just because an old version of a dependency isn't compatible with a new version of Python doesn't mean your users aren't still on an older version of Python using those older versions
3. Many projects are written by users who are not at all familiar with the concept of "supported dependencies", they are data scientists, electrical engineers, font smiths, financial quants, etc., but just because they're not familiar with good software packaging practises doesn't mean they don't write useful code.

> Another option is "patching" nwbinspecter with `s3fs~=0.6.0`, the input below takes <2s uncached.

The problem is, how would a user know to do that? Could uv reveal that somehow to its users?

Again this isn't *my* use case, I already have a good understanding of Python resolution issues, I created this issue as an example where uv didn't live up to it's claim of resolving requirements in a few seconds. However, it very close now with this example, which is great!

I am still working on this approach of unpinning upper bounded conflicting dependencies that I outlined in earlier comments with the pip's resolver, IMO this is the best approach to untangling these tricky situations. I'm not sure it will ever work for uv, but at least I'll be able to provide evidence it can work for some resolvers. 


---

_Comment by @konstin on 2024-04-11 09:21_

We provide resolution strategies `lowest` and `lowest-direct` that make it easy to tests that your lower bounds work. Just `uv pip install --resolution -e . && pytest` (or your favourite test command) and you can verify your bounds, i want to make it easier to have correct bounds than to not have them. If too aggressive bounds should be the problem, we offer overrides and error message that (hopefully) point you to the right dependency.


---

_Comment by @notatallshaw on 2024-04-11 13:31_

 > We provide resolution strategies `lowest` and `lowest-direct` that make it easy to tests that your lower bounds work. Just `uv pip install --resolution -e . && pytest` (or your favourite test command) and you can verify your bounds, i want to make it easier to have correct bounds than to not have them. If too aggressive bounds should be the problem, we offer overrides and error message that (hopefully) point you to the right dependency.

Yes, I love that uv has these features, particularly override which works how I [proposed](https://github.com/pypa/pip/issues/8076#issuecomment-1165066807) in 2022 for pip, and I think they are going to help improve the ecosystem a lot, especially once people have had the chance to write blogs on them and build up best practises.

In the original example given here I do think `lowest-direct` helps, I tried it now and adding a lower bound on the failing selenium improved performance a lot. I do think though, currently, this works well for advanced users but there are barriers to someone who does not have a good grasp on dependency resolution:

1. Understanding there are different resolution strategies
2. Understanding how to do a loop of trying, waiting for a failure, adding lower bounds on the dependency that failed
3. Understanding what is a reasonable version to put on a lower bound

I wonder how much uv can prompt users, or how much it's going to have to be built up as best practices in the python ecosystem and taught to users.



---

_Comment by @konstin on 2024-04-12 07:46_

We'll soon be in the situation that we will have to set defaults in uv, so let me try to answer this as we would in our docs:

When you start a new project or add a new dependency, the default should be starting with the current version, let's call it x.y.z, and it's compatible version range. When using `uv add foo`, for simplicity we'll `foo >=x.y.z,<(x+1).0.0`, but you are encouraged to check with the project's versioning policy.

For the project structure we recommend a ci job `minimal-versions`, our project templating will generate one. Your policy on bumping minimal version can be different depending on the project, most will bump whenever they want to use a newer feature, while core libraries and frameworks will have policies for wide ranges. If you add compatibility for foo x+1, you are encouraged to keep the old lower if it is compatible, in our example `foo >=x.y.z,<(x+2).0.0`.

> Libraries may have a very wide Python support policy, e.g. over 5 versions of Python, just because an old version of a dependency isn't compatible with a new version of Python doesn't mean your users aren't still on an older version of Python using those older versions

Similar to testing minimal dependency versions, we recommend testing on all python versions you support, or at least min and max versions, so the min version job catches the problem. Our lockfiles will have the ability to model using different versions of a dependency based on the python version if the case arises (but with the recommendation to pick dependencies that support at least as many python versions as you do). The cli will make it easy to test a different version, something like `uv run -p 3.9 pytest`, and we'll try to make the new venv creation blazingly fast ;)

---

Let me go back to the `numpy >= 2`: The core problem is that a new major release of a fundamental library causes an ecosystem split. Let us generalize this as a library B has a major update and a library A that requires B in version range R. We can model this as a predictor where a given version V, we want to predict the target "A would break" with the predictor "outside the version range R". If we do this for all libraries A that require B, we get a confusion matrix (sorry for pulling in statistics here but it's the clearest way for me to express this tradeoff):

* True positives: Library A declared as incompatible that would break with V
* False positives: Library A declared as incompatible that would be fine with V
* True negative: Library A declared as compatible that would be fine with V
* False negative: Library A declared as compatible that would break with V

For context, the usual usage is:
* True positives: Test says you have a disease and you have that disease (good)
* False positives: Test says you have a disease but you don't have that disease (bad)
* True negative: Test says you don't have a disease and you don't that disease (good)
* False negative: Test says you don't have a disease but you do have that disease (bad)

When everybody puts bounds on their dependencies (this kinda applies to both lower and upper bounds), we get false positives ("That library clearly works on numpy 1 and numpy 2, why is this breaking my dependency resolution?"), if nobody puts bounds we will get false negatives ("Why did updating torch suddenly break my app?"). The remedy for too strong bounds are overrides, the remedy for missing bounds are constraint files (or just adding them as direct dependencies). (And before overrides existed, [caps couldn't be fixes by users](https://iscinumpy.dev/post/bound-version-constraints/#planned-obsolescence), so no bounds was the only option really)

From my personal development style, i prefer having bounds, even if they are too strong. It means i can generally just update dependencies and only need to worry about incorrect constraints when resolution fails (and i get the offending package in the resolver error). Without bounds, i would have to carefully check all package updates in my dependency trees for compatibility and that we didn't get into a pathological backtracking case picking wrong versions. I prefer the confidence of maintainer-declared compatibility, even if means more work for overriding this when it fails.

---

_Comment by @notatallshaw on 2024-04-12 14:30_

> When using `uv add foo`, for simplicity we'll `foo >=x.y.z,<(x+1).0.0`, but you are encouraged to check with the project's versioning policy.

FYI, adding upper bounds on a library is **strongly** discouraged in the Python community. You've linked [this blog post](https://iscinumpy.dev/post/bound-version-constraints/ ) so you probably already know but the tl;dr is:

> Capping dependencies has long term negative effects, especially for libraries, and should never be taken lightly

I would be unhappy to use uv as a project management tool if upper bounds were added by default. And in general, while some Python users use project management tools, especially software developer engineer teams, my personal experience is that a lot of Python users are not so advanced in this area and find project management tools too complicated, so none of this will help them.

As a smaller side note, assuming semver for bounds requirements seems pretty unrealistic, I don't know any Python project that strictly follows semver.

> When everybody puts bounds on their dependencies (this kinda applies to both lower and upper bounds), we get false positives ("That library clearly works on numpy 1 and numpy 2, why is this breaking my dependency resolution?"), if nobody puts bounds we will get false negatives ("Why did updating torch suddenly break my app?"). The remedy for too strong bounds are overrides, the remedy for missing bounds are constraint files (or just adding them as direct dependencies). (And before overrides existed, [caps couldn't be fixes by users](https://iscinumpy.dev/post/bound-version-constraints/#planned-obsolescence), so no bounds was the only option really)

I strongly disagree that in general False positives are better than False negatives for many reasons:

 * Currently the most popular Python package install tools do not support overrides so most users are not helped by this feature
 * The way uv has implemented overrides I believe is currently unproven (as it's global rather than package/version specific like in other language package tools), there could be situations where it can not solve the problem without the user pinning most of their library versions of many projects they've had to manually figure out, in which case the user may as well just pin everything in your requirement file with "--no-deps"
 * A library author can not express overrides to their user when it is installed from a wheel, they must tell every user what to override
 * For complicated dependencies it's not always clear what override would fix things, it can take a very in depth understanding of the dependency graph
 * Even for simple dependency graphs the solution is non-obvious to users inexperienced with investigating dependency graphs
 * What a user means by compatible and what a library author means by compatible are often two different things, and putting extra restrictions on a dependency because it doesn't work in some cases may cause problems for users who do not rely on those use cases
 * In general the failure mode of a False positive is the user gets a nasty error from installer and they don't know how to proceed, where as the failure mode of a False negative is either nothing or their logic doesn't work and they need to add stricter constraints on their requirements
 * With sufficiently smart heuristics a resolver may have the ability to avoid a False negative automatically, the resolver can do nothing automatically about False positives

I believe this approach of "let's be over restrictive" works well in languages that can install multiple versions of the same library (e.g. rust, and nodejs), and is common practise and accepted by the respective ecosystem, but I do not believe it would work well in the Python packaging world.




---

_Comment by @konstin on 2024-04-12 16:57_

> And in general, while some Python users use project management tools, especially software developer engineer teams, my personal experience is that a lot of Python users are not so advanced in this area and find project management tools too complicated, so none of this will help them.

The plan is to remove the overhead from a project management tool that represents another, `uv add` should be easier to use than venv + `[uv] pip install`, especially for non-experts. You should not feel you are doing project management using a tool that provides an additional layer of abstraction you need to learn and manage, you should think "i need library X", run `uv add x` and next time you run your code it's there.

> As a smaller side note, assuming semver for bounds requirements seems pretty unrealistic, I don't know any Python project that strictly follows semver. 

Agreed, unlike poetry we don't assume semver, but we need to set some defaults and the current version to the next major version seems like the best guess to me. We should also have different behavior when something is clearly calver (a release in `YYYY` starting with major version `YYYY`, maybe some other heuristic too, there aren't too many packages on version >20 that could conflict.)

I've reviewed the direct dependencies' changelogs for a web project i used to work on:
* Django: Their own semantic versioning
* PyPDF2: Breaking changes only seen in major version bumps
* Wand: A quick search didn't bring up any breaking changes
* cattrs: Calver
* django-allauth: 0.x.y, backwards incompatible changes happen only when bumping x
* django-anymail: Explicit semver compliance
* django-decorator-include: Unclear from changelog
* django-elasticsearch-dsl: Versioning bound to elasticsearch
* django-environ: 0.x.y, backwards incompatible changes happen only when bumping x
* django-geojson: Breaking changes only seen in major version bumps
* django-q: Lacks proper changelog
* django-settings-export: Explicit semver compliance
* django-simple-history: Backwards incompatible changes in major and minor releases
* django-webpack-loader: Unclear from changelog
* django-widget-tweaks: Unclear from changelog
* django_csp: Breaking changes only seen in major version bumps
* elasticsearch: Versioning bound to elasticsearch
* elasticsearch-dsl: Versioning bound to elasticsearch
* geoextract: Unclear from changelog
* geopy: Breaking changes in major and minor releases
* gunicorn: Calver
* html2text: Calver
* icalendar: Breaking changes only seen in major version bumps
* jsonfield: Breaking changes only seen in major version bumps
* minio: Unclear from changelog
* mysqlclient: Unclear from changelog
* osm2geojson: No changelog
* psycopg2: Backwards incompatible changes in major and minor releases
* python-dateutil: Unclear from changelog
* python-slugify: Unclear from changelog
* requests: api changes in major and minor releases
* sentry-sdk: Breaking changes only seen in major version bumps
* splinter: Unclear from changelog
* tqdm: Semver ([ref](https://github.com/tqdm/tqdm/pull/1212#issuecomment-890495915))

For a small bot i maintain:
* pywikibot: Breaking changes only seen in major version bumps
* mwparserfromhell: 0.x.y, backwards incompatible changes happen only when bumping x
* CacheControl: Breaking changes only seen in major version bumps
* yarl: Semver ([ref](https://github.com/aio-libs/yarl/pull/217#issuecomment-411996141))
* pytest: Explicit semver compliance
* ruff: Semver (custom since no python api, [ref](https://docs.astral.sh/ruff/versioning/))

I'd love to have more data on this!

Note that when talking about semantic versioning/semver, i don't mean the Semantic Versioning 2.0.0 spec specifically. I'm interested in the class of versioning policies where incompatible changes in the public interface mean a bump of the x in x.y.z or 0.x.y. I don't think of semver as a legal contract, but as _communication_. It is a way of talking with others about properties of software releases in a distributed system of volunteers.

> The way uv has implemented overrides I believe is currently unproven (as it's global rather than package/version specific like in other language package tools), there could be situations where it can not solve the problem without the user pinning most of their library versions of many projects they've had to manually figure out, in which case the user may as well just pin everything in your requirement file with "--no-deps"

I think adding more specific overrides would be a good idea in general. I'd be interested in hearing any use case where our current overrides fail for a specific project.

> A library author can not express overrides to their user when it is installed from a wheel, they must tell every user what to override

I agree this is this is the biggest limitation right now, and i'd love to add a way to express this. We could define a uv standard for propagating hints, but i'd rather solve this on the ecosystem level than adding uv-specific extensions to wheels. I'm thinking something like a `Override-Hint: ...` METADATA filed or a `[override-hints]` pyproject.toml table.

> In general the failure mode of a False positive is the user gets a nasty error from installer and they don't know how to proceed, where as the failure mode of a False negative is either nothing or their logic doesn't work and they need to add stricter constraints on their requirements

In uv, we put a lot of effort into making the false positive a good, actionable error message. The error trace shows you all packages and versions involved (If not, please file a bug and we'll try to fix it!). For false negatives on the other hand, failures are runtime errors (import errors, attribute error, type error) and require the test suite to cover the changed code path. Apart from being associated with a version update commit, there is no indication the crash was caused by an incompatibility. Semantic changes (e.g. a parsing library changing what they consider safe inputs and what must be sanitized, changing performance characteristics in a db) go unnoticed. You are required to check the release notes for your entire dependency tree when updating. When adding a new library, you can't be certain what dependency versions to add. To do this properly, you would need to figure out the versions the release was tested with, and either use those or check the changelog and compatibility for all versions for its dependencies and transitive dependencies since.

> With sufficiently smart heuristics a resolver may have the ability to avoid a False negative automatically, the resolver can do nothing automatically about False positives

The inverse is the case: If the only solution is one that involves pathological backtracking, we have to use this solution. We had cases where A and B would conflict on C, which were "solved" by backtracking to an ancient version of B that did not yet depend on C. We can (and should) add heuristics to try the more reasonable solution first, but a solver ultimately can't tell if something is reasonable or not. For conflicts, we can show suggestions about overrides and bounds derives from the pubgrub trace.

> I believe this approach of "let's be over restrictive" works well in languages that can install multiple versions of the same library (e.g. rust, and nodejs), and is common practise and accepted by the respective ecosystem, but I do not believe it would work well in the Python packaging world.

While the problem is certainly more significant in python, the same problem does exist in rust and js, specifically when types of dependencies are part of the public api. Examples are our recent reqwest update (https://github.com/astral-sh/uv/pull/2817), but i also ran into dependency hell with react and webpack upgrades.

---

_Comment by @dimbleby on 2024-04-12 17:35_

> unlike poetry we don't assume semver, but we need to set some defaults and the current version to the next major version seems like the best guess to me

I think that is _exactly_ like poetry.  AFAIK the only sense in which poetry "assumes semver" is that it defaults to disallowing major version bumps

not sure this is really the place to debate it but fwiw I mostly agree with you.  Upper bounds and no upper bounds both have their problems - they say "semantic versioning will not save you", but I can think of several examples where it either did or would have done.

I also mostly prefer to err on the side of "no surprise breakages".  if I find myself using a dependency that is unwilling or unable to update the bounds on its own dependencies in reasonable time - well I consider myself pleased to have learned that I am using unmaintained code, and it is time to look elsewhere.

---

_Comment by @hmc-cs-mdrissi on 2024-04-12 19:09_

Yeah that specific assumption has tendency to leave poetry based projects as having too tight/fake dependency constraints. I've seen a lot of libraries have next major version as upper bound even though it works fine in practice on our unit/integration test suite. I'm very distrustful of python library upper bounds. Sometimes they are real bound based on actual error/issue, but too often I dig into repository and find no explanation on bound and it behaves fine with it relaxed. It's caused several times fake dependency conflicts where I wind up forking wheel and rewriting dependency metadata to be more relaxed to make pip happy. UV doing it by default on `uv add` command would just increase my distrust for upper bounds as meaningful.

Even "real" upper bounds of actual bugs/crashes vary in impact. Sometimes upper bound is for actual major change that will lead to crashes most of time. I've occasionally seen major libraries do upper bound for an issue that only appears in certain platform (windows for example) and as mac/linux user the upper bound was unnecessary entirely. Other times upper bound only matters for very specific usage patterns of that library my tests confirm is not a concern. It's difficult to know why upper bound constraint was added unless you read repository history directly and even there often pr adds upper bound with no commit explanation/description.

UV does have advantage of overrides file existing so I can more easily ignore conflicts caused by upper bounds. It does sorta promote though that is a user relies on uv add, that a different user that uses pip where overrides are not thing will have trouble dealing with these assumed upper bounds.

edit:

```
django-geojson: Breaking changes only seen in major version bumps
```
I have no familiarity with this specific package, but package trying to follow `semvar` means very little in my experience.  Different packages have inconsistent opinions of what counts as breaking enough. Many libraries will deprecate/remove uncommonly/rarely used functions in minor versions while claiming breaking changes is for major versions. Or breaking change is a bug fix itself. My own experience is most upgrades work, but upgrades that fail are big toss up in what version change was and often inconsistent with libraries claims of compatibility.

The article linked on upper bounds goes into this in more depth that notion of semvar/true breaking changes is very inconsistent in python ecosystem.

---

_Comment by @konstin on 2024-04-12 19:49_

> not sure this is really the place to debate it

The original report has been solved except for the specific subproblem of bounds and wrong backtracking, so i'm repurposing this thread for the more general problem of versioning and bounds.

---

> It's difficult to know why upper bound constraint was added unless you read repository history directly and even there often pr adds upper bound with no commit explanation/description.

When i add an upper bound, it's because i don't yet know how big the changes in the next major version of a dependency will be. Without a bound, i'd lock the authors of the library into not making major changes. If the new version is already released, extending the range to include this major version is totally the thing to do, or at least creating and linking an issue for the update.

Here conda being a centrally managed repository is at an advantage, they can use repodata patching to repair and extend already published versions with updated metadata information.

> UV does have advantage of overrides file existing so I can more easily ignore conflicts caused by upper bounds. It does sorta promote though that is a user relies on uv add, that a different user that uses pip where overrides are not thing will have trouble dealing with these assumed upper bounds.

I hope that pdm and uv will inspire pip to also add overrides.


---

Just to speak this clearly: I very much feel your pain about upper version bounds! I had to patch projects myself just to fix their metadata and deal with incompatibilities in general, and i see how this leads to distrusting them in general.


---

_Comment by @hauntsaninja on 2024-04-12 20:09_

I'd also prefer the default to be to not add upper bounds. If uv's high level project workflow involves a distinction between applications and libraries, maybe the default of adding upper bounds or not could depend on that (since false positive upper bounds in libraries are often only felt by the rest of the ecosystem, not the authors).

---

_Comment by @hmc-cs-mdrissi on 2024-04-12 20:42_

On backtracking pains to ancient versions I'd also be happier with lower bounds being auto-added with some heuristic of say oldest version in last ~2/3 years. Lower bounds tend to cause fewer issues. Not always correct, but I'd prefer over automatic upper bounds. Part of it is that very old backtracking is risky in it's own ways as default.

---

_Comment by @dimbleby on 2024-04-12 22:08_

> Lower bounds tend to cause fewer issues. 

It perhaps feels that way, because of a preference for resolving conflicts by taking more recent versions.  But this cannot logically be true: every conflict you have ever seen involved exactly as many lower bounds as upper bounds (namely, one of each).

Upper bounds get all the heat!  But eg the typical practice of starting a project with "latest version" as the lower bound for everything might almost be aimed at maximizing conflicts.

If something like a `minimal-versions` build catches on, broad lower bounds should be a much easier pitch than broad upper bounds.  Testing against a dependency that has already been released is much more reliable than guessing what the future might hold.

(Of course broad lower bounds _also_ have downsides, there is no silver bullet.  eg per this very thread, tighter lower bounds are often a good practical solution to deal with slow resolution)

---

_Comment by @notatallshaw on 2024-04-12 23:56_

@konstin:

> We should also have different behavior when something is clearly calver (a release in `YYYY` starting with major version `YYYY`, maybe some other heuristic too, there aren't too many packages on version >20 that could conflict.)

Pip is semi-calver but it's format is a two digit year, I doubt it's the only one.

>  I'm interested in the class of versioning policies where incompatible changes in the public interface mean a bump of the x in x.y.z or 0.x.y. I don't think of semver as a legal contract, but as communication. It is a way of talking with others about properties of software releases in a distributed system of volunteers.

I would be shocked if you find something consistent across the Python ecosystem, just looking at a very popular library like Pandas and it has a complex deprecation policy that does not fit well into this schema. They do a major bump for large breaking changes, but the public api absolutely changes and breaks between minor versions.

I really would advise against this approach, the much easier approach is to just not set a default upper bound.

> I think adding more specific overrides would be a good idea in general. I'd be interested in hearing any use case where our current overrides fail for a specific project.

Since uv was released my plan was to go through https://github.com/pypa/pip/issues/8076 and find real world examples and see if uv's overrides actually help, this would also build evidence to encourage pip to implement a similar feature. I haven't had time yet, but it's on my list!

> I agree this is this is the biggest limitation right now, and i'd love to add a way to express this. We could define a uv standard for propagating hints, but i'd rather solve this on the ecosystem level than adding uv-specific extensions to wheels. I'm thinking something like a Override-Hint: ... METADATA filed or a [override-hints] pyproject.toml table.

While I am a big proponent of tools forging ahead with a new ideas _**please**_ communicate this back to the larger packaging ecosystem, I foresee a few issues that could happen:

1. These wheels could break other tooling (including PyPI validating metadata)
2. People getting very annoyed with the fracturing of the Python packaging ecosystem
3. If communicated badly, or not at all, it could easily be perceived as a hostile takeover of Python packaging tools e.g. "my library only resolves with uv"

> In uv, we put a lot of effort into making the false positive a good, actionable error message. The error trace shows you all packages and versions involved (If not, please file a bug and we'll try to fix it!).

Well there's two parts here "actionable error message" and "shows you all packages and versions involved", I agree uv does a good job of showing the latter, I do **not** agree uv does a good job on the former. For any moderately complex resolution failure I find uv's error message breaks my brain trying to understand it, and I've spent the last 4+ years helping many users with resolution failures.

I have already filed https://github.com/astral-sh/uv/issues/2065 on a specific example of where I think uv is not being clear, I will file more issues on where I just think the information is overwhelming and unactionable.

> For false negatives on the other hand, failures are runtime errors (import errors, attribute error, type error) and require the test suite to cover the changed code path.

Your test suite only needs to cover the code logic you rely on, which if you are good at testing your own code then it's already covered implicitly. So when versions change you should need to add any new tests unless your tests reveal something has broken.

It shouldn't matter to you if some part of your dependency you don't use stopped working correctly with a different library.


> Apart from being associated with a version update commit, there is no indication the crash was caused by an incompatibility. Semantic changes (e.g. a parsing library changing what they consider safe inputs and what must be sanitized, changing performance characteristics in a db) go unnoticed. You are required to check the release notes for your entire dependency tree when updating. When adding a new library, you can't be certain what dependency versions to add. To do this properly, you would need to figure out the versions the release was tested with, and either use those or check the changelog and compatibility for all versions for its dependencies and transitive dependencies.

Yes, this is somewhat the nature of the Python ecosystem, and it is solved with good tests, not arbitrary upper bounds that will cause everything to be unresolvable with each other in a few years.

There are pros as well as cons, it gives a lot of flexibility to write a really good library, and incompatibilities don't cause failures if they're not on your code path, and old libraries can work seamlessly with new libraries.

Also because of Python’s flexibility is that an author might think something is not a breaking change, but it breaks your application anyway, so breaking changes for you might not be surfaced by major version jumps, and it's prudent to be cautious any time new versions of libraries get installed. Thinking major version bumps are the only thing that will break you in the Python world would be a real false sense of security.

I would personally prefer uv to focus on communication of versions rather than arbitrary limits of versions. e.g. “you ran upgrade and these 2 libraries bumped a major version, these 5 libraries bumped a minor version, and these 50 libraries bumped a bugfix version”.

> The inverse is the case: If the only solution is one that involves pathological backtracking, we have to use this solution. We had cases where A and B would conflict on C, which were "solved" by backtracking to an ancient version of B that did not yet depend on C. We can (and should) add heuristics to try the more reasonable solution first, but a solver ultimately can't tell if something is reasonable or not. For conflicts, we can show suggestions about overrides and bounds derives from the pubgrub trace.

I agree that in the entire mathematical set of all possible dependency graphs you can not ultimately tell if something is a reasonable solution or not.

I do not agree that one could not write a resolver for the Python ecosystem that for almost all real world requirements it could not find a reasonable solution using either good heuristics or flexible solving as I've discussed (https://github.com/astral-sh/uv/issues/1560#issue-2139737123, https://github.com/astral-sh/uv/issues/1560#issuecomment-2052673990, https://github.com/astral-sh/uv/issues/1398#issuecomment-1947572237).

> The original report has been solved except for the specific subproblem of bounds and wrong backtracking, so i'm repurposing this thread for the more general problem of versioning and bounds.

I personally don't mind if this issue is closed, this set of requirements has gone from "this is taking a lot longer than usual" to "oh that took a few seconds rather than less than a second". As I stated in https://github.com/astral-sh/uv/issues/1398#issuecomment-2046026863 I'll open some new issues, one with real world example of uv choosing bad solutions because of when it encounters bounds (I have at least one recent example), and others where uv's performance is still slower than expected (I tested some requirements recently I use in my workflow for my job and uv took over 1 minute to resolve which is probably more than expected).

> When i add an upper bound, it's because i don't yet know how big the changes in the next major version of a dependency will be. Without a bound, i'd lock the authors of the library into not making major changes. If the new version is already released, extending the range to include this major version is totally the thing to do, or at least creating and linking an issue for the update.

There is no contract between you and the library author, and users often complain of breaking changes whether it’s a major version bump or not. The problem with your approach though is that by adding an upper bound you lock _your users_ into older versions of your dependencies.

Let's say you write a library with dependency `numpy>=1.20, numpy<2`, your library doesn't actually violate any changes that will happen in `numpy` `2` but you don't know that yet so you're being cautious. Some time in the future someone tries to use your library but they have another dependency that does depend on `numpy>=2` as it uses a new API.

Now the user is stuck, for whatever reason your library is no longer being updated, and the other dependency they use can not support `numpy`. Even though everything would have worked together well, you've locked out users arbitrarily.

Let's say this user isn't using your library or the other library directly, but they're part of very deep transitive dependency chains, so when the user tries to install their package uv thinks about it for a minute and then gives a giant error that the user doesn't know what to do with.

The chances of this scenario are increased exponentially based on the number of dependencies that introduce upper bounds. That's why the blog post you linked was so adamant about being extremely careful how you introduce them and "should never be taken lightly".

> Just to speak this clearly: I very much feel your pain about upper version bounds! I had to patch projects myself just to fix their metadata and deal with incompatibilities in general, and i see how this leads to distrusting them in general.

If we end up living in a world where every user has to provide large override files to their Python package installer, that is not better than the one we have now. I would again advise to think carefully before making upper bounds a default behavior.

@hauntsaninja

> If uv's high level project workflow involves a distinction between applications and libraries, maybe the default of adding upper bounds or not could depend on that

While it's not as big of a deal for applications I would still advise against upper bounds unless you know there is an incompatibility. The solution to stop random problems from happening is lock files, you should always be installing the same version of a library unless you are explicitly asking your tool to upgrade the lock file, or you add a new library which forces other dependency changes, when that happens you can run tests and validate any changes which should be clear from the changes in the lock file.

@dimbleby

> It perhaps feels that way, because of a preference for resolving conflicts by taking more recent versions. But this cannot logically be true: every conflict you have ever seen involved exactly as many lower bounds as upper bounds (namely, one of each).

This seems mathematically intuitive but isn't representative of the real world scenario of when you add bounds. The reason is because lower bounds are making statements about the past which are known, whereas upper bounds are making statements about the future which are unknown.

You add bounds at a point in time, and at that point in time you can prove the incompatibility of lower bounds, whereas if you add a preemptive upper bound you are making a guess about the future. Once you have uploaded your package to PyPI you do not get a chance to go back and edit those bounds (something I think should seriously be considered but isn't the case yet).

While lower bounds at the moment are rather arbitrary in the Python packaging ecosystem because we've never had a good tool before uv that can resolve `lowest` and `lowest-direct`, they are less problematic, and hopefully with uv they will get less arbitrary.



---

_Comment by @dimbleby on 2024-04-13 06:55_

re lower bounds: your tone suggests that you think you are disagreeing with me, but I am not sure this is the case.  The points you make are fully consistent with what I wrote.

re upper bounds: I think we are saying nothing new at this point.  Adding a bound, or not, is a guess about future compatibility which will certainly sometimes be wrong - with negative consequences either way.

In practice I find that it makes very little difference when dealing with maintained projects - either a bound will be relaxed, or an incompatibility will be fixed.  It is the unmaintained projects that are problematic and - as earlier - one should prefer to avoid those anyway.

---

_Comment by @notatallshaw on 2024-04-13 12:57_

> re lower bounds: your tone suggests that you think you are disagreeing with me, but I am not sure this is the case. The points you make are fully consistent with what I wrote.

Apologies! My eyes were blurring over by the time I was getting to the end of that ridiculously long post, I probably lost the plot a little bit, thanks for reading through and carefully reflecting.

I really dislike writing long posts like that, but seemed important to get all my thoughts out and reflect on everything said. I don't plan to add anything more for a while unless I think there is something especially novel and unsaid missing.

---

_Comment by @notatallshaw on 2024-04-17 00:18_

This issue has fractured into many different discussions, I feel it's even difficult to respond at this point in a useful way. To summarize the different threads:

 * uv has got much faster at resolving since it became public
 * uv might be able to improve further, such as using upper bounds in a conflict as heuristic of a pathological resolution, or running multiple resolutions with different optimistic contraints (etc.), but there are limits to the flexibility of pubgrub-rs
 * Library authors **should** put some lower bounds on requirements
 * There are reasons that putting strong lower bounds can be undesirable, or users are not sufficently educated about the topic
 * uv might be able to help here in the future as more people understand uv's different resolution options and/or more tooling is built on top that
 * Strong disagreement about putting upper bounds on requirements, especially by default, especially especially for library code

Personally, I think it would be more fruitful to continue these discussions in specific issues, as though are brought up, with real examples or as/when uv adds relevant features. So I'm going to close this issue, but if there is disagreement on this I am not going to object to it being reopened.

---

_Closed by @notatallshaw on 2024-04-17 00:18_

---

_Referenced in [astral-sh/uv#3087](../../astral-sh/uv/pulls/3087.md) on 2024-04-17 16:43_

---

_Referenced in [astral-sh/uv#3078](../../astral-sh/uv/issues/3078.md) on 2024-04-18 03:44_

---

_Referenced in [astral-sh/uv#4333](../../astral-sh/uv/issues/4333.md) on 2024-06-15 02:06_

---

_Referenced in [astral-sh/uv#4372](../../astral-sh/uv/issues/4372.md) on 2024-06-18 00:34_

---

_Referenced in [astral-sh/uv#5962](../../astral-sh/uv/issues/5962.md) on 2024-08-09 22:18_

---

_Referenced in [astral-sh/uv#6404](../../astral-sh/uv/issues/6404.md) on 2024-08-22 17:04_

---

_Referenced in [astral-sh/uv#8128](../../astral-sh/uv/issues/8128.md) on 2024-10-11 17:29_

---

_Referenced in [astral-sh/uv#7810](../../astral-sh/uv/issues/7810.md) on 2024-10-15 15:14_

---

_Referenced in [pypa/pip#13001](../../pypa/pip/pulls/13001.md) on 2024-10-18 01:01_

---

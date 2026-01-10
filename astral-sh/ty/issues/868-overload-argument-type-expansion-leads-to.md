```yaml
number: 868
title: Overload argument type expansion leads to combinatoric explosion
type: issue
state: closed
author: qarmin
labels:
  - fatal
  - overloads
  - memory
assignees: []
created_at: 2025-07-22T08:33:11Z
updated_at: 2025-08-25T09:43:05Z
url: https://github.com/astral-sh/ty/issues/868
synced_at: 2026-01-10T02:06:24Z
```

# Overload argument type expansion leads to combinatoric explosion

---

_Issue opened by @qarmin on 2025-07-22 08:33_

Minimized file content(this takes ~4GB ram on my computer, original file(attached also at the bottom) takes more than 20GB)


```
    self.log(level, http_response, args, exc_info=exc_info)


def log_request(self, level, operationType, additionalCorrelationIds=None, correlationId=None,
                deploymentConfiguration=None, durationInMs=None, endpoint=None,
                endpointHostName=None, endpointPath=None, endTime=None,
                exceptionClass=None, exceptionDetail=None,
                exceptionMessage=None, flagged=None, flaggedDescription=None,
                httpMethod=None, httpStatusCode=None, messageName=None,
                messageStats_Avg=None, messageStats_Count=None,
                messageStats_ErrorCount=None, messageStats_Max=None,
                messageStats_P50=None, messageStats_P90=None,
                messageStats_P99=None, persistentConnection=None, product=None,
                retryCount=None, secondaryCorrelationId=None, sourceCloudPlatform=None,
                startTime=None, exc_info=None):
    args = dict(additionalCorrelationIds=additionalCorrelationIds,
                correlationId=correlationId,
                deploymentConfiguration=deploymentConfiguration,
                durationInMs=durationInMs,
                endpoint=endpoint,
                endpointHostName=endpointHostName,
                endpointPath=endpointPath,
                endTime=endTime,
                exceptionDetail=exceptionDetail,
                exceptionMessage=exceptionMessage,
                flagged=flagged,
                flaggedDescription=flaggedDescription,
                httpMethod=httpMethod,
                httpStatusCode=httpStatusCode,
                messageName=messageName,
                messageStats_Avg=messageStats_Avg,
                messageStats_Count=messageStats_Count,
   True              messageStats_ErrorCount=messageStats_ErrorCount,
                messageStats_Max=messageStats_Max,
                messageStats_P50=messageStats_P50,
                messageStats_P90=messageStats_P90,
```

running `ty check file.py` causes app out of memory crash.
Looks that `check_types` allocates a lot of memory

<img width="3835" height="1929" alt="Image" src="https://github.com/user-attachments/assets/ab0bf440-7926-496b-b7d7-d9114c25cfcc" />

[Untitled Folder.zip](https://github.com/user-attachments/files/21362928/Untitled.Folder.zip)

---

_Label `memory` added by @AlexWaygood on 2025-07-22 08:45_

---

_Label `fatal` added by @MichaReiser on 2025-07-22 08:56_

---

_Comment by @sharkdp on 2025-07-22 15:16_

Thank you for reporting this!

It runs to completion (after several *seconds*) on my laptop with a peak memory usage of 2.8 GiB(!).

This is probably expected for some reason, but adding a note here that the salsa memory report only accounts for 14 MiB here.

Here's a slightly cleaned-up version, which I made a bit more extreme (memory usage grows with the number of parameters, this uses 12 GiB). Note that the `True` in invalid position is required for this to reproduce:
```py
def f(
    a1=None,
    a2=None,
    a3=None,
    a4=None,
    a5=None,
    a6=None,
    a7=None,
    a8=None,
    a9=None,
    a10=None,
    a11=None,
    a12=None,
    a13=None,
    a14=None,
    a15=None,
    a16=None,
    a17=None,
    a18=None,
    a19=None,
    a20=None,
    a21=None,
    a22=None,
    a23=None,
):
    args = dict(
        a1=a1,
        a2=a2,
        a3=a3,
        a4=a4,
        a5=a5,
        a6=a6,
        a7=a7,
        a8=a8,
        a9=a9,
        a10=a10,
        a11=a11,
        a12=a12,
        a13=a13,
        a14=a14,
        a15=a15,
        a16=a16,
        a17=a17, True
        a18=a18,
        a19=a19,
        a20=a20,
        a21=a21,
        a22=a22,
        a23=a23,
    )
```

I get similar tracing results to you. In addition, here's a heaptrack profile which shows where most of the memory is allocated:

<img width="1216" height="1127" alt="Image" src="https://github.com/user-attachments/assets/67be77ae-7eb9-4c27-bfec-f91adfbbc1c2" />

---

_Comment by @MichaReiser on 2025-07-22 16:15_

> This is probably expected for some reason, but adding a note here that the salsa memory report only accounts for 14 MiB here.

That could be because we don't track heap memory usage of salsa structs yet (e.g. we don't track the heap allocations of `Unions`. There's a PR that will add this tracking).

---

_Comment by @MichaReiser on 2025-07-22 16:26_

The trace would suggest that it comes from here:

https://github.com/astral-sh/ruff/blob/07e2747a21e17da81ff3c9433e27721bb8a9d61e/crates/ty_python_semantic/src/types/call/arguments.rs#L150-L184

And I suspect the culprint could be 

```Rust
let mut expanded_arguments = Vec::with_capacity(expanded_types.len() * previous.len());
```

CC: @dhruvmanila 

We might want to add some limit on how many types we want to expand



---

_Comment by @carljm on 2025-07-22 18:30_

The issue also repros the same if the `True` argument uses valid syntax (followed by a comma) and is at a valid location (first in the argument list):

```py
def f(
    a1=None,
    a2=None,
    a3=None,
    a4=None,
    a5=None,
    a6=None,
    a7=None,
    a8=None,
    a9=None,
    a10=None,
    a11=None,
    a12=None,
    a13=None,
    a14=None,
    a15=None,
    a16=None,
    a17=None,
    a18=None,
    a19=None,
    a20=None,
    a21=None,
    a22=None,
    a23=None,
):
    args = dict(
        True,
        a1=a1,
        a2=a2,
        a3=a3,
        a4=a4,
        a5=a5,
        a6=a6,
        a7=a7,
        a8=a8,
        a9=a9,
        a10=a10,
        a11=a11,
        a12=a12,
        a13=a13,
        a14=a14,
        a15=a15,
        a16=a16,
        a17=a17,
        a18=a18,
        a19=a19,
        a20=a20,
        a21=a21,
        a22=a22,
        a23=a23,
    )
```

(That's not surprising, because after issuing the invalid-syntax error, the parser transforms the invalid versions into the equivalent of the above anyway, in the AST it generates.)

---

_Comment by @carljm on 2025-07-22 18:53_

Every `a*` variable is of type `Unknown | None`, so each of them gets expanded into the two elements of that union.

And I think the initial `True` argument is necessary to repro because without that, the call is valid on the first try, so no expansions are attempted. But that initial `True` is not assignable to `SupportsKeysAndGetItem[str, _VT]`, causing every call evaluation to fail, causing us to try every single possible expansion before finally concluding that we fail.

It's a little silly that even though the first argument is definitively wrong in this case, and will always fail, we still try expanding every _other_ argument. It feels like we should be able to detect that the wrong argument is not expandable, and stop there? But we can't make that optimization because of a case like this:

```py
from typing import overload

@overload
def f(x: int, y: Literal[False]) -> Literal[0]: ...
@overload
def f(x: int, y: Literal[True]) -> Literal[1]: ...
@overload
def f(x: str, y: bool) -> str: ...

def _(b: bool):
    f(1, b)
```

Initially we will hit the last overload and the first argument will fail to assign (`Literal[1]` is not assignable to `str`), but we can't just say "oh, first argument failed and is not expandable, stop here", because expanding the second argument allows us to consider other overloads in which the first argument does not fail.

There may still be some smarter version of this optimization where we detect that a non-expandable argument didn't match _any_ overload, and therefore skip expanding arguments?

@erictraut what optimizations/heuristics/limits does pyright use in these cases?

---

_Comment by @erictraut on 2025-07-22 20:01_

Yes, argument overload expansion can lead to combinatoric explosion. In pyright, I limit it to [256 combinatoric expansions](https://github.com/microsoft/pyright/blob/8b02aa84ec916697e76d5ee69299a89d555ec98c/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L564). I also limit expansion of a single argument type (e.g. a tuple) [to 64](https://github.com/microsoft/pyright/blob/8b02aa84ec916697e76d5ee69299a89d555ec98c/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L568). These limits are admittedly somewhat arbitrary, but they seem to work well in practice.

---

_Comment by @carljm on 2025-07-22 20:33_

I feel like even if we add these limits, it doesn't make sense to me that we hit 12GB of memory usage with this example. In addition to setting limits I think we should spend a bit of time to see if we can be more efficient in our memory usage when doing these expansions, or even just drop some things sooner once we no longer need them. 

---

_Comment by @jelle-openai on 2025-07-22 20:34_

Another heuristic you could use is that argument expansion will only help if there is some overlap between the original argument type and the parameter type of the overload. In the example in this thread, you could first filter overloads to exclude any that can't match because of argument kinds. This step doesn't look at argument types at all, so argument expansion doesn't make a difference. In this case, that means you'd end up with the overloads that support calls like `dict({}, a=1)`. For those, the first argument must be a dict or iterable, but you can see that the argument is `Literal[True]`, which doesn't overlap with any iterable type, so you can skip argument expansion and reject the overload immediately.

---

_Comment by @dhruvmanila on 2025-07-24 04:34_

> Another heuristic you could use is that argument expansion will only help if there is some overlap between the original argument type and the parameter type of the overload. In the example in this thread, you could first filter overloads to exclude any that can't match because of argument kinds.

We do have a similar heuristic for step 5 that tries to collect all the parameters that will be participating in the filtering process. This is done by checking whether the argument type is equivalent to the parameter type and is considered in the filtering process if it isn't equivalent.

https://github.com/astral-sh/ruff/blob/905b9d7f515ff4b093a9a55997e1855b54e2d73a/crates/ty_python_semantic/src/types/call/bind.rs#L1377-L1382

Although, I'm getting a bit confused because you mentioned at first to check for any overlap between the argument _type_ and the parameter _type_ but then you're mentioning to only check the argument _kind_ and not look at the argument _type_. Can you clarify this?

I think we could do a similar check as for step 5 and see if any of the argument type has **no** overlapping corresponding parameter type and avoid doing the expansion if this is the case.

Consider [Carl's example](https://github.com/astral-sh/ty/issues/868#issuecomment-3104364252), the `f(1, b)` call would see that `Literal[1]` has overlapping overloads (1 and 2) so it will continue with the expansion but `f(None, b)` does not have any overlapping overloads so it will avoid doing the expansion.

One thing that I'm worried about is whether skipping the expansion would result in more overloads that needs to be filtered at later steps but I haven't checked if this would ever occur in practice.

---

_Label `overloads` added by @dhruvmanila on 2025-07-24 04:34_

---

_Renamed from "Out of memory when analyzing file" to "Overload argument type expansion leads to combinatoric explosion" by @dhruvmanila on 2025-07-24 04:36_

---

_Comment by @jelle-openai on 2025-07-24 05:00_

Filtering out by argument kind would be step 1. For example, if an overload accepts no positional arguments, but the call provides a positional argument, you can know it never matches and forget about it.

The heuristic I am suggesting allows you to skip performing (potentially expensive) argument expansion in step 3.
In this example with a dict (the motivating example for the issue), we're passing one positional argument and a bunch of keyword arguments. That would filter out all but two of the overloads for `dict.__init__` (refer to: https://github.com/python/typeshed/blob/a1dc346ef6b2914197786c8a6be76202e929477a/stdlib/builtins.pyi#L1139). These have a positional parameter that is respectively of types `SupportsKeysAndGetItem` and `Iterable`. The positional argument in the call is `True`; the keyword args are all of type `Unknown | None`. `True` is not assignable to `SupportsKeysAndGetItem` or `Iterable`, so in step 2 we get errors for all overloads. Then step 3 calls for argument expansion. It appears that ty currently does this for all arguments at once, leading to a huge amount of combinations with all the `Unknown | None` parameters. But we can also compute `Literal[True] & SupportsKeysAndGetItem` and `Literal[True] & Iterable` and see that these types are disjoint, which means that no amount of argument expansion will make the overloads match. Therefore, we can skip doing argument expansion and conclude that the call will never succeed.

Another related heuristic you could use is that for argument expansion to help, we must be expanding an argument for which we previously got a type error. In this example, we got an error on the first positional argument (`True` is not assignable to `SupportsKeysAndGetItem`). But then we do argument expansion on the other arguments. That will never fix the type mismatch on the first argument, so argument expansion is futile and we can skip it.

This second heuristic could be used in Carl's example with the `f(1, b)` call to filter out one of the overloads before argument expansion: the third overload `(x: str, y: bool)` gives an error on step 2 when we pass the arguments (`Literal[1], bool)`. While the bool is expandable, we got an argument on the `str` argument, and we can know beforehand that expanding the bool won't make the call compatible with that first argument.

The two heuristics I suggest would not filter out the first two overloads in that example (with `Literal[True]` and `Literal[False]`), because `bool` overlaps with those types, and the error we get in step 2 is on an argument (of type `bool`) that can undergo expansion.

---

_Added to milestone `Beta` by @carljm on 2025-07-25 14:39_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-08-19 05:15_

---

_Comment by @dhruvmanila on 2025-08-19 13:31_

Thank you for the detailed writeup!

While thinking about this, I had a similar heuristic as you've mentioned but before that I want to provide some context:

We do perform arity checks which would filter out any overloads that is guaranteed to not match based on the argument kind (extra positional arguments, etc.). So, in the original example with a single `True` and multiple keyword arguments of union type, ty would only see the two remaining overloads at the time of argument type expansion which are the ones that contains `SupportsKeysAndGetItem` and `Iterable` for the positional-only parameter.

So, based on this and considering your first heuristic, we could loop over the argument types that cannot be expanded and check whether they're assignable to any of the remaining overloads. In the original example, that would make `Literal[True]` to be the argument type that cannot be expanded and it's not assignable to the remaining overloads (2 that contains the positional-only parameter).

> Another related heuristic you could use is that for argument expansion to help, we must be expanding an argument for which we previously got a type error. In this example, we got an error on the first positional argument (`True` is not assignable to `SupportsKeysAndGetItem`). But then we do argument expansion on the other arguments. That will never fix the type mismatch on the first argument, so argument expansion is futile and we can skip it.

This is interesting but it would require maintaining some kind of metadata during type checking. I think the `BindingError::InvalidArgumentType` does provide the argument index where the type checking failed and it could be used to avoid creating the metadata. In a gist, this heuristic acts as an additional filtering layer before checking whether the expanded argument lists succeeds or not but this still requires performing argument type expansion.

Here's what I'm thinking:
- Add the first heuristic
- Find out if there are any ways to avoid allocating a lot of vectors during the expansion to reduce the memory usage
- Add the second heuristic
- Fallback to limiting the expansion to some arbitrary number (Pyright has 256 for the number of argument lists in a single expansion) if there was no easy way to reduce the memory allocation

---

_Comment by @dhruvmanila on 2025-08-19 14:24_

One way to reduce the memory is to create an iterator of an iterator instead of the current approach which returns an iterator of argument lists. This way we don't need to allocate the expanded argument lists upfront but it's always better to avoid doing that if there are any heuristics to do so.

---

_Closed by @dhruvmanila on 2025-08-25 09:43_

---

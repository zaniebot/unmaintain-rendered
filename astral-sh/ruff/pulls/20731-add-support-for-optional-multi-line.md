```yaml
number: 20731
title: "Add support for optional multi-line comprehensions (#11753)"
type: pull_request
state: closed
author: kevind-kizen
labels: []
assignees: []
base: main
head: multiline-comprehensions
created_at: 2025-10-07T02:28:41Z
updated_at: 2025-10-07T15:40:11Z
url: https://github.com/astral-sh/ruff/pull/20731
synced_at: 2026-01-12T15:57:08Z
```

# Add support for optional multi-line comprehensions (#11753)

---

_@kevind-kizen_

Issue reference: https://github.com/astral-sh/ruff/issues/11753

This PR implements a new formatter option `comprehension-line-break` that allows users to preserve multi-line formatting in comprehensions, when they choose to do so, addressing #11753.

## Motivation

Currently, the ruff formatter automatically collapses multi-line comprehensions to a single line when they fit within the configured line width. This can reduce readability for complex comprehensions that were intentionally formatted across multiple lines for clarity. This change gives users control over this behavior.

## Implementation

### New Configuration Option

  Added comprehension-line-break with two modes:
  - "auto" (default): Current behavior - collapses comprehensions to single line if they fit
  - "preserve": Preserves multi-line formatting from source code

  Configuration in pyproject.toml:

```
[tool.ruff.format]
comprehension-line-break = "preserve"
```

Example Behavior

Input:
```
{
    obj["key"]: obj["value"]
    for obj
    in get_fields()
    if obj["key"] != "name"
}
```

Output with "auto" (default):
```
{obj["key"]: obj["value"] for obj in get_fields() if obj["key"] != "name"}
```

Output with "preserve":
```
{
    obj["key"]: obj["value"]
    for obj in get_fields()
    if obj["key"] != "name"
}
```

## Changes Made

  1. Added ComprehensionLineBreak enum in crates/ruff_python_formatter/src/options.rs
    - Implements Auto and Preserve modes
    - Includes serde serialization and JSON schema support
  2. Updated configuration system:
    - Added field to PyFormatOptions, FormatterSettings, and FormatConfiguration
    - Wired up configuration through workspace options
    - Updated JSON schema generation
  3. Modified all comprehension formatters to respect the new option:
    - Dictionary comprehensions (expr_dict_comp.rs)
    - List comprehensions (expr_list_comp.rs)
    - Set comprehensions (expr_set_comp.rs)
    - Generator expressions (expr_generator.rs)
  4. Implementation approach:
    - Added is_comprehension_multiline() helper to detect multi-line comprehensions in source
    - When preserve mode is active and source is multi-line, uses group().should_expand(true) to force expansion
    - Maintains backward compatibility with default auto mode
  5. Testing:
    - Added comprehensive test fixture covering all comprehension types
    - Tests both auto and preserve modes
    - All existing tests continue to pass

  Compatibility

  - Backward compatible: Default behavior unchanged
  - Black compatibility: auto mode maintains Black-compatible formatting
  - Performance: Minimal overhead - only checks source formatting when preserve mode is enabled

  Test Plan

  - Added test fixture: comprehension_line_break.py with corresponding snapshot tests
  - Tested with various comprehension types: dict, list, set, and generator expressions
  - Verified behavior with comments, nested comprehensions, and long expressions
  - All existing formatter tests pass
  - Ran ruff binary locally in my codebase

I know that this issue has not reached consensus on the philosophy, but wanted to drop an implementation draft to get the ball rolling as I believe this is a major readability flaw to always collapse comprehensions.

---

_Review requested from @MichaReiser by @kevind-kizen on 2025-10-07 02:28_

---

_Comment by @MichaReiser on 2025-10-07 05:59_

Thanks for working on this but I'll close it as I consider presering line breaks as the absolute last resort because it defeats the purpose of a formatter -- to ensure code is consistently formatted. 

https://github.com/astral-sh/ruff/issues/11753#issuecomment-3015015216 suggests an alternative that I want to try first before going down the path of just preserving line breaks as is

---

_Closed by @MichaReiser on 2025-10-07 05:59_

---

_Comment by @kevind-kizen on 2025-10-07 14:45_

Fair enough -- not my project. I understand the philosophical goal of consistent formatting, but I also believe that if the consistent formatting is less readable, then that's suboptimal. I understand your formatter values standard opinionated formatting.

A mutli-line comprehension makes it super easy to visually scan each component of the comprehension, IMO. 

I see the alternative proposed and something like that would certainly be more deterministic, which is valuable. I'll add some comments on the issue to re-open debate, but it's obviously your call -- I would really like to see some solution to this as my new organization uses the ruff formatter.

---

_Comment by @aronhoff on 2025-10-07 15:26_

@kevind-kizen I implemented an idea similar to the rustfmt solution linked in #16902, except I used fixed width values rather than percentages and I didn't know how I could decide on the numbers in the comprehension width policy, so I left it configurable. I couldn't continue on that PR without guidance on what would be acceptable. Might be useful background if you want to work on it further.

---

_Comment by @kevind-kizen on 2025-10-07 15:37_

I can explore that direction, but the main criteria I typically use for
deciding to use a multi-line comprehension has more to do with the logical
complexity rather than simply line lengths.

I find that there are many cases where single line comprehensions are
appropriate, even with slightly longer line lengths, such as the case of
descriptive variable names. And there are some short comprehensions that
have a little extra complexity, for example function calls where I want
clarity on what function is being called.

I tend to prefer them in most cases.

I can provide some examples.

On Tue, Oct 7, 2025 at 8:26 AM Áron Hoffmann ***@***.***>
wrote:

> *aronhoff* left a comment (astral-sh/ruff#20731)
> <https://github.com/astral-sh/ruff/pull/20731#issuecomment-3377436137>
>
> @kevind-kizen <https://github.com/kevind-kizen> I implemented an idea
> similar to the rustfmt solution linked in #16902
> <https://github.com/astral-sh/ruff/pull/16902>, except I used fixed width
> values rather than percentages and I didn't know how I could decide on the
> numbers in the comprehension width policy, so I left it configurable. I
> couldn't continue on that PR without guidance on what would be acceptable.
> Might be useful background if you want to work on it further.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/pull/20731#issuecomment-3377436137>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/BWQEJOWP3IDRUH5FQSYH6K33WPLTZAVCNFSM6AAAAACIO4WX2GVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTGNZXGQZTMMJTG4>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


---

_Comment by @kevind-kizen on 2025-10-07 15:38_

I think that simply allowing the preservation of line breaks when the
developer makes the decision to break the lines along the comprehension
keywords (which is what I did in the PR) is very similar in philosophy to
the magic-trailing-commas configuration option.

On Tue, Oct 7, 2025 at 8:37 AM Kevin Dolan ***@***.***> wrote:

> I can explore that direction, but the main criteria I typically use for
> deciding to use a multi-line comprehension has more to do with the logical
> complexity rather than simply line lengths.
>
> I find that there are many cases where single line comprehensions are
> appropriate, even with slightly longer line lengths, such as the case of
> descriptive variable names. And there are some short comprehensions that
> have a little extra complexity, for example function calls where I want
> clarity on what function is being called.
>
> I tend to prefer them in most cases.
>
> I can provide some examples.
>
> On Tue, Oct 7, 2025 at 8:26 AM Áron Hoffmann ***@***.***>
> wrote:
>
>> *aronhoff* left a comment (astral-sh/ruff#20731)
>> <https://github.com/astral-sh/ruff/pull/20731#issuecomment-3377436137>
>>
>> @kevind-kizen <https://github.com/kevind-kizen> I implemented an idea
>> similar to the rustfmt solution linked in #16902
>> <https://github.com/astral-sh/ruff/pull/16902>, except I used fixed
>> width values rather than percentages and I didn't know how I could decide
>> on the numbers in the comprehension width policy, so I left it
>> configurable. I couldn't continue on that PR without guidance on what would
>> be acceptable. Might be useful background if you want to work on it further.
>>
>> —
>> Reply to this email directly, view it on GitHub
>> <https://github.com/astral-sh/ruff/pull/20731#issuecomment-3377436137>,
>> or unsubscribe
>> <https://github.com/notifications/unsubscribe-auth/BWQEJOWP3IDRUH5FQSYH6K33WPLTZAVCNFSM6AAAAACIO4WX2GVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTGNZXGQZTMMJTG4>
>> .
>> You are receiving this because you were mentioned.Message ID:
>> ***@***.***>
>>
>


---

_Comment by @kevind-kizen on 2025-10-07 15:40_

I am also curious about other developers choices — I might run an analysis
of popular GitHub python projects to get you some stats and try to better
understand the most common patterns.

On Tue, Oct 7, 2025 at 8:38 AM Kevin Dolan ***@***.***> wrote:

> I think that simply allowing the preservation of line breaks when the
> developer makes the decision to break the lines along the comprehension
> keywords (which is what I did in the PR) is very similar in philosophy to
> the magic-trailing-commas configuration option.
>
> On Tue, Oct 7, 2025 at 8:37 AM Kevin Dolan ***@***.***> wrote:
>
>> I can explore that direction, but the main criteria I typically use for
>> deciding to use a multi-line comprehension has more to do with the logical
>> complexity rather than simply line lengths.
>>
>> I find that there are many cases where single line comprehensions are
>> appropriate, even with slightly longer line lengths, such as the case of
>> descriptive variable names. And there are some short comprehensions that
>> have a little extra complexity, for example function calls where I want
>> clarity on what function is being called.
>>
>> I tend to prefer them in most cases.
>>
>> I can provide some examples.
>>
>> On Tue, Oct 7, 2025 at 8:26 AM Áron Hoffmann ***@***.***>
>> wrote:
>>
>>> *aronhoff* left a comment (astral-sh/ruff#20731)
>>> <https://github.com/astral-sh/ruff/pull/20731#issuecomment-3377436137>
>>>
>>> @kevind-kizen <https://github.com/kevind-kizen> I implemented an idea
>>> similar to the rustfmt solution linked in #16902
>>> <https://github.com/astral-sh/ruff/pull/16902>, except I used fixed
>>> width values rather than percentages and I didn't know how I could decide
>>> on the numbers in the comprehension width policy, so I left it
>>> configurable. I couldn't continue on that PR without guidance on what would
>>> be acceptable. Might be useful background if you want to work on it further.
>>>
>>> —
>>> Reply to this email directly, view it on GitHub
>>> <https://github.com/astral-sh/ruff/pull/20731#issuecomment-3377436137>,
>>> or unsubscribe
>>> <https://github.com/notifications/unsubscribe-auth/BWQEJOWP3IDRUH5FQSYH6K33WPLTZAVCNFSM6AAAAACIO4WX2GVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTGNZXGQZTMMJTG4>
>>> .
>>> You are receiving this because you were mentioned.Message ID:
>>> ***@***.***>
>>>
>>


---

---
number: 8106
title: Improve line-length vs. autofix interactions (force-allow? force-disallows?)
type: issue
state: open
author: tdulcet
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-10-21T14:46:29Z
updated_at: 2025-01-01T06:22:36Z
url: https://github.com/astral-sh/ruff/issues/8106
synced_at: 2026-01-10T01:22:47Z
---

# Improve line-length vs. autofix interactions (force-allow? force-disallows?)

---

_Issue opened by @tdulcet on 2023-10-21 14:46_

Please consider adding some method to force all autofixes regardless if they might make some lines too long. When using an autoformater (e.g. Ruff), any excessively long lines would be fixed anyway. Maybe do this automatically when `E501` (line-too-long) is not selected. This would save a considerable amount of time when switching a large project to Ruff, as it would eliminate the need for unnecessary manual fixes.

This potential workaround does not work:
```
$ ruff --line-length 9999 --unsafe-fixes --fix .
error: invalid value '9999' for '--line-length <LINE_LENGTH>': The line width must be a value between 1 and 320.
```
Some fixes need more than 320 columns, so using `--line-length 320` would not work either.

```
$ ruff --version
ruff 0.1.1
```


---

_Referenced in [astral-sh/ruff#8107](../../astral-sh/ruff/issues/8107.md) on 2023-10-21 14:53_

---

_Comment by @charliermarsh on 2023-10-21 19:31_

I'm wondering if we should just remove this behavior, or mark those fixes as unsafe when they would exceed the line length. I'd sort of rather do one of those two things instead of introducing a setting or similar. \cc @zanieb 

---

_Label `needs-decision` added by @charliermarsh on 2023-10-21 19:31_

---

_Comment by @zanieb on 2023-10-21 20:28_

Could we format the fix with our formatter i.e. using range formatting?

I think changing from safe to unsafe due to a line length violation is kind of a misuse of applicability.

It'd probably make sense to just apply the fixes despite the line length; is there a good example I could see that demonstrates the problem with that?

---

_Comment by @Avasam on 2023-10-22 08:10_

My opinion: fixing one reported lint issue can introduce another (sometimes multiple layers of code smells/code simplification can happen). To me that's to be expected.

I see line length the same way. Especially since line length is often an easy manual fix. (and automated with a formatter).

Line length also doesn't break code. It feels unexpected for a safe autofix to not happen.


---

_Comment by @tdulcet on 2023-10-22 11:04_

Yes, I agree, I think it is surprising for many Ruff users that they are not seeing all possible autofixes by default, especially now that Ruff supports autoformating. This is different from every other linter I have used that supports autofixes, including ESLint, Clang-Tidy and Shellcheck. I only noticed this recently after I increased the `--line-length` option and saw that a bunch of new possible autofixes were listed. I then increasing it again and there were more autofixes still... 🤯

> It'd probably make sense to just apply the fixes despite the line length; is there a good example I could see that demonstrates the problem with that?

This sounds like the best solution. I believe there are some cases with f-strings where it is not currently possible to format them with Ruff to reduce the line length, but it is my understanding that this will be fixed after Ruff fully supports the preview style formatting, so this should be a nonissue.

(There are several autofixes that flatten code originally written over multiple lines and in some cases can cause very long lines. If these autofixes could instead preserve the existing formatting and only make minimal changes, this would be a big improvement and would also make it easier to see the actual proposed change with the `--diff` option, but this still would not handle all cases where long lines are unavoidable without some type of autoformating.)

---

_Comment by @MichaReiser on 2023-10-22 23:43_

> I'm wondering if we should just remove this behavior, or mark those fixes as unsafe when they would exceed the line length. I'd sort of rather do one of those two things instead of introducing a setting or similar. 

I'm leaning towards just removing the behavior without marking them as unsafe, because the fixes are not unsafe in the sense that they change the program's semantic. There's only the risk that they introduce new errors. 

---

_Comment by @charliermarsh on 2023-10-22 23:53_

I'm definitely open to removing it, but I should at least mention that it didn't appear out of nowhere. We introduced that logic because we received multiple issues complaining about the fact that these fixes introduced E501 errors. I found this one, as an example: https://github.com/astral-sh/ruff/issues/1766.

> Could we format the fix with our formatter i.e. using range formatting?

I think this is the ideal (and desired) solution but it's not a trivial change.


---

_Comment by @MichaReiser on 2023-10-23 00:27_

> Could we format the fix with our formatter i.e. using range formatting?

Maybe, but even when we use our formatter, there's still no guarantee that it won't trigger E501. 


---

_Comment by @tdulcet on 2023-10-23 09:42_

> We introduced that logic because we received multiple issues complaining about the fact that these fixes introduced E501 errors.

As I suggested above in my original request, maybe only remove this behavior when the user does _not_ have `E501` selected. Now that `E501` is no longer selected by default, this would likely fix the issue for the majority of users without causing any additional errors for those who have chosen to explicitly enable `E501`. If you chose this path, it might also be helpful to warn about any hidden autofixes due to `E501` so that users would be aware of the issue, similar to how Ruff now warns about hidden unsafe autofixes.

---

_Comment by @zanieb on 2023-10-23 14:35_

> Maybe, but even when we use our formatter, there's still no guarantee that it won't trigger E501

It seems like it'd be a good enough effort though. Additionally, we could check the line length _after_ formatting as a determination of whether or not the fix should be suggested although I think this is a bit much.

> As I suggested above in my original request, maybe only remove this behavior when the user does not have E501 selected. 

This sounds like a bit of a confusing experience to explain and is not a pattern I'd want to embrace in general. `line_length` is a global Ruff setting, not just E501.

> If you chose this path, it might also be helpful to warn about any hidden autofixes due to E501 so that users would be aware of the issue, similar to how Ruff now warns about hidden unsafe autofixes.

It makes me uncomfortable to special case messaging for the line length rule like this. I don't think this is actually possible with our current fix architecture either.

I think the example in #1766 is quite helpful. A formatter would not resolve that and it is a significant degradation in readability. 

---

_Referenced in [astral-sh/ruff#8158](../../astral-sh/ruff/issues/8158.md) on 2023-10-24 09:40_

---

_Comment by @charliermarsh on 2023-10-27 13:41_

@zanieb - I wonder if there's... some other way for us to gate that simplicity rule, in those cases. Like, if the body is multiline, perhaps we should skip that rule, since it's unlikely to be "simpler"?

---

_Comment by @Avasam on 2023-11-28 18:03_

Since the message `No fixes available (X hidden fix can be enabled with the '--unsafe-fixes' option).` exists. Could the same be added for line length? Like `No fixes available (X fix skipped due to line-length).` (doesn't really solve cases above 320, but at least the user is made aware, and not left to think Ruff is inconsistent)

---

_Referenced in [astral-sh/ruff#9203](../../astral-sh/ruff/issues/9203.md) on 2023-12-20 00:29_

---

_Comment by @charliermarsh on 2024-01-02 18:00_

https://github.com/astral-sh/ruff/issues/9203 contains a related request: avoid _all_ fixes that would result in line-length violations. Quoting @eli-schwartz from that issue:

> Maybe there needs to be:
> 1) a generic mechanism for autofixes to detect when they would rewrite code which formerly obeyed the line-length setting, to violate it, and mark that autofix as long-line-unsafe
> 2) a dedicated option, similar to --unsafe-fixes but *not actually called unsafe-fixes*, e.g. `--long-line-fixes`
> 
> I definitely do not think it's appropriate to violate the line length.
> - not everyone uses, or wants to use, a formatter
> - ruff combined with autofixes can be very useful for performing one-off migrations
> - many projects have CI which is gated on flake8, but not ruff, so it would be advantageous to be able to run `ruff check --fix` and get fewer fixes, but passing CI
> - but mostly, "In the face of ambiguity, refuse the temptation to guess." If ruff is going to emit an error code either way, then there is no point in *both* modifying the file *and* emitting an error code. Ruff should not make a decision about whether you prefer to get an error due to the UP015 code or get a different error due to the E501 code. Instead, ruff should simply report whichever error your codebase already has, and let *you* make the judgment call.

---

_Renamed from "Support forcing all autofixes regardless of line length when using an autoformater" to "Improve line-length vs. autofix interactions (force-allow? force-disallows?)" by @charliermarsh on 2024-01-02 18:01_

---

_Label `configuration` added by @charliermarsh on 2024-01-02 18:01_

---

_Referenced in [astral-sh/ruff#9449](../../astral-sh/ruff/pulls/9449.md) on 2024-01-09 16:58_

---

_Referenced in [astral-sh/ruff#9512](../../astral-sh/ruff/issues/9512.md) on 2024-01-14 15:13_

---

_Referenced in [astral-sh/ruff#10259](../../astral-sh/ruff/issues/10259.md) on 2024-03-07 05:48_

---

_Referenced in [astral-sh/ruff#10263](../../astral-sh/ruff/pulls/10263.md) on 2024-03-07 07:48_

---

_Comment by @Hnasar on 2024-03-26 18:24_

Just a piece of user feedback: I'm planning to disable this rule in my projects because I'd rather see a `str.format()` which fits within the line length than a very long fstring.

Ideally I'd rather an auto-split f-string that fits within the line length, but I understand that's not available right now.

---

_Comment by @NeilGirdhar on 2025-01-01 06:22_

In some cases and in my opinion, exceeding the line length really isn't the problem it's being made out to be.  For example,

I think the ternary expression is still an improvement even if it doesn't fit on one line:
```python
if not isinstance(inference_result, RivalConfiguration):
    code = None
else:
    code = np.asarray(inference_result.pooling_output, dtype=np.float64)
```
becomes
```python
code = (np.asarray(inference_result.pooling_output, dtype=np.float64)
        if isinstance(inference_result, RivalConfiguration)
        else None)
```
It's an improvement because the logic is simpler (only one assignment target).

---

_Referenced in [astral-sh/ruff#15820](../../astral-sh/ruff/issues/15820.md) on 2025-01-30 01:33_

---

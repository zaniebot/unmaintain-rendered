---
number: 10009
title: "Feature Request: Custom allowlist for Magic Numbers (PLR2004)"
type: issue
state: open
author: Spectre5
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-02-16T16:30:11Z
updated_at: 2025-09-18T20:08:27Z
url: https://github.com/astral-sh/ruff/issues/10009
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature Request: Custom allowlist for Magic Numbers (PLR2004)

---

_Issue opened by @Spectre5 on 2024-02-16 16:30_

PLR2004 has a few allowed numbers including '', 0, and 1.  #9964 has fixed a bug so that 0.0 and 1.0 are also allowed.  There is [already](https://docs.astral.sh/ruff/settings/#lint_pylint_allow-magic-value-types) an option to ignore certain types of variables from the check.  I'd like to request an option to extend the allowlist.  For example, I have a library where 2 (and 2.0) are really common and I don't want to use a global variable for it.  So I ether need many noqa comments or to disable the check entirely.

---

_Label `configuration` added by @AlexWaygood on 2024-02-18 11:33_

---

_Comment by @branidal on 2024-07-21 11:33_

This would indeed be a very nice improvement. I think there are quite a few common magic variables that are commonly ok but inconvenient to define globally (in my case 0.5) and I can imagine users wanting to be able to add any number of additional options.

---

_Comment by @charliermarsh on 2024-07-21 12:19_

I think it's also worth asking whether we should just add 2 and 2.0 to the allowed list?

---

_Comment by @dscorbett on 2024-07-21 12:25_

I hope this can be made configurable. I use the magic numbers 90, 180, 270, and 360 all over my code, but it wouldn’t make sense to add those to the allowed list for everyone.

---

_Comment by @DaniBodor on 2024-08-05 22:44_

To me it would make sense to both make it configurable, but also add 2/2.0 to the default list of allowed values.
I wonder whether there is a quick way to scrape github in a way to get the statistics of which values are most commonly used with `noqa: PLR2004`. I would guess that at least 2 and 2.0 are very common. Maybe some others as well.

---

_Label `needs-decision` added by @MichaReiser on 2024-08-06 06:05_

---

_Comment by @RubenVanEldik on 2024-10-07 10:10_

I agree with the idea that a whitelist for magic numbers should be customizable. The idea of not using magic numbers is because it's harder to infer the meaning of a line of code if random values are sprinkled throughout it. However, depending on the codebase some numbers might make total sense.

For example, I work on an energy related application. The number 3600 is commonly used to convert between Joule and kWh. Everyone in our team understands the meaning and context of these numbers and there is no value in making a variable for it first. As @dscorbett described, if you're working with angles (degrees) than 90, 180, 270, and 360 are completely self explanatory.

---

_Referenced in [python/typeshed#13307](../../python/typeshed/pulls/13307.md) on 2024-12-26 20:58_

---

_Comment by @real-or-random on 2025-02-06 09:13_

> To me it would make sense to both make it configurable, but also add 2/2.0 to the default list of allowed values.

I agree. 2 (and perhaps also 3) is common enough that it should be allowed by default. But making it configurable will make sense for values that make sense in a certain code base, e.g., 90, 180, 270, and 360.

---

_Comment by @DaniBodor on 2025-05-12 13:23_

For reference, pylint has an option to set this:

https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#magic-value-options

---

_Renamed from "Feature Request: Custom Whitelist for Magic Numbers (PLR2004)" to "Feature Request: Custom allowlist for Magic Numbers (PLR2004)" by @MichaReiser on 2025-05-12 13:25_

---

_Comment by @DaniBodor on 2025-06-25 17:07_

I'm happy to give this a shot, if that's ok.
Not sure I'll manage, but would like to try


---

_Comment by @ntBre on 2025-06-25 17:47_

The issue is still labeled `needs-decision`, so I'm not sure we'd want to merge anything until that decision is made. @MichaReiser do you remember what needed to be decided here? Whether we want this to be configurable at all?

One complication I can see is how we would handle the various types supported by the rule. It looks like pylint supports both numbers and strings for their related option. Special-casing floats could be one solution to that, though.

---

_Comment by @MichaReiser on 2025-06-25 20:29_

Yes, it's mainly if we want the option in the first place. A good start would be a proposal for how the option would work (what is it named, what are the defaults, etc)

---

_Comment by @Avasam on 2025-06-26 00:10_

If the rule's named `magic-value-comparison (PLR2004)` then an option `lint.pylint.allow-magic-values` seems appropriate.
There's already `lint.pylint.allow-magic-value-types`, so "allow" rather than "allowed" and "values" rather than "value" would keep a similar pattern.

Although that does bring into question whether  `lint.pylint.allow-magic-value-types` and `lint.pylint.allow-dunder-method-names` should have been called `allowed-*` because it's a list of options, not a boolean. As the pattern other rule options seem to follow.

---

_Comment by @DaniBodor on 2025-06-26 01:33_

> A good start would be a proposal for how the option would work (what is it named, what are the defaults, etc)

I can see an argument for 2 names. First, as mentioned by @Avasam, is `lint.allow-magic-values` as it is clearly related to `pylint.allow-magic-value-types`. The other would be `pylint.valid-magic-values`, which is what it is called in the original [pylint setting](https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#magic-value-checker). 
Note that pylint itself does not have an `allow-magic-value-types` setting. I think it makes sense to still include the types option here, both for backwards compatibility, and because it's actually useful.

The setting will allow input of (a mix of) strings, ints, floats, and bytes, which is equivalent to the types that can be listed as safe by `pylint.allow-magic-value-types`.

As per the defaults, I think definitely include to what is now implicitly allowed: `[1, 1.0, 0, 0.0, "", "__main"]`. Pytest also includes `-1` as a valid magic value, so i think for consistency it makes sense to include `-1` and `-1.0` for ruff as well. Then `2` and `2.0` seem to be requested by multiple people in this thread, so those could be included as well; this one is a bit more up in the air as far as I am concerned.
Note that pylint doesn't include any of the floats as valid magic values, but similarly to above, I like having them in ruff.

As per the interaction between `pylint.allow-magic-value-types` and `pylint.valid-magic-values`, I think that both settings can be used at the same time, and if a value is allowed by either rule it is safe. 
I can see an argument to collapse it into a single setting (which should then probably be renamed to `pylint.valid-magic-values`) that also allows inputting types to mark all inputs of this type safe. The downsides are that it will not be backward compatible and will be more complicated to implement. The upside is that it is slightly more parsimonious and user-friendly (after the initial compatibility issue wears off). I am not in favor of this option, but could see the point.

> Yes, it's mainly if we want the option in the first place.

What is the argument against having this as a configurable option? It seems a logical setting to have and pylint does in fact have it.

---

_Comment by @DaniBodor on 2025-06-26 14:29_

> The issue is still labeled `needs-decision`, so I'm not sure we'd want to merge anything until that decision is made.

So.... I was looking for a pet project to sink my teeth in for a day so decided to start working on it already, even though I understand that you may not be interested in adopting it. I think I've largely worked it out, so will open a PR showcasing what I have.

---

_Referenced in [astral-sh/ruff#18961](../../astral-sh/ruff/pulls/18961.md) on 2025-06-26 14:55_

---

_Referenced in [astral-sh/ruff#20468](../../astral-sh/ruff/issues/20468.md) on 2025-09-18 14:20_

---

_Comment by @julianstirling on 2025-09-18 20:08_

I missed this when I created #20468.

My non-magic "magic" numbers are 255 and 65535. Reading this I can see how 45,60,90,135,180,270 are not magic, in applications doing a lot of rotations in degrees. I'll quote what I said for our numbers below

> We have a project that does a lot of image handing. Generally images are unit8, sometimes unit16 if getting raw data with higher bit depth. Often certain calculations that could overflow are done as a different datatype. Before conversion you often find yourself checking for overflows.
> 
> 255 really is not magic in a codebase that does a lot of uint8 maths. But it may be considered so in other codebases. I would like (and 65535) to not be magic, but I can see reasonable arguments for them counting as magic in other codebases.

---

---
number: 15473
title: BLE001 false positive
type: issue
state: open
author: joostlek
labels:
  - question
  - type-inference
assignees: []
created_at: 2025-01-14T11:12:00Z
updated_at: 2025-08-22T17:38:47Z
url: https://github.com/astral-sh/ruff/issues/15473
synced_at: 2026-01-07T13:12:16-06:00
---

# BLE001 false positive

---

_Issue opened by @joostlek on 2025-01-14 11:12_

So I just found out that if we log the exception, we can remove the BLE001. Awesome!

So on my journey to achieve this, I found that (I am assuming) the type of the logging doesn't get from one file to the other.

So in Home Assistant core we have the config flow, which is user facing, so we want to allow the developer to catch bare exceptions to at least handle this correctly to the user. The added logging is awesome as that helps debugging and when the developers know which more specific exceptions can be raised, they can add even more situational fields.

Now we have some integrations where they have a centralized logging in `const.py` where they create one for the `__package__`. For some reason, ruff doesn't see that.

So this works
```py
_LOGGER = logging.getLogger(__name__)

class MatterConfigFlow(ConfigFlow):
    async def async_step_user(self):
        try:
            await validate_input(self.hass, user_input)
        except CannotConnect:
            errors["base"] = "cannot_connect"
        except InvalidServerVersion:
            errors["base"] = "invalid_server_version"
        except Exception:
            _LOGGER.exception("Unexpected exception")
            errors["base"] = "unknown"
        else:
            self.ws_address = user_input[CONF_URL]
```
But this does not
```py
from .const import LOGGER

class MatterConfigFlow(ConfigFlow):
    async def async_step_user(self):
        try:
            await validate_input(self.hass, user_input)
        except CannotConnect:
            errors["base"] = "cannot_connect"
        except InvalidServerVersion:
            errors["base"] = "invalid_server_version"
        except Exception:
            LOGGER.exception("Unexpected exception")
            errors["base"] = "unknown"
        else:
            self.ws_address = user_input[CONF_URL]
```

This is found in https://github.com/home-assistant/core/blob/0dc90972d893276a09aed4ede6304c7e98c93462/homeassistant/components/matter/config_flow.py#L219-L229

---

_Referenced in [home-assistant/core#135584](../../home-assistant/core/pulls/135584.md) on 2025-01-14 11:13_

---

_Label `question` added by @dylwil3 on 2025-01-14 11:39_

---

_Label `type-inference` added by @dylwil3 on 2025-01-14 11:39_

---

_Comment by @dylwil3 on 2025-01-14 11:47_

>So on my journey to achieve this, I found that (I am assuming) the type of the logging doesn't get from one file to the other.

Yep that's correct - Ruff analyzes each file separately, and does not perform any sophisticated type inference (yet) that would tell it that `LOGGER` is a logger. So it may be that for now this rule does not support your use case.

It's possible that we could add a heuristic for this rule to look for method calls named `.exception` appearing in the right place. I don't have a good sense of how many false negatives that would produce - maybe others in the community can weigh in.

Thanks for bringing this up and sorry there isn't a great immediate resolution here!

---

_Comment by @dhruvmanila on 2025-01-15 06:15_

Can you clarify what do you mean when you say that the first snippet "works" while the second one doesn't? The logic to determine whether a symbol is a logger candidate does look at some common names:

https://github.com/astral-sh/ruff/blob/a2dc8c93ef152d4c21dbc5de9dd79fc923d4a2eb/crates/ruff_python_semantic/src/analyze/logging.rs#L60-L74

I don't see `BLE001` being highlighted for both snippets: https://play.ruff.rs/7e39e399-cfd2-4d2c-831b-c1d9b8762455

---

_Comment by @MichaReiser on 2025-01-15 07:14_

> I don't have a good sense of how many false negatives that would produce - maybe others in the community can weigh in.

I tend to optimize for false positives over false negatives because false positives affect everyone, where a few false negatives are a missed opportunity. We've seen this with the pandas frame rules that caused a lot of confusion and frustration for users until we made it more strict to drastically reduce false positives (at the cost of increasing false negatives)

---

_Comment by @joostlek on 2025-01-15 08:27_

@dhruvmanila Oh that is certainly interesting. I would assume that that works as the class is named LOGGER. But reality is different

```
homeassistant/components/matter/config_flow.py:225:16: BLE001 Do not catch blind exception: `Exception`
    |
223 |         except InvalidServerVersion:
224 |             errors["base"] = "invalid_server_version"
225 |         except Exception:
    |                ^^^^^^^^^ BLE001
226 |             LOGGER.exception("Unexpected exception")
227 |             errors["base"] = "unknown"
    |

Found 1 error.
```

Reproducable by removing the noqa here
https://github.com/home-assistant/core/blob/dev/homeassistant/components/matter/config_flow.py#L219-L229

---

_Comment by @joostlek on 2025-01-15 08:29_

When I put in the whole config_flow.py file in the playground, I also don't get any BLE001 rapport, but in reality ruff does

---

_Comment by @dhruvmanila on 2025-01-15 08:42_

Oh interesting. Yes, I do get it when running locally. Also, I think this can be solved right now using the [`lint.logger-objects`](https://docs.astral.sh/ruff/settings/#lint_logger-objects) setting:

```py
[tool.ruff.lint]
logger-objects = ["homeassistant.components.matter.const.LOGGER"]
```

---

_Comment by @joostlek on 2025-01-15 08:58_

Right, but checking all the integrations Home Assistant has
```sh
cat homeassistant/components/*/const.py | grep LOGGER | wc -l
```
We have 185 loggers in const, so on one hand this would work, I also really don't feel like adding 185 lines to our ruff config

---

_Comment by @MichaReiser on 2025-01-15 09:25_

> We have 185 loggers in const, so on one hand this would work, I also really don't feel like adding 185 lines to our ruff config

That's understandable. I'm not sure if possible but it might be worth exploring depending on your answer. Would it be sufficient for you if `logger-objects` accepted a glob, e.g. `**.LOGGER`, similar to what we support for `known-first-party`.

---

_Comment by @joostlek on 2025-01-15 09:33_

I think that could work. I did see some cases where there were was a _LOGGER or something else, but I think they are exported most of the time so they should be LOGGER.

Wouldn't it also make sense if the object that we call `.exception` is named like a LOGGER?

---

_Comment by @MichaReiser on 2025-01-15 09:42_

> Wouldn't it also make sense if the object that we call .exception is named like a LOGGER?

I'm not sure if I understand what you mean by that. Do you mean to always accept `LOGGER`, regardless if it is a local definition or imported from another module? 

My concern is that it can lead to more false positives for other rules without providing them with a way to opt out other than disabling the rules entirely. 

---

_Comment by @joostlek on 2025-01-15 09:46_

I think I was more coming from if a variable is named like the rules shared before
```rs
if tail.starts_with("log") 
             || tail.ends_with("logger") 
             || tail.ends_with("logging") 
             || tail.starts_with("LOG") 
             || tail.ends_with("LOGGER") 
             || tail.ends_with("LOGGING") 
         { 
```
and we call `.exception` on that, that sounds fairly specific, but also keep in mind that I mainly work on one codebase. So if you think that this will cause problems, then feel free to disregard my comment :)

---

_Referenced in [home-assistant/core#134223](../../home-assistant/core/pulls/134223.md) on 2025-02-16 12:24_

---

_Referenced in [astral-sh/ruff#17898](../../astral-sh/ruff/issues/17898.md) on 2025-05-07 06:31_

---

_Referenced in [astral-sh/ruff#18253](../../astral-sh/ruff/issues/18253.md) on 2025-05-22 12:40_

---

_Comment by @stevenh on 2025-08-22 17:37_

> logger-objects

This would solve our use case if its checking against the listed classes. While there may be lots of variables container loggers, I suspect there are limit implementations that would need to be tested.

As a specific example using:
```toml
[tool.ruff.lint]
logger-objects = ["logfire.Logfire"]
```

---

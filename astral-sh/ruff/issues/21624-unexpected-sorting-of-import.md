---
number: 21624
title: Unexpected sorting of import
type: issue
state: closed
author: spacemanspiff2007
labels:
  - question
assignees: []
created_at: 2025-11-25T06:21:47Z
updated_at: 2025-12-10T03:56:54Z
url: https://github.com/astral-sh/ruff/issues/21624
synced_at: 2026-01-10T01:23:02Z
---

# Unexpected sorting of import

---

_Issue opened by @spacemanspiff2007 on 2025-11-25 06:21_

### Summary

~Somewhere after 0.11.12 there was an unexpected change.~
My imports change from this

````python
from .events import (
    ItemNoChangeEvent as ItemNoChangeEvent,
    ItemNoUpdateEvent as ItemNoUpdateEvent,
    ValueChangeEvent as ValueChangeEvent,
    ValueCommandEvent as ValueCommandEvent,
    ValueUpdateEvent as ValueUpdateEvent,
)
````

to this

````python
from .events import (
    ItemNoChangeEvent as ItemNoChangeEvent,
)
from .events import (
    ItemNoUpdateEvent as ItemNoUpdateEvent,
)
from .events import (
    ValueChangeEvent as ValueChangeEvent,
)
from .events import (
    ValueCommandEvent as ValueCommandEvent,
)
from .events import (
    ValueUpdateEvent as ValueUpdateEvent,
)
````

I can't seem to find a setting to disable this behavior and this only affects two files in my project so I suspect this is a bug. 
Corresponding [pre-commit run](https://github.com/spacemanspiff2007/HABApp/actions/runs/19660055930/job/56304485075)

### Version

0.14.6

Edit:
I reverted to ruff 0.11.12 but still can't make pre-commit pass which run successfully at the end of September - very strange!

---

_Comment by @MichaReiser on 2025-11-25 07:57_

You can use [`combine-as-imports`](https://docs.astral.sh/ruff/settings/#lint_isort_combine-as-imports) if you always want to combine as imports from the same module into a single import statement: [playground](https://play.ruff.rs/9a4e5576-9341-4b6b-b74d-8b2ce927f449)


---

_Label `question` added by @MichaReiser on 2025-11-25 07:57_

---

_Comment by @spacemanspiff2007 on 2025-11-25 08:24_

Thank you for your reply! Somehow the playground does not work for me so I tried it locally.

Unfortunately now these imports
````python
from datetime import datetime as dt_datetime
from datetime import timedelta as dt_timedelta
````
become this
````python
from datetime import datetime as dt_datetime, timedelta as dt_timedelta
````
which I find very hard to read.
So the `combine-as-imports` is not the setting I am looking for.
Is there any other setting or option I could use?


---

_Comment by @MichaReiser on 2025-11-25 17:44_

I don't think there is. Ruff forces splits `from .. import`s into multiple imports if they exceed the line length. However, what seems very unfortunate is that it preserves the trailing-comma when doing so. 

You could consider setting [`split-on-trailing-comma`](https://docs.astral.sh/ruff/settings/#lint_isort_split-on-trailing-comma) to `false`

Your example then gets reformatted to the following, which I find more reasonable:

```py
from .events import ItemNoChangeEvent as ItemNoChangeEvent
from .events import ItemNoUpdateEvent as ItemNoUpdateEvent
from .events import ValueChangeEvent as ValueChangeEvent
from .events import ValueCommandEvent as ValueCommandEvent
from .events import ValueUpdateEvent as ValueUpdateEvent
```



---

_Comment by @spacemanspiff2007 on 2025-12-02 07:06_

Thank you for your hint with the trailing comma. 
If I remove the trailing comma one import is indeed in one line as you showed.
However since I often have lots of imports it becomes this wall of text which is extremely hard to read.
E.g. a small excerpt from one file:
````python
from .base_event import OpenhabEvent as OpenhabEvent
from .channel_events import ChannelDescriptionChangedEvent as ChannelDescriptionChangedEvent
from .channel_events import ChannelTriggeredEvent as ChannelTriggeredEvent
from .item_events import GroupStateChangedEvent as GroupStateChangedEvent
from .item_events import GroupStateUpdatedEvent as GroupStateUpdatedEvent
from .item_events import ItemAddedEvent as ItemAddedEvent
from .item_events import ItemCommandEvent as ItemCommandEvent
from .item_events import ItemRemovedEvent as ItemRemovedEvent
from .item_events import ItemStateChangedEvent as ItemStateChangedEvent
from .item_events import ItemStateEvent as ItemStateEvent
from .item_events import ItemStatePredictedEvent as ItemStatePredictedEvent
from .item_events import ItemStateUpdatedEvent as ItemStateUpdatedEvent
from .item_events import ItemUpdatedEvent as ItemUpdatedEvent
from .thing_events import ThingAddedEvent as ThingAddedEvent
from .thing_events import ThingConfigStatusInfoEvent as ThingConfigStatusInfoEvent
from .thing_events import ThingFirmwareStatusInfoEvent as ThingFirmwareStatusInfoEvent
from .thing_events import ThingRemovedEvent as ThingRemovedEvent
from .thing_events import ThingStatusInfoChangedEvent as ThingStatusInfoChangedEvent
from .thing_events import ThingStatusInfoEvent as ThingStatusInfoEvent
from .thing_events import ThingUpdatedEvent as ThingUpdatedEvent
````

I played around a little bit more and if I remove the reexport everything works as expected.

````python
# without reexport
from .events import (
    ItemNoChangeEvent,
    ItemNoUpdateEvent,
    ValueChangeEvent,
    ValueCommandEvent,
    ValueUpdateEvent,
)

# reexports are all like this
from .filter import (
    AndFilterGroup as AndFilterGroup,
)
# or like this without a trailing comma
from .filter import EventFilter as EventFilter

````

I really like `F401` because it keeps my module imports clean and the imports sorted.
However in `__init__.py` files it's more of a burden and even results in less (!!!) readable code.
~But if I want automatic sorting of imports I have to enable it.~ Edit: Thats `I001`
So I'm stuck in this strange state where ruff is very helpful and very annoying.
Do you have any idea or tips how I can make it work?

I'm currently thinking of automatically generating an `__all__` symbol in the `__init__.py` files.
That way I can still have a nice an clean interface.

---

_Comment by @amyreese on 2025-12-02 20:03_

> I really like `F401` because it keeps my module imports clean and the imports sorted.
> However in `__init__.py` files it's more of a burden and even results in less (!!!) readable code.

You can configure ruff to ignore F401 only in `__init__.py` files: https://docs.astral.sh/ruff/settings/#lint_extend-per-file-ignores

---

_Comment by @spacemanspiff2007 on 2025-12-10 03:56_

Thank you all for your replies! I think there was a mixup on my side:
I'm already ignoring `F401` in `__init__.py` but since it's a tool/library that is used by other users I had to add the reexport because Pylance was complaining on the users computer.
The reexport was then the reason for the strange sorting.

I'm now programmatically generating `__all__` in the `__init__.py` files so I can remove the reexport and sorting works again as expected.
Thank you for your help.

---

_Closed by @spacemanspiff2007 on 2025-12-10 03:56_

---

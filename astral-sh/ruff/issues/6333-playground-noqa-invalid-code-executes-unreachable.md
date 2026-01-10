---
number: 6333
title: "Playground: `noqa: Invalid Code` executes unreachable"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - playground
assignees: []
created_at: 2023-08-04T08:43:54Z
updated_at: 2023-09-14T19:43:03Z
url: https://github.com/astral-sh/ruff/issues/6333
synced_at: 2026-01-10T01:22:45Z
---

# Playground: `noqa: Invalid Code` executes unreachable

---

_Issue opened by @MichaReiser on 2023-08-04 08:43_

[Playground](https://play.ruff.rs/?secondary=Format#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsovYAMleQbC1QW5ooPPIDxJx4upAADtIAzABGFwxXL4WDQADWKFWGGguHSWz4eEEYFAEFEMj+GH4nFemR4hEGunwcGKsVExEIPx+mWIxAw4iksiUGlRpNg5IuUCpNLpDPSuFmBKJuH4GhJZJQFKgcEQSCKTzhmjhuGkks50u5kDGGVmRHgsg6PGkP3QGuKbAAvr1wVCYRgyPC0HjdOiMZAlko0Dg0OgMDwJD9GZxMmUMurzhiPQB6QM-UHR0QxhSKONBxNJyAxv1yGPEJYSO7Rh6iLBLFBYSEB03++JYFA-ZG4C0oa22iHQ2EcDRkFBKN3atopKSELYUWZYOWopjtsGdh0cbh8ASD6OQJe8fgM3WZabEM4MRhz0R2rvYBBBzIVgSfVd0d2iOXIENlbDTjBIPFLDDQ6RIDJMFZSQkh4dkpRPKAzwdHA-mUDQlkPR8oERTgG3UbRvgMEJODCABhBA4NSRCAB0SOIEwDDIgw8LIsIyIosIQjItBgAOK0DAwFi2KtMJvEzTF+AwOo1A+PZdBcSDIGg2EBkUCRiAlB9tQkaAtEVZQFjmJJFhWOhJIgG153tWEAEdCAQRJUWQnVcGmIUMHMyyqG+NAEEICcBKgCRCC3ezZicqzXPczyZTEBAsAPTSNEciygrocKPMoLyYDkBBOEwKgpzNXQeEUQg20MjsTKKEp8DXD1d1mIYJBcqMsyMVToS80QjGgYgXTKFqoCMTIfghBtusgIw6hKNBqBlVrRtgNAjD9UlfLAoajBqlAjGWhRYAK9buWPIrjPPPg0FkTggwyJblPXJ1cHUEpoD4ORZlOn5ztRX4lFAibtWuoUgJ+Tg3StKSZJrM1sArKtxQqyk8rQw8OS5dd4hQINXgkBBxEoGduVEE1-oWYsMVLKB8twPhaowFBNsIe7-SdYpsAhek6qPMLSfJ2YqbgGnEkwP0cEUe6MhnPawCM08F1hDQXkSBJoagjoyjvDBVuxpMeqG6WeFl8pJpQrWdcJiBib6ApakV5sVegWqReByWcSDaYsDxDSL1wKckO1Z8FR8vyZly-LCrF4rz2e863cesm72sr3pnalnkNEH5nXa74fjQIa-iURRkDT6BM8QHhYGkAA6eJaSoN6fh0PXIGKcgMhbBLiFXWuCmIDJwVzhKeHK2vcEkP5vlwBNa9Unh+ss6YyG+R2hvHyei84MgS7+Re094IaeEhfhEkUb5t8zhAIUUKvYEz+ESjTlta7gLZQ2+O+hqWY+EDkTgUCQN6ljkI2xbtkqfwrKvAPNIZKl0PR1ASESWYydMhkwrMUVEeUCphTgdbF4yg9hWxtqDfYkBCT9SyGgpQGDYbYOplQPB3x9y621Og2q5DZiUIZDnBUeN8GEOSmzaAnAkG3XMqGWY49yzCQyNkHGUAABC8cACiWgGxNjvENeRijmxDQAGrcxQLIxQOd961wAPIAGVdH6KGgASUMWY8RtdZG4HfjnXAtUyY2IMZIuuEVoQ8DLno8Ru0eF8KocOARXBMgYBET+fA4jmhhVUooascDQKIJZigoO4soL2w6CQf0wEXEXTRNqKq8glAqE6jglmCN0m9D4edeW0kMjoSQILYMcAVBINbGFaJih0IfAWGofynTtR9I0AMmYlMFFbQ+CiSYHocASDIOM9qDsXqKAKVUsKxB+ouzwAGQW3BxQXikPnOgaSwqAQGI6WQHCA6oO1N03p4cvzLCEsUJWMy6AbO1FOBmxRbwPX2F89cDzZg8AQAGQiszRCQnSEgG6dRT6vDgSaKFUAYXIBui8xQmBkWRlZtqdFcK5gRTgGImaAxUWm0FoqFO2K5jL0FooPFOR1x7nupwR6KzXoYAufvBKRA1mINeGCxmCAkF0NZSgQRmQRyrIKSyuZTMOmxO+SiLYZMRZhVTCoCcmrtTpHpUKBkfZun7AVVMGYDJoD4D3lytZqIMCAjCv5Y1LxUBJDwaiFwXTGn+mKOgxIlK3mW15RI9W0kxwwKGgeWlmBp6MvVG3JYoYcVKBRbXBFB4MC4qGogKcsAyV+n3gE4ZlZmzWSBvtTEWApx9nqapdS8yiFaDxHi4EUkzQ-FhDVKGEDRDFNVmGrMxQeAAFVR4eMSEoAAIhi6NLxx14SVVvfsihZ1wqXe1L64aR3joALIY18sQydq7124APZjY94b2rSHdsYhdE7r3EFvVgAAKqeudtdd0-HfQead9184Zt4VtTIqjGzqNrogBYe7K7QGlvPNS07OD4HKiW9cWAlWMJfnzSsGQhanyDUiNCWGMaMlw4LMFBHmgdukL6KgJpwGFPXMUt+AwoMxS2MQas8z8ngU1DRtykUGP7BssU-meHKN6vXLSQiAwTRkYFvh22VaoB-GgjHJG-RCjSyFMoSKFQaP2U9uub2ES4NoRKVtUFtY1bDrykNMg0ggpoY9A2x0gt3aIN0ICAATHEtSt1CSDFRAANn8+pVkqIACs4WaWJF47oKLBlg65D+NSDQgs-T1OhI2dQxpTrWfxkpMAVTrQgCtJESr80wDAIMNAZgYAyANYAMRgHSKZaAdAABKo6ABiAANQbIAACQWAGtoDCHQZO9IgA)

Issue: Fails with *unreachable executed*

---

_Label `bug` added by @MichaReiser on 2023-08-04 08:43_

---

_Label `playground` added by @MichaReiser on 2023-08-04 08:43_

---

_Renamed from "Playground: `select: ALL` executes unreachable" to "Playground: `noqa: Invalid Code` executes unreachable" by @MichaReiser on 2023-08-04 08:48_

---

_Comment by @charliermarsh on 2023-08-04 13:25_

I can take a look at this.

---

_Comment by @charliermarsh on 2023-08-05 16:59_

So strange, I can't get this reproduce locally (and redeploying the playground didn't seem to help).

---

_Comment by @charliermarsh on 2023-08-05 17:00_

I bet it has to do with our attempt to construct a relative path for display when emitting the warning...

---

_Comment by @charliermarsh on 2023-08-05 17:18_

Can't repro locally even with a production build, so strange.

---

_Comment by @charliermarsh on 2023-08-05 17:19_

Oh wait, I'll try the WASM prod build.

---

_Comment by @charliermarsh on 2023-08-05 17:26_

Still no, what the heck...

---

_Comment by @charliermarsh on 2023-09-14 19:43_

This got fixed.

---

_Closed by @charliermarsh on 2023-09-14 19:43_

---

```yaml
number: 4390
title: Expand RUF008 to all classes, but to a new code (RUF012)
type: pull_request
state: merged
author: adampauls
labels: []
assignees: []
merged: true
base: main
head: only-mutable-fields
created_at: 2023-05-12T16:12:19Z
updated_at: 2023-06-12T16:55:21Z
url: https://github.com/astral-sh/ruff/pull/4390
synced_at: 2026-01-12T15:55:15Z
```

# Expand RUF008 to all classes, but to a new code (RUF012)

---

_@adampauls_

AFAIK, there is no reason to limit RUF008 to just dataclasses -- mutable defaults have the same problems for regular classes. 

Partially addresses https://github.com/charliermarsh/ruff/issues/4053 and broken out from https://github.com/charliermarsh/ruff/pull/4096. 

---

_Comment by @github-actions[bot] on 2023-05-12 16:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.6±0.02ms     7.3 MB/sec    1.01      5.6±0.02ms     7.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1156.4±1.82µs    14.4 MB/sec    1.00   1160.5±2.48µs    14.3 MB/sec
formatter/numpy/globals.py                 1.00    132.6±0.75µs    22.3 MB/sec    1.01    133.3±0.91µs    22.1 MB/sec
formatter/pydantic/types.py                1.00      2.5±0.01ms    10.2 MB/sec    1.01      2.5±0.01ms    10.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.08ms     2.9 MB/sec    1.00     14.0±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.9±1.25µs     7.2 MB/sec    1.01    413.0±1.04µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.08ms     4.3 MB/sec    1.00      5.9±0.08ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.8±1.84µs    11.3 MB/sec    1.00   1465.0±2.17µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.5±0.21µs    18.2 MB/sec    1.00    162.1±0.49µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.2±0.07ms     6.5 MB/sec    1.00      6.3±0.14ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1273.9±28.33µs    13.1 MB/sec    1.00  1271.2±25.46µs    13.1 MB/sec
formatter/numpy/globals.py                 1.00    142.7±4.18µs    20.7 MB/sec    1.01    144.2±4.42µs    20.5 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.04ms     9.2 MB/sec    1.01      2.8±0.06ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.19ms     2.7 MB/sec    1.01     15.4±0.29ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.05ms     4.3 MB/sec    1.00      3.8±0.07ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   439.6±10.58µs     6.7 MB/sec    1.01    442.7±9.81µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.11ms     4.0 MB/sec    1.02      6.5±0.17ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.10ms     5.4 MB/sec    1.00      7.5±0.10ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1577.1±22.59µs    10.6 MB/sec    1.00  1580.0±28.09µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.1±4.41µs    16.8 MB/sec    1.01    177.4±4.65µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.05ms     7.6 MB/sec    1.00      3.4±0.08ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-05-12 21:50_

With dataclasses, it is not apparent that a value will be shared across all of the instances. However, that is a special case. For class attributes, it is normal for a value to be shared across all of the instances. That's often the _reason_ that you would use a class attribute instead of assigning the value in `__init__`. For example, one of the few cases where I use class attributes is for tracking instances of the class or to implement a cache for all instances:

```python
class Foo:
    _instances = []
    
    def __init__(self):
        self._instances.append(self)
        
class Bar:
    _cache = {}

    def do_thing(self, x):
        if x not in self._cache:
            self._cache[x] = x + 1
        
        return self._cache[x]
```

Rarely do I want these kind of details to leak into the global namespace as suggested by adding a global variable.

The [Python documentation for class and instance attributes](https://docs.python.org/3/tutorial/classes.html#class-and-instance-variables) are quite clear about this being the intended use case

> Generally speaking, instance variables are for data unique to each instance and class variables are for attributes and methods shared by all instances of the class

This is only unclear when using dataclasses because the class attributes are being used to magically define instance construction.

To be fair, the Python documentation does also note

> shared data can have possibly surprising effects with involving [mutable](https://docs.python.org/3/glossary.html#term-mutable) objects such as lists and dictionaries. For example, the tricks list in the following code should not be used as a class variable because just a single list would be shared by all Dog instances:

but the suggestion here should be to use an instance variable not a global variable.

If you're interested in adding a rule for this, please consider make it a separate rule as is quite distinct from the dataclass case.

---

_Comment by @adampauls on 2023-05-12 23:37_

My understanding was that `ClassVar` exists precisely to precisely to [address the confusion](https://peps.python.org/pep-0526/#class-and-instance-variable-annotations) between class and instance variables. It's true that if you enable RUF008 but are not in a codebase with type hints, you might have a bad time, but that seems okay to me?

> This is only unclear when using dataclasses because the class attributes are being used to magically define instance construction.

I think this is not quite true. If you put `x: int = 5` on any class (dataclass or not), then there is an instance variable as well as a class variable:
```
>>> class Foo:
...   x: int = 5
...
>>> f = Foo()
>>> f.x=6
>>> f.x
6
>>> Foo.x
5
>>> import dataclasses
>>> @dataclasses.dataclass
... class DFoo:
...   x: int = 5
...
>>> DFoo(7)
DFoo(x=7)
>>> d = DFoo(7)
>>> d.x=6
>>> d.x
6
>>> DFoo.x
5
>>> DFoo.x=4
>>> d.x
6
>>> DFoo.x
4
>>> Foo.x=4
>>> Foo.x
4
>>> f.x
6
```

I think the reason that the original rule was written for dataclasses is that people do not commonly use attribute declarations outside of dataclasses, but I think that's more "cultural" than anything. You're probably right that outside of dataclasses, there's a pretty good chance that if have a `x: int = 5`, you are probably using it as a class variable, even though if is in fact just as much an instance variable as a class variable.

I think it's okay to have this rule and have people use `ClassVar` to declare when they are doing that, but I'm happy to be overruled and make this a separate rule. 

---

_Comment by @zanieb on 2023-05-13 00:29_

Thanks for taking the time to expand on your viewpoint!

Perhaps a compelling reason to make them different rules is that the suggestion is different:

- For dataclasses, mutable attributes should use `field(default_factory=...)` or (rarely) use `ClassVar`
- For classes, mutable attributes should use `ClassVar` or be changed to instance variables (assigned in `__init__`)

I see use of class variables on dataclasses as a bit of an antipattern and would be hesitant to suggest it to most users.


---

_Comment by @adampauls on 2023-05-13 17:44_

@madkinsz I think you are saying that class attributes "should" only be used as instance variables for dataclasses and as class variables for non-dataclasses, even though in fact, class attributes are (very confusingly!) always both in either case. You may or may not be right that the Python community does that in practice. I'll just note that RUF008 already explicitly exempts ClassVars from the mutability check, indicating that at `ruff` does not currently take the position that ClassVars are so rare for dataclasses. I think, culturally, the type of people who use `ruff` to catch bugs will be okay annotated class variables on non-dataclasses with `ClassVar[Any]` if they have to. 

I'll also just say that this whole class/instance variable thing is even worse than you think! An expanded example from PEP 526:
```python
    class Starship:
        captain: str = "Picard"
        damage: int
        stats: ClassVar[dict[str, int]] = {}

        def __init__(self, damage: int, captain: str | None = None) -> None:
            self.damage = damage
            if captain:
                self.captain = captain  # Else keep the default

        def hit(self):
            Starship.stats["hits"] = Starship.stats.get("hits", 0) + 1

    enterprise_d = Starship(3000)
    enterprise_c = Starship(2000)

    # pyright -- error: Cannot assign member "stats" for type "Starship"
    # HOWEVER, this still makes a new instance variable if you run the code (!)
    enterprise_d.stats = {}
    Starship.stats = {}  # This is OK
    enterprise_d.stats["foo"] = 1  # this is okay for pyright and only mutates the new instance variable on enterprise_d
    enterprise_c.stats["bar"] = 2  # this is okay for pyright, but mutates the class variable
    print(Starship.stats)  # prints {'bar': 2}
```

Even `pyright` can't save us from the madness! I know this example doesn't exactly help my case, since this PR would not catch any of the confusing behavior in this example. At the same time, this behavior is even more confusing if you don't declare `stats` to be a `ClassVar`.

My point is just that the behavior here is all very confusing and I think it is nice to be able to say that mutable non-`ClassVar `class attributes should just never be used, hence the new RUF008. Happy to break it out into two rules if @charliermarsh prefers though.

---

_Comment by @charliermarsh on 2023-05-15 18:52_

Hey sorry -- I know I'm being looked to for an opinion here, it's on my TODO list!

---

_Comment by @charliermarsh on 2023-05-24 03:16_

Okay, sorry, I finally got time to read through this in detail -- thank you both for the thoughtful analysis above.

I tend to agree with @madkinsz that dataclasses are materially different from other classes, and that if we do pursue these, they should be implemented as separate rules. However, I'm not yet convinced that we _should_ add those separate rules in the first place. Using mutable class attributes is a common and reasonable convention in Python.

The furthest I'd go, I think, is to flag mutable class attributes that _aren't_ annotated with `ClassVar` (and exempt dataclasses from this check, since they should be enforced with the existing rules, and we'd like to avoid duplicate diagnostics). I know that `ClassVar` actually connotes something slightly different ("This shouldn't be assigned on individual instances", whereas here we're treating it as "This is known to be mutable and shared across classes" -- you could imagine a hypothetical situation in which someone wants a mutable class attribute, like a shared `dict`, but also wants to allow individual instances to override with their own `dict`; but that strikes me as an odd pattern, and this is a linter, we can be opinionated about the patterns that we flag and don't).

Does that seem useful to folks? Too limited to be useful?

Either way, before merging, I'd want to see what violations that rule surfaces on the ecosystem CI check or on a broader set of repos (\cc @konstin), which is really something we should be doing for all novel Ruff rules anyway.


---

_Comment by @adampauls on 2023-05-25 15:43_

> The furthest I'd go, I think, is to flag mutable class attributes that aren't annotated with ClassVar

RUF008 already excludes attributes that are annotated with ClassVar, so I think this PR already does what you want, except that it doesn't split out a different error for non-dataclasses. Totally happy to do that. Or am I misunderstanding and there's some other difference in behavior you're suggesting? 

---

_Renamed from "Expand RUF008 to all classes" to "Expand RUF008 to all classes, but to a new code (RUF011)" by @adampauls on 2023-05-28 07:19_

---

_Comment by @adampauls on 2023-05-28 07:19_

@charliermarsh I split the check for non-dataclasses out into a new rule. 

---

_Comment by @konstin on 2023-05-28 17:20_

To better understand the impact of this change, i ran RUF011 against the scraped ecosystem dataset: https://gist.github.com/konstin/7391392ca16747031aa1b0c314568f21. Naturally, there's a lot of noise there (e.g. i've seen some migration files being flagged).

Background: The dataset includes 3408 github projects using ruff from [akx/ruff-usage-aggregate](https://github.com/akx/ruff-usage-aggregate/blob/4adc8337aeb1e6da3570cfbb6d2581800ff73084/data/known-github-tomls-clean.jsonl), 1174 contain at least one RUF011 violations. I ran the analysis with `scripts/ecosystem_all_check.sh check --select RUF011` and collected the results with `rg ": RUF011" target/ecosystem_all_results/*.stdout.txt > target/RUF011.txt`. The repositories use the default branch of each repository as of 2023-05-24.

The normal ecosystem ci should create a more concise report for a small number of handpicked projects, but i think it's currently skipped as cargo test is failing.

---

_Comment by @adampauls on 2023-05-28 21:31_

Oops, fixed `cargo test`. 

---

_Comment by @adampauls on 2023-05-28 21:38_

I spot checked a few of the flags in the ecosystem. It looks like there are some cases where folks are using dataclass-like classes (like `pydantic.BaseModel`, indirectly inherited [here](https://github.com/logspace-ai/langflow/blob/dev/src/backend/langflow/interface/prompts/custom.py#L68)) and just plain old classes with mutable defaults (like [here](https://github.com/apache/airflow/blob/main/airflow/www/views.py#L4485-L4493)). They all look to me like they are meant to be instance defaults and could have the same gotcha behavior as for dataclasses. 

---

_Comment by @adampauls on 2023-06-09 15:27_

@charliermarsh is this on the TODO list? Or should I close?

---

_Comment by @charliermarsh on 2023-06-09 15:32_

@adampauls - It's on my list to revisit today. Sorry for the delay.

---

_Renamed from "Expand RUF008 to all classes, but to a new code (RUF011)" to "Expand RUF008 to all classes, but to a new code (RUF012)" by @adampauls on 2023-06-11 23:03_

---

_Merged by @charliermarsh on 2023-06-12 16:54_

---

_Closed by @charliermarsh on 2023-06-12 16:54_

---

_Comment by @zanieb on 2023-06-12 16:55_

🎉 thanks @adampauls!

---

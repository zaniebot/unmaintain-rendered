```yaml
number: 14009
title: "Feature request: add rule for `unittest` - always call `super().<setUp|tearDown|setUpClass|tearDownClass>()`"
type: issue
state: open
author: AdrianB-sovo
labels:
  - rule
  - type-inference
  - accepted
assignees: []
created_at: 2024-10-31T01:04:29Z
updated_at: 2025-01-13T23:32:42Z
url: https://github.com/astral-sh/ruff/issues/14009
synced_at: 2026-01-10T11:09:55Z
```

# Feature request: add rule for `unittest` - always call `super().<setUp|tearDown|setUpClass|tearDownClass>()`

---

_Issue opened by @AdrianB-sovo on 2024-10-31 01:04_

In my tests, I'd like to make sure that all my derived classes of `unittest.TestCase` call the corresponding `super()` method for the methods `setUp()`, `tearDown()`, `setUpClass()` and `tearDownClass()`.

Could a linter rule be added to Ruff for that?

**EDIT:** the description was ambiguous. We should only enforce the call to `super()` in overriden implementations of `setUp()`, `tearDown()`, `setUpClass()` and `tearDownClass()` of **custom** test classes.
There is <ins>**no**</ins> need to enforce the use of `setUp()`, `tearDown()`, `setUpClass()` and `tearDownClass()` for every custom test classes.

---

_Comment by @AlexWaygood on 2024-10-31 10:40_

This seems reasonable to me. It would be quite a strict rule, but I think the chance of it producing invalid suggestions would be pretty low. `unittest.TestCase` already defines these methods, so it should be possible to always call `super().setUp()` etc. on any subclass of `unittest.TestCase`.

---

_Label `rule` added by @AlexWaygood on 2024-10-31 10:41_

---

_Label `accepted` added by @AlexWaygood on 2024-10-31 10:41_

---

_Comment by @mscheifer on 2025-01-13 06:58_

What's the potential danger of not calling `super()` ? The examples in the official Python docs don't call it: https://docs.python.org/3.13/library/unittest.html#organizing-test-code

---

_Comment by @MichaReiser on 2025-01-13 07:50_

I don't think there's any real downside of not calling `setUp` etc, because unit test documents that they do nothing and I see it as extremely unlikely that this will ever change because a) it would be a breaking change and b) the `unittest` framework has other means to run some code before or after every test. 

The only argument I see for enforcing calling the `super` method is that it's generally considered best practice to call `super` in overridden methods even if they do nothing today because it safeguards against missing `super` call if the parent class *does something in the future*. 

Considering that it's highly unlikely that `unittest` will ever customize `setUp` and `tearDown`, I think this rule is too restrictive without providing real value. However, one issue that I observed more than once when working on a large Java code base is that tests forgot to call the `super` methods when inheriting from *custom* test classes and the custom test class started to override `setUp` (or `tearDown`) but not all subclasses were updated to call `super.setUp`. That's why I think the rule should instead lint for custom test classes and only then enforce that `setUp`, `tearDown` etc. calls the `super` method if overriden. 

---

_Comment by @AlexWaygood on 2025-01-13 11:19_

Consider a case where you have a complex application or library where all tests need some common setup and teardown logic. For example, the `asyncio` library in the standard library is huge, so its tests are split across multiple `unittest.TestCase` subclasses in multiple test modules. However, they all need some custom setup and teardown logic so that the `asyncio` event loop is constructed prior to the test and gracefully shut down prior to the test. The way this is done is via a [`test_asyncio.utils.TestCase` class](https://github.com/python/cpython/blob/afb9dc887c6e8ae17b6a54c6124399e8bdc82253/Lib/test/test_asyncio/utils.py#L553-L565) which subclasses `unittest.TestCase`. All `TestCase` subclasses that actually provide _tests_ for asyncio then subclass this `utils.TestCase` class rather than subclassing `unittest.TestCase` directly. If these `utils.TestCase` subclasses need to provide custom `setUp` logic (e.g. [here](https://github.com/python/cpython/blob/afb9dc887c6e8ae17b6a54c6124399e8bdc82253/Lib/test/test_asyncio/test_base_events.py#L1114-L1119)), it's crucial that those subclasses call `super().setUp()`, or they won't get the benefit of the any of the logic implemented in the `setUp()` method implemented by the `test_asyncio.utils.TestCase` class; they might as well have subclassed `unittest.TestCase` directly.

So I think there is no benefit at all to calling `super().setUp()` on direct subclasses of `unittest.TestCase` classes, but quite a bit of benefit to calling `super().setUp()` on indirect subclasses of `unittest.TestCase`. Unfortunately, indirect subclasses are quite hard for Ruff to detect reliably right now, due to the lack of multifile analysis (we wouldn't be able to detect that `test_asyncio.utils.TestCase` subclasses `unittest.TestCase` in the example above if we were linting `Lib/test/test_asyncio/test_base_events.py`, which is where the `super().setUp()` call needs to occur). It's therefore quite possible that there's no reasonable way for us to implement this rule in a useful way right now, with our current capabilities.

---

_Comment by @MichaReiser on 2025-01-13 11:25_

Thanks for sharing an actual use case and it's good to know that we're on the same page. I agree that there's probably no good way for Ruff to detect indirect usages but I think that's the case where the rule would be most valuable. That's why I suggest we defer implementing this rule until we have multifile analysis support because the rule today would only be able to flag cases where overriding `setUp` (and others) isn't necessary and misses all use cases that are actually problematic. 

---

_Label `type-inference` added by @MichaReiser on 2025-01-13 11:25_

---

_Comment by @AlexWaygood on 2025-01-13 12:10_

It's possible that we _could_ implement a version of this rule now that is only able to detect the indirect subclass of `TestCase` when the direct subclass is defined in the same file as the indirect subclass. That _does_ happen sometimes -- for example, https://github.com/python/cpython/blob/6ff8f82f92a8af363b2bdd8bbaba5845eef430fc/Lib/test/test__interpreters.py#L554-L557. It goes back to the question we've had to answer before about how much of an issue lots of false negatives are while we wait for multifile analysis... I honestly don't really have a sense for how common it is for indirect subclasses to be in the same file versus a different file as the direct subclass.

---

_Comment by @njhearp on 2025-01-13 22:43_

I decided to close my pull request because it seems like this rule will need multi-file analysis to be useful and effective.

---

_Comment by @AdrianB-sovo on 2025-01-13 23:28_

Yes, I also think this rule wouldn't be that useful without multi-file analysis.

Also, to confirm: the use case is when having multiple inherited levels of custom test classes (e.g. `unittest.TestCase` → `TestWithDB` → `BaseIntegrationTest` → `MySpecificTest`).

@MichaReiser 
> I think this rule is too restrictive without providing real value. [...] That's why I think the rule should instead lint for custom test classes and only then enforce that setUp, tearDown etc. calls the super method if overriden.

Yes, my description was ambiguous: we don't need to always have `<setUp|tearDown|setUpClass|tearDownClass>()` and the corresponding `super()` called in it.
I only want to enforce that the `super()` is called _**if**_ we implement them in our custom test classes.

---

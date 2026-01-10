```yaml
number: 7844
title: Add a declarative test framework
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
draft: true
base: main
head: konsti/declarative-tests
created_at: 2024-10-01T14:23:07Z
updated_at: 2025-03-10T23:04:11Z
url: https://github.com/astral-sh/uv/pull/7844
synced_at: 2026-01-10T11:10:34Z
```

# Add a declarative test framework

---

_Pull request opened by @konstin on 2024-10-01 14:23_

While reviewing the index PRs, the tests all followed the same structure; they weren't so much test logic as a sequence of writing files, a test context command and snapshotting output and files.

Instead of defining them in code, we can write them as toml scenarios:

```toml
[[step]]
input."pyproject.toml" = """
[...]
"""
command = "add"
args =  ["git+https://github.com/astral-test/uv-public-pypackage", "--tag=0.0.1"]
output = """
[...]
"""
snapshot."pyproject.toml" = '''
[...]
'''
snapshot."uv.lock" = '''
[...]
'''
```

The tests now have a shared schema and can be written without code. We can change the tests without recompiling and massively reduce the code size in the `uv` crate (previously, each time you would apply snapshot changes you would have to wait for a recompile). The tests become easier to maintain (toml can be edited by Python should need be) and by decoupling the test declaration from code we can add improve the test runner more easily. I don't expect all tests to be migrated, there's actual custom code in a number of tests.

**Migration Strategy** Procedural and declarative tests can coexist in the same file, there is no breaking change involved. If we want to migrate existing test, we can move the existing testing to more concise abstractions using the `TestContext` with rust refactoring, then using regex to migrate many simple procedural tests.


The PR migrates two tests as an example. I would suggest merging the framework and starting to write new tests in it, extending it as needed. Once we feel sufficiently confident and the need, we can migrate older tests, too.

---

_Label `internal` added by @konstin on 2024-10-01 14:24_

---

_@konstin reviewed on 2024-10-01 14:31_

---

_Review comment by @konstin on `crates/uv/scenarios/sync/add_unnamed.toml`:77 on 2024-10-01 14:31_

We can split this field into its part for nicer visuals

---

_Comment by @BurntSushi on 2024-10-01 16:08_

I am loosely in favor I think. I did something similar (partially for the reasons you stated here, but also for other reasons) for `regex` and I'm generally happy with the result. Although in `regex`, I don't have a one-to-one correspondence between declarative tests and Rust `#[test]` annotations. Instead, each Rust `#[test]` would actually run the full set of tests. This does have some pretty awful downsides though, and tends to be most useful when you want to run each declarative test multiple different times under different configurations (as I do in `regex`). I'm not sure we necessarily need that here in uv, but I could see that being useful if expensive. Either way, this would be a first step toward that world I think.

Some other thoughts:

* Since this is TOML embedding TOML, it would be annoying if the embedded TOML contains both `"""` and `'''`. Not a major issue though since I think there are multiple ways to overcome this.
* I don't know that this saves a bunch of typing. While this does transition us to a more solidly declarative approach, I would argue that we don't need TOML for our tests to be declarative. We can do declarative testing directly with Rust code. We are pretty close to that already given that most of our tests follow a defined pattern. The advantage of doing it with Rust directly is that it will likely make it easier for tests that don't easily fit in the existing declarative framework to go off and do something different.
* When I moved the tests to TOML in `regex`, I found that I added a fair number of features to the declarative TOML to expand the scope of what kinds of tests it could cover.

---

_Closed by @konstin on 2025-03-10 23:04_

---

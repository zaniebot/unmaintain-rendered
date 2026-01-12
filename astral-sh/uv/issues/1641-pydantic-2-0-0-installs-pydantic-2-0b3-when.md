```yaml
number: 1641
title: pydantic<2.0.0 installs pydantic=2.0b3 when prereleases are allowed
type: issue
state: closed
author: erdembanak
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-02-18T11:54:52Z
updated_at: 2024-03-09T11:45:46Z
url: https://github.com/astral-sh/uv/issues/1641
synced_at: 2026-01-12T15:58:30Z
```

# pydantic<2.0.0 installs pydantic=2.0b3 when prereleases are allowed

---

_@erdembanak_

Using uv 0.1.3, when trying to install pydantic<2.0 with prerelease allow, uv installs pydantic==2.0b3. According to [pubgrub](https://pubgrub-rs-guide.netlify.app/limitations/prerelease_versions), 2.0b3 shouldn't be contained in the set:

> "For example, "2.0.0-beta" is meant to exist previous to version "2.0.0". Yet, it is not supposed to be contained in the set described by 1.0.0 <= v < 2.0.0, and only within sets where one of the bounds contains a pre-release marker such as 2.0.0-alpha <= v < 2.0.0".


Running ` uv pip install --prerelease=allow "pydantic<2.0" --verbose` provides the below output:

```
 uv::requirements::from_source source=pydantic<2.0
    0.002072s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /home/erdemb/.venv
    0.002191s DEBUG uv_interpreter::interpreter Using cached markers for: /home/erdemb/.venv/bin/python
    0.002207s DEBUG uv::commands::pip_install Using Python 3.10.12 environment at /home/erdemb/.venv/bin/python
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.003246s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.10.12
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.003287s   0ms DEBUG uv_resolver::resolver Adding direct dependency: pydantic<2.0
   uv_resolver::resolver::choose_version package=pydantic
     uv_resolver::resolver::package_wait package_name=pydantic
 uv_resolver::resolver::process_request request=Versions pydantic
   uv_client::registry_client::simple_api package=pydantic
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/pydantic.rkyv
 uv_resolver::resolver::process_request request=Prefetch pydantic <2.0
          0.004614s   1ms  WARN uv_client::cached_client Broken cache entry at /home/erdemb/.cache/uv/simple-v1/pypi/pydantic.rkyv, removing: failed to open file `/home/erdemb/.cache/uv/simple-v1/pypi/pydantic.rkyv`
          0.004832s   1ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/pydantic/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/pydantic/"
       uv_client::cached_client::new_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/pydantic.rkyv
       uv_client::registry_client::parse_simple_api package=pydantic
 uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pydantic==2.0b3
     uv_client::registry_client::wheel_metadata built_dist=pydantic==2.0b3
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/wheels-v0/pypi/pydantic/pydantic-2.0b3-py3-none-any.msgpack
        0.967483s 964ms DEBUG uv_resolver::resolver Searching for a compatible version of pydantic (<2.0)
        0.967501s 964ms DEBUG uv_resolver::resolver Selecting: pydantic==2.0b3 (pydantic-2.0b3-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=pydantic, version=2.0b3
     uv_resolver::resolver::distributions_wait package_id=pydantic-2.0b3
              0.967626s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/af/ce/dabc6cd90abaf903d84a329f29a5ce34643fd66ac2a78f3eb1c668bd4fb3/pydantic-2.0b3-py3-none-any.whl
        0.967715s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: annotated-types>=0.4.0
        0.967749s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: pydantic-core==0.39.0
        0.967804s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: typing-extensions>=4.6.1
   uv_resolver::resolver::choose_version package=annotated-types
     uv_resolver::resolver::package_wait package_name=annotated-types
 uv_resolver::resolver::process_request request=Versions annotated-types
   uv_client::registry_client::simple_api package=annotated-types
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/annotated-types.rkyv
 uv_resolver::resolver::process_request request=Versions pydantic-core
   uv_client::registry_client::simple_api package=pydantic-core
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/pydantic-core.rkyv
 uv_resolver::resolver::process_request request=Versions typing-extensions
   uv_client::registry_client::simple_api package=typing-extensions
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/typing-extensions.rkyv
 uv_resolver::resolver::process_request request=Prefetch typing-extensions >=4.6.1
 uv_resolver::resolver::process_request request=Prefetch pydantic-core ==0.39.0
 uv_resolver::resolver::process_request request=Prefetch annotated-types >=0.4.0
          0.967990s   0ms  WARN uv_client::cached_client Broken cache entry at /home/erdemb/.cache/uv/simple-v1/pypi/annotated-types.rkyv, removing: failed to open file `/home/erdemb/.cache/uv/simple-v1/pypi/annotated-types.rkyv`
          0.968012s   0ms  WARN uv_client::cached_client Broken cache entry at /home/erdemb/.cache/uv/simple-v1/pypi/pydantic-core.rkyv, removing: failed to open file `/home/erdemb/.cache/uv/simple-v1/pypi/pydantic-core.rkyv`
          0.969678s   1ms  WARN uv_client::cached_client Broken cache entry at /home/erdemb/.cache/uv/simple-v1/pypi/typing-extensions.rkyv, removing: failed to open file `/home/erdemb/.cache/uv/simple-v1/pypi/typing-extensions.rkyv`
          0.969754s   1ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/pydantic-core/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/pydantic-core/"
          0.969908s   2ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/annotated-types/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/annotated-types/"
          0.969967s   2ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/typing-extensions/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/typing-extensions/"
       uv_client::cached_client::new_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/pydantic-core.rkyv
       uv_client::registry_client::parse_simple_api package=pydantic-core
       uv_client::cached_client::new_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/typing-extensions.rkyv
       uv_client::registry_client::parse_simple_api package=typing-extensions
 uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=typing-extensions==4.10.0rc1
     uv_client::registry_client::wheel_metadata built_dist=typing-extensions==4.10.0rc1
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/wheels-v0/pypi/typing-extensions/typing_extensions-4.10.0rc1-py3-none-any.msgpack
              1.050399s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/d4/b2/4980568cd3814002f1c04b542e2dd52faa0cec11bd5bda99e43eefe9a808/typing_extensions-4.10.0rc1-py3-none-any.whl
       uv_client::cached_client::new_cache file=/home/erdemb/.cache/uv/simple-v1/pypi/annotated-types.rkyv
       uv_client::registry_client::parse_simple_api package=annotated-types
 uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=annotated-types==0.6.0
     uv_client::registry_client::wheel_metadata built_dist=annotated-types==0.6.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/wheels-v0/pypi/annotated-types/annotated_types-0.6.0-py3-none-any.msgpack
        1.184443s 216ms DEBUG uv_resolver::resolver Searching for a compatible version of annotated-types (>=0.4.0)
        1.184461s 216ms DEBUG uv_resolver::resolver Selecting: annotated-types==0.6.0 (annotated_types-0.6.0-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=annotated-types, version=0.6.0
     uv_resolver::resolver::distributions_wait package_id=annotated-types-0.6.0
              1.184758s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl
   uv_resolver::resolver::choose_version package=pydantic-core
     uv_resolver::resolver::package_wait package_name=pydantic-core
 uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pydantic-core==0.39.0
     uv_client::registry_client::wheel_metadata built_dist=pydantic-core==0.39.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/home/erdemb/.cache/uv/wheels-v0/pypi/pydantic-core/pydantic_core-0.39.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.msgpack
        6.767120s   5s  DEBUG uv_resolver::resolver Searching for a compatible version of pydantic-core (==0.39.0)
        6.767138s   5s  DEBUG uv_resolver::resolver Selecting: pydantic-core==0.39.0 (pydantic_core-0.39.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
   uv_resolver::resolver::get_dependencies package=pydantic-core, version=0.39.0
     uv_resolver::resolver::distributions_wait package_id=pydantic-core-0.39.0
              6.767260s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/10/15/82a7866bd5660dd4251c87ca23b01b8ba495ea0b7013bb8ee884dfce0311/pydantic_core-0.39.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
        6.767349s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: typing-extensions*
   uv_resolver::resolver::choose_version package=typing-extensions
     uv_resolver::resolver::package_wait package_name=typing-extensions
        6.767401s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of typing-extensions (>=4.6.1)
        6.767406s   0ms DEBUG uv_resolver::resolver Selecting: typing-extensions==4.10.0rc1 (typing_extensions-4.10.0rc1-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=typing-extensions, version=4.10.0rc1
     uv_resolver::resolver::distributions_wait package_id=typing-extensions-4.10.0rc1
Resolved 4 packages in 6.76s
    6.767756s DEBUG uv_installer::plan Requirement already cached: annotated-types==0.6.0
    6.767789s DEBUG uv_installer::plan Requirement already cached: pydantic==2.0b3
    6.767807s DEBUG uv_installer::plan Requirement already cached: pydantic-core==0.39.0
    6.767828s DEBUG uv_installer::plan Requirement already cached: typing-extensions==4.10.0rc1
 uv_installer::installer::install num_wheels=4
Installed 4 packages in 14ms
 + annotated-types==0.6.0
 + pydantic==2.0b3
 + pydantic-core==0.39.0
 + typing-extensions==4.10.0rc1
```

Pubgrub documentation also says that this is a difficult problem; therefore the expected behaviour of the --prerelease=allow should be documented if this is the desired output.

---

_Renamed from "pydantic<2.0.0 install pydantic=2.0b3 when prereleases are allowed" to "pydantic<2.0.0 installs pydantic=2.0b3 when prereleases are allowed" by @erdembanak on 2024-02-18 15:10_

---

_Comment by @notatallshaw on 2024-02-18 15:22_

How Pip behaves with **exclusive** operators only:

Does **not** install pydantic prerelease: `pip install --dry-run --pre "pydantic < 2.0"`
Does install pydantic prerelease: `pip install --dry-run --pre "pydantic < 2.0rc1"`

How Pip behaves with **inclusive** operators on June 18th 2023 (when there was only pre-releases and no final 2.0, once the final release happened inclusive operators install 2.0 final, and exclusive operators are the same):

Does install pydantic prerelease: `pip install --dry-run --pre "pydantic <= 2.0"`
Does install pydantic prerelease: `pip install --dry-run --pre "pydantic <= 2.0rc1"`

The relevant parts of the Python spec:

* https://packaging.python.org/en/latest/specifications/version-specifiers/#handling-of-pre-releases
* https://packaging.python.org/en/latest/specifications/version-specifiers/#inclusive-ordered-comparison
* https://packaging.python.org/en/latest/specifications/version-specifiers/#exclusive-ordered-comparison

Some recent discussion:

 * https://discuss.python.org/t/handling-of-pre-releases-when-backtracking
 * https://github.com/pypa/packaging/issues/776
 * https://github.com/pypa/pip/issues/12469
 * https://github.com/pypa/pip/issues/12471
 * https://github.com/pypa/pip/issues/12472

_IMO_ the Python spec leads to ambigious edge cases, _I_ think both uv's and Pip's behavior here can be interpreted to be part of the spec.

---

_Comment by @mpizenberg on 2024-02-18 16:12_

> According to [pubgrub](https://pubgrub-rs-guide.netlify.app/limitations/prerelease_versions)

@erdembanak what pubgrub doc states in that page is that the semantic meaning of "<2.00" is different than its mathematic, which makes solving difficult with solvers that cannot alter the knowledge it gathers while solving depending on context. For this reason, the resolver has to take decisions that are context independent. In the case of uv, as stated in their readme https://github.com/astral-sh/uv?tab=readme-ov-file#pre-release-handling, they choose the two rules:

1. If the package is a direct dependency, and its version markers include a pre-release specifier (e.g., flask>=2.0.0rc1).
2. If all published versions of a package are pre-releases.

But if you `--prerelease=allow` then it allows pre-releases for all dependencies. In your case, 2.0b3 is mathematically < 2.0 and you allowed pre-release for all, so it seems normal that it installs the prerelease.

---

_Comment by @dimbleby on 2024-02-18 16:24_

I do not think the spec is ambiguous

> The exclusive ordered comparison <V **MUST NOT** allow a pre-release of the specified version unless the specified version is itself a pre-release. 

(bolded capital letters from the original)

---

_Comment by @mpizenberg on 2024-02-18 16:36_

Also in the spec: https://packaging.python.org/en/latest/specifications/version-specifiers/#handling-of-pre-releases

> Dependency resolution tools SHOULD also allow users to request the following alternative behaviours:
> - **accepting pre-releases for all version specifiers**
> - excluding pre-releases for all version specifiers (reporting an error or warning if a pre-release is already installed locally, or if a pre-release is the only way to satisfy a particular specifier)


---

_Comment by @dimbleby on 2024-02-18 16:55_

I suggest that the way to read this so that the spec is consistent is: the section about handling pre-releases is discussing the behaviour for those specifiers that only implicitly disallow pre-releases.  

Exclusive ordered comparisons very clearly and very explicitly always disallow pre-releases.

---

_Label `bug` added by @zanieb on 2024-02-18 17:17_

---

_Label `resolver` added by @zanieb on 2024-02-18 17:17_

---

_Comment by @notatallshaw on 2024-02-18 17:32_

> I do not think the spec is ambiguous
> 
> > The exclusive ordered comparison <V **MUST NOT** allow a pre-release of the specified version unless the specified version is itself a pre-release.

What's ambiguous is when the resolver specifies the prerelease flag, does that implicitly mean all versions include prerelease?

If it does the uv is following the spec and pip is not.

If it doesn't what does it even mean to include a prerelease flag?



---

_Comment by @dimbleby on 2024-02-18 17:41_

> If it doesn't what does it even mean to include a prerelease flag?

see the previous comment.

I believe that the section about handling pre-releases is describing an option for eg the inclusive ordered comparisons
- for those, the comparison operator in itself does not exclude pre-releases
- but pre-releases are nevertheless in general implicitly excluded
- an "allow-prereleases" flag is intended to overrule that general implicit exclusion
- such a flag is not intended to overrule the basic meaning of a comparison eg the exclusive ordered comparison - regardless of any general implicit rules - always explicitly excludes prereleases

Eg `==2.0.0` does not accept `2.0b3` just because "allow-prereleases" is set: the version `2.0b3` does not satisfy the meaning of `==2.0.0`.  Similar reasoning holds for `<2.0.0`

---

_Comment by @notatallshaw on 2024-02-18 17:47_

That's certainly _a_ way to interpret the spec, but it doesn't explicitly state it, so one can also interpret how uv has implemented it.

---

_Comment by @dimbleby on 2024-02-18 17:50_

I certainly agree the spec could be clearer, but I think you have to work quite hard to justify ignoring

> The exclusive ordered comparison <V **MUST NOT** allow a pre-release of the specified version unless the specified version is itself a pre-release.

---

_Comment by @notatallshaw on 2024-02-18 17:53_

You only have to read the next sentence and take prerelease flag to mean that prerelease versions are included.

It takes only the work of correlating two things together.

---

_Comment by @notatallshaw on 2024-02-18 17:57_

In any case, I do agree your interpretation is reasonable and it's what pip does. So probably uv maintainers want to follow what pip does when what pip does makes sense?

---

_Comment by @mpizenberg on 2024-02-18 17:57_

Let's not fight over this. Let's wait for what the uv team has to say.

---

_Comment by @charliermarsh on 2024-02-18 18:50_

Hmm... I think my honest read of the spec is that even with `--prerelease=allow`, the specifier `<2.0.0` should not include `2.0b3`, but that it should allow `1.9b3` based on "a pre-release of the _specified_ version".

That being said, I think we're unlikely to implement it (at least not soon), since (1) it breaks fundamental properties of version solving, (2) makes the solver much harder to implement and reason about, and (3) frankly, may be even more confusing for users in some cases. In other words, I just doubt it will be high-priority enough for us to support it in the near future.


---

_Comment by @charliermarsh on 2024-02-18 18:52_

I suppose I'd also add (in our defense) that I wouldn't be surprised if there is _no_ Python resolver that is fully spec compliant on pre-releases. (I can't say this definitively, I haven't looked enough.) In particular, the last clause here is very hard to satisfy, since it means you have to expand the set of candidate packages as you proceed with a resolution:

> Pre-releases of any kind, including developmental releases, are implicitly excluded from all version specifiers, unless they are already present on the system, explicitly requested by the user, or if the only available version that satisfies the version specifier is a pre-release.

(I'm also not sure how that clause is intended to be interpreted. For example, say you have releases for Pydantic at `1.8.0`, `1.9.0a`, and `2.1.0` (but no other releases). And then you have one package that requests Pydantic at `<2.0.0`, and another that requests Pydantic at `!=1.8.0`. What should happen? Both markers can be individually satisfied without a pre-release, but taken together, they can _only_ be satisfied with a pre-release.)

---

_Comment by @notatallshaw on 2024-02-18 18:56_

> In particular, the last clause here is very hard to satisfy, since it means you have to expand the set of candidate packages as you proceed with a resolution:
> 
> > Pre-releases of any kind, including developmental releases, are implicitly excluded from all version specifiers, unless they are already present on the system, explicitly requested by the user, or if the only available version that satisfies the version specifier is a pre-release.

This is discussed, at some length, in the Python thread I linked.

I believe Poetry does attempt to follow this spec, but fails under more complicated circumstances (e.g. when backtracking on transitive dependencies), and there is an open issue on Poetry side for this.

Pip does not to follow it, as linked in the various issues I posted. One of the pip maintainers characterizes the rules pip follows in the linked Python thread pretty early on. Which I believe is the approach rip is following for this.

---

_Comment by @charliermarsh on 2024-02-18 19:00_

(Thanks, I swear I did read all of the pip issues you linked, but I didn’t make it through the entire Python thread yet.)

---

_Comment by @dimbleby on 2024-02-18 19:01_

> I wouldn't be surprised if there is no Python resolver that is fully spec compliant

For what it's worth I don't know any other solver that considers `2.0b3` to be compatible with `<2.0.0`, though I'm sure they all have bugs!

> ... What should happen? ...

it can get even more confusing than that!  What if the solver goes down a path such that dependency `foo` can only be satisfied with a pre-release; but - if only it had been omniscient - there was another path available such that `foo` did not need a pre-release?  What a mess!

---

_Comment by @notatallshaw on 2024-02-18 19:05_

Yeah, I'm sorry the discussion is so fragmented.

Honestly I got pretty frustrated with that topic, in particular it makes it extremely difficult to try and apply some static analysis resolver optimizations I wanted to try out, because "what is the right answer" appears to both involve a lot of contextual state about what the resolver has exhausted so far and actually be pretty ambiguous in relationship to the spec.

I am looking at packse, and may try and add some test cases that everyone can actually agree on, and then go from there. But I can't promise anything soon.


---

_Comment by @erdembanak on 2024-02-18 19:14_

Thanks for the comments.

> Hmm... I think my honest read of the spec is that even with --prerelease=allow, the specifier <2.0.0 should not include 2.0b3, but that it should allow 1.9b3 based on "a pre-release of the specified version".
,

I agree with this understanding. Let me tell you how I run into this issue (I wasn't just messing around). This became a problem for me when I am working with an easily reproducible requirements file:

`
azure-cli==2.46.0
pydantic<2.0.0
`

If I allow prerelease, pydantic is installed as beta and if I don't, I can't install the packages due to azure-cli depending on a prerelease:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of azure-mgmt-sql==4.0.0b8 and azure-cli==2.46.0 depends on azure-mgmt-sql==4.0.0b8, we can conclude that
      azure-cli==2.46.0 cannot be used.
      And because you require azure-cli==2.46.0, we can conclude that the requirements are unsatisfiable.

      hint: azure-mgmt-sql was requested with a pre-release marker (e.g., azure-mgmt-sql==4.0.0b8), but pre-releases weren't enabled (try:
      `--prerelease=allow`)
```

Even without --pre marker, pip installs azure-cli; so this is not a problem in pip. I am currently using overrides file (giving pydantic==1.x.x in there) to overcome this problem (the pydantic constraint is not coming from my requirement file); but it is just a workaround and makes migrating to uv from pip a bit harder than it should be.

---

_Comment by @notatallshaw on 2024-02-19 02:57_

Hmmm, the problem of "I want prereleases from one part of my requirements but not the other part of my requirements" is not supported by pip either, it just happens to be that the behavior pip has chose works out for you.

Unless I'm missing something fundamental about the soundness of the behavior I'm sure one could come up with another example where it would not work for pip but it would work for uv. But pip is so pervasive that everyone checks their use case works for it and if not changes their use case.

If you're interested in a workaround you can split this into multiple commands with constraints, I actually do this with pip constantly to keep my environment in sync, so this workflow will work with both uv and pip:

```
$ echo "azure-cli==2.46.0" | uv pip compile --prerelease=allow  - > requirements.txt
$ echo "pydantic<2.0.0" | uv pip compile --constraint requirements.txt - >> requirements.txt
$ uv pip install --requirement requirements.txt 
```


---

_Comment by @zanieb on 2024-02-19 03:45_

If anyone interested, writing some additional [test scenarios](https://github.com/zanieb/packse) for this could be really helpful.

---

_Comment by @dimbleby on 2024-02-19 08:08_

> just happens to be that the behavior pip has chose works out for you

I don't think that's right.  The requirement `azure-mgmt-sql==4.0.0b8` should find the prerelease regardless of "allow prereleases", per

> ... explicitly requested by the user, or if the only available version that satisfies the version specifier is a pre-release.

---

_Comment by @notatallshaw on 2024-02-19 12:37_

> > just happens to be that the behavior pip has chose works out for you
> 
> I don't think that's right. The requirement `azure-mgmt-sql==4.0.0b8` should find the prerelease regardless of "allow prereleases", per
> 
> > ... explicitly requested by the user, or if the only available version that satisfies the version specifier is a pre-release.

My comment about the behavior was about the choice prereleases with inclusive/exclusive operators when the prereleases flag is passed, the part of the spec you quote is highly problematic, Poetry is the only tool I'm aware of that tries to faithfully follow it but it fails to when [backtracking](https://github.com/python-poetry/poetry/issues/4405), and I'm not even sure it's logically sound to follow when backtracking (at least using any of the satisfiability algorithms any installer currently uses).

Pip certainly doesn't: https://github.com/pypa/pip/issues/12469. And rip copies pip's behavior, at least for now: https://github.com/prefix-dev/rip/issues/118. And I think uv copies this behavior by default also? I've not looked at the logic or tested but it seems to be able to install `opentelemetry-exporter-prometheus` without changing the preinstall flag.

I know I posted a lot of links earlier and it was a bit much to expect everyone to read them, but this is how pip's current behavior is best characterized in how it "follows" that part of the spec: https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/4



---

_Comment by @notatallshaw on 2024-02-19 13:06_

Btw, rather than discussing the ambigious, and/or poorly implemented by everyone, spec, maybe it would make more sense to just report the usability issue that pip can install this requirement, even without the prerelease flag, and uv cannot:

```
$ pip install --dry-run "azure-cli==2.46.0" "pydantic<2.0"
...
Would install Deprecated-1.2.14 PyGithub-1.59.1 PyJWT-2.8.0 PyMySQL-1.0.3 PyNaCl-1.5.0 PySocks-1.7.1 PyYAML-6.0.1 Pygments-2.17.2 adal-1.2.7 antlr4-python3-runtime-4.9.3 applicationinsights-0.11.10 argcomplete-2.1.2 azure-appconfiguration-1.1.1 azure-batch-13.0.0 azure-cli-2.46.0 azure-cli-core-2.46.0 azure-cli-telemetry-1.0.8 azure-common-1.1.28 azure-core-1.30.0 azure-cosmos-3.2.0 azure-data-tables-12.4.0 azure-datalake-store-0.0.53 azure-graphrbac-0.60.0 azure-keyvault-1.1.0 azure-keyvault-administration-4.0.0b3 azure-keyvault-keys-4.8.0b2 azure-loganalytics-0.1.1 azure-mgmt-advisor-9.0.0 azure-mgmt-apimanagement-3.0.0 azure-mgmt-appconfiguration-2.2.0 azure-mgmt-applicationinsights-1.0.0 azure-mgmt-authorization-3.0.0 azure-mgmt-batch-17.0.0 azure-mgmt-batchai-7.0.0b1 azure-mgmt-billing-6.0.0 azure-mgmt-botservice-2.0.0 azure-mgmt-cdn-12.0.0 azure-mgmt-cognitiveservices-13.3.0 azure-mgmt-compute-29.1.0 azure-mgmt-consumption-2.0.0 azure-mgmt-containerinstance-10.1.0 azure-mgmt-containerregistry-10.1.0 azure-mgmt-containerservice-21.2.0 azure-mgmt-core-1.4.0 azure-mgmt-cosmosdb-9.0.0 azure-mgmt-databoxedge-1.0.0 azure-mgmt-datalake-analytics-0.2.1 azure-mgmt-datalake-nspkg-3.0.1 azure-mgmt-datalake-store-0.5.0 azure-mgmt-datamigration-10.0.0 azure-mgmt-devtestlabs-4.0.0 azure-mgmt-dns-8.0.0 azure-mgmt-eventgrid-10.2.0b2 azure-mgmt-eventhub-10.1.0 azure-mgmt-extendedlocation-1.0.0b2 azure-mgmt-hdinsight-9.0.0 azure-mgmt-imagebuilder-1.1.0 azure-mgmt-iotcentral-10.0.0b2 azure-mgmt-iothub-2.3.0 azure-mgmt-iothubprovisioningservices-1.1.0 azure-mgmt-keyvault-10.1.0 azure-mgmt-kusto-0.3.0 azure-mgmt-loganalytics-13.0.0b4 azure-mgmt-managedservices-1.0.0 azure-mgmt-managementgroups-1.0.0 azure-mgmt-maps-2.0.0 azure-mgmt-marketplaceordering-1.1.0 azure-mgmt-media-9.0.0 azure-mgmt-monitor-5.0.1 azure-mgmt-msi-7.0.0 azure-mgmt-netapp-9.0.1 azure-mgmt-network-21.0.1 azure-mgmt-nspkg-3.0.2 azure-mgmt-policyinsights-1.1.0b4 azure-mgmt-privatedns-1.0.0 azure-mgmt-rdbms-10.2.0b14 azure-mgmt-recoveryservices-2.2.0 azure-mgmt-recoveryservicesbackup-5.1.0 azure-mgmt-redhatopenshift-1.2.0 azure-mgmt-redis-14.1.0 azure-mgmt-relay-0.1.0 azure-mgmt-resource-21.1.0b1 azure-mgmt-search-8.0.0 azure-mgmt-security-3.0.0 azure-mgmt-servicebus-8.2.0 azure-mgmt-servicefabric-1.0.0 azure-mgmt-servicefabricmanagedclusters-1.0.0 azure-mgmt-servicelinker-1.2.0b1 azure-mgmt-signalr-1.1.0 azure-mgmt-sql-4.0.0b8 azure-mgmt-sqlvirtualmachine-1.0.0b5 azure-mgmt-storage-21.0.0 azure-mgmt-synapse-2.1.0b5 azure-mgmt-trafficmanager-1.0.0 azure-mgmt-web-7.0.0 azure-multiapi-storage-1.0.0 azure-nspkg-3.0.2 azure-storage-common-1.4.2 azure-synapse-accesscontrol-0.5.0 azure-synapse-artifacts-0.15.0 azure-synapse-managedprivateendpoints-0.4.0 azure-synapse-spark-0.2.0 bcrypt-4.1.2 certifi-2024.2.2 cffi-1.16.0 chardet-3.0.4 charset-normalizer-3.3.2 colorama-0.4.6 cryptography-40.0.2 distro-1.9.0 fabric-2.7.1 humanfriendly-10.0 idna-3.6 invoke-1.7.3 isodate-0.6.1 javaproperties-0.5.2 jmespath-1.0.1 jsondiff-2.0.0 knack-0.10.1 msal-1.20.0 msal-extensions-1.0.0 msrest-0.7.1 msrestazure-0.6.4 oauthlib-3.2.2 packaging-23.2 paramiko-3.4.0 pathlib2-2.3.7.post1 pkginfo-1.9.6 portalocker-2.8.2 psutil-5.9.8 pyOpenSSL-23.2.0 pycparser-2.21 pydantic-1.10.14 python-dateutil-2.8.2 requests-2.31.0 requests-oauthlib-1.3.1 scp-0.13.6 semver-2.13.0 six-1.16.0 sshtunnel-0.1.5 tabulate-0.9.0 typing_extensions-4.9.0 urllib3-2.2.1 websocket-client-1.3.3 wrapt-1.16.0 xmltodict-0.13.0
```

```
$ printf "azure-cli==2.46.0\npydantic<2.0" | uv pip compile -
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of azure-mgmt-sql==4.0.0b8 and azure-cli==2.46.0 depends on azure-mgmt-sql==4.0.0b8, we can conclude that
      azure-cli==2.46.0 cannot be used.
      And because you require azure-cli==2.46.0, we can conclude that the requirements are unsatisfiable.

      hint: azure-mgmt-sql was requested with a pre-release marker (e.g., azure-mgmt-sql==4.0.0b8), but pre-releases weren't enabled (try:
      `--prerelease=allow`)
```

---

_Comment by @erdembanak-invent on 2024-02-19 13:27_

We can just say `uv pip install azure-cli==2.46.0` is not working while `pip install azure-cli==2.46.0` is working.

---

_Comment by @dimbleby on 2024-02-19 14:14_

So far as I know everyone _tries_ to follow the spec, of course some are more successful than others, and in different ways.  

All of the installers that I tried - pip / poetry / pdm - successfully install `azure-cli==2.46.0` without extra flags, and all of them agree that `2.0b3` does not satisfy `<2.0.0`.

100% agree that as a user experience this issue is best reported as `uv pip install azure-cli==2.46.0` does not succeed.  Referring to the spec is supposed to be a useful way of investigating where this is going wrong.

---

_Comment by @charliermarsh on 2024-02-19 14:26_

So, that `uv pip install azure-cli==2.46.0` does not succeed is known in our design and documented. The requirement we have is that we need to know which packages are "allowed" to have pre-releases in advance. Without this, _correctness_ becomes extremely difficult.

In the case above, we need to know that you're allowing pre-releases for `azure-mgmt-sql`. You can do this with:

```
uv pip install azure-cli==2.46.0 azure-mgmt-sql==4.0.0b8
```

In uv, the use of the pre-release marker in the _direct_ dependency tells uv to allow pre-releases for that dependency. You can see this in the hint provided above upon failure.

Unfortunately, tt seems like Azure publishes, like, everything as a pre-release?

---

_Comment by @erdembanak on 2024-02-19 14:31_

Just before I have seen your reply, I have created a separate issue for this; sorry @charliermarsh . Yes, this is the expected behavior from the documentation, just wanted to open a separate issue to make sure the discussion doesn't get lost in here; but you already replied. Thanks.

---

_Comment by @charliermarsh on 2024-02-19 14:32_

No prob. It's fair game to petition that we change the behavior, but those are the rules we settled on for now as-implemented, to make the problem tractable.

---

_Comment by @erdembanak on 2024-02-19 15:05_

It looks like to make sure I am not  installing something as prerelease when there is a release, I need to do something like:

`uv pip install azure-cli==2.46.0 azure-mgmt-sql==4.0.0b8 azure-keyvault-keys==4.8.0b2 azure-mgmt-synapse==2.1.0b5 azure-mgmt-servicelinker==1.2.0b1 azure-mgmt-batchai==7.0.0b1 azure-mgmt-extendedlocation==1.0.0b2 azure-mgmt-loganalytics==13.0.0b4 azure-mgmt-eventgrid==10.2.0b2 azure-mgmt-sqlvirtualmachine==1.0.0b5 azure-keyvault-administration==4.0.0b3 azure-mgmt-resource==21.1.0b1 azure-mgmt-rdbms==10.2.0b6 azure-mgmt-policyinsights==1.1.0b2 azure-mgmt-iotcentral==10.0.0b1`

(some of them were >= in the requirements but I converted to ==)

---

_Comment by @ofek on 2024-02-21 00:27_

> Poetry is the only tool I'm aware of that tries to faithfully follow it but it fails to when [backtracking](https://github.com/python-poetry/poetry/issues/4405)

That was fixed recently.

---

_Comment by @Marenz on 2024-02-21 13:07_

Same problem, different example:
```
python311 ❯ uv pip install --prerelease=allow "h3 >= 3, <4"
Resolved 1 package in 2ms
Installed 1 package in 17ms
 + h3==4.0.0b2
```

---

_Comment by @Czaki on 2024-02-21 13:47_

@charliermarsh couldnt you just interpret `package<n` as `package<n.a0`?

---

_Comment by @charliermarsh on 2024-02-22 03:31_

@Czaki -- Perhaps... I need to test it out. It may also have an adverse effect on error messages.

---

_Comment by @llucax on 2024-02-22 08:10_

It is a shame that unless this is fixed, `uv` can't really be used as a drop-in replacement for `pip`, as `pip` does the *right thing*.

---

_Comment by @llucax on 2024-02-22 08:22_

And if it were really the case that `pip` is implementing the specs wrongly (which I don't think is the case, actually not including a v4 pre-release if the version specifier is <4 is the only sensible thing to do and what the specs seem to say), and your goal is to be compatible with `pip`, then IMHO you should ignore the specs if they contradict what `pip` does **and** your goal is to be a drop-in replacement for `pip`. These seems contradictory:

[![image](https://github.com/astral-sh/uv/assets/1031485/9e2d611f-69f4-438d-877b-cb97282698d4)](https://github.com/astral-sh/uv?tab=readme-ov-file#uv)

[![image](https://github.com/astral-sh/uv/assets/1031485/3c9d66f4-5d9e-4932-a91d-40d09daa6bfa)](https://github.com/astral-sh/uv/issues/1641#issuecomment-1952561531)

---

_Comment by @mpizenberg on 2024-02-22 08:36_

> @Czaki -- Perhaps... I need to test it out. It may also have an adverse effect on error messages.

@charliermarsh could it be possible to store the actual original constraint in an incompat coming from dependencies? Maybe even store the package it's from? This way you could have the <4a0 constraint in the solver, but store the <4 and the package this is coming from for error messages.

---

_Comment by @charliermarsh on 2024-02-22 13:33_

@llucax - this feels like a misrepresentation of what’s been said in the thread and even a little antagonistic. The reference to “our design” is with respect to how users opt-in or out of pre-release handling. The fact that “<2.0.0” should not include “2.0.0b3” is acknowledged as a bug.

---

_Comment by @charliermarsh on 2024-02-22 17:07_

I'm gonna try to get the `package<n.a0` to work. Let's see how it plays out.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-22 17:07_

---

_Comment by @notatallshaw on 2024-02-22 17:38_

I would just like to restate my thanks again to @charliermarsh and the astral team for providing another excellent peice of open source software.

uv has been public for just 1 week, issues like this take time to find the "best" solution and come with maturity of a product as more users have time to use it and different use cases are discovered.

I think it would be important to take any information from here about the feasibility of implementing the spec and bring it back to the wider packaging community. I can assure others that pip is not following all parts of the prerelease spec (e.g. https://github.com/pypa/pip/issues/12470, https://github.com/pypa/pip/issues/12469, https://github.com/pypa/pip/issues/12471, https://github.com/pypa/packaging/issues/766). Once @charliermarsh has had time to see what is feasible for uv I plan to make a post on the Python discussion board detailing how various different installers implement/interpret the different parts of the prerelease spec.

> your goal is to be compatible with `pip`,

This brings up something I find genuinly unclear,  is that a stated goal of the uv project? The blog post says that uv aims for a compatible API, but does not talk about behavior, and actually there are a lot of differnces in the API, does this mean more that any feature in the pip API there should be some equivelent option in the uv pip API even if not called the same?

I would assume that it is a non-goal to have exact quirk/bug compatibility with pip? 

---

_Comment by @dimbleby on 2024-02-22 17:56_

> this feels like a misrepresentation of what’s been said in the thread and even a little antagonistic. The reference to “our design” is with respect to how users opt-in or out of pre-release handling. The fact that “<2.0.0” should not include “2.0.0b3” is acknowledged as a bug.

I think you may be talking past one another, at least somewhat.  

For better or worse, there are _two_ bugs discussed in this thread, and the ["our design" comment](https://github.com/astral-sh/uv/issues/1641#issuecomment-1952561531) was I think talking about the other one: namely that uv has chosen not to implement the part of the spec that requires a pre-release to be selected - without special flags - when that is the only way to satisfy the given constraint.

while I agree that the tone was "even a little antagonistic", I share some of the surprise!

---

_Comment by @charliermarsh on 2024-02-22 18:10_

Thanks fair, thanks @dimbleby!

> namely that uv has chosen not to implement the part of the spec that requires a pre-release to be chosen - without special flags - when that is the only way to satisfy the given constraint.

Yeah, it's correct that we don't do this. Ultimately I'm not yet sold on supporting it, in part because I think it's very hard to get completely right (I'd rather have something that is limited, but predictable and under user control), and in part because the spec is insufficient -- the spec is built around a world of satisfying a single version comparison, but dependency resolution isn't actually covered. Per [Paul Moore](https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/17):

> I’ve already mentioned on the pip issue that the spec doesn’t cover how things work when multiple specifiers need to be satisfied at once. That’s a spec issue.

Similarly, my understanding is that it definitively was _not_ the intention of the spec ([per one of its original authors](https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/20)) to support this kind of behavior (to the degree that it matters).


---

_Comment by @dimbleby on 2024-02-22 18:13_

fwiw if we can get into the business of rewriting specs I would be fully behind a proposal that abolished all of dev releases, alpha releases, beta releases, post releases, local releases, dev releases of post releases of local releases, and so on.  The whole thing is a minefield.

---

_Comment by @charliermarsh on 2024-02-22 18:14_

It's funny because it's something I just never thought about as a Python user.

---

_Comment by @Czaki on 2024-02-22 18:23_

> fwiw if we can get into the business of rewriting specs I would be fully behind a proposal that abolished all of dev releases, alpha releases, beta releases, post releases, local releases, dev releases of post releases of local releases, and so on. The whole thing is a minefield.

Having option of RC is really usefull when you are library maintainer. At first it is helpful to ask for tests most active part of users.

It also allow for automatic problem detection using `tox --pre -e some_env` cron job on CI.

---

_Comment by @notatallshaw on 2024-02-22 18:25_

> fwiw if we can get into the business of rewriting specs I would be fully behind a proposal that abolished all of dev releases, alpha releases, beta releases, post releases, local releases, dev releases of post releases of local releases, and so on. The whole thing is a minefield.

Epochs are what blew me away when I went through the spec recently.

It boggles the mind to think `1!1.1.1.a1.post1.dev1+local1` is a valid version number



---

_Comment by @mitsuhiko on 2024-02-22 19:45_

I can thankfully a bit impartial here. I love the existence of uv, but I have a bit of a distance to it as more of a user of it than as a developer.

I always expected uv to be _mostly pip_ compatible but not inherit necessarily every behavior of pip. And that not because pip is necessarily wrong, but because I absolutely loathe complexity and pip has a lot of it. If a 90% match can be accomplished with a lot less complexity I am a happy camper. Reducing complexity on the long run always results in a better user experience.

Without a doubt there are lots of packages on pypi today that probably require a different behavior than `uv` has today. However I would try to find a pragmatic solution and to adjust the spec, rather than clinging to either spec or pip.

</My 2 cents :)>

---

_Comment by @ofek on 2024-02-22 19:49_

I find it odd to consider changing the spec as a valid course of action when people are expressing need for that behavior in addition to the fact that the maintainers are saying that this is a known bug with which they are interested in accommodating. I have encountered this exact use case at work.

---

_Comment by @mitsuhiko on 2024-02-22 20:04_

It's not entirely odd because the creation of that very spec has over time changed the packaging ecosystem in a good way. But that has meant that some of the motivating elements for compatibility with the pre PEP 440 world are no longer quite as needed. See also the post by dstufft on it: https://discuss.python.org/t/handling-of-pre-releases-when-backtracking/40505/20

So I do think that it would be in the interest of the ecosystem to consider adjusting the spec if that results in better user experience and less complex systems.

---

_Comment by @erdembanak-invent on 2024-02-22 23:44_

As it had been said before; there are 2 discussions in here, and the original one is definitely a bug (installing 2.ob3 when user requested <2.0.0). The interesting discussion is about the second one, how to accommodate prereleases. I am still in favor of pip's behavior since the ecosystem is currently built around that. There are 2 points in my argument:
1 -uv is not a drop in replacement for pip since I needed to override some packages when I wanted to try it for production. I have to explicitly define the versions of packages which I have to install prereleases (due to some requirements). I need to list around 10 azure prereleases to use one package
3 - Current rule feels shaky. If I understood correctly, there are two conditions for using a prerelease version: "if the prerelease is explicity wanted or the library only has prerelease versions". In my current life, we have many internal libraries. Assume we have a library which requires the library xyz with the requirement files xyz = 0.1.btx. If library xyxz does not release a version, I'm OK with installing it as xyz = 0.1.btx with "uv". When they release the first version, uv will not be installing that version since there is a release version of the library. It means that, I won't be able to use a past version of my library with uv (since it depended on a prerelease version and there is a release now and I'm not allowing all prerelease versions). This means that older versions of my library which is depending on a prerelease version of another library won't be installed.

I would favor prohibiting prereleases (since it is a shaky concept); but currently using uv in production means adding too many libraries in a stricted way. As a regular user, I do not want to allow prereleases; but when there are many libraries OK with that, there is not much to do for me. Btw, I would favor a course of action which changes the specs, instead of talking about it in here.

---

_Closed by @charliermarsh on 2024-02-24 23:02_

---

_Comment by @charliermarsh on 2024-02-24 23:02_

The originating issue is fixed and will be out in the next release. (That is: `pydantic<2.0.0` will no longer allow `pydantic=2.0b3`.)

---

_Comment by @llucax on 2024-02-28 08:45_

> @llucax - this feels like a misrepresentation of what’s been said in the thread and even a little antagonistic. The reference to “our design” is with respect to how users opt-in or out of pre-release handling. The fact that “<2.0.0” should not include “2.0.0b3” is acknowledged as a bug.

Hi @charliermarsh, I'm sorry if it came across antagonistic and if I misinterpreted what's been said. Rereading some message I know see you acknowledged the bug but still said there was no intention of fixing it soon in https://github.com/astral-sh/uv/issues/1641#issuecomment-1951412679.

I just wanted to bring to your attention that if this was not addressed it sadly meant that the tool was not really a drop-in replacement of `pip` for the many cases where projects make pre-releases, and since this is not even in control of the user, it means it can't be reliable used ever, otherwise it might broke unpredictably if any dependency decides to make a pre-release.

In any case, thanks for addressing it so fast finally and for the great tool! :heart: 

---

_Comment by @Czaki on 2024-02-28 08:47_

> Hi @charliermarsh, I'm sorry if it came across antagonistic and if I misinterpreted what's been said. Rereading some message I know see you acknowledged the bug but still said there was no intention of fixing it soon in [#1641 (comment)](https://github.com/astral-sh/uv/issues/1641#issuecomment-1951412679).

Please test first. During the discussion, there was invented workaround for this problem and everything should work as expected. 

---

_Comment by @Marenz on 2024-03-06 12:11_

We tested the newest release and the behavior is def. better. 
However, it still differs and fails from `pip` when an indirect dependency is a prerelease:

```
uv pip install "frequenz-client-base @ git+https://github.com/frequenz-floss/frequenz-client-base-python.git@
57d3093" --reinstall
 Updated https://github.com/frequenz-floss/frequenz-client-base-python.git (57d3093)                                                                                            × No solution found when resolving dependencies:
  ╰─▶ Because only frequenz-channels<1.0.0b2 is available and frequenz-client-base==0.0.post31+g57d3093 depends on frequenz-channels>=1.0.0b2, we can conclude that
      frequenz-client-base==0.0.post31+g57d3093 cannot be used.
      And because only frequenz-client-base==0.0.post31+g57d3093 is available and you require frequenz-client-base, we can conclude that the requirements are unsatisfiable.

      hint: frequenz-channels was requested with a pre-release marker (e.g., frequenz-channels>=1.0.0b2), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

I had to use `--reinstall` otherwise it wouldn't say any error at all if I had it installed before, that was a bit surprising.

---

_Comment by @charliermarsh on 2024-03-06 13:35_

Thanks -- that's intentional and documented: pre-releases need to be enabled globally or on a per-package basis. Did you try enabling pre-releases per the error message?

---

_Comment by @Marenz on 2024-03-06 13:39_

Well, sure, with that flag it works...
With pip this works without any extra flag though and if the idea is still that it is a drop-in replacement for it, then this is preventing this unfortunately.

---

_Comment by @llucax on 2024-03-06 13:40_

Also it is not easy to pass extra flags when using other tools like `nox`, or is there any configuration file or env variable that could be used to pass options too?

---

_Comment by @llucax on 2024-03-06 13:49_

Also, if I understand correctly, using `--prerelease=allow` will select pre-releases whenever they are available, even if they are not used explicitly as part of the version requirements (like if an indirect dependency `x >= 2.0` in its requirements, and there is a `x` has 2 versions: `1.0.0` and `2.0.0-beta1` release, I don't want the pre-release to be selected, but if the indirect dependency were `x >= 2.0.0-beta1`, then it should work. This is what `pip` does, as far as my experience goes.

---

_Comment by @charliermarsh on 2024-03-06 13:52_

You're not required to use `--prerelease=allow` to enable prereleases for a package. If you add `x >= 2.0.0-beta1` as a direct dependency, it will enable pre-releases for `x`. You just need some way to signal to `uv` upfront (not as a transitive dependency) that prereleases should be enabled for `x`.

---

_Comment by @llucax on 2024-03-06 13:54_

That's not ideal either, my project doesn't use `x` explicitly, so if my direct dependency stops using `x`, or uses a different version, my dependency will be incorrect. That's exposing implementation details of dependencies I should not care about.

---

_Comment by @llucax on 2024-03-06 13:55_

What would be the issue with replicating `pip`'s behavior?

---

_Comment by @Marenz on 2024-03-06 13:58_

If this default behavior shouldn't be changed at all, it would be nice to have as alternative an environment variable `PIP_PRERELEASE_MODE` or whatever name fits best, that would switch on the same behavior that `pip` follows. 
That way we can also easily integrate it in build tools by just setting the ENV.

---

_Comment by @charliermarsh on 2024-03-06 14:12_

Yeah, we can add an environment variable. I'll take a look at it now.

---

_Comment by @charliermarsh on 2024-03-06 14:20_

> That's not ideal either, my project doesn't use x explicitly, so if my direct dependency stops using x, or uses a different version, my dependency will be incorrect. That's exposing implementation details of dependencies I should not care about.

For what it's worth, I think it's reasonable to ask you to care that your package relies on unreleased dependencies. I don't expect you to agree based on your comment, but it's a very reasonable position IMO.

> What would be the issue with replicating pip's behavior?

The core challenge is that you open the door to changing the "available versions" for a package over the course of resolution. And the spec does not cover "dependency resolution"; it's focused on evaluating a single version identifier, which is really different. Even in pip, there's an extensive discussion around what the "correct" behavior is, what the spec implies, and the bugs that pip has today in pre-release handling:

- https://github.com/pypa/pip/issues/12469
- https://github.com/pypa/pip/issues/12470
- https://github.com/pypa/pip/issues/12472


---

_Comment by @notatallshaw on 2024-03-06 15:42_

> > That's not ideal either, my project doesn't use x explicitly, so if my direct dependency stops using x, or uses a different version, my dependency will be incorrect. That's exposing implementation details of dependencies I should not care about.
> 
> For what it's worth, I think it's reasonable to ask you to care that your package relies on unreleased dependencies. I don't expect you to agree based on your comment, but it's a very reasonable position IMO.

A workaround to this is to pass in the constraint: `somepackage >= 0.0a0 `

This solves the problem that it does not force a direct dependency on this package and solves the problem that it lets uv know upfront to allow prereleases on this package.

---

_Comment by @notatallshaw on 2024-03-06 17:25_

I would also like to mention here pip's behavior is neither consistent nor intentional, the order of how pip compares prerelease requirements affects whether it will accept prereleases or not: https://github.com/pypa/pip/issues/12471

So for uv to match this behavior bug for bug, uv would need to:
1. Resolve dependencies and transitive dependencies in the exact same order as pip, which changes from version to version (and defeats much of the reason uv developed it's own resolver)
2. Implement pip's behavior of *sometimes* selecting prereleases based on the order it received them

Otherwise uv would forever either not select prerelease when pip does or selecting prereleases when pip doesn't.

---

_Comment by @llucax on 2024-03-07 20:31_

Hi Charlie, thanks a lot for your reply and time again.

> > That's not ideal either, my project doesn't use x explicitly, so if my direct dependency stops using x, or uses a different version, my dependency will be incorrect. That's exposing implementation details of dependencies I should not care about.
> 
> For what it's worth, I think it's reasonable to ask you to care that your package relies on unreleased dependencies. I don't expect you to agree based on your comment, but it's a very reasonable position IMO.

Yeah, we strongly disagree here. I can give more arguments why I think this is not reasonable, I'm guessing you already thought about those and I will not say anything new to you, but just in case:

I can't see any justification in why should I care at all about which dependencies my dependencies use as long as they are not direct dependencies to my project too, those being pre-releases or not. Once I chose a dependency, I decided to trust it, after all I'm executing *arbitrary code* from them. Why should I care if the maintainer of one of those dependencies decided the best course of action is to use a pre-release in their dependencies? How would I be better than them to judge if I might not even know what those dependencies are for or follow their development? I will just be copy&pasting dependencies to my project's dependencies. I don't see a point in doing that.

This also adds a lot of maintenance cost for me, as now I need to track all transitional dependencies and litter my direct dependencies. Not only that, now how my dependencies evolve is not transparent to me anymore, as if they change those dependencies I need to update mine or risk my project to be broken or use outdated dependencies.

Is really hard for me to see any advantages in the approach you are proposing.

> > What would be the issue with replicating pip's behavior?
> 
> The core challenge is that you open the door to changing the "available versions" for a package over the course of resolution. And the spec does not cover "dependency resolution"; it's focused on evaluating a single version identifier, which is really different. Even in pip, there's an extensive discussion around what the "correct" behavior is, what the spec implies, and the bugs that pip has today in pre-release handling:
> 
> * [Pre-release not selected when remotely available pre-releases for version specifiers is the only versions that satisfies the version specifier pypa/pip#12469](https://github.com/pypa/pip/issues/12469)
> * [How do I exclude Pip selecting any pre-release? pypa/pip#12470](https://github.com/pypa/pip/issues/12470)
> * [Pip inconsistent on whether it looks up a pre-release based on the operator of the specifier pypa/pip#12472](https://github.com/pypa/pip/issues/12472)

OK, I understand `pip` has bugs too, and there might be cases where if something is a bug or not might even be debatable, or there might be cases when one behavior is preferred to other.

I actually think it is very nice that `uv` gives more control over how pre-releases could be selected, that's awesome. My point here is if the idea is that `uv` is a drop-in replacement, one of these modes should be "be like pip". Otherwise it is not.

And I guess this is the part I don't understand very well and find a bit contradictory. I could totally understand if you said "oh, yes, we are not doing the same as `pip` here but we won't fix it because it's too hard and we have other more important stuff to work on", that I can totally understand, but my impression is, at least for this issue in particular, that this is not considered an issue in `uv` and thus there is no intention to fix it (where "fix it" doesn't mean is the right behavior nor is following the specs, "fix it" means behave like `pip`).

If I'm correct about this, which also I think is totally fine, is your tool and I'm still very grateful you open source it and are sharing it with the world, I just think you should probably not say it is a drop-in replacement for `pip` because this is a bit misleading, and can cause frustration and waste users time, as well as yours, and even maybe even `pip` maintainers time, as they will inevitably have to deal with the differences if users start thinking they are interchangeable.

The main reason why we decided to try `uv` is because the first highlight in the README says "⚖️ Drop-in replacement for common pip, pip-tools, and virtualenv commands.". We can't do the jump and say our projects only work with `uv` and don't care if it doesn't work with `pip`, we need to be compatible with `pip`. While writing this I just realized there is a new document highlighting the differences with `pip`, so thanks for that too, I guess it will help to clarify and set more realistic expectations.

---

_Comment by @notatallshaw on 2024-03-07 21:16_

I can't speak for astral or their communication, but given you replied to me, I will point out by your definition of drop in replacement then new versions of pip are not a drop in replacement for old versions of pip.

For example pip 20.2 can install plenty of things because of it's limited resolver compared to pip 20.3, equally the resolver behavior has changed significantly between 20.3 and 24.0, meaning different requirements will 1) Resolve or not resolve depending on what pip version you have, and 2) Produce different solutions.

I think you are vastly underestimating the challenges of package resolution, from what I've seen so far uv works in all but unusual edge cases and is usually multiple times faster, that's pretty amazing and will save a lot of devs a lot of time over the long run.

Anyway, I tested my workaround which solves not needing `--prerelease=allow` and not adding a direct dependency on to a transitive dependency.

Here is the failing example

```
$ echo "frequenz-client-base @ git+https://github.com/frequenz-floss/frequenz-client-base-python.git@57d3093" | uv pip compile -
 Updated https://github.com/frequenz-floss/frequenz-client-base-python.git (57d3093)
  × No solution found when resolving dependencies:
  ╰─▶ Because only frequenz-channels<1.0.0b2 is available and frequenz-client-base==0.1.0.post21+g57d3093 depends on frequenz-channels>=1.0.0b2, we can conclude
  	that frequenz-client-base==0.1.0.post21+g57d3093 cannot be used.
  	And because only frequenz-client-base==0.1.0.post21+g57d3093 is available and you require frequenz-client-base, we can conclude that the requirements are
  	unsatisfiable.

  	hint: frequenz-channels was requested with a pre-release marker (e.g., frequenz-channels>=1.0.0b2), but pre-releases weren't enabled (try:
  	`--prerelease=allow`)
```

But by adding the constraint with a prerelease marker then one can mark this specific package as allowing prerelease without adding it as a requirement to install:

```
$ echo "frequenz-channels >= 0.0a0" > constraint.txt
$ echo "frequenz-client-base @ git+https://github.com/frequenz-floss/frequenz-client-base-python.git@57d3093" | uv pip compile - --constraint constraint.txt
 Updated https://github.com/frequenz-floss/frequenz-client-base-python.git (57d3093
Resolved 11 packages in 361ms
# This file was autogenerated by uv via the following command:
#	uv pip compile - --constraint constraint.txt
anyio==4.3.0
	# via watchfiles
frequenz-channels==1.0.0rc1
	# via frequenz-client-base
frequenz-client-base @ git+https://github.com/frequenz-floss/frequenz-client-base-python.git@57d3093640f2def2b976a738fcbb9605a02e3576
grpcio==1.62.0
	# via
	#   frequenz-client-base
	#   grpcio-tools
grpcio-tools==1.62.0
	# via frequenz-client-base
idna==3.6
	# via anyio
protobuf==4.25.3
	# via grpcio-tools
setuptools==69.1.1
	# via grpcio-tools
sniffio==1.3.1
	# via anyio
typing-extensions==4.10.0
	# via
	#   frequenz-channels
	#   frequenz-client-base
watchfiles==0.21.0
	# via frequenz-channels
```




---

_Comment by @llucax on 2024-03-08 07:50_

> I think you are vastly underestimating the challenges of package resolution, 

Trying not to repeat myself too much here. I do understand it is a hard problem, and again if the reason not to allow this case is "it's too hard", that is totally reasonable for me. But it doesn't seem to be the case here.

> from what I've seen so far uv works in all but unusual edge cases and is usually multiple times faster, that's pretty amazing and will save a lot of devs a lot of time over the long run.

I also agree that it is unrealistic to pretend that uv behaves like pip in every obscure and niche corner case, but I honestly don't think this case (an indirect dependency using explicitly a pre-release in their dependencies) is an obscure corner case, it seems like a pretty reasonable case.

> For example pip 20.2 can install plenty of things because of it's limited resolver compared to pip 20.3, equally the resolver behavior has changed significantly between 20.3 and 24.0, meaning different requirements will 1) Resolve or not resolve depending on what pip version you have, and 2) Produce different solutions.

This for me falls into the basket of "pip has bugs too". There is also another difference, following breaking changes in pip is just advancing in time. We can say "I'm compatible with pip 24.0 or newer", but if uv diverges from pip, you are creating a fork. The problem is not leniar anymore.

> But by adding the constraint with a prerelease marker then one can mark this specific package as allowing prerelease without adding it as a requirement to install:

Thanks, but this workaround still requires manual intervention every time a dependency starts or stops using a pre-release (if I don't "disable" accepting pre-release for that dependency after my dependencies stopped using them, then I will still accept pre-releases for it even when it is not **necessary** for my project, which I don't want to allow), which is an unacceptable maintenance burden for me (and it also requires passing more options to UV, which complicate integration with other tools).

---

_Comment by @notatallshaw on 2024-03-08 13:50_

> This for me falls into the basket of "pip has bugs too".

No, it's just different choices about how to resolve, and there isn't a spec on this so there's nothing to test it against being a bug or not. (FYI, I hope we can make at least some limited progress on this by sharing tests between Python installers using packse that astral have kindly published).

In general anyway, there are actual bugs when a solution can't be found, or is found, but there does exist a solution, or doesn't, under the design resolver choices made.

> There is also another difference, following breaking changes in pip is just advancing in time. We can say "I'm compatible with pip 24.0 or newer", but if uv diverges from pip, you are creating a fork. The problem is not leniar anymore.

Pip makes no guarantee of any consistency of it's resolution from one version to the next. So to say you're compatible with exact resolution choices on more than a single version would be impossible, as pip can't say that for itself.

FYI, there is now a pip compatibility page in uv's documentation that explains the known differences. I will remind people, this project is less than a month publicly announced, identifying issues against a broader ecosystem takes time, and to expect it upfront before there's been a chance to encounter these issues is unreasonable.

> Thanks, but this workaround still requires manual intervention every time a dependency starts or stops using a pre-release

Yeah, that's the trade off of this workaround compared to allowing all prereleaes. But solves both previously raised concerns about other solutions.

And for me this is a feature, I would prefer to not have unexpected prereleases in my resolution solution unless I specifically request them.


---

_Comment by @llucax on 2024-03-09 11:45_

Thanks for your reply and detailed explanation. Once more I totally understand this is a very new project and again I'm super grateful that it is published and it's great. I can also understand there is only a limited amount of resources and not everything can get implemented. My point still stands that it is a shame that is not compatible with pip in what I think is a reasonable use case (but apparently you don't) where pip does the right thing.

I can understand the current approach is a feature for you but it is a bug for other users (at least one, me 😀), so it would be great if you can add a pre-release resolution mode that supports it, and that will make the tool a bit more compatible with pip.

Anyway I don't want to waste more of your time, I think I said everything I had to say to try to convince you there is value in supporting this user care so I'll stop now.

Thanks again for the great tool and for taking the time to reply to my comments!

---

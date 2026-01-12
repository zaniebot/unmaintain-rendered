```yaml
number: 2191
title: Can not get .gitlab-ci.yml to be searched
type: issue
state: closed
author: lexicalunit
labels:
  - invalid
assignees: []
created_at: 2022-04-22T15:17:27Z
updated_at: 2022-04-22T15:27:33Z
url: https://github.com/BurntSushi/ripgrep/issues/2191
synced_at: 2026-01-12T16:13:24Z
```

# Can not get .gitlab-ci.yml to be searched

---

_@lexicalunit_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

`brew` on macOS.

#### What operating system are you using ripgrep on?

macOS 10.16

#### Describe your bug.

No matter what I do rg will not return hits for anything in `.gitlab-ci.yml`.

#### What are the steps to reproduce the behavior?

```shell
cat .gitlab-ci.yml | grep collectstatic
    - python manage.py collectstatic --noinput
```

```
rg --no-hidden collectstatic
onboard/utils/management/commands/generate_zappa_settings.py
29:            # these files will be deployed separately via `collectstatic`).
```

```shell
rg collectstatic
onboard/utils/management/commands/generate_zappa_settings.py
29:            # these files will be deployed separately via `collectstatic`).
```

```shell
rg -iglob ".gitlab-ci.yml" collectstatic
collectstatic: No such file or directory (os error 2)
```

```shell
± rg --no-ignore collectstatic
env/lib/python3.8/site-packages/django/contrib/staticfiles/testing.py
9:    means you don't need to run collectstatic before or as a part of your tests

env/lib/python3.8/site-packages/django/contrib/staticfiles/storage.py
205:        Post process the given dictionary of files (called from collectstatic).

env/lib/python3.8/site-packages/django/contrib/staticfiles/management/commands/collectstatic.py
92:        Perform the bulk of the work of collectstatic.
273:                        # the previous collectstatic wasn't with --link) or if
275:                        # previous collectstatic was with --link), the old

env/lib/python3.8/site-packages/Django-3.1.13.dist-info/RECORD
3571:django/contrib/staticfiles/management/commands/__pycache__/collectstatic.cpython-38.pyc,,
3574:django/contrib/staticfiles/management/commands/collectstatic.py,sha256=OF-wPu9FvkHZPvfu1PHEy9yghcKwxKsoqpimqEacmzM,15136

env/lib/python3.8/site-packages/webpack_loader/signals.py
1:# will hook into collectstatic

env/lib/python3.8/site-packages/django_stubs-1.5.0.dist-info/RECORD
165:django-stubs/contrib/staticfiles/management/commands/collectstatic.pyi,sha256=aauFD0KDUGYrHRVy-El3hUVhCkTXtWty213H0YRRjHc,1125

env/lib/python3.8/site-packages/django_webpack_loader-1.4.1.dist-info/METADATA
438:**It is up to you**. There are a few ways to handle this. I like to have slightly separate configs for production and local. I tell git to ignore my local stats + bundle file but track the ones for production. Before pushing out newer version to production, I generate a new bundle using production config and commit the new stats file and bundle. I store the stats file and bundles in a directory that is added to the `STATICFILES_DIR`. This gives me integration with collectstatic for free. The generated bundles are automatically collected to the target directory and synched to S3.
469:You can also simply generate the bundles on the server before running collectstatic if that works for you.

env/lib/python3.8/site-packages/django_storages-1.10.dist-info/METADATA
351:  ``collectstatic``. (`#382`_, `#754`_)
636:* Fix ``collectstatic`` timezone handling in and add ``get_modified_time`` to ``S3BotoStorage`` (`#290`_)
747:  this prevents a crash when running ``collectstatic -c`` on Django 1.9.1 (`#112`_) fixed in `#116`_
877:* Fixes `#156`_ regarding date parsing, ValueError when running collectstatic

onboard/utils/management/commands/generate_zappa_settings.py
29:            # these files will be deployed separately via `collectstatic`).
```

#### What is the actual behavior?

If I use the `--debug` flag in addition to the `--no-ignore` flag I see this in the output:

```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.gitlab-ci.yml: Ignore(IgnoreMatch(Hidden))
```

#### What is the expected behavior?

The `--no-ignore` flag should disable ignores. The `--no-hidden` flag doesn't seem to work. I can get it to work with `-uu` but I want my ignores to work! Using `-uu` causes `rg` to return a bunch of stuff that _should_ be ignored. I just want `rg` to search my local files that start with `.`. That seems to be impossible?


---

_Comment by @lexicalunit on 2022-04-22 15:18_

`rg --hidden collectstatic` works. I assumed `--no-hidden` would turn off the "ignore hidden" setting just like `--no-ignore` turns off the "ignore" setting. Confusing.

---

_Closed by @lexicalunit on 2022-04-22 15:18_

---

_Label `invalid` added by @BurntSushi on 2022-04-22 15:27_

---

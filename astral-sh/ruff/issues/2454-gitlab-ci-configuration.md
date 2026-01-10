```yaml
number: 2454
title: Gitlab ci configuration
type: issue
state: closed
author: arist0v
labels: []
assignees: []
created_at: 2023-02-01T19:46:40Z
updated_at: 2023-02-06T13:15:45Z
url: https://github.com/astral-sh/ruff/issues/2454
synced_at: 2026-01-10T11:09:45Z
```

# Gitlab ci configuration

---

_Issue opened by @arist0v on 2023-02-01 19:46_

Hello, i try settings up ruff for Gitlab Ci code quality, i checked up lot of google result, but still can'T figure out how to make it work

my gitlab-ci.yml:
```
stages:
  - test

include:
  - template: Code-Quality.gitlab-ci.yml

Testing:
  stage: test

  rules:
    - if: "$CI_PIPELINE_SOURCE == 'merge_request_event'"
    - if: "$CI_COMMIT_TAG =~ /^v(0|[1-9]\\d*)\\.(0|[1-9]\\d*)\\.(0|[1-9]\\d*)(-alpha\\d*|-debug\\d*|-beta\\d*|-rc\\d*|)$/"

  image: "python:3.10"

  before_script:
    - 'pip3 install pytest pytest-asyncio pytest-automock pytest-cov pytest-html-reporter ruff junit2html'

  script:
    - pytest -m GCI -rap --junitxml=gci_test_report.xml
    - ruff ./ --format gitlab > gl-code-quality-report.json


  artifacts:
    name: "${CI_PROJECT_NAME}_tests_reports"
    when: always
    reports:
      codequality: gl-code-quality-report.json
      junit: gci_test_report.xml
    expire_in: never

```

the result:
![image](https://user-images.githubusercontent.com/6615040/216147329-7e435d61-6a05-45d6-8316-3da06c224f14.png)

any tips or information on how i could make it work (also a section in the documentation for github/gitlab integration could be really nice)

---

_Comment by @charliermarsh on 2023-02-01 21:09_

I've never used Gitlab, but there might be some helpful comments in the original PR? See: #1424.

@saadmk11 may be able to answer a question about it too, if he's available.


---

_Comment by @arist0v on 2023-02-01 21:11_

Yeah i already tested that without success :(

Le mer. 1 févr. 2023, 16 h 09, Charlie Marsh ***@***.***> a
écrit :

> I've never used Gitlab, but there might be some helpful comments in the
> original PR? See: #1424 <https://github.com/charliermarsh/ruff/pull/1424>.
>
> @saadmk11 <https://github.com/saadmk11> may be able to answer a question
> about it too, if he's available.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/charliermarsh/ruff/issues/2454#issuecomment-1412725331>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ABSPAADEJR7ORFLIBAPRVT3WVLGHZANCNFSM6AAAAAAUOERPGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @saadmk11 on 2023-02-01 21:14_


When I implemented this, I used this config to test it on GitLab CI:

```yaml
include:
  - template: Code-Quality.gitlab-ci.yml


code_quality:
  image: python:3.10-slim
  allow_failure: false
  script:
    - pip install ruff
    - ruff . --format gitlab > gl-code-quality-report.json
  artifacts:
    reports:
      codequality: gl-code-quality-report.json
```


Does only using this work?

---

_Comment by @arist0v on 2023-02-01 21:55_

Is it On pipeline or on autodevops?

Le mer. 1 févr. 2023, 16 h 14, Maksudul Haque ***@***.***> a
écrit :

> When I implemented this, I used this config to test it on GitLab CI:
>
> include:
>   - template: Code-Quality.gitlab-ci.yml
>
> code_quality:
>   image: python:3.10-slim
>   allow_failure: false
>   script:
>     - pip install ruff
>     - ruff . --format gitlab > gl-code-quality-report.json
>   artifacts:
>     reports:
>       codequality: gl-code-quality-report.json
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/charliermarsh/ruff/issues/2454#issuecomment-1412731806>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ABSPAACQLQLKTS3XIAQT4NLWVLG2JANCNFSM6AAAAAAUOERPGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @arist0v on 2023-02-02 01:22_

i copy/pasted the @saadmk11 code example, 

i tested with and without the include

and i still have the same error message (gitlab 14)

---

_Comment by @arist0v on 2023-02-02 01:25_

there is the code i tested

```
include:
  - template: Code-Quality.gitlab-ci.yml


code_quality:
  image: andrejreznik/python-gdal:py3.10.0-gdal3.2.3
  allow_failure: false
  script:
    - pip install ruff
    - ruff . --format gitlab > gl-code-quality-report.json
  artifacts:
    reports:
      codequality: gl-code-quality-report.json
```

---

_Comment by @arist0v on 2023-02-02 01:56_

so i don'T know why, after few try and stop it's now working.

---

_Closed by @arist0v on 2023-02-02 01:56_

---

_Comment by @charliermarsh on 2023-02-02 01:58_

Glad it's working! Sorry I couldn't be more helpful!

---

_Comment by @arist0v on 2023-02-02 02:08_

no worry, my ci is simple but complex, it's only an include with override. so we create standard ci pipeline for all project

---

_Reopened by @arist0v on 2023-02-02 15:21_

---

_Comment by @arist0v on 2023-02-02 15:22_

so, new branch, new merge request, same issue, so back to the beginning

edit:
if my branch was already merged once, and i still work on the same branch, then i will have a base pipeline report and it will work,  this beahvior is dumb, it's like if i have to create a new branch to work, then create a first MR and merge it so only THEN i could have the codequality working,

maybe i have to set a rule to run the coverage on branch creation instead of on merge request only???

---

_Comment by @saadmk11 on 2023-02-02 15:42_

You can checkout the troubleshooting section of GitLab Code Quality Docs: https://docs.gitlab.com/ee/ci/testing/code_quality.html#troubleshooting

---

_Comment by @arist0v on 2023-02-02 15:51_

Ok so look like it'S working now, 

there is a small recap in case you would like to documente the step in the official documentation:
```
# could be any name if you don't use the template (the template is not required for the process to work)
code_quality:
  stage: test
 # not sure if you could use a different stage

   rules:
    - if: "$CI_PIPELINE_SOURCE == 'merge_request_event'"
    # run pipeline on merge request event (mr open or update)
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS
      when: never
    # don'T run branch pipeline if a MR is open(avoid double run)
    - if: "$CI_PIPELINE_SOURCE == 'push' && $CI_COMMIT_BRANCH != $CI_DEFAULT_BRANCH"
    # run test on push (allow to have the basepipeline coverage to compare to
    # also don't run if in the default branch(avoir running the pipeline after merge completed


  image: "python:3.10"

  before_script:
    - 'pip3 install ruff'
    # don't forger to install ruff 

  script:
    - ruff ./ --format gitlab > gl-code-quality-report.json
    # redirect output to a json file(could be any name)


  artifacts:
    name: "${CI_PROJECT_NAME}_code_quality" 
    #artifact could have any name
    when: always
    # i want the artifact to be created even if test failed
    reports:
      # the artifact is a reports object
      codequality: gl-code-quality-report.json
      # artifact is a codequality artifact this must be your json name (any name)
      
    expire_in: never

```

inform me if you need more details

---

_Comment by @charliermarsh on 2023-02-02 15:58_

Awesome, thanks for following up!

---

_Closed by @charliermarsh on 2023-02-02 15:58_

---

_Comment by @arist0v on 2023-02-02 16:00_

i still have a fix to do, the pipeline run on main after i merged, i want to prevent that to happend, i update my post in few min with the final answer


---

_Comment by @arist0v on 2023-02-02 16:40_

OK i think i have it all fixed 

---

_Comment by @arist0v on 2023-02-06 13:15_

So new update to my ci, and now i can confirm it's working : 

```
stages:
  - code_quality

code_quality:
  stage: code_quality

  rules:
    - if: '$CODE_QUALITY_DISABLED'# don'T run if codequality disabled
      when: never
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS # don'T run on branch if MR is open
      when: never
    - if: $CI_PIPELINE_SOURCE == 'merge_request_event' # RUN IN MERGE REQUEST PIPELINE
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH # RUN QUALITY JOB IN PIPELINE ON THE DEFAULT BRANCH(BUT NOT IN OTHER BRANCH)
    - if: $CI_COMMIT_TAG # RUN IN PIPELINE FOR TAGS
    
 
  image: your_ci/image:version # CHANGE THIS TO YOUR CI IMAGE

  script:
    - ruff ./ --exclude conftest.py --format gitlab > ruff_report.json

  artifacts:
    name: "${CI_PROJECT_NAME}_code_quality"
    when: always
    reports:
      codequality: ruff_report.json
      
    expire_in: never

```

this was the important missing line : 
```
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH # RUN QUALITY JOB IN PIPELINE ON THE DEFAULT BRANCH(BUT NOT IN OTHER BRANCH)
```
this make ruff run against the main branch when you commit, so you have main version to compare to


also on my side i don't wan't to push if the code_quality ruff check failed, if you want you need to allow failure by adding:
```
allow_failure: true
```

---

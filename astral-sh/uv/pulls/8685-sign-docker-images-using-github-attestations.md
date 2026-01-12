```yaml
number: 8685
title: Sign docker images using github attestations
type: pull_request
state: merged
author: mjpieters
labels:
  - security
  - releases
assignees: []
merged: true
base: main
head: cosign-docker-images
created_at: 2024-10-29T20:41:46Z
updated_at: 2025-01-31T19:16:03Z
url: https://github.com/astral-sh/uv/pull/8685
synced_at: 2026-01-12T16:08:27Z
```

# Sign docker images using github attestations

---

_@mjpieters_

GitHub attestation signing uses the GitHub action ID token to retrieve an ephemeral code signing certificate from Fulcio, and store the signature in the Rekor transparency log.

Once an image has been successfully signed, you should be able to verify the signature with:

```sh
gh attestation verify --owner astral-uv oci://ghcr.io/astral-sh/uv:latest
```

Closes #8670


---

_Comment by @mjpieters on 2024-10-29 20:44_

A word of warning: I am not 100% certain I fully understand everything that the `build-docker.yml` pipeline does. I'd appreciate some feedback on how the digest of the final `docker-republish` image manifest is shared with the signing step, for example.

---

_Comment by @zanieb on 2024-10-29 23:20_

cc @samypr100 (as always, no obligation)

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:337 on 2024-10-29 23:24_

What does this last sentence mean? I don't quite follow.

---

_@zanieb reviewed on 2024-10-29 23:24_

---

_@zanieb reviewed on 2024-10-29 23:26_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:351 on 2024-10-29 23:26_

Do we need to sign again here? Is it correct to sign twice?

ref https://github.com/astral-sh/uv/pull/8685/files#diff-1d203d2dfb96ccf94b5e0961c7954e3bde73b4539ade27ccc301613e368b944fR274-R276

---

_@samypr100 reviewed on 2024-10-30 01:04_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:369 on 2024-10-30 01:04_

I believe this can be done in a separate step instead to keep this step less-convoluted

---

_@samypr100 reviewed on 2024-10-30 01:05_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:337 on 2024-10-30 01:05_

Might be useful to explain, in a separate step, what the command does (e.g. outputs) and clarify that the issue is that `docker buildx imagetools create` doesn't return a list of updated digests.

---

_@samypr100 reviewed on 2024-10-30 01:06_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:351 on 2024-10-30 01:06_

I think we should clarify (maybe the title) that this signs the `uv:latest` images, where as the other one signs the "extra images" (e.g. python based etc.)

---

_@mjpieters reviewed on 2024-10-30 09:40_

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:369 on 2024-10-30 09:40_

I was specifically avoiding a separate step, because that would pose a security risk.

In theory, given enough time, a docker image tag could have been moved from one digest to another. A signature on a manifest from a trusted entity has enormous value, and if an attacker found a way to _very quickly_ move the tag to a digest of their choice, before the signing step was being executed, they could steal that signature. This is why it is a reasonably big deal that `imagetools create` doesn't output the digest in a machine-readable form the moment it creates the manifest. However, since `imagetools inspect` is run within the same step, it is being executed against the same docker installation which already has the tag and manifest locally cached and so changing the tag would require the attacker to have access to the runner while this step is being executed.

If we move this part to a new step, then you introduce many more opportunities for the tag to be moved; there is a time gap, the job is likely to be handled by a different runner which will have to fetch the digest from the container registry, etc. This greatly increases the attack surface to get that tag moved to a different digest. **I really don't want to increase the attack surface here**.

I'll see about updating the comments here to make this clearer.

---

_@mjpieters reviewed on 2024-10-30 09:48_

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:351 on 2024-10-30 09:48_

Exactly; this workflow outputs more than one type of docker image. There is the plain uv image (tagged as `:[MAJOR].[MINOR].[PATCH]`, with the `:[MAJOR].[MINOR]` and `:latest` tags being re-targeted to these), and then there are the various OS / Python version combination images, which take a base image from the `image-mapping` list and  copy in the `uv` and `uvx` commands. You need to sign each of these images separately.

The combination images are being signed in the `docker-publish-extra` job, and this signing step is for the plain image, re-published after the combination images have been generated with extra annotations, to make sure it is listed at the top on the container registry page.

---

_@mjpieters reviewed on 2024-10-30 10:20_

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:337 on 2024-10-30 10:20_

I've expanded the comment, explaining why it is being done in the same step, what the digest is needed for and what each command does.

---

_@samypr100 reviewed on 2024-10-30 12:19_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:369 on 2024-10-30 12:19_

> the job is likely to be handled by a different runner which will have to fetch the digest from the container registry

Thanks for the explanation. In this particular case steps are still guaranteed to be executed within the same runner vm and job executor, hence separating into a separate step is still worthwhile for readability. It doesn't give us much of a difference in terms of security in this case and more importantly it does not increase the attack surface. We'd honestly have bigger issues to worry about if we can have an attacker inject themselves into the runner vm and process executing the steps. I think we can reconsider this once `imagetools create` has a mechanism to return the digests.

> Within the same step, we can count on the local docker cache of the tag not having changed.

This is also true for steps within the job itself. So we should update this comment.

---

_@mjpieters reviewed on 2024-10-30 12:26_

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:369 on 2024-10-30 12:26_

> In this particular case steps are still guaranteed to be executed within the same runner vm and job executor

🤦 Of course they are, duh. Yeah, you are correct, and I'll make this a separate step. I was overthinking this.

---

_@mjpieters reviewed on 2024-10-30 12:29_

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:369 on 2024-10-30 12:29_

Change pushed.

---

_@samypr100 reviewed on 2024-10-30 12:36_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:369 on 2024-10-30 12:36_

Thanks! Looks great

---

_Comment by @samypr100 on 2024-10-30 12:51_

Note, I'm running a release job on my fork with [these changes (9b162d)](https://github.com/astral-sh/uv/pull/8685/commits/9b162d3e3cdf2b0fd86a2fcc668cf88d36461f9a) as I'd like to see it working E2E

https://github.com/samypr100/uv/actions/runs/11593512252

---

_Comment by @samypr100 on 2024-10-30 13:09_

@mjpieters 

> Note, I'm running a release job on my fork with [these changes (9b162d)](https://github.com/astral-sh/uv/pull/8685/commits/9b162d3e3cdf2b0fd86a2fcc668cf88d36461f9a) as I'd like to see it working E2E
> 
> https://github.com/samypr100/uv/actions/runs/11593512252

@mjpieters signed images are here https://github.com/samypr100/uv/pkgs/container/uv in case you'd like to verify the signatures are working as expected

---

_Comment by @mjpieters on 2024-10-30 13:55_

> @mjpieters
> 
> > Note, I'm running a release job on my fork with [these changes (9b162d)](https://github.com/astral-sh/uv/pull/8685/commits/9b162d3e3cdf2b0fd86a2fcc668cf88d36461f9a) as I'd like to see it working E2E
> > https://github.com/samypr100/uv/actions/runs/11593512252
> 
> @mjpieters signed images are here https://github.com/samypr100/uv/pkgs/container/uv in case you'd like to verify the signatures are working as expected

Excellent! I was working my way through understanding cargo dist in my own fork to try this out but hadn't gotten as far as actually seeing the workflow complete.

The first thing I notice is that the the .sig files are now obscuring the actual docker images on that page, which is a bummer.

The signatures are valid however!

```
% cosign verify ghcr.io/samypr100/uv:latest --certificate-identity-regexp='.*' --certificate-oidc-issuer-regexp='.*' --output-file /tmp/samypr100-uv-verify.json

Verification for ghcr.io/samypr100/uv:latest --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - Existence of the claims in the transparency log was verified offline
  - The code-signing certificate was verified using trusted certificate authority certificates

% jq -r '.[].optional.Subject' /tmp/samypr100-uv-verify.json
https://github.com/samypr100/uv/.github/workflows/build-docker.yml@refs/heads/docker-test
https://github.com/samypr100/uv/.github/workflows/build-docker.yml@refs/heads/docker-test
https://github.com/samypr100/uv/.github/workflows/build-docker.yml@refs/heads/docker-test
```

I'll have to see what can be done about those signature files in the registry.

---

_Comment by @mjpieters on 2024-10-30 15:42_

So cosign pushes the signature as a new 'container' to the registry, tagged with `sha256-$DIGEST.sig`, with the signature contained in the manifest (see https://hackmd.io/@maelvls/how-cosign-uses-registries-to-store-signatures for an overview). It's these tags that are shown at the top of the page by ghcr.io, because it only ever shows the latest tags that have been pushed. The GH packages API and UI (which include the ghcr container registry) is very very sparse and doesn't offer any control over this.

I've looked at a number of other projects that use cosign to sign their containers (of which there are a very large number), and all the ones I looked at so far show these tags at the top. E.g. the Dependabot update bundler (a Github project!) [has signature tags at the top](https://github.com/dependabot/dependabot-core/pkgs/container/dependabot-updater-bundler). You see this on other registries too, e.g. the [boxyhq jackson container on the docker hub](https://github.com/dependabot/dependabot-core/pkgs/container/dependabot-updater-bundler).

I see but a few options here:

- Accept that the signature tags are going to be pushed last and end up on top.
- Experiment with the Github [packages API](https://docs.github.com/en/rest/packages/packages?apiVersion=2022-11-28) to delete and then restore specific tags, to see if that puts them back at the top again.
- Tag the containers after signing. Build and push the images, store the digests, sign the digests, then handle the version and 'latest' tags afterwards.
- Tell cosign to [push signatures to a separate registry](https://docs.sigstore.dev/cosign/system_config/registry_support/#specifying-registry). This would complicate verification of the images, I don't quite know how that'd work. 
- Decline this PR and forgo the security of having signed containers (which would be a huge pity, security is important, especially in supply-chain tools like uv).

---

_Comment by @zanieb on 2024-10-30 15:53_

We're already pushing the `latest` tag separately for this reason, can we push the signature before that final image tag?

---

_Comment by @mjpieters on 2024-10-30 16:30_

> We're already pushing the `latest` tag separately for this reason, can we push the signature before that final image tag?

You push not only a tag, but a manifest as well. You'd have to separate the two steps; push the manifest without tags and then tag the digest of the manifest afterwards. It could be that you can retag the same digest; I'll try this out in my own fork registry.

---

_Comment by @samypr100 on 2024-10-30 17:25_

> > We're already pushing the `latest` tag separately for this reason, can we push the signature before that final image tag?
> 
> You push not only a tag, but a manifest as well. You'd have to separate the two steps; push the manifest without tags and then tag the digest of the manifest afterwards. It could be that you can retag the same digest; I'll try this out in my own fork registry.

I think the main issue here is how GH ranks packages and gives no control over the ordering to orgs/users of what's displayed, so we have to rely on these ordering hacks related to publishing.

---

_Comment by @mjpieters on 2024-10-30 17:30_

> I think the main issue here is how GH ranks packages and gives no control over the ordering to orgs/users of what's displayed, so we have to rely on these ordering hacks related to publishing.

and from my experiments it is clear that it is order that the manifest were pushed to the registry that is used to list them. I tried deleting then restoring, and retagging (moving tags to another manifest then back again to the right manifest), and nothing shifts the signature from the top.

So the only thing that would work is to build and  *not* push, sign (and let cosign push the signature manifest), and only then push the actual image manifest.

---

_Comment by @mjpieters on 2024-10-30 18:03_

I've tried to use the `load: true` option for `docker/build-push-action` to delay pushing, but that trips over docker/buildx#59. I don't quite see how to create a multi-arch container image and _not_ push this to the registry, my docker fu is failing me at the moment, sorry, I don't fully understand what a multi-arch build does and how it could be executed in separate steps. I'll have to drop investigating this, I fear.

---

_Comment by @mjpieters on 2024-10-30 18:07_

If I find time to pick this up again, there is an excellent overview of why you can't use the docker/build-push-action action to separate building and pushing at https://github.com/NASA-PDS/devops/issues/78#issuecomment-2430132011, which points to containerd as the way forward, but... that's not yet an official option.

---

_Comment by @samypr100 on 2024-10-30 19:40_

Agreed, I believe the existing tooling is limited for these types of scenarios.

---

_Comment by @mjpieters on 2024-11-06 11:07_

To be clear: this PR is ready in every other way. If not being able to pin the current base release container to the top is an issue, then please close this as I don't think we can practically do that _and_ provide signed containers.

---

_Comment by @zanieb on 2024-11-06 16:27_

I'm not sure how to balance the trade-offs with the regression, it seems pretty confusing to users to not see the actual image tag they'd use.

---

_Comment by @samypr100 on 2024-11-06 17:28_

@mjpieters Is it possible to push the signatures (.sig files) elsewhere, e.g. different package? Maybe I missed if this was discussed.

Given the GH limitations, that's probably the next step to explore?

---

_Comment by @mjpieters on 2024-11-14 09:56_

> @mjpieters Is it possible to push the signatures (.sig files) elsewhere, e.g. different package? Maybe I missed if this was discussed.
> 
> Given the GH limitations, that's probably the next step to explore?

Yes, it is technically possible to push them to a different registry, see the [cosign documentation](https://docs.sigstore.dev/cosign/system_config/registry_support/#specifying-registry). **However** I don't recommend doing this without first verifying that you can still verify the signature. I fear that the `cosign verify` tool won't be able to find the signature data when you do this and I don't know if or how you can then verify container integrity and provenance.

I do not currently have the time to test this option; you'd have to pull my PR into a fork repository, add the `COSIGN_REPOSITORY` environment variable to the signing steps pointing to a registry the github runner can access (this may require that the step is given a [personal access token with container write permissions](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry), although it [may be possible to grant write permissions across repos in the same organisation](https://docs.github.com/en/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package)), and then trying to verify the resulting container with `cosign verify ghcr.io/$FORK_OWNER/uv:latest --certificate-identity-regexp='.*' --certificate-oidc-issuer-regexp='.*'`. Perhaps that succeeds anyway, which would be very welcome news indeed.

---

_Comment by @samypr100 on 2024-11-21 01:49_

> > @mjpieters Is it possible to push the signatures (.sig files) elsewhere, e.g. different package? Maybe I missed if this was discussed.
> > Given the GH limitations, that's probably the next step to explore?
> 
> Yes, it is technically possible to push them to a different registry, see the [cosign documentation](https://docs.sigstore.dev/cosign/system_config/registry_support/#specifying-registry). **However** I don't recommend doing this without first verifying that you can still verify the signature. I fear that the `cosign verify` tool won't be able to find the signature data when you do this and I don't know if or how you can then verify container integrity and provenance.
> 
> I do not currently have the time to test this option; you'd have to pull my PR into a fork repository, add the `COSIGN_REPOSITORY` environment variable to the signing steps pointing to a registry the github runner can access (this may require that the step is given a [personal access token with container write permissions](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry), although it [may be possible to grant write permissions across repos in the same organisation](https://docs.github.com/en/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package)), and then trying to verify the resulting container with `cosign verify ghcr.io/$FORK_OWNER/uv:latest --certificate-identity-regexp='.*' --certificate-oidc-issuer-regexp='.*'`. Perhaps that succeeds anyway, which would be very welcome news indeed.

Thanks for the details, I will try take a look at some point. I assume this approach would introduce a new package (registry) called `uv-cosign` which would have to get publicly exposed next to `uv` and documented.

I also wonder, are there other projects that you know of that use ghcr.io + cosign? I'd like to see if this is a common issue and if there's other known workarounds that we may be missing when using cosign. I'll take a look as well.

---

_Comment by @mjpieters on 2024-11-21 20:30_

> I also wonder, are there other projects that you know of that use ghcr.io + cosign? I'd like to see if this is a common issue and if there's other known workarounds that we may be missing when using cosign. I'll take a look as well.

I looked, but all those that I found put their signatures straight into the GitHub container registry by their containers (and don't try and control what's that top listed entry).

---

_Comment by @mjpieters on 2025-01-29 17:17_

Right, there is an alternative available: use the native [Github attestations store](https://github.blog/news-insights/product-news/introducing-artifact-attestations-now-in-public-beta/) to store the signatures.

You can then use the `gh attestation` command to verify signatures:

```
gh attestation verify --owner astral-sh oci://ghcr.io/astral-sh/uv:latest
```

or you can download the attestation with `gh attestation download` and then use the resulting blob file to verify the manifest for an image (bit more involved as this requires downloading the blob _and_ the image manifest, here using [`regctl`](https://github.com/regclient/regclient/blob/main/docs/regctl.md)):

```
gh attestation download --owner astral-sh oci://ghcr.io/astral-sh/uv:latest
regctl manifest get ghcr.io/astral-sh/astral-sh:latest --format raw-body > manifest.json
cosign verify-blob-attestation --bundle "sha256:$(sha256sum manifest.json | awk '{ print $1 }').jsonl" --new-bundle-format --certificate-identity-regexp='^https://github\.com/astral-sh/uv/\.github/workflows/.*' --certificate-oidc-issuer=https://token.actions.githubusercontent.com manifest.json
```

I'll have to re-tool this PR to use the https://github.com/actions/attest-build-provenance action for this.

---

_@mjpieters reviewed on 2025-01-30 17:08_

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:43 on 2025-01-30 17:08_

I added this in here so this workflow would at least build the docker image in this PR. The jobs otherwise would just fail with a [*Password required* error](https://github.com/astral-sh/uv/actions/runs/13057047206/job/36430602580).

---

_Review requested from @zanieb by @mjpieters on 2025-01-30 17:11_

---

_Review requested from @samypr100 by @mjpieters on 2025-01-30 17:11_

---

_@zanieb approved on 2025-01-30 17:56_

Thank you! I might wait for @samypr100 to give it a look too.

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:285 on 2025-01-31 03:48_

```suggestion
          subject-name: ${{ env.UV_BASE_IMG }}
```

small nit

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:374 on 2025-01-31 03:49_

```suggestion
          subject-name: ${{ env.UV_BASE_IMG }}
```

small nit

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:43 on 2025-01-31 04:09_

Note, I think there's 3 other places in this file missing this conditional (if we're in favor of adding it)

---

_@samypr100 reviewed on 2025-01-31 04:09_

---

_Comment by @samypr100 on 2025-01-31 04:11_

Good news is that this approach does keep the tags working as-is.

I tried it on a test run here https://github.com/samypr100/uv/actions/runs/13066181068

Which did keep the package versions tags intact https://github.com/samypr100/uv/pkgs/container/uv

Verifications also work!

```shell
$ gh attestation verify --owner samypr100 oci://ghcr.io/samypr100/uv:latest                          [23:14:17]
Loaded digest sha256:ebd769519134c730eb1af61ddd06a8f5629f5082d0c2d8f02a6d577fe4b2a9dc for oci://ghcr.io/samypr100/uv:latest
Loaded 1 attestation from GitHub API
✓ Verification succeeded!

sha256:ebd769519134c730eb1af61ddd06a8f5629f5082d0c2d8f02a6d577fe4b2a9dc was attested by:
REPO          PREDICATE_TYPE                  WORKFLOW
samypr100/uv  https://slsa.dev/provenance/v1  .github/workflows/build-docker.yml@refs/heads/cosign-docker-images

$ gh attestation verify --owner samypr100 oci://ghcr.io/samypr100/uv:0.5.26                          [23:14:23]
Loaded digest sha256:ebd769519134c730eb1af61ddd06a8f5629f5082d0c2d8f02a6d577fe4b2a9dc for oci://ghcr.io/samypr100/uv:0.5.26
Loaded 1 attestation from GitHub API
✓ Verification succeeded!

sha256:ebd769519134c730eb1af61ddd06a8f5629f5082d0c2d8f02a6d577fe4b2a9dc was attested by:
REPO          PREDICATE_TYPE                  WORKFLOW
samypr100/uv  https://slsa.dev/provenance/v1  .github/workflows/build-docker.yml@refs/heads/cosign-docker-images

$ gh attestation verify --owner samypr100 oci://ghcr.io/samypr100/uv:0.5                             [23:14:27]
Loaded digest sha256:ebd769519134c730eb1af61ddd06a8f5629f5082d0c2d8f02a6d577fe4b2a9dc for oci://ghcr.io/samypr100/uv:0.5
Loaded 1 attestation from GitHub API
✓ Verification succeeded!

sha256:ebd769519134c730eb1af61ddd06a8f5629f5082d0c2d8f02a6d577fe4b2a9dc was attested by:
REPO          PREDICATE_TYPE                  WORKFLOW
samypr100/uv  https://slsa.dev/provenance/v1  .github/workflows/build-docker.yml@refs/heads/cosign-docker-images

$ gh attestation verify --owner samypr100 oci://ghcr.io/samypr100/uv:python3.13-bookworm             [23:14:41]
Loaded digest sha256:5537896c0c8bd2351350d32dea35c590997c418e954fb5b31d6fcbfd96126562 for oci://ghcr.io/samypr100/uv:python3.13-bookworm
Loaded 1 attestation from GitHub API
✓ Verification succeeded!

sha256:5537896c0c8bd2351350d32dea35c590997c418e954fb5b31d6fcbfd96126562 was attested by:
REPO          PREDICATE_TYPE                  WORKFLOW
samypr100/uv  https://slsa.dev/provenance/v1  .github/workflows/build-docker.yml@refs/heads/cosign-docker-images

...
```

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:43 on 2025-01-31 10:06_

I didn't think there was any point as those 3 all are part of jobs that only run on release.

---

_@mjpieters reviewed on 2025-01-31 10:06_

---

_Review requested from @samypr100 by @mjpieters on 2025-01-31 12:47_

---

_@zanieb approved on 2025-01-31 14:59_

---

_Label `releases` added by @zanieb on 2025-01-31 14:59_

---

_Label `security` added by @zanieb on 2025-01-31 14:59_

---

_Merged by @zanieb on 2025-01-31 15:00_

---

_Closed by @zanieb on 2025-01-31 15:00_

---

_Comment by @zanieb on 2025-01-31 15:01_

Would you mind following with a brief guide on how to verify images in https://docs.astral.sh/uv/guides/integration/docker/ ?

---

_Renamed from "Sign docker images using cosign" to "Sign docker images using github attestations" by @mjpieters on 2025-01-31 15:01_

---

_Branch deleted on 2025-01-31 15:54_

---

_Comment by @mjpieters on 2025-01-31 15:54_

> Would you mind following with a brief guide on how to verify images in https://docs.astral.sh/uv/guides/integration/docker/ ?

I'll see what I can do!

---

_Comment by @mjpieters on 2025-01-31 19:16_

@zanieb PR up at #11140

---

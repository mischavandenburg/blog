---
Title: Building Multiple Docker Images Using Automatic Versioning Using GitHub Actions
date: 2024-04-30
tags:
- Article
---

# Introduction

I'm working on a project where I'm migrating an Azure Pipeline to GitHub Actions. So far I've found the GitHub Actions very intuitive to work with and it was a very easy transition from Azure Pipelines.

One requirement was to increase the version with every build. In the previous setup they were using the build ID, but I'm an advocate of always using semantic versioning if possible, so I wondered if this could be done using the GitHub Actions.

This setup increments all of the Docker images regardless if they have any changes. I did not like this way of deployment, but it was the requirement from the project so I had to implement it that way. I would have preferred to version each container image separately.

# Increasing Versions

The first problem I had to solve was to increase the tags on every commit.

I used the following action from the marketplace for increasing the versions based on conventional commits:

[Semver Conventional Commits · Actions · GitHub Marketplace](https://github.com/marketplace/actions/semver-conventional-commits)

In the example below, I'm running a separate job to increase the version based on the latest tag of the repository. The job has an output which I can use later to tag the images themselves.

```yaml
jobs:
  prepare_tag:
    outputs:
      tag: ${{ steps.semver.outputs.next }}
    permissions:
      contents: write

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      # Defaults to patch version, will bump minor on "feat" commit

      - name: Get Next Version
        id: semver
        uses: ietf-tools/semver-action@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          branch: master
          patchAll: true

      - name: Push tag
        id: tag_version
        uses: mathieudutour/github-tag-action@v6.2
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          custom_tag: ${{ steps.semver.outputs.next }}
          tag_prefix: ""

```

The Get Next Version step increments the version based on the configuration provided. I used the `patchAll: true` setting to always default on a patch incrementation if the word "feat" is not in the commit. If the commit would contain "BREAKING CHANGE", the major version would be incremented.

To push the new version to the current repo, I used this action:

[GitHub Tag · Actions · GitHub Marketplace](https://github.com/marketplace/actions/github-tag)

Since the semver-action already adds a "v" to the tag, I adjusted the `tag_prefix` to `""`.

# Managing Permissions


- On the main page of your organization, go to Packages
- Go to the package and open "Package settings" in the right sidebar
- Under Manage Actions access, add the repo as the source
- Make sure to allow Write acces

![](/gha2.png)

In the source repo, where the GitHub Actions Workflow is running, go to settings, actions, select the workflow, and add write permission there too.

![](/gha1.png)

Finally, we also need to add permissions in the workflow yaml:

```yaml
build_and_push:
    name: Build image & push
    runs-on: ubuntu-latest
    permissions:
      contents: write
      packages: write
```

See also:

[github actions - ERROR: denied: installation not allowed to Create organization package - Stack Overflow](https://stackoverflow.com/questions/76607955/error-denied-installation-not-allowed-to-create-organization-package)

[denied: installation not allowed to Create organization package · Issue #606 · docker/build-push-action (github.com)](https://github.com/docker/build-push-action/issues/606)

# Looping over multiple Dockerfiles

The last problem I needed to solve was that I needed build multiple images. To make it a bit cleaner, I didn't want to have a code block for each. After some research I found a way to loop over multiple values in GitHub Actions using matrices.

In the example below, I set three variables in the matrix and each of these are called in the Build and push step.

```yaml
  build_and_push:
    permissions:
      contents: write
      packages: write

    runs-on: ubuntu-latest

    strategy:
      matrix:
        include:
          - image: ghcr.io/ssi-dk/sap-web
            dockerfile: app/Dockerfile
            path: app
          - image: ghcr.io/ssi-dk/sap-api
            dockerfile: web/Dockerfile
            path: web
          - image: ghcr.io/ssi-dk/bifrost-queue-broker
            dockerfile: bifrost/bifrost_queue_broker/Dockerfile
            path: bifrost/bifrost_queue_broker

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Login to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      # loops over all images in the matrix defined on top

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: ${{ matrix.path }}
          platforms: linux/amd64
          tags: ${{ matrix.image }}:${{ needs.prepare_tag.outputs.tag }}
          file: ${{ matrix.dockerfile }}
          push: true
```

This was a very fun challenge to solve and I have learned a lot about GitHub actions. It didn't take much time at all to figure all of this out because all of the actions in the marketplace are open source and you can just read the code to see if they suit your needs. So far I'm very impressed with GitHub Actions, coming from Azure Pipelines.





## Links:

202404301804

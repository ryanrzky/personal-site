---
title: "How to Release a Service"
date: 2024-06-23
author: "Ryan Rizky Diantoro"
description: "How to release a service"
draft: false
categories: [DevOps]
---

In this post, I will explain how to release service using Semantic Versioning.

### What is Service?
In making an application some components make the application run properly. For example in an e-commerce application, there is a login process (login service), after login, we can see the dashboard (dashboard service), and when we want to buy goods we will checkout and make payments (payment service).
Login, Dashboard, and Payment are what we call service.

### What is Release?
Release is the process of releasing a service. Before releasing a service, the service must usually undergo a testing and validation process to determine whether the service is ready to use.

### What is Semantic Versioning?
Semantic Versioning commonly abbreviated as SemVer is a way to give a version number to a service with the format [MAJOR].[MINOR].[PATCH]. example: 1.0.1
A full explanation can be read [here](https://semver.org/).

### Why use Semantic Versioning?
If there are many services in an application, SemVer will make it easier for us to manage dependencies or compatibility between services.

## How to start
It's quite simple to release a service using SemVer, we only need to do `git tag X.X.X` on our service repository.

However, I will not do that in this post. Instead, I will use the npm package called `standard-version`. 

`standard-version` is a tool to create a version using SemVer and generate a `CHANGELOG` file supported by [Conventional Commits.](https://www.conventionalcommits.org/en/v1.0.0/) More details about the `standard-version` can be found [here](https://www.npmjs.com/package/standard-version).

## Step by Step
Before releasing using the `standard-version`, make sure the commit follows the Conventional Commit Message format.

Install `standard-version`
```bash
npm install -g standard-version
```

In the service repository, create 2 files. `version.json` and `.versionrc` and initialize the tag.

`version.json` file.
```json
{
    "version": "0.1.0"
}
```
`.versionrc`
```json
{
  "bumpFiles": [
    {
      "filename": "version.json",
      "type": "json"
    }
  ],
  "releaseCommitMessageFormat": "chore(release): release {{currentTag}}"
}
```

Tag initialization.
```bash
git tag v0.1.0
```

If all those steps above are done, test by committing. Example:
```bash
// Make changes to the service repository, then commit the changes.
git add . && git commit -m "feat: test update feature"

// Then update the service version.
standard-version

// Check if there are new tag and new commit.
git tag -l
git log --oneline
```

Done, we successfully released the service.
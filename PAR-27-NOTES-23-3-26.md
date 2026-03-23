# NOTES: PAR-27 Pubilsh, version, consume, CA and public reg pubishing of packages

- what do flows look like?

## npm publish

https://docs.aws.amazon.com/codeartifact/latest/ug/packages-overview.html#package-publishing


**NOTE: In this article the use of a  `prepare` script that pre auths against CA before running publish**
https://aws.amazon.com/blogs/devops/publishing-private-npm-packages-aws-codeartifact/

*excerpt:*
Add a prepare script to our package.json to run our login command:

```json
{
  "name": "@myorg/my-package",
  "version": "1.0.0",
  "description": "A sample private scoped npm package",
  "main": "index.js",
  "scripts": {
    "prepare": "npm run co:login",
    "co:login": "aws codeartifact login --tool npm --repository my-repo --domain my-domain",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

**TODO: Could we add further npm command hooks to pre auth for other commands, for example: `i` `ci` `publish`**?

## npm version

npm version is fully supported. [npm-version cli docs](https://docs.npmjs.com/cli/v7/commands/npm-version) has relevant developer documentation.  Developers should also account for supported and unsupported commands

[npm semantic versioning docs](https://docs.npmjs.com/about-semantic-versioning)

## other supported npm commands

other supported npm commands [can be found in AWS documentation](https://docs.aws.amazon.com/codeartifact/latest/ug/npm-commands.html#supported-commands-that-interact-with-a-repository)

## unsupported commands

Many npm admin and auth level commands are not supported:
access
adduser
audit
hook
login
logout
whoami
owner
profile
search
token
unpublish - can use `delete-package-version` in CA.

Commands developers may use will become redundant when using CodeArtifact.

**We need to have clear documentation around this (for each impacted ecosystem) and any replacement commands should be declared.**

https://docs.aws.amazon.com/codeartifact/latest/ug/npm-commands.html#unsupported-commands

## versioning

Versioning packages happens outside of CA.
Versioning flows should remain unchanged to how we'd version packages for public registries.
For example npm pacakge versions must still conform to [SemVer](https://semver.org/)

https://docs.aws.amazon.com/codeartifact/latest/ug/codeartifact-concepts.html#welcome-concepts-package-namespace

**EVIDENCE:** Attached screens to PAR-27 for AWS console evidence showing newly versioned package as `Published` in CodeArtidfact.

- versioned locally using `npm version patch`

- published using:  `npm pubish`

- proved published to AWS CA.

### Package version status

Pacakge version status is specific to package statuses in CodeArtifact and not related to versioning packages, for example `npm version` is specific to package versioning and not related to CA.
Describes the current state and availability of a package version.
We can change the state of status in CLI, SDK, etc.

#### Status states

**Published** - Published on CA. downloadable, listed

**Unfinished** - clioent has uploaded one or more package assets, but hasn't finalised to `Published` state.

**Unlisted** - Downloadable, but version not listed. e.g. `npm view <package-name>` will not include package version. If an unlisted package is already referrenced in a project `package-lock.json` then it can still be downloaded and installed, for example running `npm ci`

**Archived** - no longer downloadable. won't be included in listed versions from package managers. consumption is blocked on clients. if you update an existing package to an archived version, it will break builds. Can change state back to Unlisted or Published.

**Disposed** - Is permanently deleted by CA. No longer downloadable. won't be included in listed versions from package managers. consumption is blocked on clients. Disposed are no longer billed for storage.

https://docs.aws.amazon.com/codeartifact/latest/ug/packages-overview.html#package-version-status

`describe-package-version` - Package version details are extracted from a package when it is published to CodeArtifact. The details in different packages vary and depend on their formats and how much information their authors added to them.

https://docs.aws.amazon.com/codeartifact/latest/ug/describe-package-version.html



## AWS CodeArtifact Commands examples

https://docs.aws.amazon.com/cli/latest/userguide/cli_codeartifact_code_examples.html

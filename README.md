# artifact-spike-package

Minimal internal npm package used for artifact registry spike work.

This repo exists to support tsting of:

- package publishing
- package installation/consumption
- versioning behaviour
- CI install/build flows
- private registry and upstream/proxy behaviour

This is not production code.


## Package details

Purpose:

- publish to candidate artifact registries.
- consume from a separate test consumer repo
- verify basic versioning and package resolution flows

## Local usage

Install dependencies: 

- `npm ci` existing packages 
- `npm i <YOUR_PACKAGE_TO_INSTALL_HERE>`

## Notes

This package is intentionally minimal.
It should remain small and simple unless the team decides to additional scenarios are required for the spike, such as:

- monorepo/workspace
- additional package ecosystems.

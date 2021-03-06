# Login.gov Design System

## Installation and usage

See [the documentation](https://design.login.gov/) for installation and usage instructions.

## Configuring for development

The following dependencies are required to build the documentation and assets within this repository:

- [Ruby](.ruby-version)
- [Node.js](.nvmrc)

After satisfying the above language dependencies and cloning this repository, install package dependencies:

```
npm install
bundle install
```

In development, build the documentation site with assets, watch source files for changes, and serve the compiled site at [localhost:4000](http://localhost:4000) by running:

```
npm start
```

## Linting

[Lint](https://en.wikipedia.org/wiki/Lint_(software)) JavaScript and Sass files in `src/` by running:

```
npm run lint
```

This project uses [Prettier](https://prettier.io/) to format code. When running the lint command above, you may notice errors relating to unexpected code formatting. It's recommended that you install [an editor integration](https://prettier.io/docs/en/editors.html) to automatically format code on save, but you can also resolve these errors automatically from the command-line by running:

```
npm run lint -- --fix
```

## Visual regression tests

When a pull request is submitted, a visual regression test will be automatically run to check for any visual changes between the working copy of the branch and the live documentation site. These will be reported as the `ci/circleci: visual-regression` GitHub status check.

A failure of this status check only indicates that a visual change was detected. Depending on the types of changes being proposed, this may be expected. Anyone with access to the CircleCI dashboard can review the specific changes by following the status check "Details" link and comparing the set of screenshots under the "Artifacts" tab. If the visual changes are acceptable, the pull request can be merged, even if the status check is reported as a failure.

## Deploying documentation updates

Documentation deploys are performed automatically upon merging to `main` by [Federalist](https://federalist.18f.gov/). Federalist performs the following steps:

- `npm install --production` (a no-op, as this package has no production dependencies)
- `npm run federalist`
- `bundle install`
- `bundle exec jekyll build`

More information can be found in Federalist’s [How Builds Work](https://federalist-docs.18f.gov/pages/how-federalist-works/how-builds-work/).

## Publishing a release to `npm`

When you're ready to release a new version of the `identity-style-guide` package there are just a few steps to take.

Before starting, make sure that all changes intended for release should be merged into the `main` branch. You will need permissions to publish the package to npm. Check current package owners by running `npm owner ls` or by consulting the list of admins through the [Services and Accounts handbook page](https://handbook.login.gov/articles/accounts.html). If you do not have access, contact an owner to have access granted or to publish on your behalf.

1. Check out the latest main branch on your local machine by running `git checkout main`, followed by `git pull`.
2. Decide the version number for the new release.
   - The `CHANGELOG.md` should ideally include all pending changes under an "Unreleased" heading.
   - This project uses [semantic versioning](https://semver.org/): breaking changes should bump the major version, backwards-compatible changes should bump the minor version, and bug fixes should bump the patch version.
3. Since the main branch is protected, you will need to bump the version in a new branch and open a pull request. Start by creating a new branch.
   - Example: `git checkout -b release-4-3-1`
4. Change the "Unreleased" heading in `CHANGELOG.md` to the version you decided in Step 3. Commit this change to your new branch.
5. Run `npm version` to bump the package version, passing one of `patch`, `minor`, or `major` depending on what you had decided in Step 3 for the next version.
   - Example: `npm version patch`
   - A new version will be created. This will update `package.json` and `package-lock.json` automatically and create a commit.
6. Do a trial publish by running `npm publish --dry-run`.
   - No need to run any special build steps — the publish script will lint the source JavaScript and Sass files, and clean and re-build all assets before including them in the published package.
   - Consider: In the files listed, are there any that should or shouldn't be included? Does the version match what you expect?
7. If everything looks alright, continue with publishing by running `npm publish`.
8. Push your release branch to the GitHub repository and open a pull request.
9. Once approved and merged, create a new release on the [GitHub "Releases" page](https://github.com/18F/identity-style-guide/releases).
   - Use `main` as the target.
   - The release version should match the version just published to `npm` (for example, `v2.1.5`).
   - Use the version name as the release title.
   - Use the release notes to link to any important issues or pull requests that were addressed in the release. You may copy this from `CHANGELOG.md`.

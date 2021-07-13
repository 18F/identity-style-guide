# Login.gov Design System

This repository contains the code for the Login.gov Design System. The documentation in here includes resources for both how to install locally, but also in various development environments.

1. [Installation](#installation)
1. [Testing](#testing)
1. [Deploying documentation updates](#deploying-documentation-updates)
1. [Publishing a release to `npm`](#publishing-a-release-to-npm)
1. [Use with Rails](#use-with-rails)
1. [Use as a JavaScript package](#use-as-a-javascript-package)

## Installation (Local)

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

## Testing

### Linting

[Lint](https://en.wikipedia.org/wiki/Lint_(software)) JavaScript and Sass files in `src/` by running:

```
npm run lint
```

This project uses [Prettier](https://prettier.io/) to format code. When running the lint command above, you may notice errors relating to unexpected code formatting. It's recommended that you install [an editor integration](https://prettier.io/docs/en/editors.html) to automatically format code on save, but you can also resolve these errors automatically from the command-line by running:

```
npm run lint -- --fix
```

### Visual regression testing

When a pull request is submitted, a visual regression test will be automatically run to check for any visual changes between the working copy of the branch and the live documentation site. These will be reported as the `ci/circleci: visual-regression` GitHub status check.

A failure of this status check only indicates that a visual change was detected. Depending on the types of changes being proposed, this may be expected. Anyone with access to the CircleCI dashboard can review the specific changes by following the status check "Details" link and comparing the set of screenshots under the "Artifacts" tab. If the visual changes are acceptable, the pull request can be merged, even if the status check is reported as a failure.

## Deploying documentation updates

Documentation deploys are performed automatically upon merging to `main` by [Federalist](https://federalist.18f.gov/). Federalist performs the following steps:

- `npm install --production` (a no-op, as this package has no production dependencies)
- `npm run federalist`
- `bundle install`
- `bundle exec jekyll build`

More information can be found in Federalist’s [How Builds Work](https://federalist-docs.18f.gov/pages/how-federalist-works/how-builds-work/).

## Releases

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

## Use with Rails

 The SCSS files natively support `asset-path()` out-of-the-box for ease of use with the Rails Asset Pipeline. To use with Rails, configure Rails to look for assets in both `node_modules` and the identity-style-guide module:

 ```ruby
 # config/initializers/assets.rb

 Rails.application.config.assets.paths << Rails.root.join('node_modules')
 Rails.application.config.assets.paths << Rails.root.join('node_modules/identity-style-guide/dist/assets')
 ```

 Finally, import the styles into your main stylesheet:

 ```scss
 // app/assets/stylesheets/application.css.scss

 $font-path: 'fonts';
 $image-path: 'img';

 @import 'identity-style-guide/dist/assets/scss/styles';
 ```

 If you're using Sprockets and precompiling assets you'll need to update your
 Sprockets manifest to include the images and fonts for production:

 ```js
 // app/assets/config/manifest.js

 //= link_tree ../../../node_modules/identity-style-guide/dist/assets/img
 //= link_tree ../../../node_modules/identity-style-guide/dist/assets/fonts
 ```

 Unfortunately, this results in the assets being saved under paths that include
 everything after the `node_modules` directory, so a helper method is useful in
 cleaning up the views:

 ```ruby
 # app/helpers/assets_helper.rb

 module AssetsHelper
   def login_design_asset_path(path)
     "identity-style-guide/dist/assets/#{path}"
   end
 end
 ```

 ```erb
 # app/views/foo.html.erb

 <%= image_tag login_design_asset_path('img/us_flag_small.png'), ... %>

 # app/views/layouts/application.html.erb

 <head>
   ...
   <%= favicon_link_tag identity_asset_path('img/favicons/favicon.ico') %>
 </head>
 ```

 Finally, if you're using Webpacker you'll need to reference the JS files in the
 default pack:

 ```js
 // app/javascript/packs/application.js

 require("identity-style-guide/dist/assets/js/main")
 ```

## Use as a JavaScript package

  If you're already using a JavaScript bundler in your project, you can import specific component implementations from the `identity-style-guide` package. Most modern bundlers that support dead-code elimination will automatically optimize the bundle size to include only the code necessary in your project.

  ```js
  import { accordion } from 'identity-style-guide';

  accordion.on();
  ```

  Note that unlike the pre-built JavaScript assets found in the `dist/assets` directory, importing the package from NPM will not automatically initialize the components on the page or include polyfills necessary to support older browsers. You will have to call the `on()` method for each component you import.

  If you need support for older browsers in your project, it's suggested you import polyfills shipped with the `uswds` package and import it before any components:

  ```
  npm install uswds
  ```

  ```js
  import 'uswds/src/js/polyfills';
  import { accordion } from 'identity-style-guide';
  ```
